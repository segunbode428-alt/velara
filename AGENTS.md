# Velara — Base44 Dev Environment

## What this is
A static games/proxy website (Express + `@tomphttp/bare-server-node`). Serves static HTML/CSS/JS from `public/` and a bare proxy server at `/bare/`.

## Running it
- `docker compose -f docker-compose.base44.yml up -d` — starts the Node server on port 3000.
- The container runs `npm install --omit=dev && node index.js` with the source bind-mounted, so edits to `public/*` appear immediately (express.static reads from disk). Changes to `index.js` require a container restart (`docker compose -f docker-compose.base44.yml restart web`), then `reload_preview`.
- No database, no migrations, no seeds.

## Environment
- `PORT` is set to `3000` in compose. No external credentials are required to boot.
- `GROQ_API_KEY` exists in `.env` (empty) and is referenced in `public/scripts/ai.js`, but that file runs client-side where `process.env` is undefined — it's non-functional as-is and not needed for the site to work.

## Verifying
- `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/` → 200
- Routes: `/` (home), `/g` (games), `/a` (apps), `/s` (settings), `/!` (search), `/m` (media), `/404`.
