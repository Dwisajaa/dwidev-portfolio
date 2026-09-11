# Deployment Strategy

## 1. Purpose

This document defines the deployment strategy for the DwiDev Portfolio platform.

The deployment architecture must support:

* Development.
* Testing.
* Staging where required.
* Production.
* Continuous development.
* Safe updates.
* Database migrations.
* Media management.
* Backup and recovery.
* Monitoring.
* Rollback.

The system is expected to remain maintainable for at least 1–2 years.

---

# 2. Deployment Principles

The deployment process follows:

```text
Reproducible
Automated where practical
Version controlled
Environment separated
Secure
Recoverable
Observable
Rollback capable
```

Production should never depend on undocumented manual steps.

---

# 3. Environment Architecture

The application uses separate environments.

```text
Development
    ↓
Testing
    ↓
Staging
    ↓
Production
```

Not every small change requires a dedicated staging deployment, but production changes must pass the appropriate validation process.

---

# 4. Development Environment

Development is used for local implementation.

Example:

```text
Developer Machine
├── Frontend
├── Backend
├── MySQL
├── Redis
└── Local storage
```

Development may use:

```text
localhost
Docker
Local database
Development environment variables
```

Development credentials must never be production credentials.

---

# 5. Testing Environment

Testing exists to run automated tests in isolation.

Example:

```text
Test Frontend
      ↓
Test API
      ↓
Test Database
```

Production databases must never be used for automated testing.

---

# 6. Staging Environment

Staging is an optional environment for changes that require production-like verification.

Example:

```text
staging.example.com
```

Staging should approximate production architecture where practical.

Staging must use:

```text
Staging database
Staging environment variables
Staging storage
```

Never use production secrets in staging unless explicitly required and properly isolated.

---

# 7. Production Environment

Production is the public environment.

Conceptually:

```text
Internet
   ↓
Domain
   ↓
Nginx
   ↓
Frontend / Application
   ↓
API
   ↓
MySQL
   ↓
Redis
```

Only necessary services should be publicly accessible.

---

# 8. Production Infrastructure

Initial production architecture:

```text
VPS
│
├── Nginx
│
├── Frontend
│
├── Laravel API
│
├── MySQL
│
├── Redis
│
└── Storage
```

The architecture should remain simple until scale requires additional infrastructure.

---

# 9. Domain Architecture

The production system should use a clear domain structure.

Example:

```text
www.dwidev.cloud
```

for the public portfolio.

API:

```text
api.dwidev.cloud
```

CMS may use:

```text
admin.dwidev.cloud
```

or an appropriate path under the primary domain.

The final structure must be decided before production deployment.

---

# 10. DNS

DNS should point the required domains/subdomains to the production infrastructure.

Conceptually:

```text
www
 ↓
VPS

api
 ↓
VPS

admin
 ↓
VPS
```

DNS changes must be documented.

---

# 11. HTTPS

Production must use HTTPS.

Required:

```text
HTTP
 ↓
HTTPS
```

TLS certificates should be managed automatically where practical.

---

# 12. Nginx

Nginx is responsible for:

```text
TLS termination
Reverse proxy
Static file serving where appropriate
Request routing
Security headers
Request size limits
```

Nginx should not expose internal services.

---

# 13. Application Process

The Laravel API must run as a managed production process.

Possible architecture:

```text
Nginx
 ↓
PHP-FPM
 ↓
Laravel
```

The exact process manager and runtime configuration may evolve.

---

# 14. Frontend Deployment

The frontend should have a reproducible production build.

Conceptually:

```text
Source Code
 ↓
Install Dependencies
 ↓
Build
 ↓
Production Artifact
 ↓
Deploy
```

The production server must not rely on an uncontrolled development server.

---

# 15. Production Build

The production build should:

```text
Install dependencies
Run type checking
Run lint
Run tests
Build application
Deploy artifact
```

Only successfully built versions should be deployed.

---

# 16. Backend Deployment

Backend deployment should follow:

```text
Pull / receive new version
 ↓
Install dependencies
 ↓
Validate environment
 ↓
Run tests
 ↓
Run migrations
 ↓
Clear/rebuild caches where necessary
 ↓
Restart workers/services
 ↓
Health check
```

The exact order may vary depending on migration compatibility.

---

# 17. Composer Dependencies

