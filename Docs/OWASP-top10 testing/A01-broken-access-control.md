# A01 — Broken Access Control

## Executive summary

This exercise tested whether one authenticated user could access another user's private note by changing the note identifier in the URL. I deliberately introduced a broken object-level authorization flaw into a disposable clone of my notes application, reproduced the vulnerability with two separate user sessions, corrected the server-side database query, and retested the application.

The vulnerable version allowed **Bob** to read **Alice's note 5** by requesting `view.php?id=5`. The repaired version required both the requested note ID and the authenticated user's ID to match the database record. After the repair, the same request made from Bob's session returned `Note not found`, while Alice retained access to her own note.

This demonstrated the difference between authentication and authorization: being logged in proves who a user is, but every request must still verify whether that user is permitted to access the requested resource.

## Classification

| Item | Details |
| --- | --- |
| OWASP category | A01: Broken Access Control |
| Vulnerability type | Insecure Direct Object Reference (IDOR) / broken object-level authorization |
| Privilege relationship | Horizontal privilege escalation between two ordinary users |
| Affected resource | Private notes |
| Security property affected | Confidentiality |
| Result | Reproduced, repaired, and successfully retested |

## Scope and authorization

The test was performed only against my own application inside an isolated VirtualBox lab. I preserved the secure original virtual machine and carried out the deliberately vulnerable test on a disposable clone. No public system, third-party account, or external target was tested.

## Lab environment

| Component | Purpose |
| --- | --- |
| VirtualBox | Hosted the isolated virtual machines |
| Ubuntu Server | Hosted the web application |
| Apache | Served the application over HTTP |
| PHP | Implemented application and authorization logic |
| SQLite | Stored users and notes |
| Windows browser | Maintained the Alice session |
| Incognito browser window | Maintained a separate Bob session |
| SSH through PowerShell | Allowed remote administration of the Ubuntu server |
| Host-Only network | Kept the lab separated from public exposure |

### Why I used Windows instead of Kali Linux

I originally planned to run Kali Linux and Ubuntu Server at the same time. My laptop has 8 GB of RAM, and running both virtual machines would have added unnecessary resource pressure for this first test. I therefore used two isolated browser sessions on Windows: a normal session for Alice and an Incognito session for Bob.

This was sufficient because the vulnerability could be tested by changing an object identifier directly in the browser's address bar. An interception proxy was not required to prove this specific access-control failure. Kali Linux and Burp Suite remain useful for later tests that require request interception, modification, repetition, or deeper inspection.

## Learning objective

The objective was to answer the following question:

> Can one authenticated user access a note owned by another authenticated user by changing the note ID supplied to the application?

The expected secure behaviour was that Alice could access Alice's notes and Bob could access Bob's notes, but neither user could access the other user's records.

## Application context

The application stores notes with two important identifiers:

- `id` identifies the individual note.
- `user_id` identifies the user who owns that note.

The browser supplies the note ID through a URL such as:

```text
http://192.168.56.20/view.php?id=5
```

Because the client controls the `id` parameter, the server cannot trust that the requested note belongs to the logged-in user. The server must compare the requested note against the authenticated identity stored in the session.

## Secure baseline

Before introducing the weakness, the application was designed to associate every note with its owner. The secure original VM was preserved, and a full clone was used for the controlled experiment. This made the exercise repeatable and prevented the intentionally vulnerable state from replacing the known-good baseline.

## Controlled vulnerable implementation

In the training clone, `view.php` required a user to be logged in, but the database query selected a note using only its note ID:

```php
$statement = $db->prepare(
    'SELECT id, body, created_at FROM notes WHERE id = :id'
);
$statement->execute(['id' => $noteId]);
```

The query was parameterized, which helped prevent SQL injection, but it did not enforce ownership. This distinction is important: a prepared statement can safely process an input while still applying incorrect authorization logic.

## Test methodology

1. I created or used two separate accounts named Alice and Bob.
2. I logged in as Alice in the normal browser session.
3. I created private notes for Alice and confirmed that one was note ID `5`.
4. I logged in as Bob in an Incognito browser window, creating a separate authenticated session.
5. I confirmed that Bob's page displayed only Bob's own notes.
6. While still authenticated as Bob, I entered `view.php?id=5` in the address bar.
7. I observed whether the server returned Alice's note.
8. I updated the server-side query to include the authenticated user's ID.
9. I repeated the same request from Bob's session.
10. I confirmed that Alice could still access note 5 from Alice's own session.

## Evidence and observations

