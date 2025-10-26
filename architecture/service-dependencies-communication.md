# Service Dependencies & Communication

## Complete Dependency Graph

```
                        Resume-SSO (Auth Provider)
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
        ▼                      ▼                      ▼
   Resume-API           Resume-Account-API     Resume-Admin-API
        │                      │                      │
   ┌────┼────┐                │                 ┌────┼────┐
   │    │    │                │                 │         │
   ▼    ▼    ▼                ▼                 ▼         ▼
Resume Resume Resume    Resume-Account-UI   Resume-Admin Resume-Mobile
 -UI  -Mobile-Processor                              Admin
      -UI

**Service Communication Matrix:**

| Service | Depends On | Consumed By |
|---------|-----------|-------------|
| Resume-SSO | None | All services |
| Resume-API | Resume-SSO | Resume-UI, Resume-Mobile-UI, Resume-Processor |
| Resume-Account-API | Resume-SSO | Resume-Account-UI, Resume-Mobile-UI |
| Resume-Admin-API | Resume-SSO | Resume-Admin, Resume-Mobile-Admin |
| Resume-Processor | Resume-SSO, Resume-API | None (background jobs) |
| Resume-UI | Resume-API, Resume-SSO | End users |
| Resume-Account-UI | Resume-Account-API, Resume-SSO | End users |
| Resume-Admin | Resume-Admin-API, Resume-SSO | Administrators |
| Resume-Mobile-UI | Resume-API, Resume-Account-API, Resume-SSO | Mobile users |
| Resume-Mobile-Admin | Resume-Admin-API, Resume-SSO | Mobile administrators |
```
