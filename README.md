# roulette-web

A lightweight, self-hosted spinning wheel you can customize with a plain text file. Pick a random winner for game nights, team assignments, giveaways, or anything else that needs a fair spin.

![Roulette Wheel](https://img.shields.io/badge/python-3.8+-blue) ![License](https://img.shields.io/badge/license-MIT-green)

## Features

- **Canvas-drawn wheel** with smooth spin animation and a fixed pointer at the top
- **Text-file configuration** — edit `segments.txt`, reload, and spin again
- **No build step** — vanilla HTML, CSS, and JavaScript
- **Comment support** in `segments.txt` (lines starting with `#` are ignored)
- **Dark UI** with segment preview and one-click reload

## Quick start

1. Clone the repository:

   ```bash
   git clone https://github.com/YOUR_USERNAME/roulette-web.git
   cd roulette-web
   ```

2. Edit `segments.txt` — one option per line:

   ```text
   Alice
   Bob
   Charlie
   # Lines starting with # are ignored
   ```

3. Start the local server:

   ```bash
   python3 server.py
   ```

4. Open [http://localhost:8000](http://localhost:8000) in your browser.

5. Click the wheel or press **Spin!** to pick a random segment.

> A local server is required because the app loads `segments.txt` over HTTP. Opening `index.html` directly in the browser (`file://`) will not work.

## Configuration

| File | Purpose |
|------|---------|
| `segments.txt` | Wheel options, one per line |
| `server.py` | Serves static files on port 8000 (change `PORT` to customize) |

After editing `segments.txt`, click **Reload from file** in the sidebar or refresh the page.

## Project structure

```
roulette-web/
├── index.html      # Wheel UI and spin logic
├── segments.txt    # Your wheel segments
├── server.py       # Minimal Python static file server
└── README.md
```

## How it works

1. On load, the app fetches `segments.txt` and draws a colored slice for each line.
2. When you spin, a random winning segment is chosen and the wheel animates to land on it.
3. The pointer stays fixed at the top; the wheel rotates underneath.

## Requirements

- Python 3.8+ (stdlib only — no pip dependencies)
- Any modern browser with Canvas support

## License

MIT — use it however you like.
