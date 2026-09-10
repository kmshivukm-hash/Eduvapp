# NCERT Interactive Quiz Bank

Everything in one folder: an Express + JWT backend and a single-file HTML/JS frontend. No build step, no React/Vite setup required.

```
ncert-quiz/
├── package.json
├── .env.example
├── server.js
├── ncertData.js
└── public/
    └── index.html
```

## Run it

```bash
npm install
cp .env.example .env   # set a real, random JWT_SECRET
npm start
```

Then open http://localhost:5000 — the same server serves both the frontend and the API, so there's nothing else to configure.

## Login

This uses **email + password** login (not "Sign in with Google"). Real Google/Gmail sign-in requires registering an OAuth app with Google Cloud Console and a redirect flow — a fair bit more setup than fits a starter project. Email/password with hashed storage below is the simpler, still-secure path; happy to add Google OAuth on top later if you want it.

## Security measures included

- **Passwords are hashed with bcrypt** — never stored or compared as plain text.
- **JWT_SECRET comes from your `.env` file**, not hardcoded in the source.
- **Helmet** sets standard security-related HTTP headers.
- **Rate limiting** on `/api/login` and `/api/register` (10 attempts per 15 minutes per IP) to slow down brute-force guessing.
- **Basic input validation**: valid email format required, password must be at least 8 characters.
- Quiz answers are never sent to the browser — `/api/questions` strips the correct answer, and grading happens server-side in `/api/submit`.

## Still worth doing before real students use this

- **Persistent storage.** Users currently live in a JavaScript array in memory — restarting the server deletes every account. Swap in Postgres, MongoDB, or Supabase for anything real.
- **HTTPS in production** (Render, Railway, etc. give you this automatically) so passwords and tokens never travel in plaintext.
- **Email verification** if you want to stop people registering with emails they don't own.