Production dependencies should be installed using production-oriented settings.

Development-only dependencies should not be unnecessarily included in the runtime environment.

---

# 18. Node Dependencies

Frontend dependencies should be installed reproducibly using the lock file.

The lock file must be committed.

Do not routinely delete the lock file to resolve dependency issues.

---

# 19. Lock Files

The repository should preserve:

```text
package-lock.json
```

or the selected package manager equivalent.

Backend:

```text
composer.lock
```

These files ensure reproducible dependency versions.

---

# 20. Environment Configuration

Production configuration must be supplied through the production environment.

Example:

```text
APP_ENV=production
APP_DEBUG=false
APP_URL=https://www.dwidev.cloud
```

Secrets must not be stored in Git.

---

# 21. Deployment Secrets

Production secrets include:

```text
APP_KEY
DB_PASSWORD
SMTP_PASSWORD
API_KEYS
TOKEN_SECRETS
STORAGE_CREDENTIALS
```

These must be stored securely.

---

# 22. Database Migration

Database changes must be performed through version-controlled migrations.

Workflow:

```text
Migration created
 ↓
Tested locally
 ↓
Reviewed
 ↓
Committed
 ↓
Deployed
 ↓
Migration executed
```

Manual production schema changes should be avoided.

---

# 23. Migration Safety

Before running production migrations:

```text
Review migration
Check affected tables
Check indexes
Check constraints
Check data migration
Check rollback implications
```

Large data migrations should be handled separately from simple schema migrations when necessary.

---

# 24. Backward Compatibility

Application releases should consider compatibility between:

```text
Current application
Current database
New application
New database
```

Prefer deployment sequences where the old and new application versions can temporarily coexist safely when zero-downtime deployment becomes necessary.

---

# 25. Database Backup Before Migration

For important production migrations:

```text
Backup
 ↓
Migration
 ↓
Health check
```

A database backup should be available before risky schema changes.

---

# 26. Storage Deployment

Uploaded media should not be treated as disposable application code.

Examples:

```text
Project images
Certificates
Resume
Profile images
```

must be stored separately from temporary deployment artifacts.

---

# 27. Media Persistence

Deploying a new application version must not delete uploaded media.

Conceptually:

```text
Application release
     │
     X
     │
Media storage
```

The deployment process must preserve media.

---

# 28. Storage Backup

Production media should be included in backup strategy.

At minimum:

```text
Project images
Resume
Certificates
Profile assets
```

should be recoverable.

---

# 29. Redis

Redis is an infrastructure dependency rather than the source of truth.

Critical persistent data must remain in MySQL or durable storage.

If Redis is lost:

```text
Application
 ↓
Rebuild cache
```

should remain possible.

---

# 30. Cache Deployment

After deployment, caches may need to be cleared or rebuilt.

Example:

```text
Config cache
Route cache
Application cache
Frontend cache
```

Only required cache operations should be performed.

---

# 31. Queue Workers

If asynchronous jobs are introduced:

```text
Laravel
 ↓
Queue
 ↓
Worker
```

Workers must be restarted or gracefully reloaded when application code changes.

---

# 32. Scheduled Tasks

If scheduled tasks are introduced:

```text
Laravel Scheduler
```

must be configured in production.

Examples:

```text
Cleanup
Notifications
Maintenance
Scheduled publishing
```

Only required schedules should exist.

---

# 33. Health Check

Production must provide a basic health check.

Example:

```text
GET /api/v1/health
```

Expected:

```json
{
  "status": "ok"
}
```

The health check must not expose sensitive infrastructure information.

---

# 34. Deployment Verification

After deployment verify:

```text
Homepage
Projects
Project detail
About
Experience
Certifications
Contact
CMS login
CMS dashboard
API health
Media
```

---

# 35. Smoke Test

The deployment smoke test should verify the most important workflows.

Example:

```text
Open homepage
 ↓
Open project
 ↓
Open contact
 ↓
Open CMS
 ↓
Login
 ↓
Open project management
 ↓
Logout
```

The smoke test should not create unwanted production records.

---

# 36. Deployment Failure

If deployment fails:

```text
Deployment
 ↓
Failure detected
 ↓
Stop deployment
 ↓
Inspect logs
 ↓
Determine impact
 ↓
Rollback or fix
```

