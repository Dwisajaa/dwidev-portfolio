# Business Rules

## 1. Purpose

This document defines the business rules and state transitions of the DwiDev Portfolio CMS.

These rules are application-level rules and must be respected by the backend, frontend, CMS, and AI-assisted development workflow.

The backend is the final authority for business rules.

---

# 2. General Rules

## BR-001 — Backend Authority

The Laravel API is the final authority for:

* Authentication
* Authorization
* Validation
* Content publication
* Content state transitions
* Data integrity
* File validation
* Rate limiting

Frontend validation is for user experience and does not replace backend validation.

---

## BR-002 — Public Content

Only content explicitly marked as published may appear on the public website.

Draft and archived content must not be included in normal public API responses.

---

## BR-003 — Administrative Access

CMS management operations require an authenticated administrator.

Public visitors do not require authentication.

---

## BR-004 — Soft Deletion

Content that may need recovery or historical preservation should use soft deletion where appropriate.

Permanent deletion must not be the default behavior for important portfolio content.

---

# 3. Project Rules

## BR-005 — Project Status

A project has one of three states:

```text
draft
published
archived
```

Default state:

```text
draft
```

---

## BR-006 — Draft Project

A draft project:

* Is visible in the CMS.
* Is editable by the administrator.
* Is not included in public project listings.
* Must not appear in the public sitemap.
* Must not be treated as published content.

---

## BR-007 — Published Project

A published project:

* Is visible publicly.
* Appears in the public project listing.
* May appear on the home page if marked featured.
* Has a publication timestamp.
* Must have all required publication fields.

---

## BR-008 — Archived Project

An archived project:

* Remains stored in the CMS.
* Is not displayed in normal public listings.
* Is not included in the public sitemap.
* Can be restored by an authorized administrator.

---

## BR-009 — Project Publication Requirements

A project cannot be published unless the required information is complete.

Minimum required fields:

```text
title
slug
short_description
description
category
at least one technology
thumbnail
```

Additional fields such as repository URL and demo URL are optional.

---

## BR-010 — Project Slug

Project slugs must:

* Be lowercase.
* Use hyphens as separators.
* Contain only URL-safe characters.
* Be unique among active projects.

Example:

```text
mekarmuda-workshop-management-system
```

Invalid examples:

```text
MekarMuda Project
project_123
Project@2026
```

---

## BR-011 — Slug Stability

Once a project is published, changing its slug should be treated as a significant operation because external URLs and search engines may reference it.

The system should require explicit confirmation before changing a published slug.

Future support for redirects may be implemented.

---

## BR-012 — Featured Project

A project may be marked:

```text
featured = true
```

Featured projects may appear on the home page.

Featured status does not automatically mean the project is published.

Therefore:

```text
draft + featured
```

must still remain invisible publicly.

Only:

```text
published + featured
```

may appear as a featured public project.

---

## BR-013 — Project Ordering

Projects displayed publicly should have deterministic ordering.

Default ordering:

1. Featured projects first.
2. Newer published projects next.
3. Older published projects after that.

The system may later introduce explicit manual ordering.

---

# 4. Technology Rules

## BR-014 — Technology Entity

Technologies are reusable entities.

Examples:

```text
Laravel
PHP
Go
Next.js
React
TypeScript
MySQL
Redis
Docker
Python
```

The same technology must not be duplicated for every project.

---

## BR-015 — Project Technology Relationship

A project may use multiple technologies.

A technology may belong to multiple projects.

Relationship:

```text
Project
   ↕
Project Technology
   ↕
Technology
```

This is a many-to-many relationship.

---

## BR-016 — Technology Name

Technology names should be unique case-insensitively.

Examples:

```text
Laravel
laravel
LARAVEL
```

must not create three separate technology records.

---

# 5. Category Rules

## BR-017 — Project Category

Each published project must have one primary category.

Examples:

```text
Web Application
Backend
API
Mobile
IoT
AI / Machine Learning
Computer Vision
Automation
```

---

## BR-018 — Category Reuse

Categories are reusable entities.

A category should not be stored as arbitrary free text on every project.

---

# 6. Profile Rules

## BR-019 — Single Active Profile

The system contains one active public profile.

The CMS may manage profile information, but there should not be multiple simultaneously active profiles.

---

## BR-020 — Age

Age must not be stored as a permanent profile field.

Age changes over time.

If age is ever required for display, it must be derived from date of birth, or preferably omitted from the professional portfolio.

---

## BR-021 — Social Links

Social links should be stored as structured profile data.

Examples:

```text
GitHub
LinkedIn
Email
Website
```

The system should allow future social platforms without requiring database redesign.

---

# 7. Skills Rules

## BR-022 — Skill Categories

Every skill belongs to a reusable skill category.

Examples:

```text
Backend
Frontend
Database
DevOps
Programming Language
IoT
AI / Machine Learning
Tools
```

