# Database Design

## 1. Purpose

This document defines the relational database structure for the DwiDev Portfolio and CMS.

The database is designed to:

* Support the public portfolio.
* Support the administrative CMS.
* Maintain referential integrity.
* Avoid unnecessary duplication.
* Support future expansion.
* Remain manageable for a single developer.
* Provide a stable foundation for approximately 1–2 years of development.

Database engine:

```text
MySQL 8.4
```

---

# 2. Database Naming Convention

Database name:

```text
portfolio
```

Table names:

* lowercase
* plural
* snake_case

Examples:

```text
users
projects
project_categories
technologies
project_technologies
skills
```

---

# 3. Primary Key

All application tables use:

```text
BIGINT UNSIGNED
```

as the primary key unless there is a specific technical reason to use another type.

Default Laravel convention:

```text
id
```

---

# 4. Timestamps

Entities that are managed through the application should normally contain:

```text
created_at
updated_at
```

Laravel timestamps are used.

---

# 5. Soft Deletes

Entities that contain important portfolio history may use:

```text
deleted_at
```

Soft deletes are preferred when accidental deletion would cause meaningful data loss.

---

# 6. Core Entity Relationship

The high-level model is:

```text
                    users
                      │
                      │
                 audit_logs
                      │
                      │
profile ──────────────┼────────────── resume
                      │
                      │
              ┌───────┴────────┐
              │                │
          projects          media
              │
       ┌──────┴───────┐
       │              │
categories     project_technologies
                      │
                      │
                technologies

skills
  │
skill_categories

experiences

educations

certifications

contact_messages

settings
```

---

# 7. users

Purpose:

Store administrator authentication accounts.

Table:

```text
users
```

Fields:

| Column            | Type            | Null | Description            |
| ----------------- | --------------- | ---: | ---------------------- |
| id                | BIGINT UNSIGNED |   No | Primary key            |
| name              | VARCHAR(255)    |   No | Administrator name     |
| email             | VARCHAR(255)    |   No | Login email            |
| email_verified_at | TIMESTAMP       |  Yes | Verification timestamp |
| password          | VARCHAR(255)    |   No | Hashed password        |
| remember_token    | VARCHAR(100)    |  Yes | Laravel authentication |
| created_at        | TIMESTAMP       |  Yes | Creation timestamp     |
| updated_at        | TIMESTAMP       |  Yes | Update timestamp       |

Constraints:

```text
email UNIQUE
```

Passwords must always be hashed.

Plain-text passwords must never be stored.

---

# 8. profiles

Purpose:

Store the public developer profile.

The initial system supports one active profile.

Fields:

| Column             | Type            | Null | Description          |
| ------------------ | --------------- | ---: | -------------------- |
| id                 | BIGINT UNSIGNED |   No | Primary key          |
| name               | VARCHAR(255)    |   No | Display name         |
| professional_title | VARCHAR(255)    |   No | Professional title   |
| short_bio          | TEXT            |   No | Short biography      |
| biography          | LONGTEXT        |  Yes | Full biography       |
| email              | VARCHAR(255)    |  Yes | Public contact email |
| phone              | VARCHAR(50)     |  Yes | Public phone         |
| location           | VARCHAR(255)    |  Yes | Public location      |
| availability       | VARCHAR(255)    |  Yes | Availability text    |
| profile_media_id   | BIGINT UNSIGNED |  Yes | Profile image        |
| created_at         | TIMESTAMP       |  Yes | Creation timestamp   |
| updated_at         | TIMESTAMP       |  Yes | Update timestamp     |

Relationship:

```text
profiles.profile_media_id
        ↓
media.id
```

The profile does not store age.

---

# 9. profile_social_links

Purpose:

Store social links associated with the profile.

Fields:

| Column     | Type            | Null | Description         |
| ---------- | --------------- | ---: | ------------------- |
| id         | BIGINT UNSIGNED |   No | Primary key         |
| profile_id | BIGINT UNSIGNED |   No | Profile             |
| platform   | VARCHAR(100)    |   No | Platform identifier |
| label      | VARCHAR(100)    |  Yes | Display label       |
| url        | VARCHAR(2048)   |   No | Social URL          |
| sort_order | INT UNSIGNED    |   No | Display order       |
| is_active  | BOOLEAN         |   No | Active status       |
| created_at | TIMESTAMP       |  Yes | Creation timestamp  |
| updated_at | TIMESTAMP       |  Yes | Update timestamp    |

Indexes:

```text
profile_id
platform
```

Unique recommendation:

```text
(profile_id, platform)
```

---

# 10. project_categories

Purpose:

Reusable project categories.

Fields:

| Column      | Type            | Null | Description         |
| ----------- | --------------- | ---: | ------------------- |
| id          | BIGINT UNSIGNED |   No | Primary key         |
| name        | VARCHAR(100)    |   No | Category name       |
| slug        | VARCHAR(120)    |   No | URL-safe identifier |
| description | TEXT            |  Yes | Description         |
| sort_order  | INT UNSIGNED    |   No | Display order       |
| created_at  | TIMESTAMP       |  Yes | Creation timestamp  |
| updated_at  | TIMESTAMP       |  Yes | Update timestamp    |
| deleted_at  | TIMESTAMP       |  Yes | Soft delete         |

Constraints:

```text
slug UNIQUE
```

---

# 11. technologies

Purpose:

Reusable technology entities.

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

Fields:

| Column      | Type            | Null | Description        |
| ----------- | --------------- | ---: | ------------------ |
| id          | BIGINT UNSIGNED |   No | Primary key        |
| name        | VARCHAR(100)    |   No | Technology name    |
| slug        | VARCHAR(120)    |   No | Technology slug    |
| description | TEXT            |  Yes | Description        |
| icon        | VARCHAR(255)    |  Yes | Icon identifier    |
| website_url | VARCHAR(2048)   |  Yes | Official website   |
| sort_order  | INT UNSIGNED    |   No | Display order      |
| is_active   | BOOLEAN         |   No | Active status      |
| created_at  | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at  | TIMESTAMP       |  Yes | Update timestamp   |
| deleted_at  | TIMESTAMP       |  Yes | Soft delete        |

Constraints:

```text
name UNIQUE
slug UNIQUE
```

Name uniqueness should be treated case-insensitively at the application layer.

---

# 12. projects

Purpose:

Store portfolio projects.

Fields:

| Column            | Type            | Null | Description              |
| ----------------- | --------------- | ---: | ------------------------ |
| id                | BIGINT UNSIGNED |   No | Primary key              |
| category_id       | BIGINT UNSIGNED |   No | Primary category         |
| title             | VARCHAR(255)    |   No | Project title            |
| slug              | VARCHAR(255)    |   No | Public URL slug          |
| short_description | TEXT            |   No | Short description        |
| description       | LONGTEXT        |   No | Full description         |
| role              | VARCHAR(255)    |  Yes | Role in project          |
| started_at        | DATE            |  Yes | Project start            |
| ended_at          | DATE            |  Yes | Project end              |
| status            | VARCHAR(30)     |   No | draft/published/archived |
| is_featured       | BOOLEAN         |   No | Featured status          |
| published_at      | TIMESTAMP       |  Yes | Publication timestamp    |
| repository_url    | VARCHAR(2048)   |  Yes | Repository               |
| demo_url          | VARCHAR(2048)   |  Yes | Live demo                |
| seo_title         | VARCHAR(255)    |  Yes | SEO title                |
| seo_description   | TEXT            |  Yes | SEO description          |
| seo_image_id      | BIGINT UNSIGNED |  Yes | SEO image                |
| created_at        | TIMESTAMP       |  Yes | Creation timestamp       |
| updated_at        | TIMESTAMP       |  Yes | Update timestamp         |
| deleted_at        | TIMESTAMP       |  Yes | Soft delete              |

Constraints:

```text
slug UNIQUE
```

Indexes:

```text
category_id
status
is_featured
published_at
```

---

# 13. project_technologies

Purpose:

Many-to-many relationship between projects and technologies.

Fields:

| Column        | Type            | Null | Description |
| ------------- | --------------- | ---: | ----------- |
| project_id    | BIGINT UNSIGNED |   No | Project     |
| technology_id | BIGINT UNSIGNED |   No | Technology  |

Primary key:

```text
(project_id, technology_id)
```

Foreign keys:

```text
project_id → projects.id
technology_id → technologies.id
```

---

# 14. project_media

Purpose:

Associate media files with projects.

Fields:

| Column     | Type            | Null | Description             |
| ---------- | --------------- | ---: | ----------------------- |
| id         | BIGINT UNSIGNED |   No | Primary key             |
| project_id | BIGINT UNSIGNED |   No | Project                 |
| media_id   | BIGINT UNSIGNED |   No | Media                   |
| type       | VARCHAR(50)     |   No | thumbnail/gallery/other |
| sort_order | INT UNSIGNED    |   No | Display order           |
| caption    | TEXT            |  Yes | Caption                 |
| created_at | TIMESTAMP       |  Yes | Creation timestamp      |
| updated_at | TIMESTAMP       |  Yes | Update timestamp        |

A project may have multiple media items.

---

# 15. project_contents

Purpose:

Provide extensibility for structured project content without placing every possible section directly into the projects table.

This table is optional for the first implementation and should only be introduced if the final project detail editor requires structured sections.

Possible fields:

| Column     | Type            | Null | Description        |
| ---------- | --------------- | ---: | ------------------ |
| id         | BIGINT UNSIGNED |   No | Primary key        |
| project_id | BIGINT UNSIGNED |   No | Project            |
| type       | VARCHAR(50)     |   No | Section type       |
| title      | VARCHAR(255)    |  Yes | Section title      |
| content    | LONGTEXT        |  Yes | Section content    |
| sort_order | INT UNSIGNED    |   No | Display order      |
| created_at | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at | TIMESTAMP       |  Yes | Update timestamp   |

Initial implementation may omit this table and use the main project description until structured project sections are actually required.

---

# 16. skill_categories

Purpose:

Group skills into reusable categories.

Fields:

| Column     | Type            | Null | Description        |
| ---------- | --------------- | ---: | ------------------ |
| id         | BIGINT UNSIGNED |   No | Primary key        |
| name       | VARCHAR(100)    |   No | Category name      |
| slug       | VARCHAR(120)    |   No | Category slug      |
| sort_order | INT UNSIGNED    |   No | Display order      |
| is_active  | BOOLEAN         |   No | Active status      |
| created_at | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at | TIMESTAMP       |  Yes | Update timestamp   |

Constraints:

```text
slug UNIQUE
```

---

# 17. skills

Purpose:

Store individual skills.

Fields:

| Column            | Type            | Null | Description        |
| ----------------- | --------------- | ---: | ------------------ |
| id                | BIGINT UNSIGNED |   No | Primary key        |
| skill_category_id | BIGINT UNSIGNED |   No | Skill category     |
| name              | VARCHAR(100)    |   No | Skill name         |
| slug              | VARCHAR(120)    |   No | Skill slug         |
| description       | TEXT            |  Yes | Description        |
| sort_order        | INT UNSIGNED    |   No | Display order      |
| is_active         | BOOLEAN         |   No | Active status      |
| created_at        | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at        | TIMESTAMP       |  Yes | Update timestamp   |

Constraints:

```text
slug UNIQUE
```

---

# 18. experiences

Purpose:

Store professional and relevant project experience.

Fields:

| Column          | Type            | Null | Description        |
| --------------- | --------------- | ---: | ------------------ |
| id              | BIGINT UNSIGNED |   No | Primary key        |
| company         | VARCHAR(255)    |   No | Company            |
| position        | VARCHAR(255)    |   No | Position           |
| employment_type | VARCHAR(100)    |  Yes | Employment type    |
| location        | VARCHAR(255)    |  Yes | Location           |
| started_at      | DATE            |   No | Start date         |
| ended_at        | DATE            |  Yes | End date           |
| description     | LONGTEXT        |  Yes | Description        |
| is_current      | BOOLEAN         |   No | Current status     |
| sort_order      | INT UNSIGNED    |   No | Display order      |
| created_at      | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at      | TIMESTAMP       |  Yes | Update timestamp   |
| deleted_at      | TIMESTAMP       |  Yes | Soft delete        |

Rules:

```text
is_current = true
→ ended_at must be null
```

```text
is_current = false
→ ended_at should normally be populated
```

---

# 19. educations

Purpose:

Store educational background.

Fields:

| Column         | Type            | Null | Description        |
| -------------- | --------------- | ---: | ------------------ |
| id             | BIGINT UNSIGNED |   No | Primary key        |
| institution    | VARCHAR(255)    |   No | Institution        |
| degree         | VARCHAR(255)    |  Yes | Degree             |
| field_of_study | VARCHAR(255)    |  Yes | Field              |
| location       | VARCHAR(255)    |  Yes | Location           |
| started_at     | DATE            |  Yes | Start date         |
| ended_at       | DATE            |  Yes | End date           |
| description    | LONGTEXT        |  Yes | Description        |
| is_current     | BOOLEAN         |   No | Current status     |
| sort_order     | INT UNSIGNED    |   No | Display order      |
| created_at     | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at     | TIMESTAMP       |  Yes | Update timestamp   |
| deleted_at     | TIMESTAMP       |  Yes | Soft delete        |

Rules:

```text
is_current = true
→ ended_at must be null
```

---

# 20. certifications

Purpose:

Store professional certifications.

Fields:

| Column               | Type            | Null | Description          |
| -------------------- | --------------- | ---: | -------------------- |
| id                   | BIGINT UNSIGNED |   No | Primary key          |
| name                 | VARCHAR(255)    |   No | Certification name   |
| issuer               | VARCHAR(255)    |   No | Issuing organization |
| credential_id        | VARCHAR(255)    |  Yes | Credential ID        |
| credential_url       | VARCHAR(2048)   |  Yes | Verification URL     |
| issued_at            | DATE            |   No | Issue date           |
| expires_at           | DATE            |  Yes | Expiration date      |
| certificate_media_id | BIGINT UNSIGNED |  Yes | Certificate file     |
| description          | TEXT            |  Yes | Description          |
| sort_order           | INT UNSIGNED    |   No | Display order        |
| created_at           | TIMESTAMP       |  Yes | Creation timestamp   |
| updated_at           | TIMESTAMP       |  Yes | Update timestamp     |
| deleted_at           | TIMESTAMP       |  Yes | Soft delete          |

Rule:

```text
expires_at >= issued_at
```

when `expires_at` is not null.

---

# 21. resumes

Purpose:

Store resume/CV versions.

Fields:

| Column        | Type            | Null | Description           |
| ------------- | --------------- | ---: | --------------------- |
| id            | BIGINT UNSIGNED |   No | Primary key           |
| media_id      | BIGINT UNSIGNED |   No | Resume file           |
| version_label | VARCHAR(100)    |  Yes | Version label         |
| is_active     | BOOLEAN         |   No | Active public resume  |
| published_at  | TIMESTAMP       |  Yes | Publication timestamp |
| created_at    | TIMESTAMP       |  Yes | Creation timestamp    |
| updated_at    | TIMESTAMP       |  Yes | Update timestamp      |

Only one resume should be active at a time.

---

# 22. media

Purpose:

Central media library.

Fields:

| Column        | Type            | Null | Description        |
| ------------- | --------------- | ---: | ------------------ |
| id            | BIGINT UNSIGNED |   No | Primary key        |
| disk          | VARCHAR(50)     |   No | Storage disk       |
| path          | VARCHAR(2048)   |   No | Storage path       |
| original_name | VARCHAR(255)    |   No | Original filename  |
| mime_type     | VARCHAR(255)    |   No | MIME type          |
| extension     | VARCHAR(20)     |  Yes | Extension          |
| size          | BIGINT UNSIGNED |   No | File size          |
| width         | INT UNSIGNED    |  Yes | Image width        |
| height        | INT UNSIGNED    |  Yes | Image height       |
| alt_text      | VARCHAR(255)    |  Yes | Accessibility text |
| created_at    | TIMESTAMP       |  Yes | Creation timestamp |
| updated_at    | TIMESTAMP       |  Yes | Update timestamp   |
| deleted_at    | TIMESTAMP       |  Yes | Soft delete        |

Binary file contents are not stored in MySQL.

Only metadata and storage references are stored.

---

# 23. contact_messages

Purpose:

Store messages submitted through the public contact form.

Fields:

| Column      | Type            | Null | Description                   |
| ----------- | --------------- | ---: | ----------------------------- |
| id          | BIGINT UNSIGNED |   No | Primary key                   |
| name        | VARCHAR(255)    |   No | Sender name                   |
| email       | VARCHAR(255)    |   No | Sender email                  |
| subject     | VARCHAR(255)    |   No | Subject                       |
| message     | LONGTEXT        |   No | Message                       |
| status      | VARCHAR(30)     |   No | unread/read/archived          |
| read_at     | TIMESTAMP       |  Yes | Read timestamp                |
| archived_at | TIMESTAMP       |  Yes | Archive timestamp             |
| ip_hash     | VARCHAR(255)    |  Yes | Privacy-preserving identifier |
| user_agent  | TEXT            |  Yes | User agent                    |
| created_at  | TIMESTAMP       |  Yes | Submission timestamp          |
| updated_at  | TIMESTAMP       |  Yes | Update timestamp              |

Indexes:

```text
status
created_at
email
```

The raw IP address should not be stored unless there is a specific operational requirement.

---

# 24. settings

Purpose:

Store configurable application settings.

Fields:

| Column     | Type            | Null | Description                 |
| ---------- | --------------- | ---: | --------------------------- |
| id         | BIGINT UNSIGNED |   No | Primary key                 |
| key        | VARCHAR(255)    |   No | Setting key                 |
| value      | LONGTEXT        |  Yes | Setting value               |
| type       | VARCHAR(30)     |   No | string/boolean/integer/json |
| created_at | TIMESTAMP       |  Yes | Creation timestamp          |
| updated_at | TIMESTAMP       |  Yes | Update timestamp            |

Constraint:

```text
key UNIQUE
```

Sensitive secrets must not be stored in this table.

Application secrets belong in environment configuration.

---

# 25. audit_logs

Purpose:

Record important administrative actions.

Fields:

| Column      | Type            | Null | Description                   |
| ----------- | --------------- | ---: | ----------------------------- |
| id          | BIGINT UNSIGNED |   No | Primary key                   |
| user_id     | BIGINT UNSIGNED |  Yes | User                          |
| action      | VARCHAR(100)    |   No | Action name                   |
| entity_type | VARCHAR(100)    |  Yes | Entity class/type             |
| entity_id   | BIGINT UNSIGNED |  Yes | Entity ID                     |
| old_values  | JSON            |  Yes | Previous values               |
| new_values  | JSON            |  Yes | New values                    |
| ip_hash     | VARCHAR(255)    |  Yes | Privacy-preserving identifier |
| user_agent  | TEXT            |  Yes | User agent                    |
| created_at  | TIMESTAMP       |  Yes | Event timestamp               |

Indexes:

```text
user_id
entity_type
entity_id
action
created_at
```

Audit logs are append-oriented.

They should not normally be edited through the CMS.

---

# 26. Foreign Key Relationships

Primary relationships:

```text
profiles.profile_media_id
    → media.id

profile_social_links.profile_id
    → profiles.id

projects.category_id
    → project_categories.id

projects.seo_image_id
    → media.id

project_technologies.project_id
    → projects.id

project_technologies.technology_id
    → technologies.id

project_media.project_id
    → projects.id

project_media.media_id
    → media.id

skills.skill_category_id
    → skill_categories.id

certifications.certificate_media_id
    → media.id

resumes.media_id
    → media.id

audit_logs.user_id
    → users.id
```

---

# 27. Delete Behavior

Foreign key delete behavior must be intentional.

Recommended approach:

```text
Project → Category
RESTRICT
```

A category should not be deleted while active projects depend on it.

```text
Project → Technologies
CASCADE on pivot
```

Deleting a project removes its project-technology relationship.

The technology itself remains.

```text
Project → Media
```

Media should not automatically be physically deleted when a project is deleted.

The media library manages media lifecycle separately.

---

# 28. Indexing Strategy

Indexes should exist for frequently queried fields.

Important indexes:

```text
projects.slug
projects.status
projects.category_id
projects.published_at
projects.is_featured

technologies.slug
project_categories.slug
skills.slug
skill_categories.slug

experiences.started_at
educations.started_at

contact_messages.status
contact_messages.created_at

audit_logs.created_at
```

Do not add indexes indiscriminately.

Indexes should correspond to real query patterns.

---

# 29. Status Storage

Status fields are initially stored as strings.

Examples:

```text
projects.status:
draft
published
archived
```

```text
contact_messages.status:
unread
read
archived
```

Application-level enums should be used in Laravel to avoid arbitrary values being introduced by application code.

---

# 30. Database Constraints

Important integrity rules should be enforced as close to the database as practical.

Examples:

```text
Unique project slug
Unique technology slug
Unique category slug
Unique skill slug
Unique setting key
Unique project/technology pair
```

Some conditional rules are better enforced by application validation because MySQL constraints are not always the most maintainable mechanism for them.

---

# 31. Migration Order

Laravel migrations should be created in dependency order.

Recommended order:

```text
1. users

2. media

3. profiles
4. profile_social_links

5. project_categories
6. technologies
7. projects
8. project_technologies
9. project_media

10. skill_categories
11. skills

12. experiences
13. educations
14. certifications

15. resumes

16. contact_messages

17. settings

18. audit_logs
```

---

# 32. Seed Data

Development seeders may create initial:

```text
project categories
technologies
skill categories
skills
```

Production seeders must not create fake portfolio content unless explicitly required.

Administrator creation should be handled securely and must not use a publicly known default password.

---

# 33. Database Versioning

All schema changes must be performed through Laravel migrations.

Never manually modify production schema as the normal workflow.

Migration workflow:

```text
Change requirement
       ↓
Update documentation
       ↓
Create migration
       ↓
Run migration locally
       ↓
Run tests
       ↓
Review
       ↓
Deploy migration
```

---

# 34. Data Lifecycle

Portfolio content follows:

```text
Create
  ↓
Draft
  ↓
Publish
  ↓
Update
  ↓
Archive / Soft Delete
```

Historical data should be preserved where it has administrative value.

---

# 35. Scalability Considerations

The initial database intentionally uses MySQL rather than introducing a separate search database or document database.

Expected scale:

```text
Projects: small
Skills: small
Experience: small
Certifications: small
Messages: moderate
Media: potentially large
```

Media storage should therefore be abstracted from the database.

If traffic or content volume increases substantially, caching and external storage can be introduced without changing the core content model.

---

# 36. Future Extensions

The following may be added later:

```text
blog_posts
blog_categories
blog_tags
blog_post_tags
redirects
page_views
newsletter_subscribers
translations
```

They are intentionally excluded from the initial schema.

The database should not contain tables solely because they might be useful someday.

---

# 37. Database Design Principle

The database should prioritize:

1. Data integrity
2. Clear relationships
3. Maintainability
4. Query simplicity
5. Extensibility
6. Appropriate normalization

Avoid premature optimization and unnecessary abstraction.
