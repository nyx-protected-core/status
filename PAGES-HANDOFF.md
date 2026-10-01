# Pages handoff — nyx-protected-core/status

Paste this whole file to another instance that can use a browser, or a token with admin on the repo. Do not create a second Nyx bot for this. A one-shot browser pass is enough.

## Goal

Enable GitHub Pages for the repo `nyx-protected-core/status` and verify the live site.

## Repo and expected URLs

- Repo: https://github.com/nyx-protected-core/status
- Pages settings: https://github.com/nyx-protected-core/status/settings/pages
- Expected live URL: https://nyx-protected-core.github.io/status/
- Source branch: `main`
- Source folder: `/` (root)
- Files already on `main`: `index.html` (status board), `status.json` (scrubbed pulse; no secrets), `.github/workflows/pages.yml` (Actions path has been failing — do not rely on it)

## Steps

1. Open https://github.com/nyx-protected-core/status/settings/pages in the browser.
2. If GitHub shows a Sign in page, stop and report that — do not ask Shaun to sign in unless the browser itself requires it.
3. Enable Pages with Deploy from branch → main / root, click Save. Prefer **Deploy from a branch**, branch **main**, folder **/ (root)**. Do not switch the source to GitHub Actions unless branch deploy is unavailable. Actions previously could not create the Pages site (`Get Pages site failed. Error: Not Found` and `Create Pages site failed. Error: Resource not accessible by integration`).
4. Report the site URL and whether the live site loads — specifically whether `index.html` serves the board or an "index of" directory listing appears. Expected URL is https://nyx-protected-core.github.io/status/. If it is still 404, wait up to about 2–3 minutes after Save and recheck. Success is HTTP 200 and the Nyx status board (plain JS polls `status.json` every 30s and updates `#board`). Failure modes to name explicitly: 404, Sign in, or a bare "Index of" listing.
5. If the browser requires Shaun's sign-in, stop and report that rather than asking him to do anything. Same rule for 2FA, passkey, or captcha you cannot pass: stop and quote what the page asked for.
6. Alternative route if UI is blocked: if a token with admin scope for the repo exists, enable Pages through the GitHub API instead of the UI. Never invent, scrape, or ask Shaun to paste a token into chat.

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

Then `GET` the same `/pages` endpoint and open `html_url` or https://nyx-protected-core.github.io/status/.

## Known blocker

A previous attempt found GitHub not signed in. The settings page showed "Page not found" and a Sign in link. Save was not done. Live https://nyx-protected-core.github.io/status/ remained 404 ("Page not found · GitHub Pages"). Browser sessions were not signed in to GitHub.

## What to report back

- Exact Pages URL you opened
- Save: yes or no
- Live load: board, "Index of", or 404
- Sign-in prompt text if any
- This file path if you update it: `nyx-protected-core/status` → `PAGES-HANDOFF.md` on `main`