---

## BR-023 — Skill Uniqueness

A skill name must be unique within the system.

---

## BR-024 — Skill Ordering

Skills may have a display order.

The CMS administrator can control ordering without changing source code.

---

## BR-025 — Skill Proficiency

The system should not require an arbitrary percentage such as:

```text
Laravel = 95%
Go = 80%
```

Proficiency percentages should not be used as the default portfolio presentation.

The portfolio should emphasize actual technologies and demonstrated projects.

---

# 8. Experience Rules

## BR-026 — Experience Status

Experience may be:

```text
current
completed
```

---

## BR-027 — Current Experience

If an experience is current:

```text
end_date = null
```

The frontend displays an appropriate current indicator.

---

## BR-028 — Completed Experience

A completed experience must have:

```text
end_date != null
```

and:

```text
end_date >= start_date
```

---

## BR-029 — Experience Ordering

Experience should be displayed in reverse chronological order.

Current and most recent experience appears first.

---

# 9. Education Rules

## BR-030 — Education Status

Education may be:

```text
current
completed
```

---

## BR-031 — Current Education

If education is current:

```text
end_date = null
```

---

## BR-032 — Completed Education

A completed education record must have an end date.

The end date must not be earlier than the start date.

---

## BR-033 — Education Ordering

Education should normally be displayed in reverse chronological order.

---

# 10. Certification Rules

## BR-034 — Certification Issue Date

Every certification must have an issue date.

---

## BR-035 — Certification Expiration

Expiration date is optional.

A certification without an expiration date is considered non-expiring unless the issuer states otherwise.

---

## BR-036 — Certification Date Validation

If an expiration date exists:

```text
expiration_date >= issue_date
```

---

## BR-037 — Credential URL

Credential URL is optional.

If provided, it must use a valid HTTP or HTTPS URL.

---

# 11. Resume Rules

## BR-038 — Active Resume

The system may contain multiple uploaded resume versions.

Only one resume should be designated as the active public resume.

---

## BR-039 — Resume Versioning

A new resume upload should not immediately destroy the previous resume.

Previous versions may be retained for administrative purposes.

---

## BR-040 — Public Resume

Only the active resume is publicly downloadable.

---

# 12. Media Rules

## BR-041 — Media Ownership

Media files are managed through the CMS media library.

---

## BR-042 — Media Metadata

Each media item should store metadata such as:

```text
filename
path
disk
mime_type
size
width
height
alt_text
created_at
```

---

## BR-043 — File Validation

Uploaded files must be validated on the backend.

Validation should consider:

* MIME type
* File extension
* File size
* Image dimensions where applicable

---

## BR-044 — Executable Files

Executable files and server-side script files must not be accepted as normal portfolio media.

Examples:

```text
.php
.exe
.sh
.bat
```

must not be accepted by image/document upload endpoints.

---

## BR-045 — Orphaned Media

Media that is no longer referenced should be identifiable by the CMS.

Automatic deletion is not required initially.

---

# 13. Contact Message Rules

## BR-046 — Public Submission

Visitors may submit contact messages without authentication.

---

## BR-047 — Required Contact Fields

Required:

```text
name
email
subject
message
```

---

## BR-048 — Email Validation

The email address must pass backend validation.

---

## BR-049 — Rate Limiting

Contact submissions must be rate-limited to reduce spam and abuse.

---

## BR-050 — Message Status

Messages have the following states:

```text
unread
read
archived
```

---

## BR-051 — New Message

A newly submitted message starts as:

```text
unread
```

---

## BR-052 — Read Message

When an administrator opens or explicitly marks a message as read:

```text
unread → read
```

---

## BR-053 — Archive Message

A read message may be archived:

```text
read → archived
```

Archived messages remain stored unless explicitly deleted according to the retention policy.

---

# 14. Authentication Rules

## BR-054 — Admin Authentication

CMS management endpoints require authentication.

---

## BR-055 — Admin Authorization

Authentication alone is not sufficient if multiple roles are introduced in the future.

Authorization must determine whether the authenticated user can perform the requested operation.

---

## BR-056 — Public API

Public endpoints must not expose administrative operations.

---

## BR-057 — Sensitive Data

Authentication credentials, tokens, passwords, API keys, and other secrets must never be returned through public APIs.

---

# 15. API Rules

## BR-058 — API Version

All application API endpoints use:

```text
/api/v1
```

---

## BR-059 — Response Consistency

API responses should use a consistent structure.

Success and error responses should be predictable for the frontend.

---

## BR-060 — Pagination

Collection endpoints that may grow significantly should support pagination.

Examples:

```text
/projects
/messages
/media
```

---

## BR-061 — Filtering

Filtering should be performed server-side when the dataset can become large.

---

# 16. Search Rules

## BR-062 — Project Search

Project search should search relevant project fields.

Initial searchable fields:

```text
title
short_description
description
```

