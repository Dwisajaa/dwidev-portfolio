# UI/UX Design System

## 1. Purpose

This document defines the UI/UX direction for the DwiDev Portfolio.

The interface must communicate:

* Technical competence.
* Professional credibility.
* Personal identity.
* Project quality.
* Clear information architecture.
* Modern engineering mindset.

The website must not look like a generic portfolio template.

The design should remain usable and visually relevant for at least 1–2 years without requiring a complete redesign.

---

# 2. Design Direction

Primary design direction:

```text
Modern
Minimal
Technical
Professional
Editorial
Clean
Confident
```

Visual impression:

```text
Developer Portfolio
+
Engineering Documentation
+
Modern Product Website
```

Avoid:

```text
Overly flashy
Excessive gradients
Excessive animations
Template-like layouts
Too many colors
Excessive glassmorphism
Unnecessary 3D elements
```

---

# 3. Design Philosophy

The website should communicate:

> "This person builds systems, not just UI screenshots."

Therefore, project presentation should emphasize:

```text
Problem
↓
Solution
↓
Architecture
↓
Technology
↓
Implementation
↓
Result
```

rather than only:

```text
Screenshot
Technology logos
GitHub button
```

---

# 4. Visual Hierarchy

The hierarchy should follow:

```text
Identity
    ↓
Professional positioning
    ↓
Proof of work
    ↓
Technical capability
    ↓
Experience
    ↓
Credentials
    ↓
Contact
```

The most important content should receive the strongest visual emphasis.

---

# 5. Color System

Use a restrained color system.

Primary:

```text
Neutral / near-black
```

Secondary:

```text
White / off-white
```

Accent:

```text
One primary accent color
```

Supporting:

```text
Muted neutral
Border neutral
Success
Warning
Error
Info
```

The exact hexadecimal values should be centralized in the design tokens.

Do not hardcode colors throughout components.

---

# 6. Color Tokens

Conceptual structure:

```text
background
foreground

surface
surface-muted

border
border-muted

primary
primary-foreground

secondary
secondary-foreground

success
warning
error
info

muted
muted-foreground
```

Implementation should use CSS variables or the selected Tailwind token system.

---

# 7. Dark Mode

Dark mode is supported.

The design must not simply invert colors.

Dark mode should be designed intentionally.

Example hierarchy:

```text
Dark background
    ↓
Elevated surface
    ↓
Primary text
    ↓
Secondary text
    ↓
Muted text
```

Contrast must remain accessible.

---

# 8. Light Mode

Light mode should prioritize:

```text
Readability
Whitespace
Professional appearance
Content hierarchy
```

Avoid excessive pure-white surfaces when they make the page visually flat.

Subtle surface differentiation may be used.

---

# 9. Typography

Typography should be:

```text
Modern
Highly readable
Professional
Technical without being gimmicky
```

Recommended typography structure:

```text
Display
Heading
Subheading
Body
Caption
Code / technical
```

Use a maximum of:

```text
1 primary UI font
1 optional monospace font
```

Do not use multiple decorative fonts.

---

# 10. Typography Scale

Use a consistent responsive scale.

Conceptually:

```text
Display
64px → 48px → 40px

H1
48px → 40px → 32px

H2
36px → 30px → 28px

H3
28px → 24px → 22px

Body
18px → 17px → 16px

Small
14px

Caption
12px
```

Exact values may be adjusted during implementation.

---

# 11. Line Height

Recommended:

```text
Display:
1.05–1.15

Heading:
1.15–1.25

Body:
1.5–1.7

Small:
1.4–1.5
```

Body content should prioritize readability.

---

# 12. Spacing System

Use a consistent spacing scale.

Base unit:

```text
4px
```

Example:

```text
4
8
12
16
20
24
32
40
48
64
80
96
128
```

Do not randomly introduce spacing values.

---

# 13. Layout Width

Main content should use a constrained container.

