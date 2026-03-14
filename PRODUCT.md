# TikTok MVP — Product Definition

## Purpose

This project aims to build a simplified TikTok-style mobile-first web application for internal testing and experimentation.

The objective is to create a functional short-video application prototype that allows users to upload and browse short videos. The application is intentionally minimal and focuses only on the core experience.

The system will also serve as a practical environment for experimenting with AI-assisted development using vertical slice architecture.

---

# Target Users

Internal test users.

The application is not intended for public deployment and does not require production-grade scalability or security.

---

# Core User Experience

The product should replicate the essential experience of short-form video platforms:

- users create accounts
- users upload short videos
- users browse videos in a vertical mobile-first feed
- users can like or unlike videos
- users can view their own uploaded content in a profile page

The interface should be simple and optimized for mobile screens.

---

# Core Features (MVP)

The MVP includes the following capabilities:

1. **User Registration and Login**
   - users can create an account
   - users can log in
   - users remain authenticated while using the app

2. **Video Upload**
   - logged-in users can upload short videos
   - users can optionally add a caption

3. **Vertical Video Feed**
   - users can browse videos in a vertically scrolling feed
   - the feed is optimized for mobile viewing

4. **Like / Unlike Videos**
   - users can like videos
   - users can remove their like

5. **Basic Profile Page**
   - users can view their profile
   - the profile displays videos uploaded by that user

---

# Product Rules

These rules define important constraints of the MVP.

- Videos must be **60 seconds or shorter**
- Supported video format: **MP4**
- Captions are optional and limited to **200 characters**
- The video feed shows **videos from other users only**
- Feed ordering is **reverse chronological (newest first)**

---

# Out of Scope (Excluded Features)

The following features are intentionally excluded from the MVP:

- comments
- messaging
- following / followers
- live streaming
- video editing tools
- recommendation algorithms
- notifications
- ads
- moderation tools
- advanced search

These features may be considered in future iterations but are not part of this MVP.

---

# Slice Roadmap

Development will proceed using vertical slices in the following order:

1. Authentication
2. Video Upload
3. Video Feed
4. Like / Unlike
5. Profile Page

Each slice will deliver a working feature before the next slice begins.

---

# Success Criteria

The MVP is considered successful when an internal test user can complete the following flow:

1. Register a new account
2. Log in
3. Upload a short video with a caption
4. Browse videos in the vertical feed
5. Like and unlike videos
6. View their uploaded videos on the profile page

The system does not need to be optimized for performance, scale, or visual polish. The priority is a working end-to-end prototype.
