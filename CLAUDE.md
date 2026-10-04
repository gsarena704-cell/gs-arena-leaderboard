# GS Arena — Free Fire Clash Squad Tournament Tracker

## Project Overview
A **live tournament tracker** web app for Free Fire Clash Squad (4v4) community tournaments.
Built as a single-page HTML + vanilla JS app backed by **Firebase Realtime Database** for live sync.

## Architecture
- **index.html** — The main tournament tracker (Firebase-backed, production)
  - Group stage standings (4 groups × 4 teams, round-robin)
  - Match breakdown with per-player kills/damage stats (M1–M6 per group)
  - Player stats leaderboard (kills, damage, MVP)
  - Knockout bracket (QF → SF → Final + 3rd place)
  - Final match stats tab
  - Admin panel (protected by Firebase Authentication) for scoreentry
- **live-leaderboard-v2.html** — Older version using localStorage-only, no Firebase

## Tech Stack
- HTML/CSS/JS — single file, no build step
- Firebase Realtime Database (compat SDK v10.7.1, loaded from CDN)
- No frameworks; vanilla DOM manipulation

## Key Modules (all in `<script>` inside index.html)
| Module | Purpose |
|--------|---------|
| `FIREBASE_CONFIG` | Firebase project credentials |
| `DB` | Firebase write + localStorage backup |
| `AppState` | In-memory tournament state |
| `Renderer` | All UI rendering (groups, matches, players, knockout, final stats) |
| `UI` | Navigation, modals, toasts |
| `Admin` | Auth, team registration, match entry, knockout management |

## Tournament Format
- **Group Stage**: 4 groups (A–D), 4 teams each, round-robin (6 matches/group, 3 per team)
  - Win = +1 point. Matches are Best-of-7 (first to 4 rounds).
- **Knockout**: QF (A1vB2, C1vD2, B1vA2, D1vC2) → SF → Final (Best-of-11, first to 6) + 3rd Place
- 16 participating teams are hardcoded in `TOURNAMENT_PARTICIPATING_TEAMS`

## Security & Auth
- **Firebase Auth (`signInWithEmailAndPassword`)** is used to access the Admin Panel.
- **Firebase Realtime Database Rules** require authentication for writes: `".write": "auth != null"`.
- Never commit the `.env` file containing the Firebase keys.

## Common Tasks
- **Add/edit match scores**: Admin panel → Match Stats tab → select group → click match → enter winner, rounds, player kills/damage
- **Knockout results**: Admin panel → Match Stats → Knockout Stage section → select stage
- **Reset tournament**: Admin panel → Register Teams → danger zone at bottom

## Style Conventions
- Dark theme with CSS custom properties (--bg-main, --accent-blue, etc.)
- All CSS is inline in `<style>` block at top of HTML
- All JS is inline in `<script>` block at bottom of HTML
- No external CSS/JS files
- Naming: PascalCase for modules (`Renderer`, `Admin`, `UI`), camelCase for methods/vars
