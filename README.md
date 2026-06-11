# Operation: Midnight Breach

A cinematic CTF murder mystery event built with React, Tailwind CSS, and Firebase.

## Tech Stack

- **Frontend:** React (Vite), Tailwind CSS v4
- **Backend:** Firebase Cloud Functions, Firestore, Anonymous Auth

## Getting Started

### 1. Install dependencies

```bash
npm install
cd functions && npm install && cd ..
```

### 2. Configure Firebase

Replace placeholder values in `src/lib/firebase.js` with your Firebase project config from the Firebase Console.

Set real puzzle flags in `functions/src/gameData.js` (server-side only — never expose flags in frontend code).

### 3. Deploy Firestore rules & functions

```bash
firebase login
firebase deploy --only firestore:rules,functions
```

### 4. Run locally

```bash
npm run dev
```

For local emulator testing, set `VITE_USE_FIREBASE_EMULATORS=true` in a `.env` file and run:

```bash
firebase emulators:start
```

## Firestore Schema — `teams/{uid}`

| Field | Type | Description |
|---|---|---|
| `currentPuzzle` | integer (0–9) | Current puzzle index |
| `strikes` | integer (0–3) | Wrong submission count |
| `startTime` | timestamp | Game start — drives 2-hour master timer |
| `puzzleStartTime` | timestamp | Puzzle start — drives 2-minute hint lock |
| `clearedSuspects` | string[] | Suspects crossed out in sidebar |

## Security

- Flag validation runs exclusively in Cloud Functions (`submitFlag`, `requestHint`).
- Firestore rules block direct client writes after team creation — all game state mutations go through Cloud Functions.
- Puzzle hints on the frontend are gated by the server-side hint timer check.

## Project Structure

```
src/
├── components/     # UI layout (Header, Console, Dossier, overlays)
├── constants/      # Shared game constants
├── data/           # Puzzle descriptions (no flags)
├── hooks/          # Game state & timer hooks
└── lib/            # Firebase init & team service
functions/
├── index.js        # Cloud Functions (submitFlag, requestHint)
└── src/gameData.js # Server-side flags & hints
```
