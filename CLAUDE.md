# CLAUDE.md — AI Development Guardrails

This file defines how AI-assisted development should proceed in this project.
Read this before writing any code.

---

## Project Summary

A TikTok-style short-video MVP for internal testing.
Mobile-first. Local dev only. No production scale requirements.
Development proceeds in vertical slices, one feature at a time.

## Architecture Rules

### Vertical Slice Architecture

- Each feature (auth, upload, feed, likes, profile) is a self-contained slice under `src/features/<slice>/`.
- A slice owns its own components, server actions, and database queries.
- Do not create shared abstractions across slices until a pattern appears at least three times.
- Complete one slice end-to-end before starting the next.

### Slice Order (do not skip ahead)

1. auth
2. video-upload
3. feed
4. likes
5. profile

### No Premature Abstraction

- Do not extract a utility, hook, or helper until it is needed by a second slice.
- Prefer inline logic over wrapping layers.
- Three similar lines of code is better than a premature abstraction.

---

## Tech Stack

| Concern       | Choice                               |
| ------------- | ------------------------------------ |
| Framework     | Next.js 14+ (App Router, full-stack) |
| Language      | TypeScript (strict mode)             |
| Styling       | Tailwind CSS (mobile-first)          |
| Database      | SQLite via Prisma ORM                |
| Auth          | iron-session (cookie-based sessions) |
| Video storage | Local disk at `public/uploads/`      |

Do not introduce additional dependencies without a clear reason.

---

## Code Rules

### TypeScript

- Strict mode is on. No `any`. No `@ts-ignore` without a comment explaining why.
- Define types close to where they are used, not in a global types file.

### Server vs. Client

- Prefer React Server Components for data fetching and page layout.
- Use `"use client"` only when interactivity or browser APIs are required (e.g. video player, scroll behavior, file input).
- Use Next.js Server Actions for all mutations (form submissions, likes, uploads).

### API Routes

- Use API routes (`/api/...`) only for cases that cannot use Server Actions (e.g. streaming video files, non-form requests).
- Video files are served from `public/uploads/` directly — no API route needed for playback.

### Database

- All database access goes through Prisma.
- Write queries inside the feature slice that owns them (e.g. `src/features/feed/queries.ts`).
- Do not write raw SQL.

### Auth

- Session is managed via iron-session with a signed, HTTP-only cookie.
- Never expose the session secret in client code.
- Protect all non-public pages and mutations by checking the session at the top of the Server Component or Server Action.

### Styling

- Use Tailwind utility classes directly. Do not create CSS files unless absolutely necessary.
- Design for mobile screen widths first (375px base). Desktop is secondary.
- The feed uses full-screen snap-scroll: each video occupies 100dvh.

---

## Feature Constraints

### Video Upload

- Accepted format: MP4 only (validated client-side via file input `accept` attribute).
- Max duration: 60 seconds (validated client-side before upload via the HTMLVideoElement duration API).
- Max file size: 100 MB (validated client-side).
- Caption: optional, max 200 characters.
- Videos are saved to `public/uploads/<uuid>.mp4` on the server.
- Videos go live immediately after upload — no processing pipeline.

### Feed

- Shows videos from all users except the currently logged-in user.
- Ordered newest-first (reverse chronological).
- Paginated: 10 videos per batch, infinite scroll loads the next batch.
- Each video autoplays and loops.
- Only one video plays at a time (pause others when not in view).

### Auth / Registration

- Registration fields: date of birth, email, password.
- No username required at signup.
- Display name is derived from the email prefix (e.g. `samir` from `samir@example.com`).
- Passwords are hashed with bcrypt before storage.
- Session persists until explicit logout or session expiry.

### Likes

- A user can like or unlike any video in the feed.
- Like count is shown on each video card.
- Like state (did this user like this video?) is loaded at page/data fetch time — no real-time updates.

### Profile

- A user can only view their own profile in this MVP.
- Profile shows: derived display name, video count, and their uploaded videos (newest-first).

---

## Folder Structure

```
Tiktok-MVP/
├── PRODUCT.md
├── CLAUDE.md
├── package.json
├── tsconfig.json
├── tailwind.config.ts
├── next.config.ts
│
├── prisma/
│   └── schema.prisma          # User, Video, Like models
│
├── public/
│   └── uploads/               # Video files stored here (gitignored)
│
└── src/
    ├── app/                   # Next.js App Router pages
    │   ├── layout.tsx
    │   ├── (auth)/
    │   │   ├── login/page.tsx
    │   │   └── register/page.tsx
    │   └── (main)/
    │       ├── feed/page.tsx
    │       ├── upload/page.tsx
    │       └── profile/page.tsx
    │
    ├── components/            # Shared reusable UI components (Button, Input, etc.)
    │
    ├── features/              # Vertical slices — one folder per feature
    │   ├── auth/
    │   │   ├── components/
    │   │   ├── actions.ts
    │   │   ├── queries.ts
    │   │   └── session.ts
    │   ├── video-upload/
    │   │   ├── components/
    │   │   ├── actions.ts
    │   │   └── queries.ts
    │   ├── feed/
    │   │   ├── components/
    │   │   └── queries.ts
    │   ├── likes/
    │   │   ├── components/
    │   │   ├── actions.ts
    │   │   └── queries.ts
    │   └── profile/
    │       ├── components/
    │       └── queries.ts
    │
    └── lib/
        ├── db.ts              # Prisma client singleton
        └── session.ts         # iron-session cookie config (secret, name, TTL)
```

---

## What Not To Do

- Do not add comments, features, or refactors beyond the current slice.
- Do not add error handling for scenarios that cannot occur within the MVP scope.
- Do not add loading skeletons, animations, or visual polish unless the core feature works first.
- Do not add environment variables or config layers that are not immediately needed.
- Do not add tests unless asked.
- Do not use Docker or containerize anything for local dev.
- Do not implement: comments, follows, notifications, search, recommendations, moderation, or any other out-of-scope feature.
