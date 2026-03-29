# Next.js Authentication

A production-style authentication demo built with Next.js App Router, Lucia, and SQLite.  
The app supports sign up, login, logout, cookie-based sessions, and route protection for authenticated pages.

## Tech Stack

- Next.js 14 (App Router)
- React 18
- Lucia v3
- SQLite (`better-sqlite3`)
- `@lucia-auth/adapter-sqlite`
- Node.js built-in `crypto` for password hashing (`scrypt`)

## Features

- Email/password sign up with server-side validation
- Login with credential verification
- Secure password storage using salted hash + timing-safe comparison
- Persistent auth session via HTTP cookies
- Logout flow with server action + session invalidation
- Protected training page (`/training`) with redirect for unauthenticated users
- Route group layout for authenticated UI shell
- Global app-level error boundary page

## Project Structure

```text
.
├─ actions/
│  └─ auth-actions.js          # server actions: signup, login, logout
├─ app/
│  ├─ (auth)/
│  │  ├─ layout.js             # authenticated layout + logout button
│  │  └─ training/page.js      # protected page
│  ├─ error.js                 # global error UI
│  ├─ layout.js                # root layout
│  └─ page.js                  # auth entry page
├─ components/
│  └─ auth-form.js             # login/signup form (client component)
├─ lib/
│  ├─ auth.js                  # Lucia setup + session helpers
│  ├─ db.js                    # SQLite schema + seed data
│  ├─ hash.js                  # password hashing/verification
│  ├─ training.js              # training data access
│  └─ user.js                  # user data access
└─ training.db                 # local SQLite database file
```

## How Authentication Works

1. User submits login or signup form from `/`.
2. `auth-actions.js` validates credentials and either creates user or verifies password.
3. On success, `createAuthSession()` creates a Lucia session and sets the session cookie.
4. Protected routes call `verifyAuth()` to validate the cookie/session.
5. Logout calls `destroySession()` to invalidate session and clear cookie.

## Getting Started

### Prerequisites

- Node.js 18.17+ (or Node.js 20+ recommended)
- npm

### Installation

```bash
npm install
```

### Run in Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### Production Build

```bash
npm run build
npm run start
```

## Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run lint` - Run ESLint checks

## Database Notes

- SQLite database file: `training.db`
- Tables are created automatically on startup (`users`, `sessions`, `trainings`)
- Trainings are seeded automatically if the table is empty