Do not continue deploying blindly after a critical failure.

---

# 37. Rollback

Application rollback should be possible.

Example:

```text
Version N
   ↓
Version N+1
   ↓
Problem
   ↓
Rollback
   ↓
Version N
```

Every production deployment should have an identifiable version or commit.

---

# 38. Database Rollback

Database rollback requires additional caution.

Not every migration should be automatically reversed.

For destructive migrations:

```text
Backup
+
Migration plan
+
Recovery plan
```

are required.

---

# 39. Git-Based Deployment

The Git repository is the source of truth for application code.

Example:

```text
GitHub
   ↓
Production deployment
```

Production code should correspond to a known Git commit or release.

---

# 40. Branch Strategy

Initial branch structure:

```text
main
develop
feature/*
fix/*
```

Conceptually:

```text
feature/*
    ↓
develop
    ↓
main
    ↓
production
```

---

# 41. Feature Branches

New functionality should use feature branches.

Example:

```text
feature/project-cms
feature/media-library
feature/contact-form
```

Bug fixes:

```text
fix/project-slug
fix/login-session
```

---

# 42. Pull Requests

Changes merged into important branches should be reviewed.

A pull request should include:

```text
What changed
Why it changed
Testing performed
Potential risks
Database changes
Environment changes
```

---

# 43. CI/CD

Continuous Integration should automatically validate code.

Initial pipeline:

```text
Push / Pull Request
       ↓
Install dependencies
       ↓
Lint
       ↓
Typecheck
       ↓
Backend tests
       ↓
Frontend tests
       ↓
Build
```

---

# 44. Production Deployment Pipeline

Production deployment may eventually become:

```text
Merge to main
 ↓
CI
 ↓
Tests
 ↓
Build
 ↓
Deploy
 ↓
Migration
 ↓
Health check
 ↓
Smoke test
```

Automatic deployment can be introduced after the manual process is stable.

---

# 45. Manual vs Automatic Deployment

Initial project phase may use controlled manual deployment.

Example:

```text
Developer
 ↓
Review
 ↓
Deploy command
```

As confidence increases:

```text
Git push
 ↓
CI/CD
 ↓
Production
```

can be introduced.

Automation should reduce mistakes, not hide them.

---

# 46. Deployment Versioning

Every deployment should identify the version.

Possible identifier:

```text
Git commit SHA
```

or:

```text
v1.0.0
v1.1.0
v1.2.0
```

Versioning makes rollback and debugging easier.

---

# 47. Release Notes

Significant releases should document:

```text
Features
Bug fixes
Database changes
Breaking changes
Deployment notes
```

---

# 48. Maintenance Window

Most portfolio deployments are expected to be low-risk.

For risky changes, schedule a maintenance window if necessary.

Examples:

```text
Major database migration
Infrastructure migration
Domain migration
Major framework upgrade
```

---

# 49. Framework Upgrades

Framework upgrades must not be performed casually in production.

Workflow:

```text
Check upgrade guide
 ↓
Create branch
 ↓
Update dependencies
 ↓
Fix breaking changes
 ↓
Run tests
 ↓
Build
 ↓
Staging
 ↓
Production
```

---

# 50. Dependency Upgrade Strategy

Dependencies should be updated periodically.

Priorities:

```text
Security updates
Bug fixes
Compatible minor updates
Major upgrades
```

Major upgrades require dedicated testing.

---

# 51. Server Maintenance

Production server should periodically receive:

```text
OS updates
Security patches
Runtime updates
Docker updates where applicable
Database updates where appropriate
```

Updates must be tested before major changes.

---

# 52. Disk Management

Monitor:

```text
Disk usage
Application logs
Database size
Media storage
Docker images
Temporary files
Backups
```

Uncontrolled disk growth can cause production failure.

---

# 53. Log Management

Logs must have a retention strategy.

Avoid allowing logs to consume the entire server disk.

Production should support:

```text
Rotation
Retention
Cleanup
```

---

# 54. Backup Strategy

Minimum backup targets:

```text
Database
Media
Resume
Certificates
Important configuration
```

Backups should be stored independently from the primary application environment.

---

# 55. Backup Frequency

The exact schedule depends on actual data change frequency.

For the portfolio:

```text
Database
→ regular scheduled backup

Media
→ regular or change-based backup

Configuration
→ version-controlled where safe
```

