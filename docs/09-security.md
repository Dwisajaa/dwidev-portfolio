# Security Specification

## 1. Purpose

This document defines the security requirements for the DwiDev Portfolio platform.

Security must be considered from the beginning of development rather than added after the application is completed.

The system contains:

* Public website.
* CMS administration.
* Authentication.
* Portfolio content management.
* Media uploads.
* Contact messages.
* Database records.
* Resume and certificate files.
* API endpoints.

The primary objective is to prevent unauthorized access, data manipulation, information disclosure, malicious uploads, abuse of public endpoints, and accidental exposure of secrets.

---

# 2. Security Principles

The application follows:

```text
Least Privilege
Defense in Depth
Secure by Default
Fail Securely
Input Validation
Output Encoding
Separation of Concerns
Secret Management
Auditability
```

Security decisions should favor:

```text
Simple
Explicit
Framework-supported
Well-tested
```

over custom security mechanisms.

---

# 3. Trust Boundaries

The system has several trust boundaries.

```text
Internet
   │
   ▼
Public Website
   │
   ▼
Laravel API
   │
   ├── MySQL
   ├── Redis
   └── Storage
```

Administrative boundary:

```text
Administrator
   │
   ▼
CMS
   │
   ▼
Authenticated API
   │
   ▼
Laravel
```

The administrator is trusted only after successful authentication and authorization.

---

# 4. Public vs Private Resources

Public resources:

```text
Published projects
Published skills
Published experience
Published education
Published certifications
Public profile
Public resume
```

Private resources:

```text
Admin dashboard
Draft projects
Archived content where applicable
Contact message management
Audit logs
User management
System settings
Private media
```

The API must explicitly determine whether a resource is public or private.

---

# 5. Authentication

The CMS requires authentication.

Unauthenticated users must not access:

```text
/admin
```

or privileged API endpoints.

Authentication should be implemented using Laravel-supported mechanisms.

The implementation should avoid custom authentication protocols unless there is a documented requirement.

---

# 6. Authentication Flow

Conceptual flow:

```text
Administrator
      ↓
Login Form
      ↓
Laravel Authentication Endpoint
      ↓
Credential Verification
      ↓
Authenticated Session / Token
      ↓
CMS
```

Failed authentication must not reveal whether:

```text
email exists
username exists
account exists
```

Use a generic authentication failure response.

---

# 7. Password Security

Passwords must never be stored in plaintext.

Passwords must use Laravel's supported password hashing mechanisms.

The application must never:

```text
log passwords
return passwords through API
store passwords in frontend storage
include passwords in audit logs
```

---

# 8. Session / Token Security

Authentication credentials must be protected.

If cookie-based authentication is used, cookies should use appropriate security attributes:

```text
Secure
HttpOnly
SameSite
```

If token-based authentication is used:

```text
Tokens must be protected
Tokens must have appropriate expiration/revocation behavior
Tokens must not be logged
```

The final mechanism must be selected based on the deployment architecture.

---

# 9. Authorization

Authentication answers:

```text
Who are you?
```

Authorization answers:

```text
What are you allowed to do?
```

Every privileged operation must perform authorization.

Example:

```text
Authenticated Admin
        ↓
Project Policy
        ↓
Create / Update / Delete
```

Never assume:

```text
authenticated == authorized
```

---

# 10. Role Design

Initial role:

```text
admin
```

Future roles may include:

```text
editor
```

Authorization should be implemented in a way that allows additional roles without rewriting every controller.

---

# 11. CMS Route Protection

All CMS routes must be protected.

Conceptually:

```text
/admin/*
    ↓
Authentication
    ↓
Authorization
    ↓
CMS
```

Unauthenticated access:

```text
/admin
```

must redirect or return an appropriate unauthorized response.

---

# 12. API Authorization

API endpoints are classified as:

```text
PUBLIC
AUTHENTICATED
ADMIN
```

Example:

```text
GET /api/v1/projects
→ PUBLIC

GET /api/v1/projects/{slug}
→ PUBLIC

POST /api/v1/projects
→ ADMIN

PUT /api/v1/projects/{id}
→ ADMIN

DELETE /api/v1/projects/{id}
→ ADMIN
```

