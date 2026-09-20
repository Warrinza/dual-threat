# Dual Threat

NCAA fantasy wrestling league site — standings, weekly matchups, rosters, and organizer tools.

## Live site

https://warrinza.github.io/dual-threat/

## Shared league data

Everyone loads the same season from [`data/league.json`](data/league.json) in this repo (owners, schedule, wrestlers, lineups, results). The page fetches that file on load with a cache-busting query string.

If the fetch fails (for example opening `index.html` as a local file), the site falls back to the embedded defaults in `index.html` and shows a gentle notice.

UI preferences (active tab, selected week, filters) still use `localStorage` key `dualThreatUI`. League data is **not** stored as source of truth in `localStorage`.

## Organizer saves (GitHub Contents API)

Roster, lineup, and results saves update `data/league.json` on the `main` branch via the GitHub Contents API. After GitHub Pages rebuilds, every visitor sees the same data.

### Create a fine-grained PAT

1. GitHub → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**.
2. Token name: e.g. `dual-threat organizer`.
3. Expiration: choose a short window you are comfortable with.
4. Resource owner: your user (`Warrinza`).
5. Repository access: **Only select repositories** → `Warrinza/dual-threat`.
6. Repository permissions: **Contents** → **Read and write** (nothing else required).
7. Generate the token and copy it once.

### Paste it in the site

1. Open https://warrinza.github.io/dual-threat/
2. Open the **Organizer** tab.
3. Paste the token into **Fine-grained PAT** and click **Save token in this browser**.
4. The token is stored only in this browser under `localStorage` key `dualThreatGhToken`. Use **Clear token** to remove it.

**Security:** Never commit a PAT to the repo, put it in `league.json`, or share it in chat. Prefer a fine-grained token scoped to this repository only. Anyone with the token can rewrite `data/league.json`.

Claude artifact publish remains an optional fallback when the Claude APIs are present and no GitHub token is set.

## Local

Serve the folder so `data/league.json` is reachable, for example:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080/`.
