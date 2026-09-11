# Technology Stack

## 1. Overview

DwiDev Portfolio menggunakan arsitektur web modern yang memisahkan frontend public/CMS interface dan backend API.

Target utama:

* Maintainable selama 1–2 tahun.
* Mudah dikembangkan dengan AI-assisted development.
* Memiliki CMS.
* Memiliki API yang terstruktur.
* Mudah di-deploy.
* Mudah diuji.
* Tidak bergantung pada teknologi yang terlalu kompleks.

---

# 2. Architecture

High-level architecture:

```text
                    Internet
                       │
                       ▼
                  Domain / HTTPS
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
         Next.js Web       Laravel API
              │                 │
              │                 ├── MySQL
              │                 ├── Redis
              │                 └── Storage
              │
              └────── API ──────┘
```

---

# 3. Frontend

Frontend menggunakan:

```text
Next.js
TypeScript
React
Tailwind CSS
```

Frontend bertanggung jawab terhadap:

```text
Public Portfolio
CMS Interface
Routing
Rendering
UI
Form Interaction
API Consumption
SEO
Responsive Design
```

---

# 4. Next.js

Next.js digunakan sebagai frontend framework.

Primary responsibilities:

```text
Routing
Rendering
SEO
Server-side capabilities where appropriate
Static generation where appropriate
API consumption
Frontend application structure
```

The project should prefer modern Next.js patterns and avoid unnecessary legacy architecture.

---

# 5. React

React digunakan sebagai UI framework.

React components should be:

```text
Reusable
Small
Predictable
Typed
Testable
```

Components should avoid unnecessary global state.

---

# 6. TypeScript

TypeScript is mandatory for frontend application code.

The frontend should avoid unnecessary use of:

```text
any
```

Prefer:

```text
Explicit types
Interfaces
Type aliases
Generics where useful
Typed API responses
```

---

# 7. Tailwind CSS

Tailwind CSS is used for styling.

The styling system should prioritize:

```text
Consistency
Responsive design
Reusable patterns
Design tokens
Accessibility
```

Avoid excessive one-off styles when reusable patterns are more appropriate.

---

# 8. UI Components

The project should use a controlled component system.

Core components may include:

```text
Button
Input
Textarea
Select
Dialog
Dropdown
Badge
Card
Table
Toast
Alert
Pagination
Form
```

The final component library should be selected based on project requirements rather than adding multiple overlapping UI libraries.

---

# 9. Backend

Backend:

```text
Laravel
PHP
REST API
```

Laravel is responsible for:

```text
Authentication
Authorization
Business Logic
Validation
Database Access
CMS Operations
Media Management
Contact Management
API
```

---

# 10. Laravel

Laravel is the primary backend framework.

The backend should use Laravel conventions wherever practical.

Preferred structure:

```text
app/
├── Http/
├── Models/
├── Services/
├── Repositories/
└── Policies/
```

The exact structure may evolve according to application complexity.

---

# 11. PHP

PHP is the backend programming language.

The production PHP version should be:

```text
Supported by the selected Laravel version
```

The project should avoid running an unsupported PHP version in production.

---

# 12. API

The backend exposes a versioned REST API.

Initial version:

```text
/api/v1
```

Example:

```text
GET /api/v1/projects
GET /api/v1/projects/{slug}
POST /api/v1/auth/login
POST /api/v1/contact
```

---

# 13. Authentication

Authentication:

```text
Laravel Sanctum
```

The exact authentication mode will depend on the frontend deployment architecture.

The authentication system must support:

```text
Login
Logout
Session/token validation
Protected API routes
Admin access
```

---

# 14. Authorization

Authorization should use Laravel authorization mechanisms.

Possible tools:

```text
Policies
Gates
Middleware
Roles where required
```

The initial system should avoid unnecessary role complexity.

If there is only one administrator, the system may use a simple administrator authorization model.

---

# 15. Database

Primary database:

```text
MySQL
```

The database stores:

```text
Users
Profile
Projects
Skills
Experiences
Education
Certifications
Media metadata
Contact messages
Settings
```

The exact schema is defined in:

```text
docs/04-database.md
```

