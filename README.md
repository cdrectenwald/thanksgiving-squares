# Gather — Free Football Squares

GitHub Pages frontend with a Cloudflare Workers + SQLite-backed Durable Objects backend. No paid runtime dependencies. Cloudflare free-tier quotas apply; use the Workers Free plan.

## Shared pools

Create a shared pool, copy its private invite link, and send it to your guests. Up to **20 participants** can join using display names. A returning browser keeps its session; clearing browser data loses that identity. This is a 20-session limit, not verified real-world identity; someone using multiple browsers can occupy multiple spots. Display names must be unique within a pool.

Participants tap open squares to claim them and can release only their own squares. Server serialization enforces the participant cap and prevents conflicting claims. All games share square positions. Numbers stay hidden until the host locks picks; the server then randomizes each game's axes and prevents further claims. The host enters quarter scores manually. Guests see updates within about five seconds while the page is active.

Keep the **host access file** private. It restores host controls on another browser. The invite link grants joining access, not host permissions. Anyone it is forwarded to can join while space remains. Joined participants retain access through their saved sessions. Pools are unlisted; API reads require a participant or host session. No email verification, payments, automatic live scoring, or participant removal is implemented.

PNG downloads, printing, and board backups are available. Local boards remain available separately for random name assignment.

## Development and deployment

Frontend: static files at repository root, published from `main` through GitHub Pages. Set `window.GATHER_API` in `config.js` to the deployed Worker origin. This URL is public configuration; never put credentials here.

Backend:

```sh
npm ci
npx wrangler login
npm run dev
# With the local backend running, in a separate terminal:
npm test
# Deploy to your signed-in Cloudflare account:
npm run deploy
```

`wrangler.jsonc` declares the Durable Object migration and permitted browser origin. Credentials are managed by Wrangler and must not be committed. Durable Object state survives deployments; do not delete its namespace. Test pools are isolated by random IDs.

Original implementation inspired by football squares; not affiliated with Easy Office Pools.