The classification must be documented.

---

# 13. IDOR Protection

The application must prevent insecure direct object references.

Example:

```text
/admin/projects/25/edit
```

must not automatically grant access simply because the user knows ID `25`.

Authorization must be performed against the requested resource.

---

# 14. Input Validation

All external input is untrusted.

Sources include:

```text
HTTP requests
Query parameters
Path parameters
Headers
Uploaded files
JSON payloads
Form data
```

Backend validation is mandatory.

Laravel Form Requests should be used where appropriate.

---

# 15. Client-Side Validation

Frontend validation improves user experience.

However:

```text
Client validation ≠ security
```

The backend must independently validate every request.

A malicious client can bypass frontend validation.

---

# 16. Mass Assignment Protection

Eloquent models must protect against unintended mass assignment.

Only explicitly permitted fields may be assigned.

Do not blindly pass request payloads into models.

Avoid patterns equivalent to:

```text
Model::create($request->all())
```

unless the input has been deliberately validated and controlled.

---

# 17. SQL Injection

Database access must use Laravel's query builder or Eloquent parameterization.

Never concatenate untrusted input into SQL.

Avoid:

```text
raw SQL + user input
```

unless parameters are properly bound.

---

# 18. XSS Protection

User-controlled content must not be rendered as trusted HTML without sanitization.

Potential sources:

```text
Project descriptions
About content
Contact messages
CMS text fields
```

If rich text is supported, HTML must be sanitized.

Do not render arbitrary HTML directly from database content.

---

# 19. Rich Text Editor

If the CMS supports rich text:

```text
HTML input
    ↓
Sanitization
    ↓
Stored content
    ↓
Safe rendering
```

The sanitizer must define allowed:

```text
tags
attributes
URLs
formatting
```

JavaScript execution must never be allowed through portfolio content.

---

# 20. CSRF Protection

State-changing requests using cookie-based authentication must have appropriate CSRF protection.

Relevant operations:

```text
POST
PUT
PATCH
DELETE
```

The final CSRF strategy must match the selected authentication architecture.

---

# 21. CORS

CORS must use an explicit allowlist.

Development may allow:

```text
localhost frontend
```

Production should allow only trusted origins.

Avoid:

```text
Access-Control-Allow-Origin: *
```

for authenticated APIs.

---

# 22. Rate Limiting

Rate limiting is required for abuse-prone endpoints.

Initial targets:

```text
Login
Contact form
Password-related operations
Media upload
Authentication endpoints
```

Example:

```text
Too many requests
      ↓
HTTP 429
      ↓
Retry later
```

---

# 23. Login Protection

The login endpoint must have protection against brute-force attempts.

Controls may include:

```text
Rate limiting
Temporary throttling
Generic error messages
Security logging
```

Do not implement CAPTCHA initially unless abuse requires it.

---

# 24. Contact Form Protection

The public contact form is an abuse target.

Required:

```text
Validation
Rate limiting
Email validation
Length limits
Spam mitigation strategy
```

Potential future:

```text
Turnstile / CAPTCHA
```

Only introduce additional anti-spam mechanisms when needed.

---

# 25. Contact Message Data

Contact messages may contain personal information.

The system should minimize stored data.

Store only information required for the workflow.

Example:

```text
name
email
subject
message
status
created_at
```

Avoid collecting unnecessary personal information.

---

# 26. Contact Message Access

Contact messages are private.

They must never appear in:

```text
public API
public search
public project responses
public website
```

Only authorized administrators may access them.

---

# 27. File Upload Security

File uploads are considered untrusted.

Every upload must validate:

```text
MIME type
File extension
File size
File content where appropriate
```

Do not trust the filename or browser-provided MIME type alone.

---

# 28. Allowed Media Types

Initial allowed images:

```text
JPEG
PNG
WebP
```

Potential document:

```text
PDF
```

Only required formats should be allowed.

Do not allow arbitrary executable file types.

---

# 29. Upload Size Limits

Uploads must have explicit limits.

