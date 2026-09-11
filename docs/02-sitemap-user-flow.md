# Sitemap & User Flow

## 1. Purpose

This document defines the information architecture and primary user flows of the DwiDev Portfolio.

The public website and CMS are separate logical areas of the application.

---

## 2. Application Areas

The application consists of two major areas:

```text
Public Website
    │
    ├── Home
    ├── About
    ├── Projects
    ├── Project Detail
    ├── Experience
    ├── Education
    ├── Certifications
    ├── Resume
    ├── Blog
    └── Contact

Admin CMS
    │
    ├── Login
    ├── Dashboard
    ├── Projects
    ├── Skills
    ├── Experience
    ├── Education
    ├── Certifications
    ├── Media
    ├── Messages
    ├── Profile
    ├── Resume
    └── Settings
```

---

# 3. Public Sitemap

## 3.1 Home

Route:

```text
/
```

Purpose:

Introduce the developer and direct visitors toward the most important portfolio content.

Sections:

1. Hero
2. Short introduction
3. Featured projects
4. Skills overview
5. Experience overview
6. Certifications overview
7. Contact CTA
8. Footer

The home page should not attempt to display every piece of portfolio content.

---

## 3.2 About

Route:

```text
/about
```

Purpose:

Provide a more detailed professional profile.

Content:

* Profile
* Biography
* Professional interests
* Development focus
* Social links
* Resume CTA

---

## 3.3 Projects

Route:

```text
/projects
```

Purpose:

Display published portfolio projects.

Features:

* Project listing
* Category filtering
* Technology filtering
* Search
* Featured indicator where appropriate

Only published projects are visible publicly.

---

## 3.4 Project Detail

Route:

```text
/projects/{slug}
```

Example:

```text
/projects/mekarmuda-workshop-management-system
```

Purpose:

Explain a project in detail.

Content:

* Project title
* Short description
* Overview
* Problem
* Solution
* Role
* Technologies
* Features
* Architecture
* Screenshots
* Repository link
* Demo link
* Results
* Lessons learned
* Related projects

Draft and archived projects must not be publicly accessible through normal project routes.

---

## 3.5 Experience

Route:

```text
/experience
```

Purpose:

Display professional and project-based experience.

Content:

* Company
* Position
* Employment type
* Location
* Start date
* End date
* Description

Current experience is represented without an end date.

---

## 3.6 Education

Route:

```text
/education
```

Purpose:

Display formal education.

Content:

* Institution
* Degree
* Field of study
* Location
* Period
* Description

---

## 3.7 Certifications

Route:

```text
/certifications
```

Purpose:

Display professional certifications.

Content:

* Certification name
* Issuer
* Credential ID
* Issue date
* Expiration date
* Credential URL

---

## 3.8 Resume

Route:

```text
/resume
```

Purpose:

Provide access to the current resume/CV.

The page may contain:

* Resume preview
* Download action
* Last updated date

The actual resume file is managed through the CMS.

---

## 3.9 Blog

Route:

```text
/blog
```

Purpose:

Display technical writing and articles.

Blog is considered a planned feature and does not block the initial portfolio launch.

Future routes:

```text
/blog
/blog/{slug}
```

Only published articles are publicly visible.

---

## 3.10 Contact

Route:

```text
/contact
```

Purpose:

Allow visitors to contact the developer.

Fields:

```text
Name
Email
Subject
Message
```

After successful submission:

```text
Form
  ↓
Validation
  ↓
API
  ↓
Database
  ↓
Success message
```

The visitor does not need an account.

---

# 4. Public Navigation

Primary navigation:

```text
Home
About
Projects
Experience
Certifications
Contact
```

Secondary actions:

```text
Resume
GitHub
LinkedIn
```

The navigation must remain usable on mobile.

Mobile navigation uses a menu/drawer rather than forcing the desktop navigation into a narrow viewport.

---

# 5. Public User Flow

## 5.1 First-Time Visitor

```text
Visitor
   ↓
Home
   ↓
Hero
   ↓
Featured Projects
   ↓
Project Detail
   ↓
About / Experience
   ↓
Contact
```

The primary conversion goal is not account registration.

The primary goals are:

```text
Understand the developer
        ↓
Evaluate skills
        ↓
Inspect projects
        ↓
Contact / View resume
```

---

# 6. Project Discovery Flow

```text
Projects
   ↓
Filter / Search
   ↓
Project Card
   ↓
Project Detail
   ↓
Repository / Demo
```

The user must be able to return to the project listing without losing the general navigation context.

---

# 7. Contact Flow

```text
Visitor
   ↓
Contact
   ↓
Fill Form
   ↓
Client Validation
   ↓
Server Validation
   ↓
Rate Limit Check
   ↓
Save Message
   ↓
Success
```

If validation fails:

```text
Form
 ↓
Validation Error
 ↓
Display Field Error
```

If the server fails:

```text
Form
 ↓
API Error
 ↓
Display Generic Error
 ↓
Retry
```

Technical errors must not be exposed to visitors.

---

# 8. Admin Sitemap

Admin base route:

```text
/admin
```

Authentication:

```text
/admin/login
```

After authentication:

```text
/admin
```

Dashboard navigation:

```text
/admin
/admin/projects
/admin/skills
/admin/experience
/admin/education
/admin/certifications
/admin/media
/admin/messages
/admin/profile
/admin/resume
/admin/settings
```

---

# 9. Admin Authentication Flow

```text
/admin/login
       ↓
Enter credentials
       ↓
Laravel authentication
       ↓
Authentication success?
      / \
    Yes  No
     │    │
     ▼    ▼
 /admin  Error
```

Unauthenticated access:

```text
/admin/projects
        ↓
Authentication check
        ↓
Not authenticated
        ↓
/admin/login
```

Authenticated access:

```text
/admin/projects
        ↓
Authentication check
        ↓
Authorized
        ↓
Projects CMS
```

---

# 10. Admin Dashboard Flow

```text
Login
  ↓
Dashboard
  │
  ├── Projects
  ├── Skills
  ├── Experience
  ├── Education
  ├── Certifications
  ├── Media
  ├── Messages
  ├── Profile
  ├── Resume
  └── Settings
```

Dashboard provides a summary rather than duplicating every management interface.

---

# 11. Project CMS Flow

## Create

```text
Projects
   ↓
New Project
   ↓
Fill Basic Information
   ↓
Select Category
   ↓
Select Technologies
   ↓
Upload Media
   ↓
Configure SEO
   ↓
Save Draft
```

Optional publishing:

```text
Draft
  ↓
Preview
  ↓
Publish
```

---

## Edit

```text
Projects
   ↓
Select Project
   ↓
Edit
   ↓
Update Information
   ↓
Save
```

---

## Publish

```text
Draft
  ↓
Publish
  ↓
Validation
  ↓
Valid?
 /   \
Yes   No
 │     │
 ▼     ▼
Published  Validation Error
```

---

## Archive

```text
Published
    ↓
Archive
    ↓
Archived
```

Archived projects are not displayed in the public project listing.

---

# 12. Media Flow

```text
Media Library
      ↓
Upload
      ↓
Validate File
      ↓
Store File
      ↓
Save Metadata
      ↓
Media Available
```

Media may then be attached to:

```text
Projects
Profile
Certifications
Resume
Blog
```

---

# 13. Message Management Flow

```text
Visitor
   ↓
Contact Form
   ↓
Message Created
   ↓
Admin
   ↓
Messages
   ↓
Open Message
   ↓
Mark as Read
```

Message states:

```text
Unread
Read
Archived
```

The system must not automatically expose messages publicly.

---

# 14. Content Publication Model

Public content follows:

```text
CMS
 ↓
Draft
 ↓
Review
 ↓
Publish
 ↓
Public Website
```

For projects:

```text
Draft
Published
Archived
```

For future blog posts:

```text
Draft
Published
Archived
```

Unpublished content must not accidentally appear on public pages.

---

# 15. Error and Access Flow

## Unauthorized

```text
Public
   ↓
Protected Resource
   ↓
401 Unauthorized
```

## Forbidden

```text
Authenticated User
        ↓
Restricted Resource
        ↓
403 Forbidden
```

## Not Found

```text
Invalid Resource
      ↓
404 Not Found
```

The frontend must display user-friendly error pages.

---

# 16. SEO Navigation Rules

Public URLs must be:

* Human-readable
* Stable
* Slug-based
* Lowercase
* Hyphen-separated

Good:

```text
/projects/mekarmuda-workshop-management-system
```

Bad:

```text
/projects/123
/projects/project?id=123
/projects/MekarMuda_Project
```

Once a public slug is established, changing it should be treated carefully because existing search-engine URLs may depend on it.

---

# 17. Canonical Public Content

Each public content type has a canonical route.

```text
Profile
/about

Projects
/projects/{slug}

Experience
/experience

Certifications
/certifications

Blog
/blog/{slug}
```

Canonical URLs are used for SEO metadata.

---

# 18. Overall User Journey

The intended visitor journey is:

```text
                    HOME
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
      ABOUT       PROJECTS      RESUME
                     │
                     ▼
              PROJECT DETAIL
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
       GITHUB                 DEMO
          │
          └──────────┬──────────┘
                     ▼
                  CONTACT
```

The website should make this journey possible without requiring the visitor to understand the underlying technology.

---

# 19. CMS Content Flow

The overall content lifecycle is:

```text
Administrator
      ↓
CMS
      ↓
Create / Edit
      ↓
Validate
      ↓
Save Draft
      ↓
Preview
      ↓
Publish
      ↓
Laravel API
      ↓
Database
      ↓
Next.js
      ↓
Public Website
```

The public website must never depend on manually editing frontend source code for normal portfolio content updates.

---

# 20. Design Principle

The public website is optimized for:

```text
Discoverability
Readability
Trust
Professional presentation
SEO
Performance
Accessibility
```

The CMS is optimized for:

```text
Content management
Efficiency
Consistency
Validation
Security
```

These two interfaces should have a related visual identity but different UX priorities.
