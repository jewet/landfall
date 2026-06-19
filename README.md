# Landfall — AI Resilience Passport for Caribbean Buildings

Single-file vanilla React app (no build step, no bundler). Three Claude API calls
chain together: site risk → compliance check → resilience passport.

## Run locally
Just open `index.html` in a browser, or serve it:
```
python3 -m http.server 8000
```
Then visit http://localhost:8000

## Deploy to GitHub Pages
1. Create a new repo (or use an existing one) and put `index.html` at the repo root
   (or in a `/docs` folder if you prefer that convention).
2. Push to GitHub:
   ```
   git init
   git add index.html README.md
   git commit -m "Landfall: AI resilience passport"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```
3. In the repo: **Settings → Pages → Source** → select `main` branch, root (`/`) folder → Save.
4. GitHub gives you a URL like `https://<your-username>.github.io/<repo-name>/` within
   a minute or two.

## Demo / testing mode
There's now a **"Use example data"** toggle in the top-right of the header, visible on
every screen. It defaults to off (live Claude calls), so deployed behaviour is
unchanged. Flip it on to run the entire risk → compliance → passport flow against
fixed example JSON instead of the live API — handy for:
- Testing the UI/flow without burning API calls or needing a key wired up
- Demoing offline, or in an environment where `api.anthropic.com` isn't reachable
  (like a fresh GitHub Pages deploy with no backend proxy — see note below)

If a live call fails, the error message now tells you to try the toggle, and the
real failure reason (bad key, network, rate limit, etc.) is logged to the browser
console (`Landfall: Claude API call failed ...` / `Landfall: Claude API responded
with <status> ...`) instead of failing silently.

## Notes
- React and ReactDOM load from the `unpkg` CDN at runtime — no `npm install` needed,
  so this deploys as a static file with zero build pipeline.
- The Claude API calls (`api.anthropic.com/v1/messages`) are made directly from the
  browser with no API key attached in this file. That only works inside environments
  (like Claude.ai Artifacts) that proxy/inject the key for you. If you deploy this
  standalone to GitHub Pages, those `fetch` calls will fail with 401s — you'll need
  to either route them through your own backend that holds the API key, or swap in
  a different LLM call you control server-side. Never put a real Anthropic API key
  directly in client-side JS on a public GitHub Pages site.


