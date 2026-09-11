# System Architecture

## 1. Purpose

This document defines the technical architecture of the DwiDev Portfolio platform.

The architecture is designed for:

* Long-term maintainability.
* 1–2 years of continued development.
* Clear separation of responsibilities.
* API-first communication.
* Independent frontend and backend deployment.
* Secure CMS administration.
* Efficient public content delivery.
* Local development using Docker.
* Future scalability without premature complexity.

---

# 2. Architecture Overview

The initial architecture consists of:

```text
                    Internet
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
        Public Website       Admin CMS
          Next.js             Next.js
              │                 │
              └────────┬────────┘
                       │
                       ▼
                 Laravel API
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       MySQL         Redis        Storage
```

Primary technologies:

```text
Frontend:
Next.js
React
TypeScript
Tailwind CSS

Backend:
Laravel
PHP

Database:
MySQL

Cache:
Redis

Storage:
Local filesystem initially
Object storage-compatible architecture later

Infrastructure:
Docker
Nginx
Linux VPS
```

---

# 3. Architectural Style

The system uses a modular monolithic backend architecture.

It is NOT a microservices architecture.

Backend:

```text
Laravel Application
├── Authentication
├── Portfolio
├── Projects
├── Skills
├── Experience
├── Education
├── Certifications
├── Media
├── Contact
├── Resume
└── Administration
```

These modules live inside one Laravel application.

---

# 4. Why Modular Monolith

The portfolio is initially operated by a small number of administrators and is expected to have relatively low traffic.

Microservices would introduce unnecessary complexity such as:

* Multiple deployments.
* Service discovery.
* Distributed authentication.
* Inter-service communication.
* Distributed logging.
* Multiple databases.
* Higher infrastructure cost.

A modular monolith provides:

```text
Simple deployment
+
Clear module boundaries
+
Low operational overhead
+
Easy development
+
Future extraction possibility
```

---

# 5. Frontend Architecture

The frontend uses:

```text
Next.js
React
TypeScript
Tailwind CSS
```

The frontend is responsible for:

* Rendering public pages.
* Rendering CMS pages.
* Managing UI state.
* Calling the Laravel API.
* Client-side interaction.
* Form interaction.
* SEO metadata generation where appropriate.
* Loading and error states.

The frontend is NOT responsible for:

* Database access.
* Authentication authority.
* Business rule enforcement.
* Direct file storage.
* Direct MySQL queries.

---

# 6. Backend Architecture

Laravel is the authoritative application backend.

Responsibilities:

```text
HTTP API
Authentication
Authorization
Validation
Business Logic
Database Access
File Management
Caching
Rate Limiting
Audit Logging
```

The backend should separate:

```text
HTTP Layer
     ↓
Application / Service Layer
     ↓
Domain Logic
     ↓
Repository / Data Access where useful
     ↓
Eloquent / Database
```

The architecture should not create abstractions merely for the sake of abstraction.

---

# 7. Request Flow

A normal public request:

```text
Browser
   ↓
Next.js
   ↓
Laravel API
   ↓
Controller
   ↓
Service / Application Logic
   ↓
Model / Query
   ↓
MySQL
   ↓
Laravel Response
   ↓
Next.js
   ↓
Browser
```

---

# 8. Admin Request Flow

```text
Administrator
      ↓
Next.js CMS
      ↓
Laravel API
      ↓
Authentication
      ↓
Authorization
      ↓
Validation
      ↓
Service
      ↓
Database
      ↓
Audit Log
      ↓
API Response
      ↓
CMS
```

Administrative operations must pass through authentication and authorization.

---

# 9. Public Website Rendering Strategy

The public website should prioritize SEO and performance.

Next.js may use:

```text
Server Components
Static Rendering
Dynamic Rendering
Revalidation
```

depending on the page.

Recommended strategy:

```text
Home
→ cached/revalidated

Projects listing
→ cached/revalidated

Project detail
→ cached/revalidated

About
→ cached/revalidated

Experience
→ cached/revalidated

Certifications
→ cached/revalidated

Contact
→ dynamic form interaction
```

The exact rendering strategy may be adjusted after implementation and performance testing.

---

# 10. CMS Rendering Strategy