Conceptual:

```text
Full viewport
      ↓
Container
      ↓
Content
```

Recommended maximum width:

```text
1200–1280px
```

Certain editorial/project detail sections may use narrower widths.

---

# 14. Grid

Desktop:

```text
12-column grid
```

Tablet:

```text
8-column grid
```

Mobile:

```text
4-column grid
```

The actual implementation may use CSS Grid/Flexbox depending on component requirements.

---

# 15. Responsive Strategy

Design from mobile to desktop.

Breakpoints should be centralized.

Typical behavior:

```text
Mobile
↓
Tablet
↓
Desktop
↓
Large Desktop
```

Responsive behavior must be intentional.

Do not simply shrink desktop components.

---

# 16. Mobile Navigation

Mobile navigation should provide:

```text
Logo / name
Menu trigger
Navigation drawer
Primary CTA
```

The menu must be usable with touch.

Minimum interactive target:

```text
~44px
```

---

# 17. Desktop Navigation

Desktop navigation:

```text
Logo
Projects
About
Experience
Certifications
Contact
```

Optional:

```text
Theme toggle
Resume
```

Navigation should remain visually simple.

---

# 18. Homepage Structure

Recommended homepage:

```text
Hero
 ↓
Selected Projects
 ↓
Technical Stack / Skills
 ↓
Experience
 ↓
About
 ↓
Certifications / Education
 ↓
Contact CTA
 ↓
Footer
```

The homepage should not contain every piece of information.

Its purpose is to create a strong overview and lead users into deeper pages.

---

# 19. Hero Section

Hero should communicate three things immediately:

```text
Who
What
Why care
```

Example structure:

```text
Muhammad Dwi Riswanto

Software Developer
Building practical web systems, APIs,
and technology-driven applications.

[View Projects]
[Download Resume]
```

Avoid generic statements such as:

```text
"Welcome to my portfolio"
```

as the primary headline.

---

# 20. Hero Supporting Information

Optional supporting metadata:

```text
Based in Indonesia
Open to opportunities
Backend / Full-stack
Laravel / Go / Next.js
```

Only display information that is actually useful.

---

# 21. Project Section

Selected projects should be the strongest proof of capability.

Each project card may contain:

```text
Thumbnail
Category
Title
Short description
Technology
Role
Year
```

Optional:

```text
Featured badge
Status
```

---

# 22. Project Card

Conceptual:

```text
┌─────────────────────────────┐
│                             │
│          IMAGE              │
│                             │
├─────────────────────────────┤
│ WEB APPLICATION             │
│                             │
│ MekarMuda                   │
│ Workshop Management System  │
│                             │
│ Laravel · MySQL · REST API  │
│                             │
│ View Project →              │
└─────────────────────────────┘
```

Cards should not contain excessive text.

---

# 23. Project Detail Page

Project detail should function as a technical case study.

Recommended structure:

```text
Project Header
 ↓
Overview
 ↓
Problem
 ↓
Solution
 ↓
Key Features
 ↓
Architecture
 ↓
Technology
 ↓
Implementation
 ↓
Challenges
 ↓
Result
 ↓
Screenshots
 ↓
Links
 ↓
Related Projects
```

Not every project must use every section.

---

# 24. Project Case Study

The strongest projects should explain:

```text
What problem existed?
What did I build?
Why did I choose this architecture?
What technologies were used?
What technical challenges occurred?
What was the result?
```

This is particularly important for technical recruiters.

---

# 25. Technology Display

Technologies should not dominate the design.

Preferred:

```text
Laravel
MySQL
Redis
Docker
Next.js
Go
```

Avoid displaying 20+ technology logos merely to demonstrate breadth.

Quality of explanation is more valuable than quantity of badges.

---

# 26. Skills Section

Skills should be grouped.

Example:

```text
Backend
Laravel
PHP
Go
REST API

Frontend
Next.js
React
TypeScript
Tailwind CSS

Database
MySQL
PostgreSQL

Infrastructure
Docker
Linux
Nginx
Git

Additional
Python
Computer Vision
IoT
```

The CMS should allow categories to evolve.

---

# 27. Experience Section

Experience should use a timeline or structured list.

Example:

```text
2026
Software Developer
Company / Organization

Description
Responsibilities
Achievements
Technologies
```

The layout should prioritize clarity over visual decoration.

---

# 28. Education Section

Education should be concise.

Example:

```text
Universitas 17 Agustus 1945 Surabaya
S1 Teknik Informatika

2022 – Present
```

Additional academic achievements may be added when relevant.

---

# 29. Certifications

Certification cards may contain:

```text
Certificate
Issuer
Issue date
Credential ID
Verification URL
```

Only show verification links when available.

---

# 30. Resume

Resume CTA should be visible but not dominant.

Possible placement:

```text
Hero
Navigation
About
Footer
```

Resume should be a PDF or other clearly defined document.

The active resume is controlled by the CMS.

---

# 31. Contact Section

Contact should be straightforward.

Fields:

```text
Name
Email
Subject
Message
```

CTA:

```text
Send Message
```

Avoid excessive form fields.

---

# 32. Contact UX

States:

```text
Idle
Submitting
Success
Validation Error
Server Error
Rate Limited
```

Example success:

```text
Your message has been sent successfully.
```

The user should never be left wondering whether the submission worked.

---

# 33. Footer

Footer should contain:

```text
Name / Brand
Short description
Navigation
Social links
Email
Copyright
```

Avoid unnecessary footer complexity.

---

# 34. CMS Dashboard

CMS dashboard should prioritize operational information.

Example:

```text
Dashboard
├── Content Summary
├── Projects
├── Messages
├── Recent Activity
└── Quick Actions
```

The dashboard is not a marketing page.

---

# 35. CMS Navigation

Recommended:

```text
Dashboard

Content
├── Projects
├── Categories
├── Technologies
├── Skills
├── Experience
├── Education
└── Certifications

Profile
├── Profile
├── Social Links
└── Resume

Media

Messages

Settings
```

Navigation should remain scalable as new modules are added.

---

# 36. CMS CRUD UX

Every CRUD interface should provide:

```text
List
Search
Filter
Create
Edit
Delete
Status
Pagination
```

Where relevant:

```text
Publish
Archive
Restore
Preview
```

---

# 37. CMS Form Design

Forms should group fields logically.

Example Project form:

```text
Basic Information
├── Title
├── Slug
├── Category
└── Description

Project Details
├── Role
├── Technologies
├── Repository
└── Demo

Media
├── Thumbnail
└── Gallery

SEO
├── SEO Title
└── SEO Description

Publishing
├── Status
└── Featured
```

Avoid presenting 30 fields in one undifferentiated form.

---

# 38. Form Validation UX

Validation should occur:

```text
Client side
+
Server side
```

Client validation improves UX.

Server validation is authoritative.

Validation errors must be displayed near the relevant field.

---

# 39. Destructive Actions

Actions such as:

```text
Delete
Archive
Remove Media
```

must require explicit confirmation when appropriate.

Example:

```text
Delete Project?

This action will remove the project from the CMS.
```

For irreversible actions, the confirmation must clearly communicate consequences.

---

# 40. Loading States

Every asynchronous interface must have a defined loading state.

Examples:

```text
Skeleton
Spinner
Disabled button
Progress indicator
```

Do not leave empty areas without explanation.

---

# 41. Empty States

Examples:

```text
No projects found.

Try changing your search or filter.
```

CMS:

```text
No projects have been created yet.

[Create Project]
```

Empty states should provide a useful next action where appropriate.

---

# 42. Error States

Errors should be:

```text
Specific
Actionable
Concise
Non-technical for public users
Technical enough for administrators
```

