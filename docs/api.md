# Authentication API

All endpoints are under /api/auth.

| Method | Endpoint | Success |
|---|---|---|
| POST | /register | 201 with safe user + accessToken + refresh cookie |
| POST | /login | 200 with safe user + accessToken + refresh cookie |
| POST | /refresh | 200 with safe user + new accessToken + rotated refresh cookie |
| POST | /logout | 204 after server-side revocation |

Errors use:
{ "error": { "code": "...", "message": "...", "details": {} } }

Protected endpoints use Authorization: Bearer <accessToken>.

The refresh cookie is HttpOnly, SameSite=Lax, scoped to /api/auth, and Secure in production.