Example:

```text
Images
→ reasonable maximum size

Resume PDF
→ reasonable maximum size
```

The exact limits should be defined in application configuration.

Never allow unlimited uploads.

---

# 30. Filename Security

Original filenames must not be trusted.

The application should generate safe storage filenames.

Avoid directly using:

```text
../../../file.php
```

or other user-controlled filenames as filesystem paths.

---

# 31. Path Traversal Protection

User input must never directly determine filesystem paths.

The application must prevent:

```text
../
..\ 
absolute paths
unexpected path traversal
```

Use Laravel's filesystem abstraction.

---

# 32. Uploaded File Execution

Uploaded files must not be executable.

Storage should be configured so uploaded content cannot become server-side executable code.

For example:

```text
PHP uploaded as media
→ rejected
```

---

# 33. Public Storage

Only files intended for public access should be publicly accessible.

Example:

```text
Published project image
→ public

Private admin file
→ private
```

Private files should require authorized access.

---

# 34. Resume Security

If the resume is public:

```text
Published resume
→ public
```

Draft or replacement files:

```text
Draft resume
→ private
```

Only the active resume should be exposed publicly.

---

# 35. Certificate Security

Certificates may be public if intentionally published.

The CMS should control:

```text
published
draft
archived
```

Only published certificates appear on the public website.

---

# 36. API Data Exposure

API responses must return only necessary fields.

Never expose:

```text
password
password_hash
authentication tokens
internal secrets
private notes
audit internals
database credentials
server paths
```

Use API Resources to explicitly control response data.

---

# 37. API Error Responses

Production API errors should be controlled.

Do not expose:

```text
stack trace
SQL query
database credentials
filesystem paths
framework internals
environment variables
```

Example:

```json
{
  "message": "An unexpected error occurred."
}
```

Detailed information remains in server logs.

---

# 38. Secret Management

Secrets must never be committed to Git.

Examples:

```text
APP_KEY
DB_PASSWORD
SMTP_PASSWORD
API_KEYS
ACCESS_TOKENS
STORAGE_SECRET
```

must exist only in secure environment configuration.

---

# 39. Environment Files

Repository may contain:

```text
.env.example
```

It must contain placeholders only.

Example:

```text
DB_PASSWORD=
API_KEY=
```

Never place production secrets in:

```text
README
documentation
source code
Git commits
screenshots
test fixtures
```

---

# 40. Git Security

Before committing:

```text
.env
credentials
private keys
tokens
database dumps
production files
```

must be checked.

`.gitignore` must protect sensitive local files.

If a secret is accidentally committed:

```text
Revoke secret
Rotate secret
Remove from repository history where necessary
```

Deleting the file alone is not sufficient.

---

# 41. Dependency Security

Dependencies must be kept reasonably current.

Before adding a dependency:

```text
Check maintenance
Check license
Check security history
Check popularity / adoption
Check whether framework already provides the feature
```

Do not add dependencies merely for convenience.

---

# 42. Dependency Updates

Dependency updates should be controlled.

Recommended workflow:

```text
Update
 ↓
Install
 ↓
Run tests
 ↓
Run build
 ↓
Review breaking changes
 ↓
Commit
```

Do not blindly update every dependency in production.

---

# 43. Security Headers

Production should use appropriate security headers.

Potential headers:

```text
Strict-Transport-Security
Content-Security-Policy
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
```

Exact configuration should be tested against:

```text
Next.js
Laravel
CMS
Media
Analytics
```

before enforcement.

---

# 44. HTTPS

Production must use HTTPS.

HTTP should redirect to HTTPS.

Authentication credentials and sensitive requests must never be transmitted over plaintext HTTP.

---

# 45. TLS

TLS certificates should be automatically renewed where possible.

Certificate renewal must be monitored.

Expired certificates can make the entire application inaccessible.

---

# 46. Nginx Security

Nginx should:

```text
Terminate TLS
Proxy requests
Limit inappropriate request sizes
Hide unnecessary server information
Serve only intended files
```

Sensitive directories must never be publicly exposed.

Examples:

```text
.env
.git
storage internals
database files
logs
```

---

# 47. Laravel Security Configuration

Production:

```text
APP_ENV=production
APP_DEBUG=false
```

Never run production with:

```text
APP_DEBUG=true
```

Debug information can expose sensitive application details.

---

# 48. Database Security

MySQL must not be exposed publicly unless absolutely necessary.

Preferred:

```text
Internet
   X
   │
MySQL
```

Instead:

```text
Laravel
   ↓
Private MySQL
```

Database credentials should use least privilege.

---

# 49. Database User

Application database user should have only the privileges required by the application.

Avoid using:

```text
root
```

as the normal Laravel application database account.

---

# 50. Redis Security

Redis should remain on a private network.

It should not be directly accessible from the public Internet.

If authentication is required, configure it appropriately.

Redis should not contain long-lived secrets unless there is a clear reason.

---

# 51. Docker Security

Containers should follow least privilege where practical.

Avoid:

```text
privileged containers
unnecessary host mounts
unnecessary exposed ports
root processes where avoidable
```

Only expose required services.

Example:

```text
Nginx
→ public

MySQL
→ private

Redis
→ private
```

---

# 52. Production Ports

Public ports should normally be limited to:

```text
80
443
```

Internal services should communicate through the Docker/private network.

Development ports may differ.

---

# 53. SSH Security

VPS administration should use secure SSH configuration.

Recommended:

```text
SSH keys
Limited users
Firewall
Fail2ban or equivalent where appropriate
No unnecessary exposed services
```

The application itself should not depend on SSH availability for normal runtime operation.

---

# 54. Firewall

Production firewall should allow only required traffic.

Conceptually:

```text
80   → HTTP
443  → HTTPS
22   → SSH, preferably restricted
```

Database and Redis ports should not be publicly exposed.

---

# 55. Audit Logging

Important administrative actions should be logged.

Examples:

```text
Login
Logout where useful
Create project
Update project
Delete project
Publish project
Upload media
Delete media
Update profile
Change settings
```

Audit logs should include:

```text
actor
action
resource
resource_id
timestamp
```

---

# 56. Audit Log Protection

Audit logs must not be editable by ordinary CMS operations.

They should be treated as append-oriented records.

Administrators should not be able to casually modify historical audit records.

---

# 57. Logging Sensitive Data

Never log:

```text
Passwords
Tokens
API keys
Session credentials
Database passwords
Full authentication headers
```

Contact messages should also not be unnecessarily duplicated into application logs.

---

# 58. Monitoring

Production monitoring should track:

```text
Server availability
CPU
RAM
Disk
Database
Redis
HTTP errors
Application errors
SSL certificate
```

Initial monitoring can remain simple.

---

# 59. Health Checks

The backend should expose:

```text
GET /api/v1/health
```

The endpoint must not reveal internal infrastructure details.

Public response:

```json
{
  "status": "ok"
}
```

Detailed dependency diagnostics should be restricted or handled separately.

---

# 60. Backup

Production data must be backed up.

At minimum:

```text
MySQL
Media storage
Resume
Certificates
Project images
```

Backup credentials must be stored separately from the application.

---

# 61. Backup Security

Backups contain potentially sensitive data.

They must be:

```text
Protected
Access-controlled
Stored separately
Not publicly accessible
```

Do not place production backups in the public web root.

---

# 62. Backup Testing

A backup is not considered reliable merely because it exists.

Periodically verify:

```text
Backup exists
Backup is readable
Backup can be restored
Application can use restored database
Media references remain valid
```

---

# 63. Data Retention

The application should define retention policies where necessary.

Example:

```text
Contact messages
Audit logs
Old media
Old resumes
```

Do not retain unnecessary data indefinitely without a reason.

---

# 64. Privacy

The public website should collect minimal personal information.

The contact form should explain what information is collected when appropriate.

Analytics, if introduced, should also be reviewed for privacy implications.

---

# 65. Third-Party Services

Before integrating a third-party service:

```text
Identify data sent
Identify credentials required
Review security
Review privacy implications
Review dependency
```

Do not send user data to third parties unnecessarily.

