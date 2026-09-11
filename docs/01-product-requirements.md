# Product Requirements

## 1. Product

DwiDev Portfolio is a personal developer portfolio website with an integrated Content Management System.

The website is intended to remain useful and maintainable for approximately 1–2 years without requiring a major architectural rewrite.

## 2. Primary Goals

The system must:

1. Present a professional developer profile.
2. Showcase selected software projects.
3. Present technical skills.
4. Present work experience.
5. Present education.
6. Present certifications.
7. Provide a downloadable resume.
8. Provide a contact form.
9. Provide a CMS for managing portfolio content.
10. Support SEO-friendly public pages.
11. Be responsive on mobile, tablet, and desktop.
12. Be maintainable by a single developer.
13. Be suitable for future feature expansion.

## 3. Public Website

The public website contains:

* Home
* About
* Projects
* Project Detail
* Skills
* Experience
* Education
* Certifications
* Resume
* Contact
* Blog (planned for a later version)

## 4. CMS

The CMS must allow the administrator to manage:

* Dashboard
* Projects
* Categories
* Technologies
* Skills
* Skill Categories
* Experience
* Education
* Certifications
* Media
* Contact Messages
* Profile
* Resume
* Settings

## 5. Project Management

Each project may contain:

* Title
* Slug
* Short description
* Full description
* Role
* Category
* Technologies
* Project images
* Repository URL
* Demo URL
* Start date
* End date
* Featured status
* Publication status
* Publication date
* SEO metadata

Project states:

* Draft
* Published
* Archived

## 6. Profile

The profile contains:

* Name
* Professional title
* Short biography
* Full biography
* Profile image
* Email
* Phone
* Location
* Availability
* Social links

Age must not be stored as permanent profile data because it changes over time.

## 7. Skills

Skills are grouped into categories.

Examples:

* Backend
* Frontend
* Database
* DevOps
* Programming Language
* Tools
* IoT
* AI / Machine Learning

The frontend must not hardcode skill categories.

## 8. Experience

Experience contains:

* Company
* Position
* Employment type
* Location
* Start date
* End date
* Description
* Current status

If an experience is current, the end date must be null.

## 9. Education

Education contains:

* Institution
* Degree
* Field of study
* Location
* Start date
* End date
* Description
* Current status

## 10. Certifications

Certification contains:

* Name
* Issuer
* Credential ID
* Credential URL
* Issue date
* Expiration date
* Certificate file
* Description

Expiration date is optional.

## 11. Contact

Visitors can send a message containing:

* Name
* Email
* Subject
* Message

Messages are stored in the CMS.

Contact submissions must have rate limiting and server-side validation.

## 12. Media

The system must have a reusable media library.

Media may be used for:

* Profile image
* Project images
* Resume
* Certificates
* Blog images

Binary files must not be stored directly in MySQL.

## 13. SEO

Public content should support:

* SEO title
* SEO description
* Canonical URL
* Open Graph image
* Sitemap
* Search-engine-friendly URLs

Project and future blog content should support reusable SEO metadata.

## 14. Design

The visual direction is:

* Minimal
* Professional
* Technical
* Clean
* Content-focused
* Responsive

The design should avoid excessive dependence on short-lived visual trends.

Dark mode is supported.

## 15. Responsive Design

The website must support:

* Mobile
* Tablet
* Desktop

Mobile layout must be designed intentionally rather than treated as a reduced desktop layout.

## 16. Authentication

The CMS is private and requires administrator authentication.

Laravel Sanctum is the authentication mechanism.

Public portfolio endpoints do not require authentication.

## 17. API

The backend exposes a versioned REST API:

```text
/api/v1
```

Public API and admin API must be separated logically.

Public:

```text
/api/v1/profile
/api/v1/projects
/api/v1/skills
/api/v1/experiences
/api/v1/educations
/api/v1/certifications
/api/v1/resume
/api/v1/contact
```

Admin:

```text
/api/v1/admin/*
```

## 18. Architecture

The application consists of:

```text
Next.js
    ↓
Laravel REST API
    ↓
MySQL
```

Redis is available for:

* Cache
* Rate limiting
* Queue

Storage is abstracted through Laravel's filesystem layer.

## 19. Security

The system must include:

* HTTPS in production
* Authentication
* Authorization
* Request validation
* CSRF protection where applicable
* Rate limiting
* Secure file upload validation
* SQL injection protection
* Mass assignment protection
* Secure environment variables
* No production secrets in Git
* No stack traces exposed to public users

## 20. Development Philosophy

The project follows:

* Documentation-first development
* Small incremental changes
* Automated testing
* Explicit business rules
* Minimal unnecessary dependencies
* Reviewable Git commits
* AI-assisted development with human verification

AI must not change architecture or business rules without explicit approval.
