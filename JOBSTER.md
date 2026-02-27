# Jobster API (Backend)

Backend for the Jobster app. Provides user auth, job tracking APIs, and serves the prebuilt React client from `client/build`.

## Tech Stack
- Node.js + Express
- MongoDB + Mongoose
- JWT auth (`Authorization: Bearer <token>`)
- Security: helmet, xss-clean
- Rate limiting on auth routes

## Setup
1) Install deps  
   ```sh
   npm install
   ```
2) Create `.env` with:
   ```
   MONGO_URI=your-mongodb-connection-string
   JWT_SECRET=your-secret
   JWT_LIFETIME=30d
   ```
3) Start the server  
   ```sh
   npm run dev   # nodemon
   # or
   npm start
   ```
   Runs on `PORT` or `5000`.

## API Overview
- Auth
  - `POST /api/v1/auth/register` – create user
  - `POST /api/v1/auth/login` – get JWT
  - `PATCH /api/v1/auth/updateUser` – update profile (JWT required)
- Jobs (JWT required)
  - `GET /api/v1/jobs` – list (filters: `search`, `status`, `jobType`; sorting; pagination)
  - `POST /api/v1/jobs` – create
  - `GET /api/v1/jobs/:id` – get one
  - `PATCH /api/v1/jobs/:id` – update
  - `DELETE /api/v1/jobs/:id` – delete
  - `GET /api/v1/jobs/stats` – status counts + 6‑month timeline

## Models
- User: `name`, `email` (unique), `password` (hashed), `lastName`, `location`
- Job: `company`, `position`, `status` (`interview|declined|pending`), `jobType` (`full-time|part-time|remote|internship`), `jobLocation`, `createdBy`

## Frontend
- React client already built; Express serves static assets from `client/build`.
- Non-API routes fall back to `index.html`.

## Mock Data
- Seed sample jobs with `node populate.js` (uses `mock-data.json` and your `.env` connection).

## Notes
- Requests from the configured test user (`userId === 6960305544d804e588f40174`) are read-only.
- Auth routes are rate-limited (10 requests per 15 minutes per IP).
