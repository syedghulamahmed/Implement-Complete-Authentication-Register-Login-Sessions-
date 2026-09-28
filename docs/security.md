# Authentication security notes

## Authentication vs authorization
Authentication establishes who the user is by verifying the access JWT. Authorization is separate: requireRole checks whether the authenticated user's role may access a route.

## Access vs refresh tokens
The access token is a signed JWT with a 15-minute default lifetime. It is returned to the browser and held in React memory. The refresh token is an opaque random secret stored server-side only as a SHA-256 hash and sent to the browser as an HttpOnly cookie.

## Storage choice
No authentication token is written to localStorage or sessionStorage. OWASP warns that web storage is readable by JavaScript and can expose authentication material to XSS. The refresh secret is HttpOnly, while the access token exists only in application memory.

## Three applied OWASP rules
1. **Strong password hashing:** bcrypt is used instead of plaintext or fast hashes. OWASP recommends slow password hashing such as Argon2id, bcrypt, or PBKDF2.
2. **Secure session cookie:** refresh cookies are HttpOnly and SameSite=Lax, with Secure enabled in production. These controls limit JavaScript access and cross-site transmission.
3. **Short-lived access credentials:** JWT access tokens expire after 15 minutes by default; refresh sessions have a separate seven-day lifetime and are revocable/rotated.

## Stateless vs stateful
JWT access-token validation is stateless. Refresh sessions are stateful because PostgreSQL tracks their hash, expiry, and revocation. This provides short-lived stateless API authorization with server-side control over long-lived sessions.

## Cookie vs localStorage tradeoff
HttpOnly prevents ordinary JavaScript from reading the refresh secret, reducing the value of an XSS payload for stealing that secret. SameSite=Lax limits cross-site cookie transmission. localStorage is convenient but directly readable by JavaScript, so this implementation avoids it for authentication tokens.

## Sensitive-data rules
- Passwords are never returned.
- Password hashes are never selected into auth responses.
- Raw refresh secrets are never logged or returned.
- JWT secrets and database credentials are environment variables.
- Logout revokes the refresh session server-side; client state clearing alone is not considered logout.

## Sources
- https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
- https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html
- https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie
