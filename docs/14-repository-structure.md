# Repository Structure

## 1. Purpose

This document defines the repository structure for the DwiDev Portfolio platform.

The repository uses a monorepo structure so that frontend, backend, infrastructure, documentation, and development utilities can be maintained within one Git repository.

The structure is designed for:

* Long-term maintenance.
* AI-assisted development.
* Clear separation of responsibilities.
* Independent frontend and backend development.
* Shared project documentation.
* Reproducible development.
* Deployment automation.

---

# 2. Repository Root

The initial repository structure is:

```text
dwidev-portfolio/
│
├── apps/
│   ├── web/
│   └── api/
│
├── docs/
│
├── infra/
│
├── scripts/
│
├── .github/
│
├── .gitignore
├── .env.example
├── README.md
└── docker-compose.yml
```

The exact files may increase as the application develops.

---

# 3. apps/

The `apps/` directory contains deployable applications.

```text
apps/
├── web/
└── api/
```

---

# 4. apps/web

The frontend application is located at:

```text
apps/web/
```

It contains the Next.js application.

Responsibilities:

```text
Public Portfolio
CMS Interface
UI Components
Frontend Routing
SEO
API Integration
Client-side Interaction
```

---

# 5. apps/api

The backend application is located at:

```text
apps/api/
```

It contains the Laravel application.

Responsibilities:

```text
REST API
Authentication
Authorization
Business Logic
Database Access
CMS Operations
Media Management
Contact Management
Validation
```

---

# 6. docs/

The `docs/` directory contains project documentation.

Current structure:

```text
docs/
├── 01-product-requirements.md
├── 02-sitemap-user-flow.md
├── 03-business-rules.md
├── 04-database.md
├── 05-api-contract.md
├── 06-system-architecture.md
├── 07-ui-ux.md
├── 08-project-structure.md
├── 09-security.md
├── 10-testing-strategy.md
├── 11-deployment-strategy.md
├── 12-maintenance-strategy.md
├── 13-technology-stack.md
└── 14-repository-structure.md
```

Documentation should remain version controlled.

---

# 7. Documentation Principle

Documentation should describe the current system.

When an architectural decision changes:

```text
Code
+
Documentation
```

must be updated together.

---

# 8. infra/

The `infra/` directory contains infrastructure-related configuration.

Initial structure:

```text
infra/
├── docker/
├── nginx/
└── scripts/
```

The directory may evolve as deployment requirements increase.

---

# 9. infra/docker/

Docker-specific configuration belongs here when configuration becomes complex enough to justify separation.

Possible structure:

```text
infra/docker/
├── web/
├── api/
├── mysql/
└── redis/
```

Do not create unnecessary Docker configuration before it is needed.

---

# 10. infra/nginx/

Production Nginx configuration belongs here.

Example:

```text
infra/nginx/
├── web.conf
└── api.conf
```

The exact configuration depends on the final domain architecture.

---

# 11. infra/scripts/

Infrastructure scripts may be stored here.

Examples:

```text
backup-database.sh
restore-database.sh
deploy.sh
health-check.sh
```

Scripts should be added only when they provide repeatable operational value.

---

# 12. scripts/

The root-level `scripts/` directory contains project development utilities that are not specific to infrastructure.

Possible examples:

```text
scripts/
├── setup
├── test
├── lint
└── verify
```

The exact implementation depends on the final development workflow.

---

# 13. Difference Between scripts/ and infra/scripts/

Use:

```text
scripts/
```

for general project development tasks.

Use:

```text
infra/scripts/
```

for deployment and infrastructure operations.

Example:

```text
scripts/
→ setup development environment

infra/scripts/
→ backup production database
```

---

# 14. .github/

GitHub automation configuration belongs here.

Initial structure:

```text
.github/
└── workflows/
```

Possible future workflows:

```text
.github/workflows/
├── ci.yml
├── deploy.yml
└── security.yml
```

---

# 15. CI Workflow

CI should eventually perform:

```text
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

# 16. Deployment Workflow

Deployment automation may eventually be:

```text
Merge to main
 ↓
CI
 ↓
Build
 ↓
Deploy
 ↓
Migration
 ↓
Health check
```

Automatic production deployment should only be enabled after the deployment process is reliable.

---

# 17. .gitignore

The repository must ignore:

```text
Environment secrets
Dependencies
Build output
Logs
Temporary files
IDE configuration
OS-generated files
Local database files
```

Examples:

```text
.env
.env.*
!.env.example

node_modules/
vendor/

.next/
dist/
build/

