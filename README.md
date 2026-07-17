# Self-hosting n8n on Kubernetes

This repository deploys a single [n8n](https://n8n.io/) instance to a Kubernetes cluster (such as K3s). n8n runs in the `n8n` namespace and stores its application data in a Supabase PostgreSQL schema.

## What is included

| File | Purpose |
| --- | --- |
| `n8n/namespace.yaml` | Creates the `n8n` namespace. |
| `n8n/configmap.yaml` | Non-sensitive n8n, SMTP, webhook, and database settings. |
| `n8n/secret.yaml.example` | Template for the SMTP password, encryption key, and database password. |
| `n8n/deployment.yaml` | One n8n pod, resource settings, and health probes. |
| `n8n/service.yaml` | Internal ClusterIP service on port `5678`. |

## Prerequisites

- A Kubernetes cluster and `kubectl` configured for it.
- A Supabase PostgreSQL database reachable from the cluster.
- A DNS name and reverse proxy/tunnel if n8n will receive public webhooks.

## Configure

1. Review [`n8n/configmap.yaml`](n8n/configmap.yaml) and update values for your environment, especially:

   - `N8N_HOST` and `WEBHOOK_URL`
   - `GENERIC_TIMEZONE`
   - the SMTP settings
   - Supabase PostgreSQL host, user, database, and schema

2. Create the real secret locally. It is intentionally not committed.

   ```bash
   cp n8n/secret.yaml.example n8n/secret.yaml
   ```

3. Edit `n8n/secret.yaml` and set:

   - `DB_POSTGRESDB_PASSWORD` to the Supabase database password.
   - `N8N_ENCRYPTION_KEY` to a long, random, stable secret. Do not change it after creating credentials in n8n; existing credentials rely on it.
   - `N8N_SMTP_PASS` to the SMTP account or app password.

   Keep `n8n/secret.yaml` out of source control. Prefer a secrets manager or a sealed/external secret for shared or production deployments.

## Deploy

Run the following from the repository root:

```bash
kubectl apply -f n8n/namespace.yaml
kubectl apply -f n8n/configmap.yaml
kubectl apply -f n8n/secret.yaml
kubectl apply -f n8n/deployment.yaml
kubectl apply -f n8n/service.yaml
```

Wait for n8n to become ready:

```bash
kubectl rollout status deployment/n8n -n n8n
kubectl get pods,service -n n8n
```

For a local check without exposing the service publicly:

```bash
kubectl port-forward -n n8n service/n8n-service 5678:5678
```

Then open `http://localhost:5678` and complete n8n's initial owner setup.

## Public access and webhooks

The supplied service is `ClusterIP`, so it is reachable only inside the cluster at:

```text
http://n8n-service.n8n.svc.cluster.local:5678
```

To receive external webhooks, configure an Ingress, reverse proxy, or tunnel to forward HTTPS traffic to `n8n-service:5678`. The public hostname must match `N8N_HOST`, and `WEBHOOK_URL` must be the complete public HTTPS URL. Because n8n is configured with `N8N_PROXY_HOPS=1`, place exactly one trusted proxy hop in front of it or adjust that value to suit your topology.

## Operations

View status and logs:

```bash
kubectl get all -n n8n
kubectl logs -n n8n deployment/n8n --follow
kubectl describe pod -n n8n -l app=n8n
```

After changing the ConfigMap or Secret, restart the pod so environment variables are reloaded:

```bash
kubectl apply -f n8n/configmap.yaml
kubectl apply -f n8n/secret.yaml
kubectl rollout restart deployment/n8n -n n8n
kubectl rollout status deployment/n8n -n n8n
```

## Important considerations

- This manifest uses one replica. n8n queue mode and horizontal scaling need additional Redis and worker configuration.
- Workflow data and credentials live in PostgreSQL; back up the database and retain the encryption key securely.
- The deployment currently uses a rolling-update strategy. With a single replica, Kubernetes may briefly run two n8n pods during an update. If this causes duplicate workflow execution in your use case, use a `Recreate` strategy or plan a queue-mode deployment.
- `N8N_SECURE_COOKIE` is set to `false` for the present configuration. Set it to `true` when serving n8n over HTTPS through a correctly configured proxy.
- Pin and regularly review the n8n image version before upgrading production workloads.

## Remove

To remove all resources created by this repository:

```bash
kubectl delete -f n8n/service.yaml
kubectl delete -f n8n/deployment.yaml
kubectl delete -f n8n/secret.yaml
kubectl delete -f n8n/configmap.yaml
kubectl delete -f n8n/namespace.yaml
```

Deleting the namespace removes all resources in it. It does not delete the Supabase database or its data.
