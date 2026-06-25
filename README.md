# RC Race Scoreboard

A self-contained live scoring overlay for RC racing powered by LiveRC.

---

## Option 1 — Docker (recommended for cloud servers)

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/) installed

### Run with Docker Compose

```bash
docker compose up
```

Open **http://localhost:3000** in your browser.

To run in the background:

```bash
docker compose up -d
```

To stop:

```bash
docker compose down
```

### Custom port

Edit `docker-compose.yml` and change `"3000:3000"` to e.g. `"8080:3000"`.

---

## Option 2 — Node.js (no Docker)

### Prerequisites
- [Node.js 18 or later](https://nodejs.org)

```bash
npm start
```

Custom port:

```bash
PORT=8080 npm start
```

---

## Option 3 — Deploy to Fly.io (cloud hosting)

### Prerequisites
- A free [Fly.io account](https://fly.io)
- [Fly CLI installed](https://fly.io/docs/hands-on/install-flyctl/)

### Steps

```bash
# 1. Log in
fly auth login

# 2. Create the app and deploy (run once, inside this folder)
#    When prompted, choose a unique app name — it becomes your URL.
#    Select the region closest to your race track.
fly launch

# 3. Future updates
fly deploy
```

> **Note:** After `fly launch` edit `fly.toml` and replace `app = "rc-scoreboard"`
> with the name you chose, then run `fly deploy`.

Your scoreboard will be live at **https://YOUR-APP-NAME.fly.dev**

> The server is kept running at all times (required for the LiveRC relay
> to maintain its live connection). This uses one shared-CPU machine
> (~$2–3/month on Fly.io's pay-as-you-go plan).

---

## Connecting to a Track

Click the track name in the top-left corner and enter the LiveRC URL slug.

**Example:** for `insanityracetrack.liverc.com` the slug is `insanityracetrack`.

Pre-configure via URL:

```
http://localhost:3000/?track=yourtrackslug
```

## OBS / Streaming Setup

Add a **Browser Source** in OBS:
- URL: `http://localhost:3000`
- Width: `1920`, Height: `1080`

## Track Config Persistence

The selected track is saved to `track.json` next to the server.
With Docker Compose this file is volume-mounted so it survives restarts.

## Switch Track via API

```bash
curl -X POST http://localhost:3000/api/config \
     -H "Content-Type: application/json" \
     -d '{"trackUrlId":"yourtrackname"}'
```
