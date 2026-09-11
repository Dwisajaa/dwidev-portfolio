# API Contract

## 1. Purpose

This document defines the API contract between the DwiDev Portfolio frontend, CMS, and Laravel backend.

The API is designed as a versioned REST API.

Base path:

```text
/api/v1
```

The Laravel backend is the authoritative source for:

* Authentication
* Authorization
* Validation
* Business rules
* Content publication
* Data persistence
* File validation
* Rate limiting

---

# 2. API Architecture

The application consists of two API areas:

```text
/api/v1/public/*
/api/v1/admin/*
```

Public API:

```text
Next.js Public Website
        ↓
Laravel Public API
        ↓
MySQL
```

Admin API:

```text
CMS
 ↓
Laravel Admin API
 ↓
Authentication
 ↓
Authorization
 ↓
MySQL
```

---

# 3. API Versioning

Current API version:

```text
v1
```

Example:

```text
/api/v1/public/projects
```

Future breaking changes should use:

```text
/api/v2
```

The existing version should not be silently changed in a breaking manner.

---

# 4. Content Types

The initial API exposes:

```text
profile
projects
categories
technologies
skills
experiences
educations
certifications
resume
contact messages
media
settings
```

---

# 5. HTTP Methods

The API uses standard HTTP methods.

```text
GET
```

Read resource.

```text
POST
```

Create resource or execute an action.

```text
PATCH
```

Partially update resource.

```text
PUT
```

Replace a resource when full replacement is appropriate.

```text
DELETE
```

Delete or soft-delete a resource.

---

# 6. Authentication

Admin authentication uses token-based authentication.

The frontend sends:

```http
Authorization: Bearer {token}
```

Authentication-protected endpoints must reject requests without a valid token.

Unauthenticated response:

```http
401 Unauthorized
```

---

# 7. Public API

Public API endpoints do not require authentication.

Base:

```text
/api/v1/public
```

---

# 8. Public Profile

## GET /public/profile

Returns the public developer profile.

Request:

```http
GET /api/v1/public/profile
```

Response:

```json
{
  "data": {
    "name": "Muhammad Dwi Riswanto",
    "professional_title": "Software Developer",
    "short_bio": "Short professional introduction",
    "biography": "Full biography",
    "email": "email@example.com",
    "location": "Surabaya, Indonesia",
    "availability": "Available for opportunities",
    "social_links": []
  }
}
```

The exact profile fields may evolve with the frontend requirements.

Sensitive administrative information must never be returned.

---

# 9. Public Projects

## GET /public/projects

Returns published projects.

Request:

```http
GET /api/v1/public/projects
```

Optional query parameters:

```text
page
per_page
search
category
technology
featured
```

Example:

```http
GET /api/v1/public/projects?page=1&per_page=12
```

---

# 10. Project Filtering

Example:

```http
GET /api/v1/public/projects?category=backend
```

Technology:

```http
GET /api/v1/public/projects?technology=laravel
```

Search:

```http
GET /api/v1/public/projects?search=inventory
```

Featured:

```http
GET /api/v1/public/projects?featured=true
```

Multiple filters may be combined.

---

# 11. Public Project Response

Example:

```json
{
  "data": [
    {
      "id": 1,
      "title": "MekarMuda Workshop Management System",
      "slug": "mekarmuda-workshop-management-system",
      "short_description": "Workshop and inventory management application.",
      "role": "Backend Developer",
      "category": {
        "id": 1,
        "name": "Web Application",
        "slug": "web-application"
      },
      "technologies": [
        {
          "id": 1,
          "name": "Laravel",
          "slug": "laravel"
        },
        {
          "id": 2,
          "name": "MySQL",
          "slug": "mysql"
        }
      ],
      "thumbnail": {},
      "is_featured": true,
      "published_at": "2026-01-01T00:00:00Z"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 12,
    "total": 1,
    "last_page": 1
  }
}
```

