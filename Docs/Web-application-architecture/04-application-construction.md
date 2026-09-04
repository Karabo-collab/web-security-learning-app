# 04 — Application Construction

## Project goal

I built a multi-user notes application to understand normal web application behavior before testing security failures. Its small but meaningful feature set involves identity, sessions, user input, persistent data, and authorization.

## Development progression

The application developed in stages:

1. Confirm Apache was serving its default page.
2. Replace the default page with a simple PHP page.
3. Confirm PHP executed and produced dynamic content.
4. Add a notes interface and SQLite persistence.
5. Add note creation and deletion.
6. Add registration, login, logout, and password hashing.
7. Separate the authentication pages from the dashboard.
8. Associate every note with its owner.
9. Add security controls and preserve a secure baseline.

Testing each layer before adding the next feature made problems easier to isolate.

## Main application files

### `bootstrap.php`

Located at `/var/www/web-lab/bootstrap.php`, this shared file initializes sessions, opens the SQLite connection, creates a CSRF token, and provides reusable functions for output encoding, CSRF checking, and login enforcement. It is outside the public document root because it contains internal setup rather than a directly requested page.

### `register.php`

Located at `/var/www/html/register.php`, this page validates the submitted username and password, hashes the password, and creates the user record. Passwords are stored as hashes rather than plaintext.

### `login.php`

Located at `/var/www/html/login.php`, this page retrieves a user with a parameterized query and verifies the supplied password against the stored hash. A successful login regenerates the session identifier and stores the user's identity in the session.

### `index.php`

Located at `/var/www/html/index.php`, this authenticated dashboard displays only the current user's notes. It also handles note creation, deletion, and logout.

### `view.php`

Located at `/var/www/html/view.php`, this page displays one note selected through an `id` query parameter. It became the focus of the first broken-access-control exercise. The secure version requires both the note ID and owner ID:

```sql
SELECT id, body, created_at
FROM notes
WHERE id = :id AND user_id = :user_id
```

## Database model

```text
User
 ├── id
 ├── username
 ├── password_hash
 └── created_at

Note
 ├── id
 ├── user_id → identifies the owner
 ├── body
 └── created_at
```

The `user_id` relationship makes per-user authorization possible. Authentication identifies the active user, while the ownership condition restricts that user to their own notes.

## Input and output handling

The application treats browser input as untrusted. Controls include integer validation for note IDs, username and password rules, note-length limits, parameterized database queries, HTML output encoding, and CSRF tokens for state-changing forms.

These controls address different risks. Parameterized queries protect SQL structure, while output encoding reduces the risk of stored text being interpreted as markup or script.

## Lessons learned

Constructing the application showed me where security decisions live. A form, button, or hidden link does not enforce security. The PHP backend must validate input, identify the user, enforce ownership, communicate safely with the database, and encode its response.

## Next report

Continue to [05 — Application Architecture](05-application-architecture.md).
