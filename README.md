# NFL Score Ticker for OBS

A slim, scrolling NFL score ticker built for OBS browser sources. It pulls scores from ESPN's public scoreboard feed and updates on its own.

- Shows the whole week: live games first (clock, down and distance, possession, red zone highlight), then finals, then upcoming kickoffs
- Keeps last week's finals up until the new week's first kickoff (skips the Pro Bowl; preseason results never carry into Week 1)
- Team color stripes and logos, transparent background
- Scores update in place without interrupting the scroll
- Retries quickly and switches to a mirror host if ESPN rejects a request; the badge turns amber and reads **Delayed** if scores haven't updated for 2 minutes
- The bar hides itself when there's nothing to show

## Setup in OBS

1. Add a **Browser** source.
2. Point it at the ticker:
   - **Hosted:** `https://azborek.github.io/nfl-obs-ticker/` (options work here too, e.g. `/?team=KC`)
   - **Local:** `file:///C:/path/to/nfl-ticker.html` (uncheck Local file so URL options work)
3. Set width to your canvas width (e.g. `1920`) and height to `40`.
4. Leave **Shutdown source when not visible** unchecked.

## Options

Add these to the end of the URL, for example `nfl-ticker.html?team=KC&speed=90`.

| Param | What it does | Default |
|---|---|---|
| `speed` | Scroll speed in px per second | `70` |
| `refresh` | Seconds between score updates | `20` |
| `height` | Bar height in px (everything scales) | `40` |
| `team` | Highlight team(s), e.g. `KC,DAL` | none |
| `only=1` | Show only the highlighted teams' games | off |
| `logos=0` | Hide team logos | on |
| `bg` | Bar opacity, `0` to `1` | `0.94` |

If you change `height`, match the browser source height in OBS.

## Troubleshooting

- **Bar never appears, or stays on "Delayed":** ESPN is rejecting requests from your network. Right-click the source, choose **Interact**, and check for errors, or click **Refresh cache of current page** in the source properties. Opening the file from a local server (`python -m http.server 8000`) also helps if your browser blocks requests from local files.
- **No logos:** your network may be blocking ESPN's image server.

## Testing

These options are for checking behavior outside game time. They do nothing unless they're in the URL.

| Param | What it does |
|---|---|
| `seasontype`, `week`, `year` | Load a specific week, e.g. `?seasontype=3&week=4&year=2025` (the ticker treats it as the current week) |
| `simulate=fail` | Fail every request after the first load, to see the retries and the **Delayed** badge |
| `simulate=primary` | Fail only the primary ESPN host, to see the mirror take over |

`seasontype` is `1` preseason, `2` regular season, `3` postseason. `year` is the season's starting year.

## Notes

This uses ESPN's unofficial scoreboard endpoint (`site.api.espn.com`), which can change without notice. NFL and team names and logos are trademarks of their respective owners. This project is not affiliated with the NFL or ESPN.
