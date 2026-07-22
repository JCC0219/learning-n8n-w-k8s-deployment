portfolio-agent/
│
├── AGENT_PROMPT.md              # n8n System Prompt
│
├── rag-knowledge/               # 只放进 Pinecone 的知识库
│   │
│   ├── profile/                 # 我是谁
│   │   └── about_me.md
│   │
│   ├── experience/              # 工作经历
│   │   └── work_history.md
│   │
│   ├── projects/                # 项目经验（最重要）
│   │   ├── project_1.md
│   │   ├── project_2.md
│   │   └── architecture_cases.md
│   │
│   ├── skills/                  # 技术能力
│   │   ├── cloud.md
│   │   ├── programming.md
│   │   ├── devops.md
│   │   └── certifications.md
│   │
│   ├── interview/               # 面试准备
│   │   └── architecture_questions.md
│   │
│   ├── personal/                # 非技术人格信息
│   │   └── interests.md
│   │
│   └── assets/                  # 图片、diagram、附件
│
└── README.md                    # 项目说明（不进 RAG）