# Trailerio Lite

Minimal movie trailer addon for Fusion. Runs on Cloudflare Workers (free tier).

## What it does

Finds the best available trailer for any movie/series by checking multiple sources in order:

1. **Apple TV** - 4K HDR HLS streams
2. **Plex** - 1080p from IVA CDN
3. **Rotten Tomatoes** - 1080p from Fandango CDN
4. **Digital Digest** - 4K from PeerTube
5. **IMDb** - 1080p fallback

Returns the first successful match. Results are cached for 24 hours.

## Features

- **Zero cost** - Runs on Cloudflare Workers free tier (100k requests/day)
- **Zero storage** - Uses edge caching, no database needed
- **Global** - Deployed to 300+ edge locations worldwide
- **Fast** - Returns cached results instantly, fresh lookups in 2-8 seconds

## Deploy to Cloudflare Workers

### Option 1: Dashboard (Easiest)

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com)
2. Click **Workers & Pages** → **Create application** → **Create Worker**
3. Name it (e.g., `trailers`)
4. Click **Deploy**
5. Click **Edit code**
6. Delete everything, paste contents of `src/index.js`
7. Click **Save and deploy**

Your addon URL: `https://trailers.YOUR-SUBDOMAIN.workers.dev/manifest.json`

### Option 2: Git Integration

1. Fork this repo
2. Go to Cloudflare Dashboard → **Workers & Pages**
3. Click **Create application** → **Connect to Git**
4. Select your forked repo
5. Deploy

Auto-deploys on every push.

### Option 3: Wrangler CLI

```bash
git clone https://github.com/9mousaa/trailerio-lite.git
cd trailerio-lite
npm install
npx wrangler login
npm run deploy
```

## Add to Fusion

1. Open Fusion
2. Go to **Addons**
3. Enter your addon URL:
   ```
   https://YOUR-WORKER.workers.dev/manifest.json
   ```
4. Install

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `/manifest.json` | Addon manifest |
| `/stream/movie/{imdbId}.json` | Get movie trailer |
| `/stream/series/{imdbId}.json` | Get series trailer |
| `/health` | Health check |

## Example

Request:
```
GET /stream/movie/tt15398776.json
```

Response:
```json
{
  "streams": [{
    "url": "https://play.itunes.apple.com/.../playlist.m3u8",
    "name": "▶️ 4K",
    "title": "Trailer (Apple TV)"
  }]
}
```

## Limits

Cloudflare Workers Free Tier:
- 100,000 requests/day
- 10ms CPU time per request

For higher limits, Workers Paid is $5/month for 10 million requests.

## License

MIT