Future support may include technology and category.

---

## BR-063 — Search Safety

Search parameters must be validated and handled through the framework/database query builder.

Raw user input must never be concatenated directly into SQL queries.

---

# 17. SEO Rules

## BR-064 — Published Content

Only published content can generate public SEO pages.

---

## BR-065 — Draft Content

Draft content must not appear in:

```text
sitemap
public listing
canonical public routes
```

---

## BR-066 — Archived Content

Archived content must not appear in public sitemap or normal public listings.

---

## BR-067 — Metadata

Public content should have reusable SEO metadata.

Default fallback metadata may be generated from the content title and description if custom metadata is not provided.

---

# 18. Caching Rules

## BR-068 — Cacheable Content

The following content may be cached:

```text
profile
published projects
skills
experience
education
certifications
```

---

## BR-069 — Draft Isolation

Draft and administrative data must not accidentally enter public cache.

---

## BR-070 — Cache Invalidation

When published content changes, related public cache must be invalidated or revalidated.

---

# 19. Audit Rules

## BR-071 — Important Administrative Actions

Important CMS operations should be auditable.

Examples:

```text
project published
project archived
project deleted
resume changed
profile changed
media deleted
```

---

## BR-072 — Audit Information

Audit records should contain enough information to determine:

```text
who
what
when
```

Additional metadata may be added later.

---

# 20. Data Integrity Rules

## BR-073 — Foreign Keys

Database relationships must use foreign key constraints where appropriate.

---

## BR-074 — Required Relationships

Records that require a parent entity must not exist without a valid parent.

---

## BR-075 — Transactional Operations

Operations that modify multiple related records must use a database transaction when partial completion could create inconsistent data.

Example:

```text
Publish Project
    ↓
Update Project
    ↓
Update Publication Metadata
    ↓
Create Audit Log
```

These operations should be handled atomically where required.

---

# 21. Deletion Rules

## BR-076 — Important Content

Important content should use soft deletion where practical.

Examples:

```text
projects
experiences
certifications
```

---

## BR-077 — Cascade Deletion

Cascade deletion must be intentional.

Deleting a project must not unexpectedly delete reusable technologies or categories.

---

# 22. Frontend Rules

## BR-078 — Frontend Is Not the Source of Truth

Frontend state must not be treated as authoritative for business rules.

---

## BR-079 — API Errors

Technical backend errors must not be displayed directly to visitors.

The frontend should show user-friendly error messages.

---

## BR-080 — Loading State

Asynchronous operations must provide appropriate loading states.

---

## BR-081 — Empty State

Collections with no records must provide meaningful empty states.

---

# 23. CMS Rules

## BR-082 — Draft First

New content should normally start as draft.

---

## BR-083 — Explicit Publishing

Content should only become public after an explicit publish action.

Saving a draft must not publish content.

---

## BR-084 — Preview

The CMS should support previewing content before publication where practical.

---

## BR-085 — Destructive Actions

Destructive operations such as deletion must require explicit confirmation.

---

# 24. State Machines

## Project

```text
        ┌───────────────┐
        │               │
        ▼               │
      DRAFT ────────────┘
        │
        │ publish
        ▼
   PUBLISHED
      │   │
      │   │ archive
      │   ▼
      │ ARCHIVED
      │    │
      │    │ restore
      │    │
      │    └──────────► PUBLISHED
      │
      │ edit
      ▼
   PUBLISHED
```

Valid primary transitions:

```text
draft → published
published → archived
archived → published
published → draft
```

The implementation may restrict some transitions depending on the final CMS UX.

---

## Contact Message

```text
UNREAD
   │
   │ mark as read
   ▼
 READ
   │
   │ archive
   ▼
ARCHIVED
```

---

## Experience

```text
CURRENT
   │
   │ end experience
   ▼
COMPLETED
```

---

## Education

```text
CURRENT
   │
   │ graduate
   ▼
COMPLETED
```

---

# 25. Business Rule Priority

When conflicting requirements occur, apply the following priority:

```text
1. Security
2. Data integrity
3. Business rules
4. API contract
5. User experience
6. Visual preference
```

A visual requirement must not override a security or data-integrity requirement.

---

# 26. AI Development Rule

AI-assisted development must not invent business rules.

If an implementation requires a business decision that is not defined in this document, the AI should:

1. Identify the ambiguity.
2. Explain the affected behavior.
3. Ask for a decision or propose explicit alternatives.
4. Update the documentation before implementing the new rule.

The AI must not silently introduce new business behavior.

---

# 27. Future Extension

The following are intentionally not required for the initial version:

* Multiple administrators
* Role hierarchy
* Public user accounts
* Comments
* Likes
* Social authentication
* Newsletter
* Advanced analytics
* Full-text search engine
* Multi-language content
* Headless CMS for third parties

These features may be introduced later without changing the core business model unnecessarily.