---

# 16. ORM

Database access should primarily use:

```text
Laravel Eloquent
```

Eloquent should be preferred for normal application operations.

Raw SQL may be used when there is a demonstrated reason.

---

# 17. Database Migrations

All schema changes must use Laravel migrations.

Example:

```text
database/migrations/
```

Production schema changes must not depend on undocumented manual SQL.

---

# 18. Database Seeders

Seeders are used for:

```text
Development data
Default data
Demo data
Initial administrator setup where appropriate
```

Seeders must not contain production secrets.

---

# 19. Factories

Factories are used primarily for automated testing.

Example:

```text
database/factories/
```

Factories should generate realistic test data.

---

# 20. Cache

Initial cache technology:

```text
Redis
```

Redis may be used for:

```text
Cache
Rate limiting
Queue
Temporary application state
```

Redis is not the source of truth for portfolio data.

---

# 21. Storage

Laravel Storage handles application-managed files.

Examples:

```text
Project images
Profile image
Resume
Certificates
```

The storage implementation should allow migration to object storage in the future.

---

# 22. Media Strategy

Media should not be tightly coupled to the frontend.

The backend manages:

```text
Upload
Validation
Metadata
Storage
Deletion
Authorization
```

The frontend consumes media through URLs or API responses.

---

# 23. API Client

The frontend should have a centralized API client.

Conceptually:

```text
lib/
└── api/
    ├── client.ts
    ├── auth.ts
    ├── projects.ts
    ├── profile.ts
    └── contact.ts
```

The exact structure may evolve.

Components should not contain repeated raw HTTP configuration.

---

# 24. HTTP Client

The project should initially prefer the platform's existing HTTP capabilities rather than adding an HTTP library without a concrete requirement.

An external client library may be introduced later if it provides meaningful value.

---

# 25. Validation

Frontend validation:

```text
TypeScript
Form validation library where justified
```

Backend validation:

```text
Laravel Form Requests
```

The backend remains the final authority for validation.

---

# 26. Error Handling

API errors should follow a consistent structure.

Example:

```json
{
  "message": "Validation failed",
  "errors": {
    "email": [
      "The email field is required."
    ]
  }
}
```

The exact API error contract is defined separately.

---

# 27. SEO

SEO is handled primarily by Next.js.

The website should support:

```text
Metadata
Title
Description
Canonical URLs
Open Graph
Twitter/X metadata where applicable
Sitemap
Robots
Structured data where appropriate
```

---

# 28. SEO Data

SEO-sensitive content should be manageable without code changes where practical.

Examples:

```text
Site title
Site description
Project metadata
Profile description
```

---

# 29. Testing

Backend:

```text
PHPUnit / Pest
Laravel Feature Tests
Laravel Unit Tests
```

Frontend:

```text
Type checking
Linting
Component tests where useful
```

E2E:

```text
Playwright
```

The exact final test framework configuration will be established during implementation.

---

# 30. Code Quality

Frontend:

```text
ESLint
TypeScript
Prettier where adopted
```

Backend:

```text
Laravel conventions
PHP formatter
Static analysis where justified
```

The project should avoid unnecessary formatting/tooling duplication.

---

# 31. Package Management

Frontend:

```text
npm
```

Backend:

```text
Composer
```

Lock files must be committed.

Frontend:

```text
package-lock.json
```

Backend:

```text
composer.lock
```

---

# 32. Version Control

Git is the source control system.

Repository:

```text
Git
```

Remote:

```text
GitHub
```

The repository contains:

```text
Source code
Documentation
Tests
Configuration templates
Migrations
```

Secrets must not be committed.

---

# 33. Environment Variables

Environment-specific configuration uses environment variables.

Examples:

```text
APP_ENV
APP_URL
DB_HOST
DB_DATABASE
DB_USERNAME
DB_PASSWORD
REDIS_HOST
API_URL
```

A safe example file should be provided:

```text
.env.example
```

Production `.env` must remain outside version control.

---

# 34. Docker

Docker is recommended for development and deployment consistency.

Potential services:

```text
frontend
api
mysql
redis
```