The CMS is an authenticated application.

CMS pages are primarily interactive.

Recommended approach:

```text
Browser
   ↓
Next.js CMS
   ↓
Laravel API
```

CMS content should not be statically generated as public pages.

---

# 11. API Boundary

The API is the only application boundary between frontend and backend.

```text
Next.js
   │
   │ HTTP/JSON
   ▼
Laravel
```

The frontend must not access:

```text
MySQL
Redis
Server filesystem
Laravel models
```

directly.

---

# 12. Database Boundary

Only Laravel accesses MySQL.

```text
Next.js
   X
   │
   │ no direct connection
   ▼
MySQL

Laravel
   │
   ▼
MySQL
```

This protects database credentials and keeps business logic centralized.

---

# 13. Redis Architecture

Redis is an infrastructure dependency rather than the primary data store.

Used for:

```text
Caching
Rate limiting
Temporary application data
```

Potential cache keys:

```text
portfolio:profile
portfolio:projects
portfolio:project:{slug}
portfolio:skills
portfolio:experiences
portfolio:educations
portfolio:certifications
portfolio:resume
```

Redis must not become the source of truth for portfolio content.

MySQL remains the source of truth.

---

# 14. Cache Flow

Example:

```text
Request Project
      ↓
Check Redis
      │
   ┌──┴───┐
   │      │
  HIT    MISS
   │      │
   ▼      ▼
 Redis   MySQL
          │
          ▼
        Redis
          │
          ▼
       Response
```

---

# 15. Cache Invalidation

When an administrator changes published content:

```text
CMS
 ↓
Laravel
 ↓
Database Update
 ↓
Invalidate Related Cache
 ↓
Next.js
 ↓
New Content
```

Example:

```text
Update project
      ↓
projects/{slug}
      ↓
Invalidate cache
      ↓
Public request receives updated content
```

---

# 16. Storage Architecture

Application files are separated from database data.

Database:

```text
Metadata
```

Storage:

```text
Actual files
```

Examples:

```text
project images
profile image
certificates
resume PDF
```

The database stores references such as:

```text
disk
path
mime_type
size
```

not the binary file itself.

---

# 17. Storage Abstraction

The application should use Laravel's filesystem abstraction.

Initial environment:

```text
local/public storage
```

Future environment:

```text
S3-compatible object storage
```

Possible providers:

```text
AWS S3
Cloudflare R2
DigitalOcean Spaces
MinIO
```

The application should not hardcode provider-specific storage logic into business logic.

---

# 18. Media Processing

Media upload flow:

```text
Browser
   ↓
Next.js
   ↓
Laravel API
   ↓
Authentication
   ↓
Validation
   ↓
Storage
   ↓
Metadata Database
```

For images, future processing may include:

```text
Resize
Optimization
Thumbnail generation
WebP / AVIF conversion
```

These should be introduced based on actual performance requirements.

---

# 19. Authentication Architecture

Initial authentication:

```text
Admin Browser
      ↓
Next.js CMS
      ↓
Laravel Authentication API
      ↓
Authentication Token / Session
```

The exact token/session implementation will be selected during backend implementation.

Authentication credentials must never be stored in plain text.

---

# 20. Authorization Architecture

Authentication:

```text
Who is the user?
```

Authorization:

```text
What may the user do?
```

The initial system may have a single administrator role.

The architecture should still avoid hardcoding authorization checks throughout controllers.

Future roles may include:

```text
admin
editor
```

without requiring a complete rewrite.

---

# 21. CMS Security Boundary

The CMS is considered a privileged application.

```text
Public Website
    │
    │ public API
    ▼
Laravel

CMS
    │
    │ authenticated API
    ▼
Laravel
```

CMS endpoints must never be exposed as unauthenticated public endpoints.

---

# 22. CORS

Laravel API must define an explicit CORS policy.

Development:

```text
localhost frontend
```

Production:

```text
portfolio domain
CMS domain if separate
```

Wildcard CORS should not be used for authenticated APIs unless there is a specific reason.

---

# 23. Rate Limiting

Rate limiting applies to sensitive endpoints.

Initial candidates:

```text
login
contact
media upload
authentication endpoints
```

Public read endpoints may use more permissive limits.

