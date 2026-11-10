# ♜ School Chess Coach

A chess coaching platform designed for Swedish school students. Play against AI opponents of varying difficulty, solve tactical puzzles, and receive real-time coaching feedback on every move - all running completely offline on school Chromebooks.

## Features

- **Home hub** - one clean screen per mode instead of everything at once; larger, readable text throughout
- **5 AI opponents** - from Peon (~200 ELO) to my attempt at a master (~2200 ELO), chosen on a setup screen before the game. Bots think in the background, so the page never freezes
- **Drag or click** - move pieces by dragging (mouse or touch) or by clicking the piece and then the square
- **Game report** - after each game: moves without mistakes, the best move and one to learn from, and buttons straight to the right trainer
- **Live coaching** - every move is analysed with feedback on tactics (forks, pins, skewers, …), positional play, and common mistakes, in English or Swedish
- **Chess clock for real boards** - full-screen clock to put between two students (see below)
- **1,000+ tactical puzzles** - sourced from the Lichess database, filterable by difficulty and theme; alternative checkmates are accepted
- **Puzzle Rush** - solve as many puzzles as possible in 3 or 5 minutes (or survival mode); difficulty rises as you go
- **Tactics academy** - 10 tactical patterns with practice positions (several positions corrected in v6)
- **Openings & counters** - 24 openings and traps, each with a plan, move-by-move notes, a counter-line showing how to beat it, and a "play it from memory" drill with star ratings
- **Arcade** - Square Sprint (board coordinates) and Knight Quest (18 knight-movement levels)
- **Ranks & XP** - students earn XP and climb from Pawn to Grandmaster; progress is stored only in the browser on that device
- **Fun facts** - 38 chess facts in English and Swedish on the home screen and the clock
- **Bilingual** - full English and Swedish language support, including Swedish piece letters (S, L, T, D)
- **Board themes** - Classic, Ocean and Emerald colour schemes
- **Fully offline** - zero network dependencies, runs from a single HTML file

## How hard are the bots?

Measured with the app's own engine (400 games per pairing; Teacher BOT vs Blademaster 17 games):

| Bot | Plays like | Beats the bot below it |
|---|---|---|
| 🐣 Peon (~200) | Mostly random — gives away a piece every ~15 moves | — |
| ⚔️ Grunt (~600) | Grabs material without checking recaptures | 98% vs Peon |
| 🐺 Raider (~1000) | Avoids obvious hanging pieces; gives material away every ~27 moves | 76% vs Grunt |
| 🗡️ Blademaster (~1400) | Never leaves a piece hanging, but only looks one move ahead — forks and two-move tactics beat it | 87% vs Raider |
| 💀 Teacher BOT (~2200) | Searches 5+ moves ahead, thinks ~3–4 s per move (in the background) | 17 of 17 vs Blademaster |

There is a deliberate jump before Teacher BOT, the "boss". A good path for students: beat Raider, then Blademaster (look for forks), then try Teacher BOT.

## Chess Clock

Open **Chess clock** from the home screen and choose a time control (1+0 up to 45+15, or custom). Each player can have their own time, which is useful for giving a stronger player less time.

| Key | Action |
|---|---|
| Space | End your turn (or tap your own side of the screen) |
| A / L | Left / right player's own key - only works on that player's turn |
| P | Pause (declare a result or adjust time from the pause menu) |
| F | Full screen |

Extras: hourglass mode (time you spend is added to your opponent), a "lying flat" layout that turns each half towards its player, low-time beeps, fun facts for the waiting player, and move statistics at the end. The screen is kept awake while the clock runs where the browser supports it.

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

The entire application is a single self-contained HTML file. All game logic, AI computation, coaching analysis, puzzle data, sound effects, and UI rendering happen locally in the browser. There are no external API calls, no CDN dependencies, and no cookies or tracking. XP, stars, language and clock settings are saved in the browser's local storage on that device only, and can be reset from the ⚙ menu.

Students select a mode (Play vs Bot, Puzzles, or 2-Player), and the coaching engine provides real-time feedback on every move. The AI opponents range from nearly random (Peon) to a full alpha-beta search engine with positional evaluation (Change the name to the person in your school who handles chess club). 
Puzzles are sourced from the Lichess open database and cover a wide range of tactical themes and difficulty levels.

## Project Structure

```
index.html                    The complete application (deploy this file — it is built, don't edit it by hand)
source/                       Source code: edit here, then build with source/build.html (see source/README.md)
chess-coach-v5.jsx            Old v5 source — archive only, does not contain the v6 changes
chess-platform-roadmap.docx   Development roadmap (Phases 1–6)
lichess_db_puzzle.csv/        Puzzle generation tools
  ├── lichess-puzzle-filter.js    Filter puzzles from Lichess CSV
  ├── inject-puzzles.js           (v5 only — use source/scripts/import-puzzles.js instead)
  └── README-puzzle-upgrade.md    Puzzle toolchain documentation
```

## Making changes

Everything is edited in `source/` and built into `index.html` — **nothing to install**:

- **Try changes:** open `source/index.dev.html` in Chrome (runs straight from the source files).
- **Build:** open `source/build.html` in Chrome, choose the `source` folder, download the new `index.html` and replace this one. Or, with Node: `node source/tools/build.js --out index.html`.

Every build first replays all openings, puzzles and lessons through the chess engine and refuses to build if something is wrong. `source/README.md` explains where things live and has step-by-step recipes.

## Regenerating Puzzles

1. Download the puzzle CSV from [database.lichess.org/#puzzles](https://database.lichess.org/#puzzles)
2. Decompress it (`zstd -d` or `bzip2 -d` or use 7-Zip)
3. Run the filter: `node lichess-puzzle-filter.js --csv lichess_db_puzzle.csv --output puzzles.json`
4. Import into the source: `node source/tools/import-puzzles.js puzzles.json`
5. Rebuild with `source/build.html` or `node source/tools/build.js --out index.html`

See `lichess_db_puzzle.csv/README-puzzle-upgrade.md` for filter options and troubleshooting.

## Licence

The Lichess puzzle data is licensed under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public domain).

## Acknowledgements

- [Lichess](https://lichess.org) - open puzzle database (CC0)
- [React](https://react.dev) - UI framework
- Built for the students of School, Stockholm