Avoid:

```text
Something went wrong!!!
```

without context.

---

# 43. Toast Notifications

Use toast notifications for:

```text
Successful save
Successful update
Successful delete
Publish
Archive
Minor system feedback
```

Do not use toast notifications for information that requires persistent attention.

---

# 44. Modal Usage

Use modals for:

```text
Confirmation
Short focused actions
Small forms
```

Avoid putting large multi-step forms inside modals.

---

# 45. Tables

CMS tables should support:

```text
Responsive behavior
Pagination
Sorting where necessary
Filtering
Row actions
Status indicators
```

On mobile, tables may transform into cards rather than forcing horizontal scrolling.

---

# 46. Status Indicators

Use semantic status styles.

Examples:

```text
Draft
Published
Archived

Unread
Read
Archived
```

Status should not rely on color alone.

Use:

```text
Color
+
Text
```

---

# 47. Buttons

Primary button:

```text
Main action
```

Secondary:

```text
Alternative action
```

Ghost:

```text
Low-emphasis action
```

Destructive:

```text
Delete / irreversible action
```

Buttons must have clear verbs.

Prefer:

```text
Create Project
Save Changes
Publish Project
Delete Project
```

instead of:

```text
Submit
Click Here
OK
```

---

# 48. Icons

Icons should support meaning rather than decoration.

Use one consistent icon library.

Icons should not replace text when the meaning is ambiguous.

---

# 49. Animation

Animation is allowed but restrained.

Use animation for:

```text
Page transitions
Hover feedback
Menu transitions
Modal transitions
Loading
Subtle reveal
```

Avoid:

```text
Constant motion
Large parallax effects
Excessive scroll animation
Animation that delays content access
```

Animations must respect:

```text
prefers-reduced-motion
```

---

# 50. Accessibility

The website must target WCAG 2.2 AA principles.

Minimum requirements:

```text
Semantic HTML
Keyboard navigation
Visible focus state
Sufficient contrast
Accessible forms
Alt text
ARIA only where necessary
Reduced motion support
Screen-reader-friendly labels
```

Accessibility is a system requirement, not a final polish step.

---

# 51. SEO UX

Public pages should support:

```text
Title
Meta description
Canonical URL
Open Graph metadata
Twitter/X metadata
Structured data where appropriate
```

Project pages should have unique metadata.

---

# 52. Images

Images must:

```text
Have meaningful alt text when informative
Use empty alt text when decorative
Be optimized
Have defined dimensions where possible
Avoid layout shift
```

Large original images should not be loaded when a smaller variant is sufficient.

---

# 53. Content Density

The portfolio should avoid excessive information density.

Prioritize:

```text
Important information
Whitespace
Readable line length
Clear sections
Consistent rhythm
```

Long technical explanations belong primarily on project detail pages.

---

# 54. Editorial Style

Content should use concise professional language.

Avoid excessive self-promotion.

Prefer evidence:

```text
Built
Designed
Implemented
Integrated
Optimized
Deployed
```

over vague claims:

```text
Passionate
Hard-working
Best developer
Expert in everything
```

---

# 55. Personal Branding

Brand identity should be based on:

```text
Name
Technical capability
Project evidence
Writing quality
Consistency
```

The portfolio should not depend on a complicated logo.

A simple typographic identity is sufficient.

---

# 56. Component Architecture

Reusable UI components should be created for repeated patterns.

Example:

```text
components/
├── ui/
│   ├── Button
│   ├── Input
│   ├── Select
│   ├── Modal
│   ├── Badge
│   ├── Card
│   └── Table
│
├── portfolio/
│   ├── ProjectCard
│   ├── SkillList
│   ├── ExperienceTimeline
│   └── CertificationCard
│
└── cms/
    ├── DataTable
    ├── FormField
    ├── StatusBadge
    └── MediaUploader
```

The exact structure will be defined in:

```text
08-project-structure.md
```

