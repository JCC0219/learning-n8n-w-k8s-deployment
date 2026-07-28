# Appointment AI

## Side Project Overview

Appointment AI is a full-stack AI-powered sales and appointment management platform designed to help businesses reduce lost leads, recover cold customers, and improve appointment conversion.

The platform combines AI-powered customer engagement, automated sales workflows, appointment scheduling, lead assignment, WhatsApp communication, and centralized sales management into a single system.

Application:
https://appointment-ai.alvinchiew.com/#/login


## Project Status

- Started: July 2025
- Status: Present / Ongoing
- Type: Private Team Project
- Role: Full-stack Developer / Cloud Engineer / System Architect


## Core Problems

The platform was designed to address common sales workflow problems such as:

- Customer inquiries being missed or responded to too slowly.
- Cold leads not receiving timely follow-ups.
- Sales teams manually managing appointments and leads.
- Difficulty identifying high-value prospects.
- Manual customer broadcasting and follow-up processes.
- Fragmented systems across messaging, sales workflows, databases, and internal services.


## Key Features

### AI Customer Engagement and Profiling

The platform supports automated customer inquiry handling and customer profiling.

The system processes customer interactions and relevant customer information to support lead classification and sales workflows.

AI capabilities are integrated into the sales workflow to help businesses respond to customer inquiries continuously and provide more context-aware customer engagement.

The goal is to help identify potentially valuable leads more efficiently and reduce the amount of manual work required from sales teams.


### Automated Sales Funnel and Follow-ups

The platform automates follow-up workflows for leads and customers.

Workflow automation and backend services can trigger follow-up processes based on business logic and customer activity.

This helps reduce manual sales operations and ensures that potential customers can be re-engaged through automated follow-up workflows.


### AI Smart Booking and Lead Assignment

The platform supports automated appointment scheduling and lead assignment.

The system manages appointment availability and distributes incoming leads to sales personnel based on predefined business logic.

The goal is to ensure that new and high-intent inquiries can be handled quickly and assigned to the appropriate sales personnel.


### Instant Blasting and AI Studio

The platform supports personalized broadcast messaging for customer outreach.

The AI Studio integrates AI capabilities with product and business knowledge to support different sales and communication scenarios.

This allows AI-powered workflows and customer interactions to be adapted based on business context and product information.


## System Architecture

The application is a full-stack system consisting of a Flutter Web frontend, Node.js microservices, self-hosted Supabase, PostgreSQL, Redis, BullMQ, n8n, WhatsApp integration services, and AI services.

The current infrastructure is self-hosted on Ubuntu servers and orchestrated using a self-hosted K3s Kubernetes cluster.


### Frontend

- Flutter Web


### Application Layer

The backend uses Node.js and Express.js microservices to implement application logic and API services.

REST APIs are used to connect the frontend with backend services and external integrations.

The backend services handle application logic, customer data, authentication, sales workflows, messaging integrations, and communication with external services.


### Database

The platform uses Supabase and PostgreSQL as the primary data layer.

Supabase provides backend services including:

- PostgreSQL database
- Authentication
- Storage
- Edge Functions

The PostgreSQL environment is managed using CloudNativePG (CNPG) within the Kubernetes cluster.

CloudNativePG provides Kubernetes-native PostgreSQL management and supports database high-availability, lifecycle management, and backup integration.


### Authentication

The platform uses Google OAuth 2.0 for user authentication.

Supabase Auth (GoTrue) manages authentication and authorization using JWT-based access tokens.

Authentication is integrated across the frontend and backend services to control access to protected APIs and application features.


## AI and Agentic Development

n8n is a core component of the platform's automation and AI architecture.

The platform uses n8n not only for traditional workflow automation but also for AI Agentic Development and AI-powered workflow orchestration.

n8n's AI capabilities integrate with LangChain-based components and concepts, allowing AI agents to interact with tools, external services, workflows, and business logic.

This enables the development of AI-driven workflows where AI can be connected with application services and automation logic rather than being used only as a standalone chatbot.

Examples of AI and agentic workflows include:

- AI-powered customer engagement
- Customer inquiry handling
- Customer profiling
- Product and business knowledge integration
- AI-assisted sales workflows
- Automated follow-up processes
- AI-driven decision-making within business workflows

n8n is used as the orchestration layer connecting AI capabilities with backend services, APIs, databases, messaging systems, and business workflows.

This allows AI agents and automated workflows to interact with the wider application ecosystem.


## Workflow Automation

n8n is used to implement workflow automation, AI agent workflows, and business process orchestration.

The workflows connect different services and automate processes such as:

- Customer follow-ups
- Lead processing
- Customer messaging
- Sales workflows
- Import processes
- Broadcast messaging
- AI-powered customer interactions
- AI agent workflows
- Integration between AI services and backend APIs

Instead of implementing every automation workflow directly inside application code, n8n is used as an orchestration layer for workflow logic and AI-driven processes.

This reduces the amount of custom integration code required and allows workflows to be modified and extended more quickly.


## Messaging

The platform integrates WhatsApp-based communication services.

