# DwiDev Portfolio

Personal developer portfolio and content management system.

## Overview

This project is a long-term personal portfolio website designed to showcase:

* Professional profile
* Technical skills
* Projects
* Work experience
* Education
* Certifications
* Resume
* Contact information
* Blog content

The website also includes an administrative CMS for managing portfolio content without modifying source code.

## Architecture

The system uses a separate frontend and backend architecture:

* Frontend: Next.js + TypeScript
* Backend: Laravel 12 REST API
* Database: MySQL 8.4
* Cache: Redis
* Authentication: Laravel Sanctum
* UI: Tailwind CSS + shadcn/ui
* Containerization: Docker
* Version Control: Git + GitHub

## Repository Structure

```text
apps/
├── web/       # Next.js frontend
└── api/       # Laravel backend

docs/          # Project documentation
docker/        # Docker-related configuration
```

## API Version

The API is versioned from the beginning:

```text
/api/v1
```

## Development Principle

This project follows a documentation-first and test-oriented development workflow.

Before implementing a significant feature:

1. Read the relevant documentation.
2. Define the implementation plan.
3. Implement the feature.
4. Run automated checks.
5. Review the changes.
6. Commit the completed work.

## Status

Early development.
