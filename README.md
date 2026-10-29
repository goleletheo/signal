# Aviator Signal Lab — Vercel / Next.js

A JavaScript-only Next.js dashboard (no Python) for **historical** Betway South Africa Aviator crash multiplier analysis, reproducible simulation, and signal auditing.

## Deploy to Vercel

1. Upload this folder's contents to a new GitHub repository (package.json must be at the repository root).
2. In https://vercel.com/new, import that repository.
3. Select **Next.js** and deploy. No environment variables required.
4. Vercel provides a URL once the deployment succeeds.

Alternatively run `npm install`, `npm run dev` locally or `npx vercel --prod` from the project folder.

## Betway data: current status

**No verified, authorized public Betway SA Aviator round-history API was identified. This application is NOT connected to live Betway.** Betway describes an in-game Stats/live feed. If you can lawfully export your own history, save it as a CSV with `time,multiplier` headers, e.g.:

```csv
time,multiplier
07:04:32,41.05
07:11:55,204.34
```

Upload via the dashboard. Times are interpreted as South Africa local time (SAST). For full signal accuracy testing, obtain complete round history (including losing rounds), not just screenshots of successful signals. Upload data remains in your browser memory only and disappears on refresh; no data is sent to a server.

## Probability model

For each simulated round, draw U uniformly in [0,1). If U < 0.03, outcome is 1.00x. Otherwise draw an independent V uniformly in (0,1), and return floor(100/V)/100. For t > 1, P(M >= t) is approximately 0.97/t (subject to cent rounding), yielding 19.4% for ≥5x. This is an illustrative inverse-tail distribution, **not** Betway's verified algorithm. The model's 97% RTP label describes the conventional threshold payout approximation; it does not imply the unbounded multiplier has a finite expectation.

**Simulation seeds are reproducibility inputs only, not Betway server seeds.** Future outcomes cannot be derived from previous multipliers or screenshots.

## Input format

`time,multiplier` or `timestamp,crash`; HH:MM:SS can be embedded in timestamps. CSV values should use decimal points. Signal windows use HH:MM/HH:MM or HH:MM/MM. Window endpoints are exclusive. All results must correspond to the same day as the signals for valid comparison; this version does not match dates.


## Betway screenshot update
The dashboard includes 25 multipliers transcribed from the supplied Betway screenshot (newest first). The screenshot does not expose timestamps for individual rounds. A separate verified screenshot shows round 7784605 at 21:36:31 with 55.10x.

The provably fair section computes SHA-256 for a revealed server seed to compare with a complete previously recorded commitment. It does not derive future results or claim to reproduce the game's SHA-512 multiplier conversion.

No Betway live API is connected. CSV upload accepts time,multiplier columns. Data stays in the browser and is not stored server-side.
