# TalentBridge — Implement Complete Authentication

NeuroFive Solutions — Week 3 Full Stack Web Development internship task.

## Scope

This repository is intentionally separate from the previous internship tasks.

- Student and company registration
- bcrypt password hashing
- JWT access tokens with 15-minute default expiry
- PostgreSQL refresh-token sessions with SHA-256 hashes
- Refresh-token rotation and server-side revocation
- HttpOnly/SameSite refresh cookie, Secure in production
- Authentication middleware and role-based authorization
- React AuthContext and silent refresh on app load
- Protected routes and login redirect-back
- Consistent error envelope
- No authentication tokens in browser storage
- Expired access-token verification script
- Authentication flow and security documentation

## Structure

backend/
  src/controllers/ HTTP handlers
  src/middleware/ authentication and errors
  src/routes/ auth and protected routes
  src/schemas/ Zod validation
  src/services/ JWT and refresh-session logic
  prisma/ schema, migration, seed
frontend/
  src/auth/ AuthContext
  src/components/ ProtectedRoute
  src/pages/ Login, Register, Dashboard
  src/services/ API client
docs/ security and flow notes

## Run locally

### Backend

Copy backend/.env.example to backend/.env, set DATABASE_URL and a strong JWT_ACCESS_SECRET, then:

npm install
npx prisma generate
npx prisma migrate deploy
npm run prisma:seed
npm run dev

API: http://localhost:4000

### Frontend

npm install
npm run dev

Frontend: http://localhost:5173

### Demo accounts

After seeding:
- student@example.com
- company@example.com

Both use DemoPass1! for local demonstration only.

## Verification

From backend:

npm run build
npm run verify:auth

The verification script signs a one-second JWT, waits for expiry, and confirms verification rejects it. Full endpoint integration requires a configured PostgreSQL instance.

## Security design

The short-lived access token is returned by login/refresh and kept in React memory. The longer-lived refresh secret is only held by the browser as an HttpOnly cookie and only its hash is persisted in PostgreSQL. This avoids persistent JavaScript-readable authentication tokens.

See docs/auth-flow.md, docs/security.md, and docs/api.md.

## Research references

OWASP Authentication Cheat Sheet
OWASP Password Storage Cheat Sheet
OWASP Session Management Cheat Sheet
MDN Set-Cookie and secure cookie guidance