storage/logs/
```

The exact rules depend on the frameworks.

---

# 18. Environment Files

The repository may contain:

```text
.env.example
```

but must not contain production secrets.

Example:

```text
DB_HOST=
DB_DATABASE=
DB_USERNAME=
DB_PASSWORD=
```

Values should be placeholders.

---

# 19. README.md

The root README is the entry point for developers.

It should eventually contain:

```text
Project overview
Features
Architecture
Requirements
Installation
Development
Testing
Deployment
Documentation
Environment setup
```

---

# 20. Root Docker Compose

A root-level:

```text
docker-compose.yml
```

may orchestrate development infrastructure.

Possible services:

```text
web
api
mysql
redis
```

The exact configuration will be created during implementation.

---

# 21. Frontend Structure

The Next.js application should follow a clear structure.

Initial conceptual structure:

```text
apps/web/
├── app/
├── components/
├── features/
├── lib/
├── hooks/
├── types/
├── public/
├── tests/
├── package.json
├── tsconfig.json
└── ...
```

---

# 22. app/

The `app/` directory contains Next.js routing and page structure.

Example:

```text
app/
├── (public)/
├── (cms)/
├── login/
├── projects/
└── ...
```

The exact route structure follows:

```text
docs/02-sitemap-user-flow.md
```

---

# 23. components/

Reusable UI components belong here.

Example:

```text
components/
├── ui/
├── layout/
├── navigation/
├── forms/
└── portfolio/
```

Components should be reusable and focused.

---

# 24. features/

Feature-specific frontend logic belongs here.

Example:

```text
features/
├── auth/
├── projects/
├── profile/
├── experience/
├── certifications/
└── contact/
```

This prevents the application from becoming a large collection of unrelated files.

---

# 25. lib/

Shared frontend utilities belong here.

Example:

```text
lib/
├── api/
├── auth/
├── utils/
└── constants/
```

---

# 26. hooks/

Reusable React hooks belong here.

Examples:

```text
hooks/
├── use-auth.ts
├── use-projects.ts
└── ...
```

Only shared hooks should be placed here.

Feature-specific hooks may remain inside their feature directory.

---

# 27. types/

Shared TypeScript types belong here.

Examples:

```text
types/
├── api.ts
├── project.ts
├── profile.ts
└── ...
```

API response types should remain synchronized with the backend contract.

---

# 28. public/

Static frontend assets belong here.

Examples:

```text
public/
├── favicon
├── icons
└── static-assets
```

Uploaded CMS media should not be stored here.

---

# 29. Frontend Tests

Frontend tests belong under:

```text
apps/web/tests/
```

Possible structure:

```text
tests/
├── components/
├── integration/
└── e2e/
```

---

# 30. Backend Structure

Laravel structure:

```text
apps/api/
├── app/
├── bootstrap/
├── config/
├── database/
├── routes/
├── resources/
├── storage/
├── tests/
├── artisan
├── composer.json
└── ...
```

Laravel's standard directory structure should be preserved where possible.

---

# 31. Backend app/

Important areas:

```text
app/
├── Http/
├── Models/
├── Policies/
├── Services/
└── ...
```

Additional directories should only be introduced when justified.

---

# 32. HTTP Layer

HTTP-related code belongs in:

```text
app/Http/
```

Examples:

```text
Controllers
Requests
Resources
Middleware
```

---

# 33. Controllers

Controllers should coordinate HTTP requests.

They should avoid containing excessive business logic.

Preferred:

```text
Request
 ↓
Controller
 ↓
Service / Domain Logic
 ↓
Model / Repository
```

when the complexity justifies service separation.

---

# 34. Form Requests

Validation belongs primarily in Laravel Form Requests.

Example:

```text
app/Http/Requests/
```

Possible:

```text
StoreProjectRequest
UpdateProjectRequest
LoginRequest
ContactRequest
```

---

# 35. API Resources

API response transformation should use Laravel API Resources where appropriate.

Example:

```text
app/Http/Resources/
```

This helps keep API responses consistent.

---

# 36. Models

Database models belong in:

```text
app/Models/
```

Models should represent persistence and relationships.

Complex business logic should not automatically be placed into models.

---

# 37. Policies

Authorization rules belong in:

```text
app/Policies/
```

Policies should define resource-level authorization.

---

# 38. Services

Services may be introduced for meaningful application operations.

Example:

```text
app/Services/
├── ProjectService.php
├── MediaService.php
└── ...
```

Do not create a service class merely to wrap a single Eloquent call.

---

# 39. Repositories

Repositories are optional.

They should only be introduced if they provide a meaningful abstraction.

The project should not create repositories for every model by default.

---

# 40. Database Structure

Laravel database files:

```text
apps/api/database/
├── factories/
├── migrations/
└── seeders/
```

---

# 41. Routes

API routes belong in:

```text
apps/api/routes/
```

Primary route file:

```text
routes/api.php
```

Authentication and public API endpoints should be organized clearly.

---

# 42. Tests

Backend tests:

```text
apps/api/tests/
├── Feature/
└── Unit/
```

Feature tests should cover major application workflows.

---

# 43. Storage

Laravel storage:

```text
apps/api/storage/
```

Production uploaded media must be persisted independently from disposable deployment artifacts.

---

# 44. Separation of Concerns

The repository separates:

```text
apps/
→ Application code

docs/
→ Knowledge and specifications

infra/
→ Infrastructure

scripts/
→ Development utilities

.github/
→ GitHub automation
```

This separation should remain stable.

---

# 45. Dependency Boundaries

Frontend dependencies belong to:

```text
apps/web/package.json
```

Backend dependencies belong to:

```text
apps/api/composer.json
```

Do not install frontend dependencies at the backend level or vice versa.

---

# 46. Environment Boundary

Frontend and backend may have different environment variables.

Example:

```text
apps/web
→ NEXT_PUBLIC_API_URL

apps/api
→ DB_*
→ REDIS_*
→ APP_KEY
```

Secrets must remain server-side.

---

# 47. Public Environment Variables

Only values explicitly intended for browser exposure may use public frontend environment variables.

Example:

```text
NEXT_PUBLIC_API_URL
```

Never expose:

```text
DB_PASSWORD
APP_KEY
PRIVATE_API_KEY
SMTP_PASSWORD
```

through frontend public variables.

---

# 48. Generated Files

Generated files should not normally be committed.

Examples:

```text
node_modules/
vendor/
.next/
coverage/
logs/
temporary build files
```

Exceptions should be documented.

---

# 49. Generated API Types

If API types are generated automatically in the future:

```text
generated/
```

may be introduced.

Generated code must have a documented source and regeneration process.

---

# 50. Naming Convention

Use predictable naming.

Directories:

```text
lowercase
```

TypeScript components:

```text
PascalCase.tsx
```

TypeScript utilities:

```text
kebab-case.ts
```

Laravel classes:

```text
PascalCase.php
```

The exact naming conventions should remain consistent.

---

# 51. Feature Ownership

Each feature should have a recognizable location.

Example:

```text
Project
├── frontend
│   └── features/projects/
│
└── backend
    ├── Models/Project.php
    ├── Controllers/ProjectController.php
    └── Services/ProjectService.php
```

This makes AI-assisted development easier because related code can be located quickly.

---

# 52. AI Navigation Principle

The repository should allow an AI coding assistant to answer:

```text
Where is the project model?
Where is the project API?
Where is the project UI?
Where is project validation?
Where is project authorization?
Where are project tests?
```

without scanning the entire repository.

---

# 53. Documentation Navigation

AI and developers should use:

```text
docs/
```

as the primary source for project intent.

Implementation should follow:

```text
Requirements
 ↓
Business Rules
 ↓
Architecture
 ↓
API Contract
 ↓
Code
 ↓
Tests
```

---

# 54. Monorepo Principle

The repository is a monorepo but the applications remain logically independent.

```text
apps/web
```

does not directly access:

```text
apps/api
```

source code.

Communication occurs through the API contract.

---

# 55. Frontend / Backend Communication

Preferred:

```text
Next.js
 ↓
HTTP
 ↓
Laravel API
```

Avoid:

```text
Next.js
 ↓
Direct database access
```

The frontend must never connect directly to MySQL.

---

# 56. Shared Code

Frontend and backend should not share runtime source code directly.

Shared concepts are communicated through:

```text
API contract
Documentation
Generated types where appropriate
```

---

# 57. Repository Growth

As the project grows:

```text
Current
apps/
├── web
└── api
```

may eventually become:

```text
apps/
├── web
├── api
└── worker
```

only if a separate worker becomes necessary.

Do not create applications for hypothetical future requirements.

---

# 58. Infrastructure Growth

Likewise:

```text
infra/
```

may eventually contain:

```text
docker/
nginx/
monitoring/
backup/
```

Only introduce directories when the infrastructure actually requires them.

---

# 59. Initial Final Structure

The expected initial repository:

```text
dwidev-portfolio/
│
├── apps/
│   ├── web/
│   │   ├── app/
│   │   ├── components/
│   │   ├── features/
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── types/
│   │   ├── public/
│   │   └── tests/
│   │
│   └── api/
│       ├── app/
│       ├── bootstrap/
│       ├── config/
│       ├── database/
│       ├── resources/
│       ├── routes/
│       ├── storage/
│       └── tests/
│
├── docs/
│
├── infra/
│   ├── docker/
│   ├── nginx/
│   └── scripts/
│
├── scripts/
│
├── .github/
│   └── workflows/
│
├── .env.example
├── .gitignore
├── README.md
└── docker-compose.yml
```

---

# 60. Repository Rules

The repository follows these rules:

```text
1. Keep frontend and backend separated.
2. Keep documentation version controlled.
3. Never commit secrets.
4. Do not duplicate configuration unnecessarily.
5. Do not introduce directories without a purpose.
6. Keep feature code discoverable.
7. Keep tests close to the application they verify.
8. Keep infrastructure separate from application code.
9. Keep production configuration outside Git secrets.
10. Update documentation when architecture changes.
```

---

# 61. Long-Term Goal

The repository should remain understandable even after substantial growth.

A developer should be able to identify:

```text
Frontend
Backend
Database
API
Tests
Infrastructure
Documentation
Deployment
```

without requiring tribal knowledge.

---

# 62. Final Principle

The repository structure should reduce cognitive load.

The intended flow is:

```text
Question
 ↓
Documentation
 ↓
Feature
 ↓
Implementation
 ↓
Test
 ↓
Deployment
```

The repository is structured to support that workflow for the entire lifetime of the project.