The schedule should be automated where practical.

---

# 56. Recovery Testing

Backup restoration should be tested periodically.

A backup that has never been restored should not be considered fully verified.

---

# 57. Disaster Recovery

If the VPS is lost:

```text
New VPS
 ↓
Install infrastructure
 ↓
Deploy application
 ↓
Configure environment
 ↓
Restore database
 ↓
Restore media
 ↓
Configure domain
 ↓
Configure HTTPS
 ↓
Health check
```

The documentation should eventually contain the exact recovery procedure.

---

# 58. Infrastructure Documentation

Production infrastructure should be documented.

Example:

```text
docs/
└── deployment/
    ├── production.md
    ├── backup.md
    └── recovery.md
```

Detailed operational documentation may be created later.

---

# 59. Production Access

Production access should be limited.

Only required administrators should have:

```text
SSH access
Server access
Database access
Deployment access
```

---

# 60. Database Access

Production database should normally be accessed through:

```text
Application
```

or controlled administrative access.

Avoid exposing MySQL directly to the public Internet.

---

# 61. Redis Access

Redis should remain private.

Public Internet access to Redis is not required for normal application operation.

---

# 62. Firewall

Production firewall should expose only required services.

Typical:

```text
80
443
22
```

SSH access should be restricted where practical.

---

# 63. Monitoring

Production monitoring should track:

```text
Uptime
HTTP errors
CPU
RAM
Disk
Database
Redis
SSL
Application logs
```

---

# 64. Alerting

Alerts should focus on actionable problems.

Examples:

```text
Application unavailable
High error rate
Disk nearly full
Database unavailable
SSL expiration approaching
```

Avoid creating alerts for harmless events.

---

# 65. Deployment Audit

After significant deployment, record:

```text
Deployment version
Date
Changes
Migration
Result
Rollback if applicable
```

This provides operational history.

---

# 66. Production Checklist

Before deployment:

```text
[ ] Code reviewed
[ ] Tests passing
[ ] Lint passing
[ ] Typecheck passing
[ ] Build passing
[ ] Migration reviewed
[ ] Environment variables verified
[ ] Backup available
[ ] Deployment version identified
```

---

# 67. Post-Deployment Checklist

After deployment:

```text
[ ] Health endpoint works
[ ] Homepage works
[ ] Project list works
[ ] Project detail works
[ ] Contact works
[ ] CMS login works
[ ] CMS CRUD works
[ ] Media works
[ ] No critical errors
[ ] Logs reviewed
```

---

# 68. Rollback Checklist

If necessary:

```text
[ ] Identify failing version
[ ] Identify previous stable version
[ ] Determine database impact
[ ] Backup current state if possible
[ ] Roll back application
[ ] Restore database only if necessary
[ ] Verify health
[ ] Verify critical pages
[ ] Document incident
```

---

# 69. Long-Term Maintainability

The deployment architecture should allow the portfolio to evolve.

Possible future additions:

```text
CDN
Object storage
Managed database
Automated CI/CD
Error tracking
Advanced monitoring
Search
Analytics
```

These should only be introduced when justified.

---

# 70. Avoid Premature Infrastructure Complexity

The initial system should not unnecessarily introduce:

```text
Kubernetes
Microservices
Multiple VPS
Complex service mesh
Multiple databases
Event-driven infrastructure
```

unless actual requirements justify them.

The portfolio is initially a modular monolithic application.

---

# 71. Scaling Strategy

If traffic increases:

```text
Current VPS
 ↓
Optimize application
 ↓
Add caching
 ↓
CDN
 ↓
Increase VPS resources
 ↓
Separate services if necessary
```

Infrastructure should scale according to actual bottlenecks.

---

# 72. Deployment Philosophy

The deployment system should optimize for:

```text
Reliability
Simplicity
Recoverability
Security
Maintainability
```

rather than infrastructure complexity.

---

# 73. Final Principle

The portfolio should be deployable repeatedly without relying on memory or undocumented manual procedures.

The target state is:

```text
Code
 ↓
Test
 ↓
Build
 ↓
Deploy
 ↓
Migrate
 ↓
Verify
 ↓
Monitor
 ↓
Backup
```

A deployment is considered successful only when the application is verified after deployment, not merely when the deployment command finishes successfully.
