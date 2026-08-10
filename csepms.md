I am uploading a ZIP file containing my complete PHP + MySQL college project management system called CSEPMS.

Your task is to perform a COMPLETE CODEBASE AUDIT of the project.

IMPORTANT:
- Analyze the actual uploaded project files.
- Do NOT assume that a feature works just because the UI exists.
- Do NOT give generic coding advice.
- Trace the actual execution flow from UI → JavaScript/AJAX/form → PHP/API → database → response → UI.
- Do NOT modify any files.
- Do NOT rewrite the project.
- Do NOT fix anything yet.
- First analyze and report the problems.
- If something cannot be verified from the code, explicitly say "Cannot verify from the provided code" instead of guessing.
- Distinguish confirmed bugs from possible risks and recommendations.

==================================================
1. COMPLETE PROJECT STRUCTURE ANALYSIS
==================================================

First analyze the complete folder structure.

Identify:
- PHP files
- API files
- configuration files
- database/schema SQL files
- JavaScript files
- CSS files
- upload directories
- authentication/session files
- role-specific pages
- common includes
- external libraries
- AJAX endpoints
- cron/background logic if any
- report/PDF generation
- notification system
- chat system
- evaluation system
- academic-year system

Create a high-level architecture showing how these components interact.

==================================================
2. AUTHENTICATION AND AUTHORIZATION AUDIT
==================================================

Analyze the complete login/authentication system.

Check:
- Login flow
- Logout flow
- PHP sessions
- Session initialization
- Session expiration
- Session fixation risks
- Session hijacking risks
- Cookie settings
- Role validation
- Student authorization
- Guide authorization
- Coordinator authorization
- Team-lead authorization
- API authorization
- Direct URL access to protected pages
- Direct API access without authentication
- Whether changing session values can bypass authorization
- Whether role checks are consistently applied
- Whether users can access another user's data by changing IDs in URLs/AJAX requests
- IDOR vulnerabilities
- Privilege escalation
- Missing authorization checks
- Authentication bypass possibilities

Pay special attention to:
- requireLogin()
- requireRole()
- requireRoleAPI()
- auth.class.php
- config/config.php
- $_SESSION values
- role_id
- user_id
- is_team_lead
- academic_year_id

==================================================
3. STUDENT FEATURES
==================================================

Verify every student feature actually works logically.

Check:
- Dashboard
- Profile
- Group/team information
- Team lead functionality
- Upload
- Re-upload
- Delete upload
- Download upload
- View submissions
- Evaluation/grades
- Notifications
- Circulars
- Chat
- Messages
- Academic-year restrictions
- Completed-year read-only behavior
- Any other student functionality present in the code

For each feature determine:

1. Is the UI present?
2. Is the backend/API present?
3. Does the frontend call the correct endpoint?
4. Does the endpoint perform the correct authorization?
5. Does it query the correct database tables?
6. Does it use the correct user/group/student ID?
7. Does it validate ownership?
8. Does it handle errors?
9. Does it work for the intended role?
10. Are there edge cases that can break it?

==================================================
4. GUIDE FEATURES
==================================================

Audit every guide feature.

Check:
- Guide dashboard
- Profile
- Assigned groups
- Students
- Evaluations
- Evaluation submission
- Grade entry
- Notifications
- Circulars
- Chat/messages
- File access/download
- Academic-year handling
- Completed-year restrictions
- Any other guide functionality

Pay particular attention to whether a guide can:
- access another guide's students
- modify another guide's evaluation
- access another group's files
- access data from another academic year

==================================================
5. COORDINATOR FEATURES
==================================================

Audit every coordinator feature.

Check:
- Dashboard
- Student management
- Guide management
- Group management
- Batch management
- Academic-year management
- Evaluation sheets
- Evaluation authorization
- Circulars
- Notifications
- Chat
- Reports
- File uploads
- User management
- Any other coordinator feature

Verify all coordinator actions are correctly restricted to coordinators.

==================================================
6. ACADEMIC YEAR SYSTEM
==================================================

This is an important part of the project.

Trace how academic years are handled throughout the entire project.

Check:
- ACTIVE year
- COMPLETED year
- Session academic_year_id
- Student academic year
- Guide academic year
- Coordinator selected academic year
- New academic year creation
- Year switching
- Deleted academic years
- Read-only behavior
- Data filtering by academic_year_id
- Whether queries accidentally mix data from different years
- Whether IDs are reused incorrectly
- Whether old-year data can be modified
- Whether new-year data can accidentally overwrite old-year data

Find every place where academic_year_id is used and verify consistency.

==================================================
7. DATABASE AUDIT
==================================================

Analyze all SQL/schema files and compare them with the PHP queries.

Check:
- Table structures
- Primary keys
- Foreign keys
- UNIQUE constraints
- ENUM values
- NOT NULL fields
- DEFAULT values
- Data types
- Foreign-key relationships
- Cascading behavior
- Missing indexes
- Duplicate/inconsistent columns
- Columns used by PHP but missing from schema
- Columns defined in schema but never used
- SQL queries that don't match the current schema
- INSERT statements missing required fields
- UPDATE statements
- DELETE statements
- JOIN correctness
- GROUP BY correctness
- SQL compatibility with strict mode
- Invalid ENUM values
- Invalid dates
- NULL handling
- Truncated values
- SQL errors hidden by the application

Pay particular attention to strict MySQL compatibility.

The application should not depend on permissive SQL behavior.

==================================================
8. SQL INJECTION AND DATABASE SECURITY
==================================================

Search the entire project for SQL queries.

Check whether user-controlled values are directly concatenated into SQL.

Identify:
- SQL injection vulnerabilities
- Missing prepared statements
- Incorrect bind_param types
- Unsafe dynamic queries
- Unsafe ORDER BY / LIMIT handling
- Unsafe database/table/column selection
- Unvalidated IDs

For every finding provide the exact file and line/section.

==================================================
9. FILE UPLOAD / RE-UPLOAD / ATTACHMENT AUDIT
==================================================

This is VERY IMPORTANT.

Audit ALL file upload functionality.

Search the entire project for:
- move_uploaded_file()
- $_FILES
- unlink()
- file_exists()
- fopen()
- readfile()
- download handlers
- upload directories
- attachment paths

Check:
- Student uploads
- Student re-upload
- Student file deletion
- Circular attachments
- Guide uploads
- Coordinator uploads
- Chat attachments
- Report files
- Any other uploaded files

For each upload feature verify:

- Correct destination directory
- Directory creation
- Directory permissions
- Absolute vs relative paths
- Filename generation
- Filename sanitization
- File extension validation
- MIME type validation
- File size validation
- Dangerous file types
- PHP/script upload possibility
- Path traversal
- Directory traversal
- Overwriting existing files
- File deletion authorization
- Download authorization
- Whether one user can download another user's file
- Whether one student can replace another student's file
- Whether re-upload deletes the correct old file
- Whether database path matches physical file path
- Whether the code works on the deployed server
- Whether the code assumes localhost/XAMPP permissions




==================================================
10. CIRCULARS AND NOTIFICATIONS
==================================================

Audit:
- Send circular
- Circular attachments
- Target role selection
- All users
- Students only
- Guides only
- Coordinators
- Batch targeting
- Notification creation
- Notification read status
- Attachment download
- Notification deletion if present

Check that database ENUM values match the values actually inserted by PHP.

For example, compare every target_type value used in PHP against the actual notifications.target_type ENUM.

Find any mismatch such as:
PHP inserts a value that the database ENUM does not allow.

==================================================
11. CHAT SYSTEM
==================================================

Audit:
- Coordinator → batch
- Coordinator → guide
- Guide → student
- Student → guide
- Student → coordinator
- Recent chats
- Unread counts
- Mark as read
- Message retrieval
- Message sending
- Chat attachments if present

Verify:
- Correct sender/receiver IDs
- Correct sender/receiver roles
- Group/batch targeting
- Authorization
- Data isolation
- Academic-year filtering
- Whether a user can manipulate receiver IDs
- Whether a user can read another user's conversations

==================================================
12. EVALUATION SYSTEM
==================================================

Audit the complete evaluation workflow.

Check:
- Evaluation sheet creation
- Editing
- Major project
- Mini project
- Evaluation criteria
- Marks
- Guide submission
- Coordinator authorization
- Student viewing
- Academic-year association
- Status transitions
- Duplicate sheets
- Completed-year restrictions

Verify that database constraints match actual application behavior.

For Mini Project specifically:
- There should be no unnecessary user-entered title if the application design does not require one.
- Verify how one evaluation sheet is associated with each academic year.
- Verify whether editing updates the existing sheet instead of creating unintended duplicates.
- Verify that historical academic years remain intact.

==================================================
13. SECURITY AUDIT
==================================================

Perform a security audit for:

- SQL injection
- XSS
- CSRF
- IDOR
- Authentication bypass
- Authorization bypass
- Privilege escalation
- Session vulnerabilities
- File upload vulnerabilities
- Path traversal
- Arbitrary file access
- Arbitrary file deletion
- Sensitive information exposure
- Password handling
- Hard-coded credentials
- Debug mode
- PHP error display
- Stack traces
- Database errors exposed to users
- API endpoint exposure
- Missing access control
- Unsafe redirects
- Open redirects
- Insecure cookies
- Missing security headers

Rate each finding:
CRITICAL / HIGH / MEDIUM / LOW

==================================================
14. ERROR HANDLING
==================================================

Find:
- PHP warnings
- Fatal errors
- uncaught exceptions
- empty catch blocks
- database errors hidden from users
- APIs returning HTTP 200 for failed operations
- inconsistent JSON responses
- JavaScript expecting a different response format
- errors that work on localhost but fail in production

Check whether production has:

display_errors = 1

and explain whether that should be changed.

==================================================
15. LOCALHOST VS PRODUCTION COMPATIBILITY
==================================================

Analyze whether the project depends on:

- XAMPP
- Windows paths
- localhost
- MySQL/MariaDB differences
- permissive SQL mode
- filesystem permissions
- relative paths
- hard-coded /csepms/ paths
- Apache configuration
- PHP configuration
- upload limits
- session settings

Identify anything that could work on localhost but fail on a deployed Ubuntu/Apache server.

==================================================
16. HARDCODED VALUES
==================================================

Search the entire project for:

- /csepms/
- localhost
- 127.0.0.1
- C:/
- Windows paths
- hard-coded ports
- hard-coded database names
- hard-coded URLs
- hard-coded user IDs
- hard-coded role values
- hard-coded academic-year IDs
- hard-coded file paths
- credentials/API keys

For each one, determine whether it is intentional or a portability problem.

==================================================
17. FRONTEND ↔ BACKEND CONSISTENCY
==================================================

For every AJAX endpoint and form:

Check:

Frontend request
      ↓
URL
      ↓
HTTP method
      ↓
parameters
      ↓
PHP/API
      ↓
validation
      ↓
database
      ↓
JSON response
      ↓
frontend handling

Identify mismatches such as:
- wrong parameter names
- wrong HTTP methods
- missing parameters
- wrong response property names
- endpoint path errors
- JavaScript expecting success when PHP returns failure
- HTTP 200 returned for errors
- stale endpoints
- unused endpoints

==================================================
18. DEAD CODE / DUPLICATE CODE
==================================================

Find:
- unused PHP files
- duplicate APIs
- duplicate functions
- old versions of files
- unreachable code
- unused database tables
- unused columns
- unused JavaScript
- conflicting implementations
- backup files accidentally accessible from the web

Check for files like:
- *.bak
- *.old
- *_old.php
- test files
- debug files
- temporary files

==================================================
19. BUSINESS LOGIC / EDGE CASES
==================================================

Test logically for:

- no students
- no guides
- no groups
- empty batch
- duplicate groups
- duplicate users
- deleted academic year
- completed academic year
- guide with no assigned groups
- student without group
- team lead changed
- group with multiple team leads
- file deleted before re-upload
- missing attachment
- duplicate notification
- duplicate evaluation sheet
- empty message
- very large message
- invalid IDs
- deleted users
- unauthorized users
- concurrent requests

==================================================
20. FINAL FEATURE CHECKLIST
==================================================

Create a table:

| Feature | Frontend | Backend | Database | Authorization | Error Handling | Status |
|---------|-----------|---------|----------|---------------|----------------|--------|

Use:
PASS
FAIL
PARTIAL
CANNOT VERIFY

Do not mark something PASS simply because code exists.

==================================================
21. BUG REPORT
==================================================

Create a prioritized bug list:

| Priority | File | Problem | Why it happens | Impact | Recommended change |
|----------|------|---------|----------------|--------|--------------------|

Separate:

CONFIRMED BUGS
POSSIBLE BUGS
SECURITY VULNERABILITIES
DEPLOYMENT ISSUES
CODE QUALITY ISSUES
RECOMMENDATIONS

==================================================
22. MOST IMPORTANT FINAL REPORT
==================================================

At the end provide:

A. Top 10 problems that MUST be fixed before deployment

B. Security vulnerabilities

C. Database/schema problems

D. Upload/file-handling problems

E. Authentication/session problems

F. Authorization problems

G. Localhost vs production problems

H. Features that are incomplete or logically incorrect

I. Features that appear correctly implemented

J. Exact files that need changes

K. Recommended testing checklist

IMPORTANT:
Do not make any changes to the project.
Do not generate replacement code unless I explicitly ask for it later.
First give me the audit and explain exactly what is wrong, why it is wrong, and where it needs to be changed.
