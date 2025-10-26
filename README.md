# Resume Builder SaaS - Documentation Hub

Central documentation repository for the Resume Builder SaaS platform - a comprehensive microservices-based resume creation and management system built on Cloudflare's serverless infrastructure.

## Overview

This repository serves as the single source of truth for all system architecture, design specifications, API contracts, and technical documentation across the Resume Builder SaaS platform.

## Platform Architecture

The Resume Builder SaaS is built as a **microservices architecture** with 10 independent repositories, each serving a specific purpose:

### Authentication & Core Services

1. **[Resume-SSO](https://github.com/MEYWD-Labs/Resume-SSO)** - Foundation authentication service
   - JWT token generation and validation
   - OAuth integration (Google, GitHub, Microsoft)
   - Session management
   - *No dependencies - all other services depend on this*

2. **[Resume-API](https://github.com/MEYWD-Labs/Resume-API)** - Core backend API
   - Resume CRUD operations
   - Template management
   - Real-time collaboration (Durable Objects)
   - PDF generation (Queues)

### Account & Billing

3. **[Resume-Account-API](https://github.com/MEYWD-Labs/Resume-Account-API)** - Account management backend
   - Stripe payment integration
   - Subscription management
   - Billing and invoices
   - User profile management

4. **[Resume-Account-UI](https://github.com/MEYWD-Labs/Resume-Account-UI)** - Account management frontend
   - Next.js web application
   - Account settings interface
   - Billing dashboard
   - Subscription management UI

### Administration

5. **[Resume-Admin-API](https://github.com/MEYWD-Labs/Resume-Admin-API)** - Admin backend
   - Role-Based Access Control (RBAC)
   - Content moderation (NSFW detection via Workers AI)
   - AI-powered security and threat detection
   - User management and banning system
   - Audit logging

6. **[Resume-Admin](https://github.com/MEYWD-Labs/Resume-Admin)** - Admin dashboard
   - Next.js admin interface
   - User management UI
   - Content moderation queue
   - Security monitoring dashboard
   - Analytics and reporting

### Frontend Applications

7. **[Resume-UI](https://github.com/MEYWD-Labs/Resume-UI)** - Main web application
   - Next.js 14+ with React Server Components
   - Resume builder interface
   - Template selection and customization
   - Real-time collaboration
   - Export to PDF/DOCX

### Mobile Applications

8. **[Resume-Mobile-UI](https://github.com/MEYWD-Labs/Resume-Mobile-UI)** - User mobile app
   - React Native (iOS/Android)
   - Offline-first architecture
   - Push notifications
   - Biometric authentication
   - Camera integration for document scanning

9. **[Resume-Mobile-Admin](https://github.com/MEYWD-Labs/Resume-Mobile-Admin)** - Admin mobile app
   - React Native (iOS/Android)
   - On-the-go content moderation
   - User management
   - Security alerts and monitoring

### Background Processing

10. **[Resume-Processor](https://github.com/MEYWD-Labs/Resume-Processor)** - Background job processor
    - Cloudflare Queues consumer
    - PDF generation
    - AI-powered content enhancement
    - Email delivery
    - Export processing

## Technology Stack

### Infrastructure (Cloudflare)
- **Workers** - Serverless compute for all backend services
- **D1** - Global SQLite database (4 databases)
- **R2** - Object storage for PDFs, images, assets
- **KV** - Key-value store for caching and sessions
- **Durable Objects** - Real-time collaboration and stateful applications
- **Queues** - Background job processing
- **Pages** - Static site hosting for web frontends
- **Workers AI** - Content moderation and AI features
- **Email Workers** - Transactional email delivery

### Frontend
- **Next.js 14+** - Web applications with React Server Components
- **React 19** - UI library
- **React Native 0.74+** - Mobile applications (iOS/Android)
- **Tailwind CSS** - Styling
- **shadcn/ui** - Component library

### Backend
- **TypeScript** - Primary language
- **Hono** - Lightweight web framework for Workers
- **Drizzle ORM** - Type-safe database operations
- **Zod** - Runtime validation

### DevOps
- **GitHub Actions** - CI/CD pipelines
- **Wrangler** - Cloudflare deployment tool
- **Terraform** - Infrastructure as Code (optional)

## Documentation Structure

```
Resume-Docs/
├── README.md (this file)
├── architecture/
│   ├── index.md (start here for architecture overview)
│   ├── executive-summary.md
│   ├── cloudflare-serverless-architecture.md
│   ├── multi-repository-service-architecture.md
│   ├── service-dependencies-communication.md
│   ├── data-flow-diagrams.md
│   ├── authentication-flow.md
│   ├── resource-dependencies.md
│   ├── api-contracts.md
│   ├── user-flows.md
│   ├── design-system-ui-standards.md
│   ├── background-processing-architecture.md
│   ├── real-time-features.md
│   ├── mobile-architecture-strategy.md
│   ├── performance-scaling-strategy.md
│   ├── security-architecture.md
│   ├── deployment-architecture.md
│   ├── disaster-recovery.md
│   ├── cost-optimization.md
│   ├── payment-processing-architecture.md
│   ├── content-moderation-nsfw-detection.md
│   ├── ai-powered-security-threat-detection.md
│   ├── admin-platform-management.md
│   └── conclusion.md
└── LICENSE
```

## Getting Started

### For Developers
1. **Read the Architecture Overview**: Start with [`architecture/index.md`](./architecture/index.md)
2. **Understand Service Dependencies**: Review [`architecture/service-dependencies-communication.md`](./architecture/service-dependencies-communication.md)
3. **Review API Contracts**: Check [`architecture/api-contracts.md`](./architecture/api-contracts.md)
4. **Clone Relevant Repositories**: Based on your work area (frontend, backend, mobile)

### For Architects
1. **System Design**: [`architecture/cloudflare-serverless-architecture.md`](./architecture/cloudflare-serverless-architecture.md)
2. **Service Architecture**: [`architecture/multi-repository-service-architecture.md`](./architecture/multi-repository-service-architecture.md)
3. **Security**: [`architecture/security-architecture.md`](./architecture/security-architecture.md)
4. **Scalability**: [`architecture/performance-scaling-strategy.md`](./architecture/performance-scaling-strategy.md)

### For Product Managers
1. **User Flows**: [`architecture/user-flows.md`](./architecture/user-flows.md)
2. **Features Overview**: [`architecture/executive-summary.md`](./architecture/executive-summary.md)
3. **Payment System**: [`architecture/payment-processing-architecture.md`](./architecture/payment-processing-architecture.md)

## Service Communication

All services authenticate via **Resume-SSO** using JWT tokens. The dependency hierarchy is:

```
Resume-SSO (Foundation)
    ├── Resume-API
    │   ├── Resume-UI
    │   ├── Resume-Mobile-UI
    │   └── Resume-Processor
    ├── Resume-Account-API
    │   ├── Resume-Account-UI
    │   └── Resume-Mobile-UI
    └── Resume-Admin-API
        ├── Resume-Admin
        └── Resume-Mobile-Admin
```

## Key Features

### Core Functionality
- Resume creation and editing
- Template library with customization
- Real-time collaborative editing
- PDF/DOCX export
- AI-powered content suggestions

### Account Management
- Stripe payment integration
- Subscription tiers (Free, Pro, Enterprise)
- Invoice management
- Usage tracking

### Security & Moderation
- 5-layer content moderation system
- AI-powered NSFW detection (Workers AI)
- Real-time threat detection
- Behavioral analytics
- Risk scoring

### Mobile Features
- Offline-first architecture
- Biometric authentication
- Push notifications
- Document scanning
- Cross-platform sync

## Contributing

### Documentation Updates
1. Make changes to the relevant markdown files in `architecture/`
2. Update `architecture/index.md` if adding new sections
3. Submit a pull request with clear description

### Architecture Decisions
- Document all significant architectural decisions
- Include rationale, alternatives considered, and trade-offs
- Update relevant diagrams and flow charts

## Contact

For questions or suggestions about the documentation:
- Open an issue in this repository
- Contact the architecture team
- Review the [architecture index](./architecture/index.md)

## License

See [LICENSE](./LICENSE) for details.

---

**Last Updated**: 2025-10-25
**Architecture Version**: 1.0
**Total Services**: 10 repositories
