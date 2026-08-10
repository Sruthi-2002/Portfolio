# SruthiOS — Portfolio

A Windows XP–style desktop portfolio for Sruthi Raghavan. Single HTML file, no build step,
no backend, no dependencies — just static files.

## Files you need in the folder

```
portfolio/
  index.html        <- the whole site
  download.webp     <- Bliss wallpaper
  photo.jpeg        <- profile photo
```

If an image is missing the site still works — the wallpaper falls back to a CSS gradient and the photo falls back to an "SR" circle.

## Run it locally

1. Open the folder in VS Code
2. Install the **Live Server** extension
3. Right-click `index.html` → **Open with Live Server**

Opening `index.html` directly in a browser also works. (Live Server is only nicer because it
auto-reloads on save; voice input needs `localhost` or HTTPS, not `file://`.)

## Deploy to GitHub Pages

```bash
git init
git add .
git commit -m "SruthiOS portfolio"
git branch -M main
git remote add origin https://github.com/Sruthi-2002/sruthi-portfolio.git
git push -u origin main
```

Then: repo → **Settings → Pages → Source: main / (root) → Save**.

Live at `https://sruthi-2002.github.io/sruthi-portfolio` in ~2 minutes.

## SruthiGPT: how the chat works

SruthiGPT is **scripted, not an LLM**. There is no API, no API key, and no network request —
everything runs in the visitor's browser, so it works on GitHub Pages (or any static host)
with nothing to configure and nothing that can cost money.

Replies come from an ordered keyword table, `KB`, near the bottom of the `<script>` in
`index.html`:

```js
const KB=[
  {keywords:['research','faber','ocr','redlining','qwen'], answer:"..."},
  {keywords:['govlab','unesco','pwa'],                     answer:"..."},
  ...
];
```

The user's message is lowercased and checked against each entry in order — **first match
wins**, so specific topics (`hcl`, `siteiq`, `gora`) are listed *before* general ones
(`intern`, `project`). Keywords match at a word start, so short entries like `hi` don't fire
inside "this". If nothing matches, `FALLBACK` points the visitor at email.

The reply is shown after a 600–900 ms delay so the typing indicator reads naturally.

### Editing the chat

- **New answer:** add a `{keywords:[...], answer:"..."}` object to `KB`, positioned above any
  more general entry that would otherwise swallow it.
- **Wrong answer:** the culprit is almost always an earlier entry matching first — move your
  entry up, or make its keywords more specific.
- Keep the four suggestion chips in the `sg-chips` div in sync with `KB`, otherwise a chip
  will land on the fallback.

## Features

- Boot screen with loading bar
- Draggable, minimizable windows with taskbar
- Right-click desktop menu
- Wallpaper picker (Bliss / NYC Night / Dark)
- Screensaver after 30s idle
- Snake game (Start menu → Games)
- SruthiGPT scripted chat with typing indicator + voice input (Chrome/Edge)
- Live clock with date

## Editing content

All content is plain HTML inside `index.html` — search for the window you want (`win-proj`,
`win-exp`, etc.) and edit directly. When you update experience or projects, also update the
matching `KB` answer near the bottom of the script so SruthiGPT stays accurate.
