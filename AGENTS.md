# Velara — Base44 Dev Notes

## What this is
A Node.js Express static-file server (unblocked games/proxy site) powered by Ultraviolet + a Bare server. It serves static HTML/CSS/JS from `public/` and routes a few paths (`/g`, `/a`, `/s`, `/m`, `/!`, `/404`) to specific HTML files. A `@tomphttp/bare-server-node` instance is mounted at `/bare/`.

## Run
`docker compose -f docker-compose.base44.yml up -d` — uses `node:22`, bind-mounts the repo, runs `npm install && node index.js`, exposes port 3000 (via `PORT=3000` env).

## Live reload
There is **no** framework dev server / HMR. `node index.js` serves static files directly via `express.static`, so edits to files under `public/` appear on browser refresh with no restart. Edits to `index.js` (server logic) require a service restart: `docker compose -f docker-compose.base44.yml restart web`, then `reload_preview`.

## Credentials
None required to boot. `GROQ_API_KEY` (in `.env`) is referenced only by `public/scripts/ai.js`, an optional frontend AI feature; it is not needed for the server to start and is not wired into the compose service. No external services are called at startup.

## Verify
`curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/` → expect `200`, and the served HTML is the cloned `public/index.html`.
