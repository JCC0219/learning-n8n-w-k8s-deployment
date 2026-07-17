# n8n - Workflow Automation

n8n deployment with automatic backup restore from Supabase.

## 📁 Files

- `deployment.yaml` - n8n pod with auto-restore initContainer
- `service.yaml` - ClusterIP service (port 5678)
- `pvc.yaml` - 10Gi persistent storage for workflows
- `configmap.yaml` - n8n configuration + backup URL
- `secret.yaml.example` - SMTP password template
- `README.md` - This file

## 🚀 Quick Start

### 1. Create secret

```bash
cp kubernetes/base/backend/n8n/secret.yaml.example \
   kubernetes/base/backend/n8n/secret.yaml
# Edit with your SMTP password: gbmyvxejasqgvwbz
```

### 2. Update backup URL (if needed)

Edit `configmap.yaml` and update `N8N_BACKUP_URL` with your Supabase signed URL.
Current URL is already set and valid until 2125.

### 3. Deploy

```bash
kubectl apply -k kubernetes/overlays/preview
```

**That's it!** The initContainer automatically:
- Downloads backup from Supabase
- Extracts to `/home/node/.n8n`
- Sets permissions
- Creates `.restored` marker (skips restore on subsequent restarts)

## 🔄 How Auto-Restore Works

On first pod start:
```
1. InitContainer checks if /data/.restored exists
2. If not → wget backup from Supabase URL
3. Extract tar to PVC
4. chown -R 1000:1000 (n8n user)
5. touch /data/.restored
6. n8n container starts with your workflows
```

On subsequent restarts:
```
1. InitContainer sees .restored marker → skips
2. n8n starts immediately
```

## 🔗 Access

**Internal:** `http://n8n-service-preview.appointment-ai-preview.svc.cluster.local:5678`

**External (via Cloudflare Tunnel):**
- Service: `n8n-service-preview:5678`
- Hostname: `acn8n.alvinchiew.com`

## 📊 Resources

- **Requests:** 256Mi RAM, 200m CPU
- **Limits:** 512Mi RAM, 500m CPU
- **Storage:** 10Gi PVC (adjust in `pvc.yaml` if needed)

## 🔐 Configuration

**ConfigMap** - n8n settings, SMTP config, backup URL
**Secret** - SMTP password only
**PVC** - Workflows persist across restarts

## 💡 Tips

### Update backup URL
Edit `configmap.yaml` → change `N8N_BACKUP_URL`, then:
```bash
kubectl apply -k kubernetes/overlays/preview
```

### Skip auto-restore
Remove or comment out `N8N_BACKUP_URL` in `configmap.yaml`.
The initContainer will skip (key is `optional: true`).

### Create new backup
```bash
N8N_POD=$(kubectl get pod -n appointment-ai-preview -l app=n8n -o jsonpath='{.items[0].metadata.name}')
kubectl exec -n appointment-ai-preview $N8N_POD -- \
  tar czf /tmp/backup.tar.gz -C /home/node/.n8n .
kubectl cp appointment-ai-preview/$N8N_POD:/tmp/backup.tar.gz \
  ./n8n_backup_$(date +%Y%m%d).tar.gz
```

Upload to Supabase, get new signed URL, update ConfigMap.

### Force re-restore
```bash
kubectl exec -n appointment-ai-preview -l app=n8n -- rm /home/node/.n8n/.restored
kubectl rollout restart deployment n8n-preview -n appointment-ai-preview
```

## ⚠️ Important Notes

- **Preview only:** n8n is deployed in preview environment
- **One replica:** Stateful data requires single pod (strategy: Recreate)
- **PVC:** Data survives pod restarts, persists in `n8n-data-preview`
- **Supabase URL:** Signed URL expires in 2125 (you're good for ~100 years!)
- **Storage limit:** Ensure PVC size (10Gi) fits your workflow data

