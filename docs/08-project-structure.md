# Project Structure

## 1. Purpose

This document defines the repository structure for the DwiDev Portfolio platform.

The structure is designed for:

* Long-term development.
* AI-assisted / vibe coding.
* Clear separation of frontend and backend.
* Maintainable CMS development.
* Modular Laravel architecture.
* Reusable Next.js components.
* Predictable file locations.
* Easy onboarding.
* Easy deployment.

The project must avoid unnecessary architectural complexity.

---

# 2. Repository Architecture

The project uses a monorepo structure.

```text
dwidev-portfolio/
│
├── apps/
│   ├── web/
│   └── api/
│
├── docs/
│
├── infrastructure/
│
├── scripts/
│
├── .github/
│
├── .gitignore
├── README.md
└── docker-compose.yml
```

Responsibilities:

```text
apps/web
→ Next.js frontend and CMS

apps/api
→ Laravel API

docs
→ project documentation

infrastructure
→ deployment and infrastructure configuration

scripts
→ development and maintenance scripts

.github
→ CI/CD and repository automation
```

---

# 3. Frontend Structure

Frontend:

```text
apps/web/
```

Technology:

```text
Next.js
React
TypeScript
Tailwind CSS
```

Structure:

```text
apps/web/
├── app/
├── components/
├── lib/
├── hooks/
├── types/
├── public/
├── styles/
├── tests/
├── middleware.ts
├── next.config.ts
├── package.json
├── tsconfig.json
└── ...
```

---

# 4. Next.js App Router

Use the Next.js App Router.

```text
app/
```

Public routes:

```text
app/
├── page.tsx
├── about/
├── projects/
├── experience/
├── certifications/
└── contact/
```

CMS:

```text
app/
└── admin/
```

---

# 5. Public Route Structure

Recommended:

```text
app/
├── page.tsx
├── about/
│   └── page.tsx
│
├── projects/
│   ├── page.tsx
│   └── [slug]/
│       └── page.tsx
│
├── experience/
│   └── page.tsx
│
├── certifications/
│   └── page.tsx
│
└── contact/
    └── page.tsx
```

The project detail route uses:

```text
/projects/[slug]
```

instead of numeric IDs.

---

# 6. CMS Route Structure

CMS lives under:

```text
app/admin/
```

Example:

```text
app/admin/
├── page.tsx
│
├── projects/
│   ├── page.tsx
│   ├── create/
│   │   └── page.tsx
│   └── [id]/
│       └── edit/
│           └── page.tsx
│
├── skills/
├── experiences/
├── education/
├── certifications/
├── categories/
├── technologies/
├── media/
├── messages/
├── profile/
├── resume/
└── settings/
```

---

# 7. Route Groups

Next.js route groups may be used to separate application concerns without affecting URLs.

Example:

```text
app/
├── (public)/
│   ├── page.tsx
│   ├── about/
│   └── projects/
│
└── (admin)/
    └── admin/
```

Use route groups only when they improve organization.

Do not introduce unnecessary nesting.

---

# 8. Layouts

Layouts should be used for shared UI.

Public:

```text
app/
└── layout.tsx
```

Admin:

```text
app/admin/
└── layout.tsx
```

Conceptually:

```text
Public Layout
├── Header
├── Main
└── Footer
```

CMS Layout:

```text
Admin Layout
├── Sidebar
├── Header
├── Main
└── Notifications
```

---

# 9. Loading and Error Boundaries

Next.js routes should use appropriate boundaries.

Example:

```text
projects/
├── page.tsx
├── loading.tsx
└── error.tsx
```

Dynamic pages may also use:

```text
projects/[slug]/
├── page.tsx
├── loading.tsx
└── not-found.tsx
```

Do not create these files unless the route requires specialized behavior.

---

# 10. Components

Components are divided into three categories:

```text
components/
├── ui/
├── portfolio/
└── admin/
```

---

# 11. UI Components

Reusable primitive components:

```text
components/ui/
├── button.tsx
├── input.tsx
├── textarea.tsx
├── select.tsx
├── badge.tsx
├── card.tsx
├── dialog.tsx
├── dropdown.tsx
├── table.tsx
├── toast.tsx
├── skeleton.tsx
└── ...
```

These components should remain generic.

They should not contain project-specific business logic.

---

# 12. Portfolio Components

Portfolio-specific components:

```text
components/portfolio/
├── hero.tsx
├── project-card.tsx
├── project-grid.tsx
├── skill-list.tsx
├── experience-timeline.tsx
├── certification-card.tsx
├── resume-cta.tsx
├── contact-form.tsx
└── footer.tsx
```

These components may consume portfolio-specific types and API data.

---

# 13. Admin Components

CMS-specific components:

```text
components/admin/
├── admin-sidebar.tsx
├── admin-header.tsx
├── data-table.tsx
├── page-header.tsx
├── form-section.tsx
├── status-badge.tsx
├── media-uploader.tsx
├── confirm-dialog.tsx
└── ...
```

Admin components must not leak into public website components.

---

# 14. Component Naming

Use predictable naming.

Files:

```text
project-card.tsx
project-form.tsx
project-table.tsx
```

Avoid:

```text
ProjectCardNew.tsx
ProjectCardFinal.tsx
ProjectCardV2.tsx
AwesomeProjectCard.tsx
```

The codebase should not accumulate version-numbered components.

---

# 15. Feature Components

If a module becomes large, feature-specific components may be grouped.

Example:

```text
components/
└── admin/
    └── projects/
        ├── project-form.tsx
        ├── project-table.tsx
        ├── project-filters.tsx
        └── project-status.tsx
```

Do this only when the module becomes sufficiently complex.

---

# 16. API Client

All API communication should be centralized.

```text
lib/
└── api/
    ├── client.ts
    ├── auth.ts
    ├── projects.ts
    ├── profile.ts
    ├── skills.ts
    ├── experiences.ts
    ├── education.ts
    ├── certifications.ts
    ├── media.ts
    └── messages.ts
```

Example responsibility:

```text
projects.ts
→ list projects
→ get project
→ create project
→ update project
→ delete project
```

---

# 17. API Client Rules

Components should not directly construct API URLs.

Avoid:

```text
fetch(`${API_URL}/projects`)
```

throughout the component tree.

Preferred:

```text
projectsApi.list()
```

or equivalent abstraction.

The exact implementation can evolve.

---

# 18. Authentication Client

Authentication logic belongs in:

```text
lib/
└── auth/
```

Example:

```text
lib/auth/
├── session.ts
├── permissions.ts
└── constants.ts
```

Authentication state should not be duplicated across unrelated components.

---

# 19. Hooks

Custom React hooks:

```text
hooks/
```

Examples:

```text
hooks/
├── use-debounce.ts
├── use-media-query.ts
├── use-pagination.ts
└── ...
```

Only create a hook when logic is reused or when the hook clearly improves component readability.

---

# 20. Types

Shared frontend TypeScript types:

```text
types/
├── project.ts
├── profile.ts
├── skill.ts
├── experience.ts
├── education.ts
├── certification.ts
├── media.ts
├── message.ts
├── api.ts
└── auth.ts
```

Types should describe API/domain data.

Do not duplicate the same interface in multiple components.

---

# 21. API Response Types

Common API response structures should be centralized.

Example concepts:

```text
ApiResponse<T>
PaginatedResponse<T>
ApiError
PaginationMeta
```

This keeps frontend API handling predictable.

---

# 22. Utility Functions

Generic utilities:

```text
lib/
└── utils/
    ├── date.ts
    ├── format.ts
    ├── validation.ts
    └── ...
```

Do not put business logic inside generic utility files.

---

# 23. Constants

Application constants:

```text
lib/
└── constants/
```

Examples:

```text
routes
status
navigation
```

Avoid scattering magic strings throughout the codebase.

---

# 24. Public Assets

Static assets:

```text
public/
├── icons/
├── images/
└── fonts/
```

Uploaded CMS media should not normally be committed into `public/`.

CMS media belongs to the application storage system.

---

# 25. Styling

Global styles:

```text
styles/
└── globals.css
```

Tailwind configuration and design tokens should be centralized according to the installed Tailwind version.

Do not create many global CSS files without a clear reason.

---

# 26. Frontend Tests

Frontend tests:

```text
tests/
├── components/
├── integration/
└── e2e/
```

Examples:

```text
components/
→ component behavior

integration/
→ frontend + API behavior

e2e/
→ complete user flow
```

---

# 27. Backend Structure

Backend:

```text
apps/api/
```

Technology:

```text
Laravel
PHP
MySQL
Redis
```