The WhatsApp backend handles automated customer replies and connects messaging events with internal workflows and automation logic.

WhatsApp communication can be connected with n8n workflows, backend microservices, AI services, and customer data to support automated customer engagement and sales processes.


## Background Processing

Redis and BullMQ are used for asynchronous job processing.

BullMQ manages background jobs and distributed workloads that do not need to be processed synchronously by the main application.

This allows tasks such as messaging, imports, broadcasts, and other long-running operations to be processed asynchronously.

The queue-based architecture helps prevent long-running workloads from blocking synchronous API requests and allows background processing to be handled independently from the main application services.


## Infrastructure

The application is currently hosted on self-managed Ubuntu servers.

A self-hosted K3s Kubernetes cluster is used to orchestrate the application's backend services and supporting infrastructure.

The Kubernetes environment hosts services including:

- Self-hosted Supabase
- PostgreSQL managed by CloudNativePG
- Redis
- BullMQ
- Node.js / Express.js microservices
- Self-hosted n8n
- WhatsApp integration services

The infrastructure was previously deployed on Google Kubernetes Engine (GKE).

The application was later migrated from GKE to self-managed Ubuntu servers running K3s primarily due to infrastructure cost considerations.

The GKE environment cost more than RM600 per month, while the self-hosted Ubuntu and K3s environment reduced the infrastructure cost to approximately RM30+ per month.

The migration significantly reduced recurring infrastructure costs while giving greater control over the Kubernetes environment and infrastructure configuration.

The migration also provided practical experience operating Kubernetes infrastructure directly rather than relying entirely on a managed Kubernetes service.


## Infrastructure Evolution

The infrastructure evolved through two main stages.

### Stage 1: Google Kubernetes Engine

The application was initially deployed on Google Kubernetes Engine (GKE) to run the Kubernetes-based application environment.

GKE provided managed Kubernetes capabilities and simplified cluster management.

However, the recurring infrastructure cost became a concern for the scale and requirements of the project.

The GKE environment cost more than RM600 per month.


### Stage 2: Self-hosted Ubuntu and K3s

The application was migrated from GKE to self-managed Ubuntu servers running K3s.

The migration reduced the recurring infrastructure cost to approximately RM30+ per month.

The self-hosted environment also provided greater control over:

- Kubernetes configuration
- Application deployments
- Networking
- Infrastructure resources
- Database infrastructure
- Supporting services

The current platform is therefore operated as a self-hosted Kubernetes environment rather than a fully managed cloud Kubernetes platform.


## Security and Network Access

Cloudflare is used as part of the platform's network and security architecture.

Cloudflare Zero Trust is used to control access to services that need to be exposed externally.

This provides controlled access to the self-hosted infrastructure without directly exposing internal services to the public internet.

The security architecture helps protect the self-hosted Kubernetes environment and control how external users and services access the application and backend services.


## Backup and Data Protection

The backend infrastructure is self-hosted on Ubuntu servers and orchestrated using K3s.

The platform runs multiple backend services within the Kubernetes environment, including self-hosted Supabase, n8n, Node.js microservices, Redis, and PostgreSQL.

PostgreSQL databases are managed using CloudNativePG (CNPG).

For PostgreSQL data protection and recovery, CloudNativePG is integrated with Barman Cloud for automated database backups and continuous WAL archiving.

Cloudflare R2 is used as the external object storage destination for PostgreSQL backups and WAL archives.

This architecture provides an additional layer of data protection for the PostgreSQL databases that support the self-hosted backend ecosystem, including Supabase and other application services running on the K3s cluster.

The backup architecture is focused on protecting the underlying PostgreSQL data layer used by the backend services rather than directly backing up every individual application service.


## Technologies

### Frontend

- Flutter Web


### Backend

- Node.js
- Express.js
- REST API


### Database

- Supabase
- PostgreSQL
- CloudNativePG (CNPG)


### Authentication

- Supabase Auth (GoTrue)
- Google OAuth 2.0
- JWT


### Messaging and Processing

- WhatsApp Baileys API
- Redis
- BullMQ


### Automation and AI Agentic Development

- n8n
- LangChain-based AI integrations
- AI Agent workflows
- Workflow orchestration
- OpenAI API


### Infrastructure

- Ubuntu Server
- Kubernetes
- K3s
- Docker
- CloudNativePG


### Cloud and Security

- Google Kubernetes Engine (GKE) - Previous Infrastructure
- Cloudflare
- Cloudflare Zero Trust
- Cloudflare R2


## My Responsibilities

I worked across multiple layers of the system, including:

- Full-stack application development
- Flutter frontend development
- Backend API development
- Node.js and Express.js microservice development
- Supabase backend development
- PostgreSQL database architecture
- CloudNativePG deployment and management
- Authentication and authorization
- Google OAuth integration
- JWT-based authorization
- AI Agentic Development using n8n
- AI-powered workflow orchestration
- Workflow automation
- WhatsApp integration
- Redis and BullMQ background job processing
- Kubernetes infrastructure
- Self-hosted K3s deployment
- Migration from GKE to self-hosted K3s
- Infrastructure cost optimization
- PostgreSQL high-availability and backup architecture
- Barman Cloud backup and WAL archiving
- Cloudflare Zero Trust network security
- Controlled external access to self-hosted services


