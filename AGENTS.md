# Multivac — AGENTS.md

## What is this?

Multivac is a single-file static deploy dashboard for Viostream. It lives at `index.html` and has no build step, no dependencies, no backend.

## Stack

- Pure HTML/CSS/JS, no framework
- Calls the GitHub REST API directly from the browser
- Deployed via GitHub Pages to `multivac.viostream.xyz`

## Key files

| File | Purpose |
|---|---|
| `index.html` | The entire app |
| `viostream.png` | Logo (200×200 PNG) |
| `CNAME` | GitHub Pages custom domain |
| `.github/workflows/deploy.yml` | Deploys to GitHub Pages on push to main |

## App list

The `APPS` array in `index.html` defines which workflows appear in the dashboard. Each entry has:
- `name` — display name
- `workflow` — filename in `.github/workflows/` of `viostream/viostream`

To add or remove an app, edit this array.

## Data sources

- **Deployment status** — `GET /repos/Viostream/viostream/deployments?environment={env}&per_page=1` then `GET /repos/.../deployments/{id}/statuses?per_page=1`
- **Version string** — `description` field on the latest deployment status (written by the `Set deployment description to version` step in the deployinator)
- **Branches** — `GET /repos/Viostream/viostream/branches?per_page=100`
- **Ahead-by** — `GET /repos/Viostream/viostream/compare/{base}...{head}`

## Triggering deploys

`POST /repos/Viostream/viostream/actions/workflows/{workflow}/dispatches` with `{ ref: branch }`.

Requires a classic PAT with `repo` + `workflow` scopes.

## DNS

The `multivac.viostream.xyz` CNAME record needs to point to `viostream.github.io`. This is managed in `terraform-account-components`. The GitHub Pages custom domain is set via the `CNAME` file in this repo.

## GitHub Pages setup (one-time)

After the repo is created:
1. Go to Settings → Pages
2. Source: GitHub Actions (the workflow handles it)
3. Custom domain: `multivac.viostream.xyz`
4. Enforce HTTPS: yes
