# compsec_project_34

A computer security course project: a small todo app built **insecure on purpose**, then hardened. We attacked the vulnerable version and wrote up the fixes in a report (not included in this repo).

**Do not deploy the `insecure` branch.**

## Branches

| Branch | Description |
|---|---|
| `insecure` | Original app with deliberate vulnerabilities |
| `secure` (default) | Same app with the vulnerabilities fixed |

## Vulnerabilities and fixes

| Vulnerability | `insecure` | `secure` |
|---|---|---|
| SQL injection | Search builds its query with `format!` string interpolation | Parameterised queries (`sqlx::query!`) |
| Broken access control | User ID taken from the URL (`/todos/user/{id}`); any todo can be deleted by ID | User taken from the session; every query filtered by `user_id`; routes behind `login_required!` |
| Cryptographic failure | Passwords stored in plaintext | Argon2 hashes (`password-auth`) |
| Sensitive data exposure | Credentials printed to stdout on login | Removed |
| Authentication | No sessions | Signed session cookies (`axum-login` + `tower-sessions`, SQLite store) |
| CORS | Permissive (`Any`) | Single allowed origin, with credentials |

## Stack

- **Backend:** Rust, Axum, SQLx, SQLite (`axum_api/`)
- **Frontend:** plain HTML/JS (`frontend/`)

## Running

```bash
# backend on :3000, seeded with admin/admin, user1/password1, user2/password2
cd axum_api && cargo run

# frontend (the secure branch's CORS policy allows http://localhost:5000)
python3 -m http.server -d frontend 5000
```