Base structure:

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
├── public/
├── artisan
├── composer.json
└── ...
```

---

# 28. Laravel Application Structure

Use Laravel's standard structure as the foundation.

```text
app/
├── Http/
├── Models/
├── Policies/
├── Services/
├── Actions/
└── Support/
```

Do not replace Laravel conventions unnecessarily.

---

# 29. Controllers

Controllers:

```text
app/Http/Controllers/
├── Api/
│   └── V1/
│       ├── Auth/
│       ├── Projects/
│       ├── Profile/
│       ├── Skills/
│       ├── Experiences/
│       ├── Education/
│       ├── Certifications/
│       ├── Media/
│       └── Messages/
```

Controllers should remain thin.

They should coordinate:

```text
Request
→ validation
→ action/service
→ response
```

They should not contain large business algorithms.

---

# 30. Form Requests

Validation belongs in:

```text
app/Http/Requests/
```

Example:

```text
app/Http/Requests/
├── Auth/
├── Projects/
├── Profile/
├── Skills/
├── Experiences/
├── Education/
├── Certifications/
├── Media/
└── Messages/
```

Example:

```text
StoreProjectRequest
UpdateProjectRequest
```

---

# 31. API Resources

API transformation belongs in:

```text
app/Http/Resources/
```

Example:

```text
app/Http/Resources/
├── ProjectResource.php
├── ProfileResource.php
├── SkillResource.php
├── ExperienceResource.php
├── EducationResource.php
├── CertificationResource.php
├── MediaResource.php
└── MessageResource.php
```

Resources control the public API representation.

---

# 32. Models

Eloquent models:

```text
app/Models/
├── User.php
├── Project.php
├── ProjectCategory.php
├── Technology.php
├── Skill.php
├── Experience.php
├── Education.php
├── Certification.php
├── Media.php
├── ContactMessage.php
└── AuditLog.php
```

The final list follows the actual database design.

---

# 33. Services

Services contain reusable application operations.

```text
app/Services/
├── ProjectService.php
├── MediaService.php
├── ContactService.php
└── ...
```

Do not create a service class simply because a controller exists.

Use services when an operation contains meaningful application logic or is reused.

---

# 34. Actions

Actions represent focused operations.

Example:

```text
app/Actions/
├── Projects/
│   ├── CreateProject.php
│   ├── UpdateProject.php
│   ├── PublishProject.php
│   └── DeleteProject.php
│
├── Media/
│   ├── UploadMedia.php
│   └── DeleteMedia.php
│
└── Messages/
    └── SendContactMessage.php
```

Actions should have one clear responsibility.

---

# 35. Services vs Actions

Use:

```text
Service
→ broader reusable application logic

Action
→ one focused business operation
```

Example:

```text
ProjectService
→ project-related orchestration

PublishProject
→ publish one project
```

Do not create both for the same responsibility without justification.

---

# 36. Policies

Authorization:

```text
app/Policies/
```

Example:

```text
ProjectPolicy.php
MediaPolicy.php
ProfilePolicy.php
```

Policies determine whether the authenticated user can perform privileged actions.

---

# 37. Middleware

Middleware:

```text
app/Http/Middleware/
```

Examples:

```text
Authenticate
EnsureAdmin
RateLimit
```

Only create custom middleware when middleware semantics are actually appropriate.

---

# 38. Exceptions

Application exceptions:

```text
app/Exceptions/
```

Use exceptions for exceptional application conditions.

Do not use exceptions as normal control flow.

---

# 39. Support Layer

Generic backend support utilities:

```text
app/Support/
├── Api/
├── Cache/
├── Files/
└── ...
```

This directory must not become a dumping ground.

Every class should have a clear technical responsibility.

---

# 40. Database

Database structure:

```text
database/
├── factories/
├── migrations/
└── seeders/
```

Seeders:

```text
database/seeders/
├── DatabaseSeeder.php
├── AdminUserSeeder.php
├── PortfolioSeeder.php
└── ...
```

Seeders should provide useful development/demo data.

---

# 41. Migrations

All schema changes use migrations.

Example:

```text
database/migrations/
├── create_users_table.php
├── create_projects_table.php
├── create_skills_table.php
└── ...
```

Never manually modify production schema as the standard workflow.

---

# 42. API Routes

API routes:

```text
routes/
├── api.php
└── web.php
```

API endpoints should be versioned:

```text
/api/v1/...
```

Example:

```text
/api/v1/projects
/api/v1/projects/{slug}
/api/v1/skills
```

---

# 43. API Route Organization

As the API grows, route files may be split.

Possible:

```text
routes/
├── api.php
└── api/
    └── v1/
        ├── auth.php
        ├── projects.php
        ├── profile.php
        └── admin.php