However, services should only be containerized when the deployment architecture requires it.

---

# 35. Development Infrastructure

Initial local architecture:

```text
Docker
├── MySQL
├── Redis
└── optional application containers
```

The exact Docker Compose architecture will be finalized during project setup.

---

# 36. Production Infrastructure

Initial production target:

```text
VPS
Nginx
Next.js
Laravel
MySQL
Redis
```

The system should remain deployable to a single VPS initially.

---

# 37. Reverse Proxy

Nginx is responsible for routing requests.

Conceptually:

```text
Browser
   ↓
Nginx
   ├── Website
   └── API
```

---

# 38. HTTPS

Production must use HTTPS.

All production traffic should use:

```text
HTTPS
```

HTTP should redirect to HTTPS.

---

# 39. CI/CD

CI/CD will initially verify:

```text
Install
Lint
Typecheck
Test
Build
```

Production deployment automation may be introduced after the application is stable.

---

# 40. Monitoring

Initial monitoring should remain simple.

Minimum:

```text
Uptime
Application logs
Server resources
Disk usage
Database availability
```

More advanced monitoring can be introduced later.

---

# 41. Analytics

Analytics is optional.

If implemented, it should be:

```text
Privacy-conscious
Lightweight
Documented
```

Analytics must not become a core application dependency.

---

# 42. Email

Email may be used for:

```text
Contact notifications
Administrative notifications
Password/account workflows if required
```

The application should use an SMTP-compatible provider.

---

# 43. External Services

External services should be minimized.

Potential services:

```text
Email provider
Object storage
Analytics
Monitoring
```

Each external dependency should have a clear purpose.

---

# 44. Technology Selection Rule

Before adding a new dependency, ask:

```text
Does it solve a real problem?
Can existing project capabilities solve it?
Does it introduce maintenance cost?
Is it actively maintained?
Is it compatible with the current stack?
```

If the answer is unclear, do not add it.

---

# 45. AI-Assisted Development

AI may be used extensively during development.

AI-generated code must follow:

```text
Existing architecture
Business rules
API contract
Security requirements
Testing strategy
Coding conventions
```

AI should not independently introduce major technology changes.

---

# 46. AI Dependency Rule

AI must not automatically install a package simply because it is convenient.

Before installing a package:

```text
Identify requirement
 ↓
Check existing capabilities
 ↓
Evaluate package
 ↓
Check maintenance
 ↓
Check compatibility
 ↓
Install
```

---

# 47. Long-Term Upgrade Strategy

The stack should be upgradeable.

Expected upgrade areas:

```text
PHP
Laravel
Node.js
Next.js
React
TypeScript
Tailwind CSS
Composer packages
NPM packages
MySQL
Redis
```

Upgrades must follow the maintenance and testing strategy.

---

# 48. Initial Technology Decision

The initial stack is:

```text
Frontend
Next.js
React
TypeScript
Tailwind CSS

Backend
Laravel
PHP
REST API
Laravel Sanctum

Database
MySQL

Cache
Redis

Storage
Laravel Storage

Testing
PHPUnit / Pest
Playwright

Web Server
Nginx

Infrastructure
Docker
VPS

Version Control
Git
GitHub
```

---

# 49. Technologies Explicitly Avoided Initially

The initial architecture does not require:

```text
Kubernetes
Microservices
GraphQL
Kafka
RabbitMQ
Elasticsearch
Multiple databases
Complex event-driven architecture
```

These may be evaluated in the future only when requirements justify them.

---

# 50. Architecture Goal

The final technology stack should provide:

```text
Simple development
Strong typing
Reliable backend
Structured API
Manageable CMS
Good SEO
Responsive UI
Automated testing
Secure deployment
Long-term maintainability
```

The architecture should remain simple enough for one developer to understand and operate.

---

# 51. Final Principle

Technology exists to support the portfolio.

The project should not become a demonstration of how many technologies can be used.

The preferred architecture is:

```text
Next.js
    ↓
Laravel API
    ↓
MySQL
    ↓
Redis / Storage
```

with:

```text
Git
Docker
Tests
CI/CD
Monitoring
Backups
```

supporting the system around it.
