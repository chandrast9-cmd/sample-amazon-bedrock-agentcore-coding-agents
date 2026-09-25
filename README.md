# Meadow Drift — A Pleasant Survivor

A compact browser game where you guide a bunny through a sunny meadow, collecting wildflowers while dodging drifting rain clouds.

## How to Play

- **Arrow keys** or **WASD** to move your bunny
- Collect **wildflowers** scattered across the meadow to earn points
- Avoid **rain clouds** that drift across the field
- You have **3 lives** — touching a cloud costs one life (with brief invincibility after)
- The game ends when all lives are lost
- Enter your name to save your score to the persistent high-score table

## How Scoring Works

Each flower collected awards **10 + (difficulty x 2)** points. Difficulty increases by 1 every 10 seconds of survival, so flowers become more valuable the longer you survive. Clouds also spawn faster and move quicker at higher difficulty, making later flowers harder to reach. Your final score reflects both collection skill and survival time.

## Running the Game

### Prerequisites

- Node.js 22+

### Setup and Start

```bash
npm install
npm start
```

The server starts on the port specified by the `PORT` environment variable, defaulting to **3000**:

```bash
PORT=8080 npm start
```

Then open `http://localhost:<port>/` in your browser.

The server stays in the foreground and serves both the game page and the score API.

### Score Persistence

Scores are stored in a SQLite database (`scores.db` in the project root, configurable via `DB_PATH` env var). The database file persists across server restarts.

## API

| Method | Path          | Description                        |
|--------|---------------|------------------------------------|
| GET    | `/api/scores` | Returns top 20 scores as JSON      |
| POST   | `/api/scores` | Submit a score `{ name, score }`   |

### POST /api/scores

**Request body:**
```json
{ "name": "Alice", "score": 420 }
```

- `name`: non-empty string, max 30 characters
- `score`: integer between 0 and 99999

**Success:** 201 with `{ id, name, score }`
**Error:** 400 with `{ error: "..." }`

## Hosting Notes

- All URLs in the game page are relative — works behind a reverse proxy with a path prefix
- No cookies, localStorage, or external resources required
- CORS is enabled for cross-origin API access
- The game renders on an HTML5 canvas with no external dependencies
- Form submission is handled via JavaScript fetch with preventDefault()
- No popups, new tabs, or browser dialogs are used
- Pointer and keyboard controls work within the canvas

## Creative Direction

**Pleasant survivor** — the game aims for a warm, gentle atmosphere despite the survival mechanic. Soft pastel colors (sky blues, meadow greens, warm creams), a cute bunny character with rosy cheeks, bobbing wildflowers, and swaying grass create a cozy world. Rain clouds are the hazard but feel natural rather than hostile. The difficulty ramp is gradual, letting players enjoy the meadow before the challenge intensifies.