## Engineering Experience

This project provided practical experience designing, building, deploying, and operating a full-stack application across both application and infrastructure layers.

I worked across the entire technology stack, from Flutter frontend development and Node.js microservices to PostgreSQL database architecture, Kubernetes infrastructure, AI agent workflows, and network security.

Instead of relying entirely on managed cloud services, I built and operated a self-hosted Kubernetes environment on Ubuntu servers using K3s.

The infrastructure was initially deployed on GKE and later migrated to self-hosted K3s due to infrastructure cost considerations, reducing monthly infrastructure costs from more than RM600 to approximately RM30+.

The project also involved integrating multiple systems, including Flutter, Node.js, Supabase, PostgreSQL, CloudNativePG, n8n, Redis, BullMQ, WhatsApp, OpenAI, and Cloudflare.

A major part of the engineering work involved connecting these systems into a cohesive platform, including:

- AI-powered customer engagement
- AI agent workflows
- Automated sales processes
- Asynchronous background processing
- WhatsApp communication
- Database management
- Kubernetes orchestration
- Secure external access
- Automated database backup and recovery

The project provided practical experience in designing and operating a self-hosted full-stack platform while balancing infrastructure cost, operational complexity, scalability, security, and reliability.


## Architecture Decisions

### Why K3s?

K3s was selected as the Kubernetes platform for the self-hosted environment.

The application was previously deployed on Google Kubernetes Engine (GKE), but the recurring infrastructure cost was relatively high for the scale and requirements of the project.

The GKE environment cost more than RM600 per month.

The infrastructure was migrated to self-managed Ubuntu servers running K3s, reducing the recurring infrastructure cost to approximately RM30+ per month.

K3s provides Kubernetes orchestration capabilities while remaining lightweight enough for a smaller self-managed environment.

The migration also provided greater control over the underlying infrastructure and Kubernetes environment.


### Why CloudNativePG?

CloudNativePG was selected to manage PostgreSQL inside Kubernetes.

It provides Kubernetes-native PostgreSQL management and integrates database lifecycle management with the Kubernetes environment.

CloudNativePG also provides capabilities for PostgreSQL high availability, backup management, and database recovery workflows.

This allows the PostgreSQL data layer supporting Supabase and other backend services to be managed within the same Kubernetes infrastructure.


### Why Barman Cloud and WAL Archiving?

Barman Cloud is used together with CloudNativePG for PostgreSQL backup and recovery.

Automated backups and continuous WAL archiving provide additional protection for PostgreSQL data and support recovery scenarios.

Cloudflare R2 is used as external object storage for the backup data and WAL archives.

Using external backup storage provides an additional layer of protection beyond the primary PostgreSQL environment running inside the K3s cluster.


### Why BullMQ?

BullMQ was introduced for background and asynchronous processing.

Tasks that do not need to block the main application request can be handled through Redis-backed queues.

This allows workloads such as messaging, imports, broadcasts, and other long-running tasks to be processed asynchronously.

The queue-based architecture improves application responsiveness and separates background workloads from synchronous API processing.


### Why n8n?

n8n was selected as the workflow and AI agent orchestration layer.

The platform uses n8n not only for traditional workflow automation but also for AI Agentic Development.

n8n's AI capabilities integrate with LangChain-based components and concepts, allowing AI agents to interact with tools, APIs, external services, and business workflows.

This allows AI capabilities to be integrated directly into business processes rather than operating as isolated chat interfaces.

For example, AI agents and AI-powered workflows can be connected to customer data, product knowledge, backend APIs, messaging systems, and automated sales processes.

Using n8n allows AI workflows and agent logic to be developed and orchestrated without implementing every integration directly inside the Node.js application layer.

This provides a flexible approach for combining AI, automation, APIs, and business logic within the same platform.


## Key Engineering Trade-offs

### Managed Kubernetes vs Self-hosted K3s

GKE provided managed Kubernetes capabilities and reduced the operational responsibility of managing the Kubernetes control plane.

However, the recurring infrastructure cost was more than RM600 per month.

Moving to self-hosted Ubuntu servers running K3s significantly reduced the infrastructure cost to approximately RM30+ per month.

The trade-off was increased operational responsibility.

With self-hosted K3s, I became responsible for managing more aspects of the infrastructure, including:

- Server resources
- Kubernetes operations
- Service deployment
- Networking
- Database infrastructure
- Backup and recovery
- Security and external access

The decision was made to accept the additional operational responsibility in exchange for significantly lower infrastructure costs and greater control over the environment.


### Custom Application Logic vs n8n Workflows

Application-specific business logic can be implemented directly in Node.js services, while workflow-oriented processes can be handled through n8n.

The trade-off is between centralized application code and more flexible visual workflow orchestration.

n8n is particularly useful for integrating AI agents, external APIs, messaging services, and business processes without requiring every workflow to be implemented as custom backend code.