---

# 12. Public Project Detail

## GET /public/projects/{slug}

Example:

```http
GET /api/v1/public/projects/mekarmuda-workshop-management-system
```

Returns the complete published project.

Response may contain:

```json
{
  "data": {
    "id": 1,
    "title": "MekarMuda Workshop Management System",
    "slug": "mekarmuda-workshop-management-system",
    "short_description": "Workshop and inventory management application.",
    "description": "Full project description.",
    "role": "Backend Developer",
    "category": {},
    "technologies": [],
    "media": [],
    "repository_url": "https://github.com/example/project",
    "demo_url": null,
    "seo": {
      "title": "MekarMuda Workshop Management System",
      "description": "Workshop and inventory management application."
    },
    "published_at": "2026-01-01T00:00:00Z"
  }
}
```

Draft and archived projects must not be returned.

If the slug does not correspond to a published project:

```http
404 Not Found
```

---

# 13. Public Skills

## GET /public/skills

Returns active skills.

```http
GET /api/v1/public/skills
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Laravel",
      "slug": "laravel",
      "category": {
        "id": 1,
        "name": "Backend"
      }
    }
  ]
}
```

---

# 14. Public Experience

## GET /public/experiences

```http
GET /api/v1/public/experiences
```

Only non-deleted experience records are returned.

Recommended ordering:

```text
current first
then newest start date
```

---

# 15. Public Education

## GET /public/educations

```http
GET /api/v1/public/educations
```

Returns public education records.

---

# 16. Public Certifications

## GET /public/certifications

```http
GET /api/v1/public/certifications
```

Returns public certifications.

---

# 17. Public Resume

## GET /public/resume

Returns metadata for the active resume.

```http
GET /api/v1/public/resume
```

Response:

```json
{
  "data": {
    "version_label": "2026",
    "published_at": "2026-01-01T00:00:00Z",
    "download_url": "/storage/resume/resume-2026.pdf"
  }
}
```

Only the active public resume is returned.

---

# 18. Public Contact

## POST /public/contact

Creates a contact message.

```http
POST /api/v1/public/contact
```

Request:

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "subject": "Job Opportunity",
  "message": "Hello, I would like to discuss..."
}
```

Required fields:

```text
name
email
subject
message
```

---

# 19. Contact Validation

Example validation:

```text
name
required|string|max:255

email
required|email|max:255

subject
required|string|max:255

message
required|string|max:10000
```

Additional anti-spam controls may be applied.

---

# 20. Contact Success

Successful response:

```http
201 Created
```

Example:

```json
{
  "message": "Message submitted successfully."
}
```

The API should not return unnecessary internal database information.

---

# 21. Contact Rate Limiting

The endpoint:

```text
POST /public/contact
```

must be rate-limited.

Rate limits are implementation configuration and should not be hardcoded into the API contract unless a specific product requirement exists.

Excessive requests:

```http
429 Too Many Requests
```

---

# 22. Public Categories

## GET /public/categories

Returns active project categories.

```http
GET /api/v1/public/categories
```

Example:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Web Application",
      "slug": "web-application"
    }
  ]
}
```

---

# 23. Public Technologies

## GET /public/technologies

Returns active technologies.

```http
GET /api/v1/public/technologies
```

---

# 24. Admin API

Base:

```text
/api/v1/admin
```

All admin endpoints require authentication.

---

# 25. Admin Dashboard

## GET /admin/dashboard

```http
GET /api/v1/admin/dashboard
```

Returns administrative summary information.

Example:

```json
{
  "data": {
    "projects": {
      "total": 12,
      "published": 8,
      "draft": 3,
      "archived": 1
    },
    "messages": {
      "total": 25,
      "unread": 4
    },
    "certifications": 6
  }
}
```

Dashboard data should remain lightweight.

---

# 26. Admin Projects

## GET /admin/projects

```http
GET /api/v1/admin/projects
```

Supports:

```text
page
per_page
search
status
category
technology
featured
```

Unlike the public endpoint, the admin endpoint may return:

```text
draft
published
archived
```

projects.

---

# 27. Admin Project Detail

## GET /admin/projects/{id}

```http
GET /api/v1/admin/projects/{id}
```

Returns complete project information for editing.

---

# 28. Create Project

## POST /admin/projects

```http
POST /api/v1/admin/projects
```

Example:

```json
{
  "title": "MekarMuda Workshop Management System",
  "slug": "mekarmuda-workshop-management-system",
  "short_description": "Workshop management system.",
  "description": "Full project description.",
  "category_id": 1,
  "technology_ids": [1, 2, 3],
  "role": "Backend Developer",
  "repository_url": "https://github.com/example/project",
  "demo_url": null,
  "is_featured": false,
  "status": "draft"
}
```

The default status should be:

```text
draft
```

---

# 29. Update Project

## PATCH /admin/projects/{id}

```http
PATCH /api/v1/admin/projects/{id}
```

Only supplied fields need to be updated.

---

# 30. Delete Project

## DELETE /admin/projects/{id}

```http
DELETE /api/v1/admin/projects/{id}
```

Important project records should use soft deletion.

The exact response:

```http
204 No Content
```

or a standard success response may be selected during implementation.

---

# 31. Publish Project

## POST /admin/projects/{id}/publish

```http
POST /api/v1/admin/projects/{id}/publish
```

The backend must validate publication requirements.

If requirements are incomplete:

```http
422 Unprocessable Entity
```

Example:

```json
{
  "message": "Project cannot be published.",
  "errors": {
    "thumbnail": [
      "A thumbnail is required before publishing."
    ]
  }
}
```

---

# 32. Archive Project

## POST /admin/projects/{id}/archive

```http
POST /api/v1/admin/projects/{id}/archive
```

Changes:

```text
published → archived
```

---

# 33. Restore Project

## POST /admin/projects/{id}/restore

```http
POST /api/v1/admin/projects/{id}/restore
```

Restores an archived project.

The final implementation must determine whether restoration results in:

```text
archived → published
```

or:

```text
archived → draft
```

The preferred default is:

```text
archived → draft
```

to prevent accidental publication.

---

# 34. Admin Categories

## GET

```http
GET /api/v1/admin/categories
```

## POST

```http
POST /api/v1/admin/categories
```

## GET DETAIL

```http
GET /api/v1/admin/categories/{id}
```

## PATCH

```http
PATCH /api/v1/admin/categories/{id}
```

## DELETE

```http
DELETE /api/v1/admin/categories/{id}
```

---

# 35. Admin Technologies

## GET

```http
GET /api/v1/admin/technologies
```

## POST

```http
POST /api/v1/admin/technologies
```

## GET DETAIL

```http
GET /api/v1/admin/technologies/{id}
```

## PATCH

```http
PATCH /api/v1/admin/technologies/{id}
```

## DELETE

```http
DELETE /api/v1/admin/technologies/{id}
```

---

# 36. Admin Skills

## GET

```http
GET /api/v1/admin/skills
```

## POST

```http
POST /api/v1/admin/skills
```

## GET DETAIL

```http
GET /api/v1/admin/skills/{id}
```

## PATCH

```http
PATCH /api/v1/admin/skills/{id}
```

## DELETE

```http
DELETE /api/v1/admin/skills/{id}
```

---

# 37. Admin Experience

## GET

```http
GET /api/v1/admin/experiences
```

## POST

```http
POST /api/v1/admin/experiences
```

## GET DETAIL

```http
GET /api/v1/admin/experiences/{id}
```

## PATCH

```http
PATCH /api/v1/admin/experiences/{id}
```

## DELETE

```http
DELETE /api/v1/admin/experiences/{id}
```

---

# 38. Admin Education

## GET

```http
GET /api/v1/admin/educations
```

