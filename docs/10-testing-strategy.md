# Testing Strategy

## 1. Purpose

This document defines the testing strategy for the DwiDev Portfolio platform.

Testing is part of the development process and is not treated as an activity performed only before production deployment.

The purpose of testing is to ensure that:

* Features work according to requirements.
* API contracts remain stable.
* CMS operations are protected.
* Public portfolio pages remain functional.
* Authentication and authorization work correctly.
* Database operations are reliable.
* Media uploads are secure.
* Existing functionality does not break when new features are introduced.
* AI-generated code can be verified objectively.

---

# 2. Testing Principles

The project follows these principles:

```text
Test behavior, not implementation details.
Test critical business rules.
Test security boundaries.
Test API contracts.
Prefer automated tests.
Keep tests deterministic.
Keep tests maintainable.
Run tests before deployment.
```

Testing should focus on actual user and system behavior.

---

# 3. Testing Pyramid

The project follows a practical testing pyramid.

```text
              E2E
             /   \
            /     \
        Integration
          /       \
         /         \
       Unit Tests
```

The approximate priority is:

```text
Unit
→ many

Feature / Integration
→ moderate

E2E
→ fewer but critical
```

E2E tests should not replace lower-level tests.

---

# 4. Testing Layers

The system contains several testing layers:

```text
Frontend Unit / Component Tests
Backend Unit Tests
Backend Feature Tests
API Contract Tests
Integration Tests
E2E Tests
Security Tests
Build Tests
```

---

# 5. Backend Testing

Backend tests use Laravel's testing infrastructure.

Primary directories:

```text
apps/api/tests/
├── Feature/
├── Unit/
└── TestCase.php
```

---

# 6. Unit Tests

Unit tests verify isolated logic.

Examples:

```text
Slug generation
Data transformation
Formatting
Business calculations if introduced
Utility classes
Domain rules
```

Unit tests should avoid unnecessary external dependencies.

---

# 7. Feature Tests

Feature tests verify complete application behavior.

Examples:

```text
Login
Logout
Project CRUD
Project publishing
Profile update
Media upload
Contact submission
Authorization
API responses
```

Feature tests are expected to be the primary backend test layer.

---

# 8. Authentication Tests

Authentication must be tested automatically.

Required cases:

```text
Valid credentials
Invalid credentials
Missing credentials
Unknown credentials
Successful logout
Expired session/token
Unauthorized API request
```

Expected behavior must be defined for each case.

---

# 9. Authorization Tests

Authorization is security-critical.

Test at minimum:

```text
Guest
Authenticated user
Admin
```

against privileged operations.

Example:

```text
Guest
→ cannot create project

Authenticated non-admin
→ cannot create project

Admin
→ can create project
```

---

# 10. Project Tests

Project management is a core CMS feature.

Test:

```text
Create project
View project
Update project
Delete project
Publish project
Unpublish project if supported
Draft project
Published project
Slug uniqueness
Invalid project data
Unauthorized modification
```

---

# 11. Public Project Tests

Public project behavior:

```text
Published project
→ visible publicly

Draft project
→ not visible publicly

Archived project
→ not visible publicly if archive status exists
```

Project detail:

```text
/projects/{slug}
```

must resolve the correct project.

Unknown slug should produce:

```text
404
```

or the application's defined not-found behavior.

---

# 12. Profile Tests

Test:

```text
View public profile
Update profile
Validation
Authorization
Public/private field separation
```

Sensitive administrator information must never appear in public profile responses.

---

# 13. Skills Tests

Test:

```text
Create skill
Update skill
Delete skill
Publish/show skill
Hide skill
Validation
Authorization
```

If skills are directly associated with projects, relationship behavior must also be tested.

---

# 14. Experience Tests

Test:

```text
Create experience
Update experience
Delete experience
Ordering
Visibility
Validation
Authorization
```

Dates must be validated consistently.

---

# 15. Education Tests

Test:

```text
Create education
Update education
Delete education
Ordering
Visibility
Validation
Authorization
```

---

# 16. Certification Tests

Test:

```text
Create certification
Update certification
Delete certification
Publish certification
Hide certification
Certificate URL/file
Validation
Authorization
```

---

# 17. Media Tests

Media handling is security-sensitive.

Test:

```text
Valid image
Invalid extension
Invalid MIME
Oversized file
Invalid file
Unauthorized upload
Unauthorized deletion
Successful upload
Successful deletion
```

---

# 18. Contact Tests

Contact form:

```text
Valid submission
Missing name
Invalid email
Empty message
Message too long
Rate limit
Successful storage
```

Contact messages must not become publicly accessible.

---

# 19. API Response Tests

API responses should be tested against the documented contract.

Example:

```text
GET /api/v1/projects
```

must return:

```text
HTTP status
JSON structure
Expected fields
Pagination metadata if applicable
```

---

# 20. API Error Tests

Test standard errors:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
422 Validation Error
429 Too Many Requests
500 Internal Server Error
```

Only applicable errors need to be implemented for each endpoint.

---

# 21. API Contract

The API contract defined in:

```text
docs/05-api-contract.md
```

is the reference for API behavior.

When API behavior changes:

```text
API Contract
↓
Backend implementation
↓
Backend tests
↓
Frontend implementation
↓
Frontend tests
```

must remain synchronized.

---

# 22. Database Testing

Database tests should use an isolated testing database.

Tests must not accidentally modify production data.

Testing environment:

```text
Development
→ development database

Testing
→ isolated testing database

Production
→ production database
```

These environments must remain separate.

---

# 23. Database Reset

Tests should be repeatable.

The database should be reset or refreshed between tests where required.

Laravel database testing utilities should be preferred over manually deleting records.

---

# 24. Factories

Factories should generate realistic test data.

Example:

```text
ProjectFactory
UserFactory
SkillFactory
ExperienceFactory
CertificationFactory
MediaFactory
ContactMessageFactory
```

Factories should not create unnecessary complexity.

---

# 25. Seeders vs Factories

Use:

```text
Seeder
→ predefined application/demo data

Factory
→ dynamic test data
```

Example:

```text
PortfolioSeeder
→ default portfolio content

ProjectFactory
→ generate projects for tests
```

---

# 26. Frontend Testing

Frontend tests should be located in:

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

# 27. Component Tests

Component tests verify UI behavior.

Examples:

```text
Button
Form
ProjectCard
ProjectForm
ProjectTable
Modal
Navigation
Pagination
```

Test behavior such as:

```text
renders correctly
accepts interaction
displays validation
handles loading
handles errors
```

Do not test implementation details unnecessarily.

---

# 28. Public Page Tests

Test important public pages:

```text
Home
About
Projects
Project Detail
Experience
Certifications
Contact
```

Verify:

```text
Page renders
Expected content exists
Navigation works
Loading state works where applicable
Error state works
```

---

# 29. CMS Page Tests

Critical CMS flows:

```text
Login
Dashboard
Project list
Create project
Edit project
Delete project
Publish project
Media upload
Profile update
Message management
Logout
```

These flows should eventually have E2E coverage.

---

# 30. E2E Testing

End-to-end tests verify the complete system from the user's perspective.

Example:

```text
Browser
 ↓
Next.js
 ↓
Laravel API
 ↓
Database
```

E2E tests should represent important real-world workflows.

---

# 31. Critical E2E Flows

Initial E2E coverage:

```text
Admin login
Admin logout
Create project
Edit project
Publish project
View project publicly
Delete project
Upload project image
Submit contact form
View contact message
```

---

# 32. CMS Project Workflow

Critical workflow:

```text
Login
 ↓
Projects
 ↓
Create
 ↓
Enter data
 ↓
Save draft
 ↓
Edit
 ↓
Publish
 ↓
Open public project
 ↓
Verify content
```

This workflow should eventually be automated.

---

# 33. Contact Workflow

Critical workflow:

```text
Public website
 ↓
Contact
 ↓
Fill form
 ↓
Submit
 ↓
Success response
 ↓
Admin CMS
 ↓
Messages
 ↓
Verify message
```

---

# 34. Authentication E2E

Test:

```text
Open CMS
 ↓
