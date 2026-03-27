# ♜ Hässelby Chess Coach

A chess coaching platform designed for Swedish school students. Play against AI opponents of varying difficulty, solve tactical puzzles, and receive real-time coaching feedback on every move - all running completely offline on school Chromebooks.

## Features

- **5 AI opponents**  from Peon (~200 ELO) to my attempt at a master (~2200 ELO), each with distinct playing styles
- **1,000+ tactical puzzles** - sourced from the Lichess database, filterable by difficulty and theme
- **Live coaching** - every move is analysed with feedback on tactics, positional play, and common mistakes
- **Pattern recognition** - detects forks, pins, skewers, discovered attacks, back rank threats, and 12+ tactical patterns
- **Opening identification** - recognises 45+ named openings and explains their ideas
- **Contextual lessons** - teaching popups triggered by in-game events, using language accessible to teenagers
- **Chess clock** - configurable time controls (bullet, blitz, rapid, or no clock)
- **Bilingual** - full English and Swedish language support
- **Board themes** - Classic, Nordic, Forest, and Midnight colour schemes
- **2-player mode** - local play on the same device
- **Fully offline** - zero network dependencies, runs from a single HTML file

## Quick Start

### Option A: Open directly (simplest)

1. Download `index.html` from this repository
2. Open it in Chrome on any computer or Chromebook
3. Done - no installation, no server, no internet required

### Option B: Deploy to GitHub Pages (school-wide access)

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch**, select `main`, folder `/ (root)`
4. Click **Save**
5. Your site will be live at `https://<username>.github.io/<repo-name>/`
6. Share the URL with students - it works on any device with a modern browser

### Option C: Deploy on school Chromebooks (kiosk mode)

1. Download `index.html`
2. Place it on a shared drive or distribute via your school's device management
3. Set the Chromebook kiosk URL to the local file path, or host on an internal web server
4. No internet access required after deployment

## System Requirements

| Requirement | Minimum |
|---|---|
| Browser | Chrome 90+, Edge 90+, Firefox 95+, Safari 15+ |
| Device | Any computer, Chromebook, tablet, or phone |
| Internet | Not required (fully offline) |
| Storage | ~1 MB (single HTML file) |

## How It Works

The entire application is a single self-contained HTML file. All game logic, AI computation, coaching analysis, puzzle data, sound effects, and UI rendering happen locally in the browser. There are no external API calls, no CDN dependencies, and no cookies or tracking.

Students select a mode (Play vs Bot, Puzzles, or 2-Player), and the coaching engine provides real-time feedback on every move. The AI opponents range from nearly random (Peon) to a full alpha-beta search engine with positional evaluation (Change the name to the person in your school who handles chess club). 
Puzzles are sourced from the Lichess open database and cover a wide range of tactical themes and difficulty levels.

## Project Structure

```
index.html                    The complete application (deploy this file)
chess-coach-v5.jsx            Development source (React JSX)
chess-platform-roadmap.docx   Development roadmap (Phases 1–6)
lichess_db_puzzle.csv/        Puzzle generation tools
  ├── lichess-puzzle-filter.js    Filter puzzles from Lichess CSV
  ├── inject-puzzles.js           Inject puzzles into the HTML
  └── README-puzzle-upgrade.md    Puzzle toolchain documentation
```

## Regenerating Puzzles

To update the puzzle set with fresh puzzles from the Lichess database:

1. Download the puzzle CSV from [database.lichess.org/#puzzles](https://database.lichess.org/#puzzles)
2. Decompress it (`zstd -d` or `bzip2 -d` or use 7-Zip)
3. Run the filter: `node lichess-puzzle-filter.js --csv lichess_db_puzzle.csv --output puzzles.json`
4. Inject into the app: `node inject-puzzles.js --json puzzles.json --html index.html --output index-upgraded.html`

See `lichess_db_puzzle.csv/README-puzzle-upgrade.md` for full options and troubleshooting.

## Licence

The Lichess puzzle data is licensed under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public domain).

## Acknowledgements

- [Lichess](https://lichess.org) - open puzzle database (CC0)
- [React](https://react.dev) - UI framework
- Built for the students of Hässelby, Stockholm
