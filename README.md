# 🔐 Secure Authentication Lab

A beginner-friendly cybersecurity portfolio project demonstrating defensive authentication practices in a local Flask web app. Includes a polished dark UI, password hashing, CSRF protection, login rate limiting, session controls, security headers, and automated tests.

> **Scope:** This is a learning lab for localhost. It is not a production identity system and must not be exposed to the public internet as-is.

## Features

- **Password hashing:** Werkzeug's password hashing utilities; no plaintext password storage.
- **Registration validation:** username allowlist, length constraints, password confirmation, and unique usernames.
- **Parameterized SQL:** avoids SQL injection through user-supplied query values.
- **CSRF protection:** Flask-WTF validates form submissions.
- **Login throttling:** Flask-Limiter limits registration and login POSTs.
- **Session hygiene:** session cleared on successful login and logout, with a limited lifetime.
- **Cookie settings:** HttpOnly and SameSite=Lax; Secure cookie option for HTTPS deployments.
- **Security headers:** CSP, X-Content-Type-Options, X-Frame-Options, Referrer-Policy, and no-store caching.
- **Automated tests:** pytest checks authentication boundaries and key defensive controls.
- **Responsive interface:** custom HTML/CSS templates for login, registration, and dashboard.

## Screenshots

Add your own screenshots here after running the app. Suggested images:
- `docs/login.png`
- `docs/register.png`
- `docs/dashboard.png`

Do not include real credentials, secret keys, or private data in screenshots.

## Requirements

- Python 3.10 or newer
- pip

## Quick start

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/secure-authentication-lab.git
cd secure-authentication-lab
```

### 2. Create and activate a virtual environment

**Windows PowerShell**
```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS / Linux**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the secret key

Copy `.env.example` to `.env`, then replace `SECRET_KEY` with a random value.

**Generate a key**
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

**Windows PowerShell**
```powershell
Copy-Item .env.example .env
notepad .env
```

**macOS / Linux**
```bash
cp .env.example .env
nano .env
```

Keep `.env` private. It is excluded by `.gitignore`. Never commit real secrets.

### 5. Run the app

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000).

The local database `auth_lab.db` is created automatically and is excluded from Git.

## Run tests

```bash
pytest -q
```

Tests use a temporary database and test-only configuration. The test fixture disables CSRF and rate limiting to keep unit tests focused and repeatable; those controls remain enabled in normal app configuration. Add dedicated integration tests for those controls before claiming full coverage.

## Security testing checklist

- [ ] Register with a valid username and strong test password.
- [ ] Confirm the database contains a password hash, not the original password.
- [ ] Verify an incorrect password does not create a session.
- [ ] Request `/dashboard` without authenticating; access should be denied.
- [ ] Verify logout clears the session.
- [ ] Submit a form without a CSRF token in a normal (non-test) run; it should be rejected.
- [ ] Send repeated login POSTs from localhost; rate limiting should return HTTP 429.
- [ ] Inspect response headers in browser developer tools.
- [ ] Verify `.env` and `auth_lab.db` are not staged by Git.

Only mark a check as passed after you run it and observe the expected result.

## Configuration

| Variable | Purpose | Example |
|---|---|---|
| `SECRET_KEY` | Signs sessions and CSRF tokens | Random secret, at least 32 random bytes |
| `COOKIE_SECURE` | Enables Secure cookies | `0` for localhost HTTP; `1` behind HTTPS |
| `DATABASE_PATH` | SQLite database path | `auth_lab.db` |
| `RATELIMIT_STORAGE_URI` | Rate-limit storage backend | `memory://` for local learning |

The in-memory rate-limit store is suitable only for local practice. For a multi-process deployment, use a shared backend such as Redis. Enable `COOKIE_SECURE=1` only when serving over HTTPS.

## Project structure

```text
secure-authentication-lab/
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
├── templates/
│   ├── base.html
│   ├── login.html
│   ├── register.html
│   ├── dashboard.html
│   ├── 401.html
│   └── error.html
├── static/
│   └── css/
│       └── style.css
└── tests/
    ├── conftest.py
    └── test_auth.py
```

## Limitations and next steps

This project is intentionally small. Before using authentication in a real service, consider:

- Use a production WSGI server, HTTPS, and securely managed secrets.
- Configure persistent/shared rate-limit storage and monitor abuse.
- Add account recovery, email verification, MFA, and account lockout policies carefully.
- Add database migrations, structured audit logging, and dependency updates.
- Add integration tests for CSRF rejection, rate-limit behavior, cookie attributes, and session expiration.
- Consider Argon2id with a maintained library and tune its parameters for your deployment.
- Review the app against the current OWASP Authentication Cheat Sheet and Session Management Cheat Sheet.

## Responsible use

Test only this application and systems you own or have explicit permission to assess. Use made-up credentials for all demonstrations.

## License

MIT — add a `LICENSE` file if you want to distribute the project under the MIT License.