## POST

```http
POST /api/v1/admin/educations
```

## GET DETAIL

```http
GET /api/v1/admin/educations/{id}
```

## PATCH

```http
PATCH /api/v1/admin/educations/{id}
```

## DELETE

```http
DELETE /api/v1/admin/educations/{id}
```

---

# 39. Admin Certifications

## GET

```http
GET /api/v1/admin/certifications
```

## POST

```http
POST /api/v1/admin/certifications
```

## GET DETAIL

```http
GET /api/v1/admin/certifications/{id}
```

## PATCH

```http
PATCH /api/v1/admin/certifications/{id}
```

## DELETE

```http
DELETE /api/v1/admin/certifications/{id}
```

---

# 40. Admin Profile

## GET

```http
GET /api/v1/admin/profile
```

## PATCH

```http
PATCH /api/v1/admin/profile
```

The admin profile endpoint manages the public developer profile.

---

# 41. Admin Social Links

## GET

```http
GET /api/v1/admin/profile/social-links
```

## POST

```http
POST /api/v1/admin/profile/social-links
```

## PATCH

```http
PATCH /api/v1/admin/profile/social-links/{id}
```

## DELETE

```http
DELETE /api/v1/admin/profile/social-links/{id}
```

---

# 42. Admin Resume

## GET

```http
GET /api/v1/admin/resumes
```

## POST

```http
POST /api/v1/admin/resumes
```

## PATCH

```http
PATCH /api/v1/admin/resumes/{id}
```

## DELETE

```http
DELETE /api/v1/admin/resumes/{id}
```

Resume upload uses multipart form data.

---

# 43. Admin Media

## GET

```http
GET /api/v1/admin/media
```

## POST

```http
POST /api/v1/admin/media
```

Upload request:

```text
Content-Type: multipart/form-data
```

Example fields:

```text
file
alt_text
```

---

# 44. Media Validation

The backend validates:

```text
MIME type
extension
file size
image dimensions
```

Executable and unsafe file types must be rejected.

---

# 45. Delete Media

## DELETE /admin/media/{id}

```http
DELETE /api/v1/admin/media/{id}
```

Deleting media from the CMS must not automatically mean immediate physical deletion if the file is still referenced.

Reference checks should be performed before permanent cleanup.

---

# 46. Admin Messages

## GET /admin/messages

```http
GET /api/v1/admin/messages
```

Supports:

```text
page
per_page
search
status
```

---

# 47. Admin Message Detail

```http
GET /api/v1/admin/messages/{id}
```

Returns:

```json
{
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "subject": "Job Opportunity",
    "message": "Hello...",
    "status": "unread",
    "created_at": "2026-01-01T10:00:00Z"
  }
}
```

---

# 48. Mark Message as Read

```http
POST /api/v1/admin/messages/{id}/read
```

Transition:

```text
unread → read
```

---

# 49. Archive Message

```http
POST /api/v1/admin/messages/{id}/archive
```

Transition:

```text
read → archived
```

---

# 50. Admin Settings

## GET

```http
GET /api/v1/admin/settings
```

## PATCH

```http
PATCH /api/v1/admin/settings
```

Only non-sensitive application settings may be managed here.

Secrets such as:

```text
API keys
database passwords
application keys
private credentials
```

must remain in environment configuration.

---

# 51. Authentication Endpoints

Base:

```text
/api/v1/auth
```

## POST /auth/login

```http
POST /api/v1/auth/login
```

Request:

```json
{
  "email": "admin@example.com",
  "password": "password"
}
```

Successful response:

```json
{
  "data": {
    "user": {
      "id": 1,
      "name": "Administrator",
      "email": "admin@example.com"
    },
    "token": "..."
  }
}
```

The actual token representation depends on the authentication implementation.

---

# 52. Current User

```http
GET /api/v1/auth/me
```

Returns the authenticated administrator.

---

# 53. Logout