---

# 24. Error Handling

Backend exceptions are handled centrally.

Production responses must not expose:

```text
stack trace
SQL
filesystem path
credentials
environment variables
internal exception details
```

Frontend receives a controlled API error response.

---

# 25. Logging

Application logs should record operational failures and important events.

Examples:

```text
API exception
Authentication failure
Storage failure
Database failure
Unexpected application error
```

Logs must not contain:

```text
passwords
authentication tokens
API secrets
private credentials
```

---

# 26. Audit Logging

Important administrative operations should generate audit records.

Example:

```text
Admin
 ↓
Publish Project
 ↓
Database
 ↓
Audit Log
```

Audit information:

```text
user
action
entity
entity_id
timestamp
old values where appropriate
new values where appropriate
```

---

# 27. Frontend API Client

Next.js should use a centralized API client.

Conceptually:

```text
lib/
└── api/
    ├── client
    ├── projects
    ├── profile
    ├── skills
    ├── experiences
    ├── certifications
    └── messages
```

Components should not construct arbitrary API URLs throughout the application.

Bad:

```text
fetch("http://localhost:8000/api/v1/...")
```

inside many components.

Preferred:

```text
projectsApi.list()
projectsApi.getBySlug(slug)
```

This makes API changes easier to maintain.

---

# 28. Environment Configuration

Environment-specific configuration must use environment variables.

Examples:

```text
APP_ENV
APP_URL
API_URL
DATABASE_URL
DB_HOST
DB_PORT
DB_DATABASE
DB_USERNAME
DB_PASSWORD
REDIS_HOST
REDIS_PORT
STORAGE_DISK
```

Secrets must not be committed to Git.

---

# 29. Environment Separation

Three logical environments:

```text
local
staging
production
```

Initial development may operate with:

```text
local
production
```

A staging environment can be introduced when deployment complexity increases.

---

# 30. Local Development

Local architecture:

```text
Docker Compose
│
├── frontend
├── backend
├── mysql
├── redis
└── mail service
```

The exact container names may be decided during implementation.

---

# 31. Local Request Flow

```text
Browser
   ↓
localhost
   ↓
Next.js
   ↓
Laravel
   ↓
MySQL
```

Redis:

```text
Laravel
   ↓
Redis
```

Mail testing:

```text
Laravel
   ↓
Mailpit
```

Mailpit is for development only.

---

# 32. Production Architecture

Initial VPS architecture:

```text
                     Internet
                        │
                        ▼
                      Nginx
                     /     \
                    /       \
                   ▼         ▼
              Next.js      Laravel
                             │
                  ┌──────────┼──────────┐
                  ▼          ▼          ▼
                MySQL      Redis      Storage
```

Nginx handles:

```text
TLS termination
Reverse proxy
Static file handling where appropriate
Request routing
```

---

# 33. Domain Architecture

The final domain structure may be:

```text
www.example.com
```

for the public website.

API:

```text
api.example.com
```

CMS:

```text
admin.example.com
```

Alternatively, CMS may use:

```text
www.example.com/admin
```

The final choice should consider deployment simplicity and authentication security.

---

# 34. Recommended Initial Domain Structure

For the first production deployment:

```text
Portfolio:
example.com

API:
api.example.com

CMS:
example.com/admin
```

This reduces the number of separately deployed frontend applications.

A separate CMS domain can be introduced later if needed.

---

# 35. Deployment Model

Initial deployment:

```text
GitHub
   ↓
VPS
   ↓
Docker Compose
   ↓
Application
```

Later CI/CD:

```text
GitHub
   ↓
GitHub Actions
   ↓
Test
   ↓
Build
   ↓
Deploy
   ↓
VPS
```

Deployment automation should be introduced after the application is stable enough to justify it.

---

# 36. Docker Architecture

Development and production should use reproducible containers where practical.

Example:

```text
docker/
├── nginx/
├── php/
├── node/
└── mysql/
```

However, Docker configuration should not duplicate configuration unnecessarily.

The final structure will be defined in:

```text
08-project-structure.md
```

---

# 37. Dependency Management

Backend:

```text
Composer
```

Frontend:

```text
npm
```

Dependencies must be locked.

Backend:

```text
composer.lock
```

Frontend:

```text
package-lock.json
```

These lock files should be committed.

---

# 38. Database Migration

Database schema changes must use Laravel migrations.

Development:

```text
migration
 ↓
local MySQL
```

Production:

```text
migration
 ↓
production MySQL
```

Manual schema editing should not be the normal deployment process.

---

# 39. Backup Architecture

Production database should have regular backups.

Minimum:

```text
MySQL backup
```

Important files:

```text
media
resume
certificates
project images
```

must also be considered in backup strategy.

Backup and restore procedures should be documented before production launch.

---

# 40. Availability

The initial system does not require high-availability architecture.

Single VPS is acceptable.

Target architecture:

```text
Single VPS
+
Automated backups
+
Monitoring
+
Health checks
```

If traffic grows significantly, infrastructure can later be separated.

---

# 41. Health Check

Backend should expose:

```text
GET /api/v1/health
```

Example response:

```json
{
  "status": "ok"
}
```

The health endpoint should be lightweight.

It may later check critical dependencies separately.

---

# 42. Observability

Initial observability:

```text
Application logs
Nginx logs
Docker logs
Database monitoring
Server resource monitoring
```

Future:

```text
Error tracking
Metrics
Uptime monitoring
Performance monitoring
```

These are not required before the initial MVP.

---

# 43. Performance Strategy

Performance priorities:

```text
1. Server response time
2. Database query efficiency
3. API caching
4. Image optimization
5. Frontend bundle size
6. CDN / edge caching if necessary
```

Do not optimize based on assumptions alone.

Use measurements when performance becomes a concern.

---

# 44. Security Architecture

Security layers:

```text
HTTPS
   ↓
Nginx
   ↓
Application
   ↓
Authentication
   ↓
Authorization
   ↓
Validation
   ↓
Database
```

Additional controls:

```text
Rate limiting
Secure cookies/tokens
CORS
CSRF protection where applicable
File validation
Security headers
Secret management
Backups
Audit logging
```

Detailed security rules are defined separately in:

```text
09-security.md
```

---

# 45. Scalability Strategy

The architecture scales vertically first:

```text
More CPU
More RAM
Faster storage
```

Then horizontally where justified:

```text
Separate frontend
Separate API
Managed database
External Redis
Object storage
CDN
```

No horizontal scaling is required for the initial portfolio.

---

# 46. Future Architecture

Potential future architecture:

```text
                    CDN
                     │
                     ▼
                 Next.js
                     │
                     ▼
                Laravel API
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
    Managed DB    Redis       Object Storage
```

Optional additions:

```text
Search engine
Queue worker
Monitoring
Error tracking
Analytics
```

These should only be introduced when justified by requirements.

---

# 47. Queue Architecture

Background jobs are not required for the initial MVP.

Potential future jobs:

```text
Image processing
Email notifications
Large file processing
Analytics aggregation
Scheduled maintenance
```

When required:

```text
Laravel
   ↓
Queue
   ↓
Redis
   ↓
Worker
```

---

# 48. Email Architecture

Development:

```text
Laravel
 ↓
Mailpit
```

Production:

```text
Laravel
 ↓
SMTP / Transactional Email Provider
```

The application should use Laravel's mail abstraction rather than hardcoding a provider-specific implementation.

---

# 49. Architecture Boundaries

The following boundaries must remain clear:

```text
Next.js
→ presentation

Laravel
→ application + business logic

MySQL
→ persistent data

Redis
→ cache / temporary infrastructure

Storage
→ files
```

No layer should silently take responsibility for another layer.

---

# 50. Architectural Decision Rules

Before introducing a new infrastructure component, ask:

1. What problem does it solve?
2. Is the problem measurable?
3. Can the existing architecture solve it simply?
4. What operational cost does it introduce?
5. Can it be removed later?
6. Does it complicate local development?

If the benefit is not clear, do not introduce the component.

---

# 51. Architecture Principle

The system should follow:

```text
Simple first
Modular from the beginning
Secure by default
API-first
Database as source of truth
Cache as optimization
Storage abstraction
Observable in production
Easy to deploy
Easy to replace
```

The architecture should evolve based on real requirements rather than speculative scale.
