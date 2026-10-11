# Apex Grid Manager

Apex Grid Manager is an original, fan-made motorsport team management game. It is not affiliated with Formula 1, the FIA, or any constructor, driver, or racing series. The roster uses the 2026 F1 driver, team, and principal names; ratings and race outcomes are simulated for this independent fan-made game. The app does not represent official F1 ratings or results.

## Requirements

- Node.js 22.5 or newer (no package install required; SQLite is included in Node.js)

## Run locally

```sh
cp .env.example .env
npm start
```

Open [http://localhost:3000](http://localhost:3000). The app creates `data/game.sqlite` on first start. League picks, team assignments, driver progression, and completed season records are stored in SQLite. To use another port or data location, set `PORT` and `DATA_FILE` in your environment. `npm run dev` restarts the server when source files change.

## Play

On a new save, choose a constructor, then a team principal, then any two real 2026 drivers. Start the season from the dashboard. Each seeded season has qualifying results plus a separately randomized 22-car starting grid; the same seed reproduces the same grid and race results. Open any round for the full classification and race-week summary. My team shows your chosen lineup and constructor strengths; Calendar, Standings, and History show the season results.

The private local league has one human manager and three computer managers. Picks use a snake order; the opposing managers automatically make their picks between human turns.

## API

- `GET /api/state` — current roster, calendar, league, assignments, and saved seasons
- `POST /api/draft/pick` — `{ "memberId":"you", "driverId":"driver-1" }`; alternatively use `constructorId` or `principalId`
- `POST /api/draft/reset` — clear draft picks
- `POST /api/team/select` — `{ "teamId":"team-1", "principalId":"principal-1", "driverIds":["driver-1","driver-2"] }`; locks the initial lineup
- `POST /api/seasons/simulate` — `{ "seed":2026 }`; requires a completed team selection

Requests are JSON. Invalid payloads return a descriptive JSON error and an appropriate 4xx status.

## Tests

```sh
npm test
```

The suite checks repeatable seeded results, race and championship scoring, draft uniqueness and snake order, API validation, API season generation, and disk persistence.

## Deployment

Run `npm start` on a Node.js 22.5+ service and set `PORT` as required by the host. Attach persistent storage for `DATA_FILE` (the default is `./data/game.sqlite`); SQLite is intended for a single app process. For multi-instance deployments, use a shared database service and adapt `lib/store.js` while retaining the API and game functions. Put a TLS-terminating reverse proxy in front of the app for public access. No secrets or third-party credentials are required.