```http
POST /api/v1/auth/logout
```

Invalidates the current authentication session/token according to the selected authentication strategy.

---

# 54. Standard Success Response

Single resource:

```json
{
  "data": {}
}
```

Collection:

```json
{
  "data": [],
  "meta": {}
}
```

Action:

```json
{
  "message": "Operation completed successfully."
}
```

---

# 55. Standard Error Response

Validation:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "field": [
      "The field is required."
    ]
  }
}
```

General error:

```json
{
  "message": "Unable to process the request."
}
```

Do not expose:

```text
stack traces
SQL queries
database credentials
internal exception details
server filesystem paths
```

in production API responses.

---

# 56. HTTP Status Codes

Expected status codes:

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
429 Too Many Requests
500 Internal Server Error
```

---

# 57. Pagination

Collection endpoints use:

```text
page
per_page
```

Example:

```http
GET /api/v1/public/projects?page=2&per_page=12
```

Default page size:

```text
12
```

Maximum page size should be enforced server-side.

---

# 58. Resource Filtering

Filtering must use explicit query parameters.

Example:

```text
?status=published
?category=backend
?technology=laravel
?featured=true
```

Unknown filters should not silently alter query semantics.

---

# 59. Sorting

Sorting parameters may be introduced where required.

Example:

```text
?sort=-published_at
```

Allowed sort fields must be whitelisted.

User input must not be directly converted into raw SQL ordering expressions.

---

# 60. Search

Search uses:

```text
?search=query
```

Searchable fields are explicitly defined by the backend.

Search parameters must be safely passed through the query builder.

---

# 61. URL Rules

Public project URLs use:

```text
/projects/{slug}
```

Admin resources primarily use IDs:

```text
/admin/projects/{id}
```

This separates human-readable public URLs from administrative resource identification.

---

# 62. API Security

The API must implement:

```text
Authentication
Authorization
Validation
Rate limiting
CORS policy
Input sanitization
Mass-assignment protection
Secure file validation
Consistent error handling
```

---

# 63. Mass Assignment

Laravel models must explicitly define allowed fields.

The application must not blindly accept arbitrary request fields.

---

# 64. Authorization

Admin endpoints must verify:

```text
authenticated
+
authorized
```

Authentication means:

```text
Who are you?
```

Authorization means:

```text
Are you allowed to perform this action?
```

---

# 65. Public Data Isolation

Public serializers/resources must explicitly define what fields are exposed.

Do not return entire database models directly.

The API should use API Resources or equivalent response transformers.

---

# 66. N+1 Prevention

Collection endpoints must eager-load required relationships.

Example:

```text
projects
 ├── category
 ├── technologies
 └── media
```

The backend should avoid generating one database query per project relationship.

---

# 67. API Transactions

Operations involving multiple related writes should use database transactions where required.

Example:

```text
Create Project
    ↓
Attach Technologies
    ↓
Attach Media
    ↓
Audit
```

If a failure occurs in a transactional operation, the database should not remain partially updated.

---

# 68. API Caching

Public GET endpoints may be cached.

Potential cache targets:

```text
/public/profile
/public/projects
/public/projects/{slug}
/public/skills
/public/experiences
/public/educations
/public/certifications
```

Administrative endpoints should not use the same public cache layer.

---

# 69. Cache Invalidation

When content changes:

```text
Draft → Published
Published → Updated
Published → Archived
```

related public cache must be invalidated or revalidated.

---

# 70. API Documentation

The API should eventually be documented using OpenAPI.

Target:

```text
docs/openapi.yaml
```

The OpenAPI specification must reflect this API contract.

The API implementation and OpenAPI document should remain synchronized.

---

# 71. Contract Change Policy

Any breaking API change requires:

1. Update API contract.
2. Update OpenAPI specification.
3. Update backend implementation.
4. Update frontend client.
5. Update tests.
6. Document migration requirements.

Do not change API response structures casually.

---

