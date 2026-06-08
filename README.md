# Athey Creek Sermon Search (Demo / MVP)

Search a sermon transcript and click any line to jump to that exact moment in the audio.

## Run it

`fetch()` does not work from `file://`, so serve the folder over HTTP:

```bash
cd /Volumes/MiniHDX/Github/athey-library
python3 -m http.server 8000
```

Then open <http://localhost:8000/>.

## How it works

- **`index.html`** — the whole app (vanilla JS, no build step).
- **`final_output/{ID}.json`** — sentence-level transcripts: `[{ start, end, text }]` (seconds).
- Search is case-insensitive substring/phrase matching, in memory.
- Each result shows `mm:ss`, the matched sentence (matched term highlighted, HTML-escaped),
  and the neighboring sentences for context.
- Clicking a result sets `audio.currentTime = start` and plays.

## Configuration (top of the `<script>` in `index.html`)

- **`SERMONS`** — registry. Add a sermon: drop `final_output/{ID}.json` and add `{ id, title }`.
- **`AUDIO_MODE`** — `"cloudfront"` (prod) streams from
  `https://d2ecbaqsz6tho7.cloudfront.net/audio/teachings/{ID}.mp3`;
  `"local"` plays from `media/{ID}.mp3` (offline dev).

## Scope

MVP only: client-side search + click-to-jump on the sermons in `final_output/`.
Out of scope for now: accounts, favorites, sharing, Vimeo embed, the transcription
pipeline, and the full multi-sermon catalog + metadata scraping. Move to SQLite FTS5
or Postgres when scaling to thousands of sermons.
