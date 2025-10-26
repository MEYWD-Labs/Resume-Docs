# Multi-Repository Service Architecture

The platform follows a microservices architecture with each service in its own repository:

```
┌─────────────────────────────────────────────────────────────────┐
│                     Resume Builder SaaS Platform                 │
├───────────────────────────────────────────────────────────────── │
│                                                                   │
│  Authentication Layer                                            │
│  ┌─────────────────┐                                            │
│  │  Resume-SSO     │ ← Foundation Service (JWT Auth)            │
│  └────────┬────────┘                                            │
│           │                                                      │
│  Core Services Layer                                            │
│  ┌────────▼────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │  Resume-API     │←→│Resume-Account│←→│Resume-Admin  │     │
│  │  (Core Backend) │  │     API      │  │     API      │     │
│  └────────┬────────┘  └──────┬───────┘  └──────┬───────┘     │
│           │                   │                  │              │
│  Frontend Layer              │                  │              │
│  ┌────────▼────────┐  ┌──────▼───────┐  ┌──────▼───────┐     │
│  │  Resume-UI      │  │Resume-Account│  │Resume-Admin  │     │
│  │  (Web App)      │  │     UI       │  │  (Dashboard) │     │
│  └─────────────────┘  └──────────────┘  └──────────────┘     │
│                                                                 │
│  Mobile Layer                                                  │
│  ┌──────────────────┐                    ┌──────────────┐     │
│  │Resume-Mobile-UI  │                    │Resume-Mobile-│     │
│  │  (User App)      │                    │    Admin     │     │
│  └──────────────────┘                    └──────────────┘     │
│                                                                 │
│  Background Processing                                         │
│  ┌──────────────────┐                                         │
│  │Resume-Processor  │ ← Queue Workers                         │
│  └──────────────────┘                                         │
└─────────────────────────────────────────────────────────────────┘
```

**1. Resume-SSO** - Authentication service (foundation service)
**2. Resume-API** - Core backend API for resume operations
**3. Resume-UI** - Next.js web frontend application
**4. Resume-Admin** - Admin dashboard (Next.js)
**5. Resume-Admin-API** - Admin backend operations
**6. Resume-Processor** - Background job processing
**7. Resume-Mobile-UI** - React Native mobile applications for end users
**8. Resume-Account-API** - Dedicated backend for user account management, billing, subscriptions
**9. Resume-Account-UI** - Web interface for account settings, billing, subscription management
**10. Resume-Mobile-Admin** - React Native admin mobile application for platform administrators