---

# 66. API Security Contract

Every endpoint must define:

```text
Authentication
Authorization
Input validation
Rate limit
Response fields
Error behavior
```

Example:

```text
POST /api/v1/projects

Auth:
ADMIN

Validation:
required title
required slug
...

Rate limit:
authenticated API limit

Response:
ProjectResource
```

---

# 67. Security Testing

Security testing should include:

```text
Authentication tests
Authorization tests
Validation tests
Upload tests
Rate-limit tests
CORS tests
XSS tests
IDOR tests
API exposure tests
```

---

# 68. Authentication Test Cases

At minimum:

```text
Valid login
Invalid password
Unknown account
Missing credentials
Rate limit
Logout
Expired authentication
Unauthorized access
```

---

# 69. Authorization Test Cases

Test:

```text
Unauthenticated user
Authenticated non-admin
Admin
```

against:

```text
Create
Read
Update
Delete
Publish
Archive
```

where applicable.

---

# 70. Upload Security Test Cases

Test:

```text
Valid JPEG
Valid PNG
Valid WebP
Valid PDF
Invalid extension
Invalid MIME
Oversized file
Executable file
Malformed file
Suspicious filename
Path traversal attempt
```

---

# 71. CMS Security UX

Security should not make the CMS unnecessarily difficult to use.

Example:

```text
Delete Project
      ↓
Confirmation
      ↓
Delete
```

For high-impact actions, make consequences clear.

---

# 72. Session Expiration UX

If the administrator session expires:

```text
API
 ↓
401 Unauthorized
 ↓
CMS detects expired session
 ↓
Redirect to login
 ↓
Display clear message
```

The CMS should not silently fail.

---

# 73. CSRF / Authentication Errors

Frontend should distinguish:

```text
401 Unauthorized
403 Forbidden
419 CSRF / session issue where applicable
422 Validation Error
429 Rate Limited
500 Server Error
```

Each should have appropriate UX.

---

# 74. Production Error Handling

Users should receive controlled messages.

Administrators may receive additional operational information through:

```text
logs
monitoring
server console
error tracking
```

Do not expose infrastructure diagnostics through public API responses.

---

# 75. Security and AI Coding

AI-generated code must follow this security document.

AI must not:

```text
Disable authentication
Disable authorization
Disable CSRF without justification
Allow wildcard CORS
Expose .env
Expose database
Disable validation
Trust uploaded filenames
Store plaintext passwords
Return sensitive fields
Log secrets
Set APP_DEBUG=true in production
```

---

# 76. AI Dependency Rule

AI must not install security-related packages without evaluating whether Laravel already provides the capability.

Framework-native security mechanisms should be preferred.

---

# 77. AI Code Review Rule

Before accepting AI-generated code involving:

```text
Authentication
Authorization
File upload
Database
API
Payment if ever introduced
Secrets
Middleware
CORS
```

the implementation must be reviewed explicitly for security.

---

# 78. Security Change Rule

Security-sensitive changes require extra caution.

Examples:

```text
Authentication changes
Authorization changes
Middleware changes
CORS changes
Upload changes
Nginx changes
Docker networking changes
Database exposure changes
```

These changes must be tested before deployment.

---

# 79. Security Incident Procedure

If a credential or secret is exposed:

```text
1. Revoke the credential.
2. Generate a replacement.
3. Update production configuration.
4. Review logs for misuse.
5. Remove exposed secret from repository history if necessary.
6. Identify how exposure occurred.
7. Prevent recurrence.
```

Do not merely delete the exposed value from the latest commit.

---

# 80. Security Priority

Security priorities:

```text
1. Authentication
2. Authorization
3. Secret protection
4. Input validation
5. File upload security
6. Database security
7. API security
8. Infrastructure security
9. Monitoring
10. Continuous improvement
```

---

# 81. Security Principle

The final rule:

> Never trust data simply because it came from the frontend, CMS, database, uploaded file, or another internal-looking component.

Every trust boundary must be explicit.

Security should be enforced primarily by the backend and infrastructure, while the frontend provides the appropriate user experience.