```

Do not split route files prematurely.

Start simple.

---

# 44. API Versioning

Initial API:

```text
v1
```

Example:

```text
/api/v1/projects
```

Breaking changes should result in a new version where appropriate:

```text
/api/v2/projects
```

Do not create a new version for every small change.

---

# 45. Backend Tests

Laravel tests:

```text
tests/
├── Feature/
├── Unit/
└── TestCase.php
```

Feature tests should cover:

```text
Authentication
Authorization
Projects
CMS CRUD
Contact
Media
API responses
```

Unit tests should cover isolated business logic.

---

# 46. Documentation

Documentation:

```text
docs/
```

Current documentation:

```text
docs/
├── 01-product-requirements.md
├── 02-sitemap-user-flow.md
├── 03-business-rules.md
├── 04-database.md
├── 05-api-contract.md
├── 06-system-architecture.md
├── 07-ui-ux.md
└── 08-project-structure.md
```

Documentation numbering should follow project development order.

---

# 47. Infrastructure

Infrastructure:

```text
infrastructure/
├── docker/
│   ├── nginx/
│   ├── php/
│   └── node/
│
├── production/
└── scripts/
```

Infrastructure files must not contain application business logic.

---

# 48. Docker Compose

Root:

```text
docker-compose.yml
```

Development services may include:

```text
frontend
backend
mysql
redis
mailpit
```

Optional services should only be included when actually required.

---

# 49. Environment Files

Development environment:

```text
.env
```

Environment examples:

```text
.env.example
```

Never commit:

```text
.env
```

if it contains secrets.

The repository should contain safe example configuration.

---

# 50. GitHub Configuration

Repository automation:

```text
.github/
└── workflows/
    ├── ci.yml
    └── deploy.yml
```

CI should eventually validate:

```text
Frontend lint
Frontend typecheck
Backend tests
Backend lint / formatting
Build
```

Deployment should happen only after CI requirements pass.

---

# 51. Scripts

Reusable project scripts:

```text
scripts/
├── setup.sh
├── deploy.sh
└── ...
```

Scripts should automate repetitive project operations.

Do not create scripts for one-time commands that are unlikely to be reused.

---

# 52. Root README

The root `README.md` should explain:

```text
Project overview
Features
Architecture
Requirements
Installation
Development
Testing
Deployment
Environment variables
```

It should be enough for another developer to start the project.

---

# 53. Root Directory Rule

Do not place arbitrary files in the root directory.

Avoid:

```text
test.php
fix.php
temp.sql
notes.txt
debug.js
final-final.zip
```

Temporary files belong outside the repository or should be deleted.

---

# 54. Naming Conventions

General:

```text
directories → kebab-case where appropriate
React files → kebab-case
TypeScript types → PascalCase
Laravel classes → PascalCase
database tables → snake_case
database columns → snake_case
API endpoints → plural nouns
```

Examples:

```text
project-card.tsx
ProjectService.php
project_categories
created_at
/api/v1/projects
```

---

# 55. Business Logic Rule

Business logic must not be placed directly inside:

```text
React components
Blade templates
Controllers
Routes
```

Business logic belongs in the backend application layer.

Frontend should primarily handle:

```text
presentation
interaction
client state
API consumption
```

---

# 56. Database Access Rule

Frontend:

```text
NO direct database access
```

Backend:

```text
YES
```

Only Laravel communicates with MySQL.

---

# 57. API Access Rule

All frontend-backend communication must use the defined API contract.

Do not create undocumented endpoints during feature development.

If an endpoint changes:

```text
API Contract
↓
Backend
↓
Frontend
```

must be updated consistently.

---

# 58. Component Reuse Rule

Before creating a new component, check:

```text
Does an existing component already solve this?
```

If yes:

```text
Reuse it.
```

If not:

```text
Create a reusable component when appropriate.
```

Avoid duplicate components.

---

# 59. Abstraction Rule

Do not create abstractions before they are needed.

Bad:

```text
BaseRepository
BaseService
BaseController
BaseManager
BaseFactory
```

without a concrete reason.

Prefer simple code first.

Introduce abstractions when:

```text
duplication exists
complexity exists
reuse exists
testing benefits
```

---

# 60. AI / Vibe Coding Rule

AI agents must follow this project structure.

Before creating a file, the AI should determine:

```text
1. What responsibility does the file have?
2. Which layer owns that responsibility?
3. Does an existing file already handle it?
4. Can an existing component/service be reused?
5. Does creating this file increase unnecessary complexity?
```

AI must not reorganize the project architecture without explicit approval.

---

# 61. AI Modification Rule

When modifying an existing feature:

```text
Read existing implementation
↓
Understand architecture
↓
Reuse existing patterns
↓
Modify minimum required files
↓
Run tests
```

Do not rewrite an entire module for a small change.

---

# 62. AI Dependency Rule

AI must not install a new dependency simply to solve a small problem.

Before adding a dependency:

```text
1. Check whether the framework already provides the capability.
2. Check existing dependencies.
3. Evaluate maintenance cost.
4. Evaluate security.
5. Evaluate bundle/server impact.
6. Add dependency only when justified.
```

---

# 63. AI File Creation Rule

AI should avoid files such as:

```text
helper2.ts
utils2.ts
service-new.ts
project-final.tsx
project-v2.tsx
```

When requirements change, update the canonical implementation.

---

# 64. Frontend Data Flow

Recommended:

```text
Page
 ↓