---

# 57. Design Token Rule

All repeated design values should be centralized.

Examples:

```text
Colors
Typography
Spacing
Radius
Shadow
Breakpoints
Transitions
```

Components should consume tokens rather than defining arbitrary values.

---

# 58. Border Radius

Use a restrained radius system.

Example:

```text
small
medium
large
full
```

Do not make every element heavily rounded.

The visual language should remain professional.

---

# 59. Shadows

Shadows should be subtle.

Prefer:

```text
Elevation through surface
+
Border
```

rather than strong shadows everywhere.

---

# 60. Cards

Cards should be used when they improve grouping.

Do not put every section inside a card.

Editorial sections may use open layouts without containers.

---

# 61. Visual Rhythm

The interface should establish a predictable rhythm:

```text
Section label
Headline
Supporting text
Content
```

Example:

```text
SELECTED WORK

Projects I've built and the problems
they were designed to solve.

[Project grid]
```

---

# 62. Project Screenshot Strategy

Screenshots should demonstrate actual functionality.

Preferred:

```text
Dashboard
Workflow
Data visualization
Application interface
System output
```

Avoid filling project pages with decorative screenshots that do not communicate functionality.

---

# 63. Technical Diagram Strategy

For major projects, diagrams may be included:

```text
System Architecture
Database
API Flow
Business Process
```

Diagrams should be simple and readable.

Avoid decorative diagrams that add no technical information.

---

# 64. Public vs CMS Design

Public:

```text
Brand-oriented
Editorial
Content-focused
Performance-focused
```

CMS:

```text
Operational
Dense
Functional
Efficient
```

The two interfaces may share design tokens but should not have identical layouts.

---

# 65. UI Consistency Rules

The same concept must look the same everywhere.

For example:

```text
Primary button
Status badge
Input
Modal
Card
Table
Toast
```

should have one canonical implementation.

Do not create slightly different versions for every page.

---

# 66. AI UI Generation Rules

When using AI/vibe coding:

AI must follow this document.

AI must not:

```text
Invent random colors
Invent random fonts
Add excessive gradients
Add unnecessary animations
Add excessive glassmorphism
Create inconsistent spacing
Create multiple button styles without reason
Create duplicate components
Use placeholder content in production
```

If an existing component can be reused:

```text
Reuse it.
```

Do not create another component with the same responsibility.

---

# 67. Responsive AI Rule

AI-generated UI must be tested at minimum:

```text
Mobile
Tablet
Desktop
Large Desktop
```

The AI must consider:

```text
overflow
text wrapping
image scaling
navigation
tables
forms
touch targets
```

---

# 68. Accessibility AI Rule

AI-generated components must consider:

```text
keyboard access
focus states
semantic elements
labels
ARIA
contrast
reduced motion
```

Accessibility fixes should not be postponed until the end.

---

# 69. Content Rule

The UI should never be designed around fake content that differs significantly from the final content structure.

Use realistic content lengths during development.

This prevents:

```text
overflow
broken layouts
unexpected card heights
poor responsive behavior
```

---

# 70. Design Evolution

The design system must support future additions:

```text
Blog
Articles
Open-source projects
Speaking
Achievements
Services
Testimonials
Case studies
Analytics
```

These are future possibilities, not mandatory MVP features.

New modules should reuse existing design tokens and components.

---

# 71. MVP Design Priority

The first release prioritizes:

```text
1. Homepage
2. Projects
3. Project detail
4. About
5. Experience
6. Skills
7. Certifications
8. Resume
9. Contact
10. CMS
```

Visual complexity is lower priority than:

```text
Usability
Performance
Accessibility
Content quality
Maintainability
```

---

# 72. Final Design Principle

The portfolio should feel like:

```text
A professional developer's digital workspace
```

rather than:

```text
A template demonstrating UI effects.
```

The strongest design element should be the quality and clarity of the work being presented.