Login
 ↓
Dashboard
 ↓
Logout
 ↓
Attempt admin access
 ↓
Redirect to login
```

This confirms the entire authentication flow.

---

# 35. Security Testing

Security tests must cover:

```text
Authentication
Authorization
IDOR
Validation
XSS
CSRF where applicable
CORS
Rate limiting
File upload
Sensitive data exposure
```

---

# 36. IDOR Test

Example:

```text
Admin A
→ Project 1

Request:
PUT /api/v1/projects/1
```

The application must verify authorization.

If multiple roles/users are introduced later, test access to another user's resources.

---

# 37. XSS Test

Attempt malicious content:

```text
<script>alert('x')</script>
```

through fields that accept user-controlled content.

Expected:

```text
Script does not execute.
```

If rich text is supported, sanitization must be verified.

---

# 38. Upload Security Test

Attempt:

```text
malicious.php
malicious.exe
oversized.jpg
invalid.pdf
../../../file.jpg
```

Expected:

```text
Rejected
```

according to the application's upload policy.

---

# 39. Rate Limit Test

Repeated requests should eventually produce:

```text
HTTP 429
```

for endpoints protected by rate limiting.

Examples:

```text
Login
Contact
Media upload
```

---

# 40. Sensitive Data Test

API responses must not contain:

```text
password
password_hash
token
secret
database_password
private_key
```

Tests should explicitly verify sensitive fields are absent.

---

# 41. Regression Testing

Every bug that reaches development or production should ideally receive a regression test.

Workflow:

```text
Bug discovered
 ↓
Fix implemented
 ↓
Regression test added
 ↓
Test suite passes
```

This prevents the same bug from returning.

---

# 42. Test Naming

Test names should describe behavior.

Good:

```text
admin_can_create_project
guest_cannot_create_project
draft_project_is_not_visible_publicly
invalid_image_upload_is_rejected
contact_form_requires_valid_email
```

Avoid:

```text
test1
testProject
testSomething
works
```

---

# 43. Deterministic Tests

Tests must produce consistent results.

Avoid dependencies on:

```text
Current external API
Random production data
Real email provider
Real payment provider
Real filesystem where unnecessary
```

Use mocks, fakes, factories, or isolated test services where appropriate.

---

# 44. External Services

External services should generally not be required for normal automated tests.

Examples:

```text
SMTP
Cloud storage
Analytics
External APIs
```

Tests should use:

```text
fake
mock
local service
```

where appropriate.

---

# 45. Email Testing

Contact notifications should not send real emails during automated tests.

Use Laravel's mail testing facilities.

Test:

```text
Notification triggered
Correct recipient
Correct subject
Expected message
```

without sending an actual email.

---

# 46. Cache Testing

If Redis/cache is introduced:

```text
Cache miss
→ database query

Cache hit
→ cached result

Cache invalidation
→ updated data becomes available
```

Cache behavior should be tested for critical endpoints.

---

# 47. Build Testing

CI must verify the application can build.

Frontend:

```text
npm run lint
npm run typecheck
npm run build
```

Backend:

```text
composer validate
php artisan test
```

Exact commands may change according to the final toolchain.

---

# 48. Static Analysis

Where practical, use:

```text
PHP static analysis
TypeScript type checking
ESLint
PHP code formatting
```

Static analysis complements automated tests.

It does not replace them.

---

# 49. CI Testing

Every important pull request should run:

```text
Frontend lint
Frontend typecheck
Frontend tests
Backend tests
Backend static analysis where configured
Frontend build
Backend validation
```

The exact CI pipeline may evolve.

---

# 50. Local Development Workflow

Before committing:

```text
Modify code
 ↓
Run relevant tests
 ↓
Run lint/typecheck
 ↓
Run build when appropriate
 ↓
Review diff
 ↓
Commit
```

Do not rely exclusively on CI.

---

# 51. Feature Development Workflow

For a new feature:

```text
Requirement
 ↓
Business rule
 ↓
Database changes
 ↓
API contract
 ↓
Backend implementation
 ↓