API Client
 ↓
Laravel API
 ↓
API Response
 ↓
Type
 ↓
Component
```

Do not embed API calls throughout deeply nested UI components unless there is a clear reason.

---

# 65. CMS Data Flow

```text
CMS Page
 ↓
Form
 ↓
API Client
 ↓
Laravel
 ↓
Request Validation
 ↓
Action / Service
 ↓
Database
 ↓
Resource
 ↓
API Response
 ↓
CMS UI
```

---

# 66. Public Data Flow

```text
Public Page
 ↓
Server-side data request where appropriate
 ↓
API Client
 ↓
Laravel
 ↓
Cache
 ↓
Database if cache miss
 ↓
API Resource
 ↓
Next.js
 ↓
HTML
```

---

# 67. Project Detail Data Flow

```text
/projects/[slug]
        ↓
projectsApi.getBySlug(slug)
        ↓
GET /api/v1/projects/{slug}
        ↓
Laravel
        ↓
Cache
        ↓
Project
        ↓
ProjectResource
        ↓
Next.js
        ↓
Project Case Study
```

---

# 68. Media Data Flow

```text
CMS
 ↓
Media Uploader
 ↓
Laravel API
 ↓
Validation
 ↓
Storage
 ↓
Media Record
 ↓
API Resource
 ↓
CMS
```

Public display:

```text
Database media reference
 ↓
Storage URL
 ↓
Next.js Image
```

---

# 69. Contact Data Flow

```text
Contact Form
 ↓
Validation
 ↓
POST /api/v1/messages
 ↓
Laravel
 ↓
Rate Limit
 ↓
Validation
 ↓
Database
 ↓
Optional notification
 ↓
Response
```

---

# 70. Future Extension Strategy

Possible future modules:

```text
Blog
Articles
Open Source
Services
Testimonials
Analytics
Search
Newsletter
```

New modules should follow the same structure.

Example:

```text
Backend:
Project model
Project requests
Project resources
Project actions
Project policy
Project tests

Frontend:
Project API
Project types
Project components
Project routes
```

---

# 71. Architecture Stability

The project structure should not change frequently.

Structural changes require a clear reason:

```text
Major feature
Scalability problem
Maintainability problem
Security requirement
Framework requirement
```

Do not reorganize the repository merely for aesthetic reasons.

---

# 72. Definition of Done for New Features

A feature is not considered complete until:

```text
Requirement implemented
Database updated if required
API implemented
Validation implemented
Authorization implemented if required
Frontend implemented
Loading state handled
Error state handled
Tests added
Documentation updated where necessary
```

---

# 73. Final Principle

The project structure follows:

```text
Predictable
Simple
Modular
Reusable
Testable
AI-friendly
Production-oriented
```

The most important rule:

> Put every piece of code in the layer that owns its responsibility, and avoid creating new layers unless the existing architecture cannot reasonably handle the requirement.
