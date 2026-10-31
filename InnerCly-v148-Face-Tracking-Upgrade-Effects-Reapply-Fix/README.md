# InnerCly V137 — Private Platform Teams & Scalable Roles

V137 is a targeted update on the complete V135 source. It adds a multi-role,
platform-wide staff system split across four private teams. Existing Discover,
Group Events, group moderation, Home, profiles, chat, Stories, login/session
flows, branding, website files, and other app features are retained.

## Four platform teams

| Team | Roles |
|---|---|
| Executive | Administrator |
| Community & Safety | Community Manager, Senior Moderator, Moderator |
| Technical | Developer Lead, Web Developer, QA Lead, QA Tester |
| Support & Creators | Support Agent, Creator |

The Owner is above all four teams, has access to every team workspace, and is
protected from assignment or removal through the role-management interface.
Accounts can hold multiple roles across teams. For example, a user can be both
QA Tester and Creator.

## Private visibility and access

- The Staff Centre navigation is hidden unless the signed-in account has at
  least one explicitly assigned platform team role (or a legacy elevated role
  being migrated).
- Staff Centre only returns team areas corresponding to that account's roles.
- Staff data is included only in the signed-in user's own authentication
  response; public profile payloads do not expose platform role assignments.
- Every staff API checks the assigned permission on the server. A normal Member
  receives HTTP 403 even when directly requesting a staff endpoint.
- Only the Owner can assign or revoke platform roles. Owner changes are written
  to a platform-role audit table and notify the affected account.
- Group-specific roles, verification badges, and Verified Creators invitations
  are independent of platform staff roles.

## Permissions included

- Administrator: Executive workspace and specified community oversight; cannot
  appoint staff or access Owner-only controls.
- Community Manager: public community page and event management.
- Senior Moderator: report review, resolution, and escalation.
- Moderator: report review and resolution.
- Developer Lead: safe diagnostics, technical issue submission/review, and QA
  review.
- Web Developer: safe diagnostics and technical issue submission/review.
- QA Lead: safe diagnostics, issue submission/review, and QA review.
- QA Tester: safe diagnostics and technical issue submission.
- Support Agent: private team area. A full support-ticket queue is not included
  in this build.
- Creator: creator analytics. Creator access does not grant staff moderation or
  administrative authority beyond the Creator role's specific permissions.
- Owner: all platform permissions.

Technical workspaces intentionally do not expose private messages, passwords,
database credentials, payment secrets, or raw private account data. Production
deployment remains Owner-controlled.

## Owner setup: assigning roles

1. Sign in as `@innercly_owner`.
2. Open **Roles & Permissions**.
3. Search for an account.
4. Select one or more checkboxes in the desired team(s).
5. Choose **Save team roles**.

The platform records grant/revoke audit entries and sends a notification. The
account should refresh or sign in again to see the current Staff Centre links.
Removing all platform roles removes access; server-side checks apply immediately
on the next API request.

## Run locally on Windows

1. Extract the ZIP into its own folder.
2. Double-click `START_INNERCLY.bat`.
3. Keep the terminal window open while using InnerCly.
4. Sign in using the existing account and database setup.

Do not open `index.html` directly; InnerCly requires the API and database. The
launcher checks `/api/build` and only opens the V137 build.

## Database and backups

V137 adds three tables: `rt_platform_role_assignments`,
`rt_platform_role_audit`, and `rt_staff_issues`. Existing tables and rows are not
recreated or reset. As with any production schema update, take a database backup
before deploying a new build.

The package includes `backups/InnerCly-v40-master-backup.html` and
`backups/InnerCly-v80-master-backup.html`. Both files were hash-checked against
the preceding complete V135 package and are unchanged.

## Validation and deployment

Python compilation, disposable-database FastAPI integration tests, role access
and revocation checks, and JavaScript parsing of all inline script blocks passed.
A real-browser visual interaction test was not available in the build
environment. This ZIP is a local source package; it does not push code or deploy
itself. The hosted app remains on its last deployed version until V137 is
committed to the existing connected repository and deployed. Reuse the existing
production database; do not replace its connection string or create a new DB.