Backend tests
 ↓
Frontend implementation
 ↓
Frontend tests
 ↓
E2E if critical
 ↓
Documentation
```

---

# 52. Bug Fix Workflow

For a bug:

```text
Reproduce
 ↓
Identify cause
 ↓
Write failing test where practical
 ↓
Fix
 ↓
Run test
 ↓
Run regression suite
 ↓
Review
```

---

# 53. AI / Vibe Coding Testing Rule

AI-generated code is not considered complete simply because:

```text
code compiles
```

or:

```text
page appears to work
```

AI-generated features must satisfy:

```text
Requirement
+
Tests
+
Security
+
Existing architecture
```

---

# 54. AI Coding Workflow

AI should follow:

```text
Understand requirement
 ↓
Inspect existing code
 ↓
Identify affected files
 ↓
Implement
 ↓
Write/update tests
 ↓
Run tests
 ↓
Fix failures
 ↓
Review diff
```

AI should not skip testing because the change appears simple.

---

# 55. AI Test Generation

When AI creates a new feature, it should identify:

```text
Happy path
Validation failure
Unauthorized access
Not found
Edge case
Error handling
```

At minimum, critical features should have tests for these categories.

---

# 56. Definition of Done

A feature is considered complete when:

```text
Requirement implemented
Database correct
API contract correct
Validation implemented
Authorization implemented
Frontend implemented
Loading state handled
Error state handled
Tests implemented
Tests passing
Lint passing
Typecheck passing
Build passing
Documentation updated where necessary
```

---

# 57. Critical Feature Definition

The following features are considered critical:

```text
Authentication
Authorization
Project CRUD
Publishing
Media upload
Contact form
Profile management
API security
```

Critical features require stronger automated coverage.

---

# 58. Test Coverage

Code coverage is a supporting metric.

The project should prioritize:

```text
Business-critical paths
Security-critical paths
Frequently changing code
Complex logic
```

rather than blindly targeting a high percentage.

A high coverage percentage does not automatically mean high quality.

---

# 59. Test Environment

The test environment should be isolated.

Example:

```text
APP_ENV=testing
```

Database:

```text
portfolio_testing
```

Redis/cache:

```text
isolated test configuration
```

No production credentials should be used.

---

# 60. Production Deployment Gate

Production deployment should require:

```text
Tests passing
Build passing
No known critical security issue
Database migration reviewed
Environment variables verified
Backup available
Deployment configuration verified
```

---

# 61. Smoke Test After Deployment

After deployment:

```text
Open homepage
Open projects
Open project detail
Open CMS
Login
Check API health
Submit/test contact flow where appropriate
Check media
Check logs
```

The exact production smoke test should avoid creating unwanted real data.

---

# 62. Rollback

If deployment introduces a critical failure:

```text
Identify failure
 ↓
Stop further rollout
 ↓
Rollback application if necessary
 ↓
Restore previous stable version
 ↓
Investigate
 ↓
Fix
 ↓
Test
 ↓
Redeploy
```

Database migrations must be designed carefully because application rollback does not automatically mean database rollback is safe.

---

# 63. Test Documentation

Testing commands should be documented in:

```text
README.md
```

and detailed testing behavior may be documented here.

Developers should not need to guess how to run the test suite.

---

# 64. Initial Testing Priority

During the first implementation phase, prioritize:

```text
1. Authentication
2. Authorization
3. Projects
4. API
5. CMS CRUD
6. Media
7. Contact
8. Public pages
```

Lower-priority UI details can receive tests after core functionality is stable.

---

# 65. Long-Term Testing Strategy

As the portfolio grows:

```text
More features
 ↓
More regression tests
 ↓
More E2E coverage for critical workflows
 ↓
Better CI
 ↓
Safer deployments
```

Testing should evolve with the application rather than becoming an obstacle to development.

---

# 66. Final Principle

The project's testing philosophy is:

> Every important behavior should be verifiable, every security boundary should be tested, and every significant regression should become a test.

The goal is not to produce the largest possible test suite.

The goal is to produce a system that can be changed confidently over the next 1–2 years.
