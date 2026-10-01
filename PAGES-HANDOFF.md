# Pages handoff — nyx-protected-core/status

Paste this whole file to another instance/bot that can use a browser (or a token with admin on the repo).

## Goal

Enable GitHub Pages for the repo `nyx-protected-core/status` and verify the live site.

## Repo / expected URLs

- Repo: https://github.com/nyx-protected-core/status
- Settings (Pages): https://github.com/nyx-protected-core/status/settings/pages
- Expected live URL: https://nyx-protected-core.github.io/status/
- Related user-site repo (optional mirror): https://github.com/nyx-protected-core/nyx-protected-core.github.io → https://nyx-protected-core.github.io/

## What’s already on `main`

- `index.html` — Nyx status board; plain JS polls `status.json` every 30s and updates `#board` in place (`setInterval(load, 30000)`).
- `status.json` — scrubbed high-level habitat pulse (no secrets/paths/tokens).
- `.github/workflows/pages.yml` — Actions deploy path (has been failing; prefer branch deploy below).

## Steps (UI)

1. Open https://github.com/nyx-protected-core/status/settings/pages in the browser.
2. If GitHub shows a **Sign in** page (or settings returns “Page not found” with a Sign in link), **stop and report that** — do not ask Shaun to sign in unless the browser itself requires it for a flow you cannot complete.
3. Enable Pages with **Deploy from a branch** → Branch **main** → folder **/ (root)** → click **Save**. (Do **not** rely on “GitHub Actions” source unless branch deploy is unavailable; Actions previously could not create the Pages site.)
4. Report the site URL and whether the live site loads — specifically whether `index.html` serves the board or an **“Index of”** directory listing appears.
5. If the browser requires Shaun’s sign-in / 2FA / passkey / captcha that you cannot pass, **stop and report exactly what it asked for** rather than asking him to do anything.
6. Wait up to ~2–3 minutes after Save if the live URL is still 404, then recheck.

## Alternative route if UI is blocked

If a token with **admin** scope for the repo exists (never invent or scrape one):

```http
POST https://api.github.com/repos/nyx-protected-core/status/pages
Accept: application/vnd.github+json
Authorization: Bearer <TOKEN>
Content-Type: application/json

{
  "build_type": "legacy",
  "source": { "branch": "main", "path": "/" }
}
```

Then GET the same `/pages` endpoint and open `html_url` / `https://nyx-protected-core.github.io/status/`.

Note: GitHub Actions `GITHUB_TOKEN` previously failed with:

- `Get Pages site failed. Error: Not Found`
- `Create Pages site failed. Error: Resource not accessible by integration`

So Actions alone is **not** a working enable path on this account unless permissions change.

## Known blocker (prior attempts)

- Browser sessions were **not signed in** to GitHub.
- Settings URL showed **“Page not found”** plus a **Sign in** link.
- **Save was not done.**
- Live `https://nyx-protected-core.github.io/status/` remained **404** (“Page not found · GitHub Pages”).
- A second Nyx agent must **not** be created for this; a one-shot browser helper is fine.

## Success criteria

- Pages Settings shows source = branch `main` / `/ (root)` (or equivalent published state).
- Live URL returns 200 and shows the Nyx status board (not 404, not bare “Index of”).
- Page includes 30s `status.json` auto-refresh (footer may say “polls every 30s”).

## Report back

Exact Pages URL; Save yes/no; live load board vs Index-of vs 404; sign-in prompt text if any; this handoff file path if you update it.
