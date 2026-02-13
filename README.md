# Smart Education Platform

A full-stack education platform with role-based dashboards (student, teacher, admin), course/content management, progress tracking, and AI-powered support.

## Tech Stack
- **Frontend:** React (CRA), TailwindCSS
- **Backend:** Node.js, Express, MongoDB, JWT
- **Workspace:** npm workspaces (`frontend`, `backend`)

## Project Structure
- `frontend/` → React client
- `backend/` → Express API
- `package.json` (root) → workspace config

## Local Setup (Run on your machine)

### 1) Prerequisites
- Node.js `24.x`
- npm `10+`
- MongoDB Atlas (or local MongoDB)

### 2) Install dependencies
From repo root:

```bash
npm install
```

### 3) Configure environment variables
Create `backend/.env`:

```env
PORT=5000
MONGODB_URI=<your_mongodb_connection_string>
JWT_SECRET=<a_long_random_secret>
GEMINI_API_KEY=<optional_for_ai_features>
# Optional fallback when SRV DNS has issues:
# MONGODB_URI_DIRECT=<mongodb://...>
```

Create `frontend/.env` (optional for local):

```env
REACT_APP_API_URL=http://localhost:5000/api
```

> If `REACT_APP_API_URL` is not set, frontend defaults to `/api` and uses proxy behavior in local dev.

### 4) Start backend
```bash
npm run dev --workspace backend
```

Backend runs at: `http://localhost:5000`

### 5) Start frontend
In a second terminal:

```bash
npm start --workspace frontend
```

Frontend runs at: `http://localhost:3000`

---

## Production Build

### Frontend build
```bash
npm run build --workspace frontend
```

### Backend start (production mode)
```bash
npm run start --workspace backend
```

---

## Deployment Guide

## Option A: Deploy separately (recommended)

### Backend (Render / Railway / Cyclic)
1. Deploy `backend` as a Node service.
2. Set environment variables:
   - `PORT` (provided by host)
   - `MONGODB_URI`
   - `JWT_SECRET`
   - `GEMINI_API_KEY` (if using AI endpoints)
3. Start command:
   ```bash
   npm run start
   ```
4. Confirm health route:
   - `/api/health`

### Frontend (Vercel / Netlify)
1. Deploy `frontend` folder.
2. Set build command:
   ```bash
   npm run build
   ```
3. Set output directory:
   - `build`
4. Set env var:
   - `REACT_APP_API_URL=https://<your-backend-domain>/api`

## Option B: Same-domain proxy setup
If you serve frontend and backend behind the same domain, keep frontend API base as `/api` and proxy `/api/*` requests to backend.

---

## Common Run Issues & Fixes

- **`Missing MONGODB_URI environment variable`**
  - Add `MONGODB_URI` in `backend/.env`.

- **401 Unauthorized after login**
  - Ensure `JWT_SECRET` is set and same backend is serving login + protected routes.

- **Network error in frontend**
  - Verify backend is running and `REACT_APP_API_URL` points to correct API URL.

- **MongoDB SRV/DNS lookup error**
  - Add `MONGODB_URI_DIRECT` (non-SRV URI) in `backend/.env`.

---

## Student Chatbot Navigation Update
The student flow now prioritizes **AI Chatbot**:
- Sidebar student navigation includes AI Chatbot.
- Old `/student/doubt-support` path redirects to `/student/chatbot`.
- Student profile includes a direct “Open AI Chatbot” shortcut card.