| Stage | Observation | Security meaning |
| --- | --- | --- |
| Alice's session | Alice's account contained note 5 with text indicating that only Alice should access it. | Established the resource and its intended owner. |
| Bob's session | Bob was authenticated separately and could see only his own notes on the notes page. | Confirmed separate accounts and sessions. |
| Vulnerable request | Bob requested `view.php?id=5`. | The user-controlled object identifier was changed directly. |
| Vulnerable response | The application displayed Alice's note to Bob. | Broken object-level authorization was confirmed. |
| Code review | The query filtered by `id` but not by `user_id`. | Identified the root cause. |
| Repaired request | Bob repeated the request for `view.php?id=5`. | Reused the same test case for validation. |
| Repaired response | The application returned `Note not found`. | The cross-user read was blocked. |
| Owner verification | Alice could still open her own note. | Confirmed the fix did not remove legitimate access. |

## Finding

The test confirmed an IDOR-style Broken Access Control vulnerability. Bob did not need to compromise Alice's password, steal her session, or bypass the login page. Bob was already a legitimate authenticated user, but the application failed to check whether he owned the requested note.

This is a horizontal privilege escalation because one standard user gained access to another standard user's data.

## Security impact

In this application, successful exploitation exposed another user's private note and therefore affected confidentiality. In a larger application, the same pattern could expose personal information, financial records, support tickets, documents, messages, or other user-owned objects.

If similar missing ownership checks existed on update or delete operations, the impact could extend to unauthorized modification or destruction of another user's data. Those actions were outside the proof performed in this exercise and are not claimed as tested findings.

## Root cause

The application verified authentication by requiring a valid logged-in session, but it did not verify authorization for the requested note. The vulnerable query treated possession of a valid note ID as sufficient permission to read the record.

The key failure was trusting an identifier supplied by the client without enforcing ownership on the server.

## Remediation

I retrieved the authenticated user's ID from the server-side session and added it to the database query:

```php
$userId = (int) $_SESSION['user_id'];

$statement = $db->prepare(
    'SELECT id, body, created_at
     FROM notes
     WHERE id = :id AND user_id = :user_id'
);

$statement->execute([
    'id' => $noteId,
    'user_id' => $userId,
]);
```

The application now returns a note only when both conditions are true:

1. The requested note exists.
2. The note belongs to the authenticated user.

The ownership check is performed in the database query rather than only in the interface. Hiding links or buttons would not be an adequate security control because a user could still construct the request manually.

## Retest result

After applying the repair, I repeated the original request from Bob's authenticated session:

```text
http://192.168.56.20/view.php?id=5
```

The application returned:

```text
Note not found.
```

Alice remained able to access note 5 from her own session. The original unauthorized access path was therefore closed without breaking the owner's legitimate access.

## Outcome flow

```text
Bob is authenticated
        |
        v
Bob requests Alice's note ID
        |
        +--> Vulnerable query checks note ID only
        |         |
        |         v
        |    Alice's note is disclosed
        |
        +--> Repaired query checks note ID + session user ID
                  |
                  v
             Note not found
```

## Recommendations

- Enforce authorization on every operation that reads, updates, or deletes a user-owned object.
- Derive the current user's identity from the server-side session, not from a client-supplied user ID.
- Apply ownership checks in server-side application or database logic.
- Use consistent denial behaviour that does not reveal whether another user's object exists.
- Add automated tests covering same-user access and cross-user denial.
- Review all endpoints for the same pattern, including note deletion and any future editing feature.
- Keep the secure baseline and use disposable clones or snapshots for deliberately vulnerable experiments.
- Preserve evidence of the vulnerable result, code change, and successful retest.

## Lessons learned

This exercise showed me that authentication and authorization solve different problems. Authentication establishes identity; authorization decides what that identity may do. A user can be correctly logged in and still exploit an application when resource-level checks are missing.

I also learned that sequential or visible identifiers are not automatically vulnerabilities. The vulnerability exists when changing an identifier gives access to an object without a valid permission check. The server must therefore enforce ownership independently of what the user interface displays.

Finally, I learned that prepared statements and authorization checks provide different protections. Parameterization protects the structure of a database query, while an ownership condition protects access to the selected data. A secure application needs both.

## Related documentation

- [Application construction](../Web-application-architecture/04-application-construction.md)
- [Application architecture](../Web-application-architecture/05-application-architecture.md)
- [Security design](../Web-application-architecture/06-security-design.md)
- [OWASP Top 10: A01 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