# 72. API Development Principle

The API should be:

```text
Predictable
Versioned
Explicit
Secure
Testable
Documented
```

The frontend should consume the API contract rather than relying on undocumented backend behavior.

---

# 73. Initial Endpoint Summary

```text
AUTH
POST   /auth/login
GET    /auth/me
POST   /auth/logout

PUBLIC
GET    /public/profile
GET    /public/projects
GET    /public/projects/{slug}
GET    /public/categories
GET    /public/technologies
GET    /public/skills
GET    /public/experiences
GET    /public/educations
GET    /public/certifications
GET    /public/resume
POST   /public/contact

ADMIN DASHBOARD
GET    /admin/dashboard

ADMIN PROJECTS
GET    /admin/projects
POST   /admin/projects
GET    /admin/projects/{id}
PATCH  /admin/projects/{id}
DELETE /admin/projects/{id}
POST   /admin/projects/{id}/publish
POST   /admin/projects/{id}/archive
POST   /admin/projects/{id}/restore

ADMIN CATEGORIES
GET    /admin/categories
POST   /admin/categories
GET    /admin/categories/{id}
PATCH  /admin/categories/{id}
DELETE /admin/categories/{id}

ADMIN TECHNOLOGIES
GET    /admin/technologies
POST   /admin/technologies
GET    /admin/technologies/{id}
PATCH  /admin/technologies/{id}
DELETE /admin/technologies/{id}

ADMIN SKILLS
GET    /admin/skills
POST   /admin/skills
GET    /admin/skills/{id}
PATCH  /admin/skills/{id}
DELETE /admin/skills/{id}

ADMIN EXPERIENCE
GET    /admin/experiences
POST   /admin/experiences
GET    /admin/experiences/{id}
PATCH  /admin/experiences/{id}
DELETE /admin/experiences/{id}

ADMIN EDUCATION
GET    /admin/educations
POST   /admin/educations
GET    /admin/educations/{id}
PATCH  /admin/educations/{id}
DELETE /admin/educations/{id}

ADMIN CERTIFICATIONS
GET    /admin/certifications
POST   /admin/certifications
GET    /admin/certifications/{id}
PATCH  /admin/certifications/{id}
DELETE /admin/certifications/{id}

ADMIN PROFILE
GET    /admin/profile
PATCH  /admin/profile

ADMIN SOCIAL LINKS
GET    /admin/profile/social-links
POST   /admin/profile/social-links
PATCH  /admin/profile/social-links/{id}
DELETE /admin/profile/social-links/{id}

ADMIN RESUME
GET    /admin/resumes
POST   /admin/resumes
PATCH  /admin/resumes/{id}
DELETE /admin/resumes/{id}

ADMIN MEDIA
GET    /admin/media
POST   /admin/media
DELETE /admin/media/{id}

ADMIN MESSAGES
GET    /admin/messages
GET    /admin/messages/{id}
POST   /admin/messages/{id}/read
POST   /admin/messages/{id}/archive

ADMIN SETTINGS
GET    /admin/settings
PATCH  /admin/settings
```

---

# 74. Contract Authority

When frontend implementation and backend implementation disagree, this document is the initial contract authority.

If the business requirement changes:

```text
Business requirement
        ↓
Business rules
        ↓
Database
        ↓
API contract
        ↓
Backend
        ↓
Frontend
```

Changes should propagate through this order rather than modifying only the frontend.

---

# 75. AI Development Rule

AI-assisted development must read:

```text
01-product-requirements.md
02-sitemap-user-flow.md
03-business-rules.md
04-database.md
05-api-contract.md
```

before implementing a feature that affects these areas.

AI must not:

* Invent endpoint names.
* Invent response structures.
* Invent business states.
* Bypass backend validation.
* Expose database models directly.
* Add undocumented fields.
* Change API contracts silently.

If a requirement is missing or ambiguous, AI must identify the ambiguity before implementation.
