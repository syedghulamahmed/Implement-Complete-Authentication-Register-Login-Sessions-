# Week 3 verification report

## Static implementation checks

- Separate Week 3 repository: confirmed.
- Shared User authentication table with STUDENT and COMPANY roles: implemented.
- bcrypt password hashing: implemented.
- 15-minute default JWT access expiry: implemented.
- Refresh token stored only as a SHA-256 hash in PostgreSQL: implemented.
- HttpOnly refresh cookie: implemented.
- Secure cookie flag in production: implemented.
- SameSite=Lax: implemented.
- Refresh rotation: implemented.
- Server-side logout revocation: implemented.
- Auth middleware and role authorization: implemented.
- React AuthContext with silent refresh: implemented.
- ProtectedRoute and redirect-back state: implemented.
- No localStorage/sessionStorage authentication-token code: implemented.
- Password hash/raw refresh token excluded from auth responses: implemented.
- Expired JWT rejection script: implemented.
- API error envelope: implemented.

## Runtime verification

The repository includes:

- npm run build
- npm run verify:auth
- Prisma migration and seed commands

A live API/database test was not run in this environment because no PostgreSQL instance or project secrets are available here. Run the commands in README.md after configuring DATABASE_URL and JWT_ACCESS_SECRET.

## Manual acceptance checklist

1. Register a student and a company.
2. Confirm a 201 response contains a safe user and accessToken, but no password/hash/refresh secret.
3. Confirm the refresh cookie has HttpOnly and SameSite=Lax; use Secure when deployed over HTTPS.
4. Call /api/me with the access token and receive the authenticated user.
5. Wait for/force access-token expiry and confirm protected requests return 401.
6. Reload the React app and confirm silent refresh restores the session.
7. Call logout, then retry refresh with the previous cookie and confirm 401.
8. Try /api/company-only as a student and confirm 403.
9. Try an invalid password and confirm 401.
10. Try a duplicate email and confirm 409.
11. Try a weak registration password and confirm 422.
