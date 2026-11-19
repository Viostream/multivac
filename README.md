# Multivac

> *"The last question was asked for the first time, half in jest, on May 21, 2061"*
> — Isaac Asimov, The Last Question

Multivac is Viostream's deploy dashboard. It shows what version of every app is deployed to each environment, what's behind, and lets you trigger deploys directly from the browser.

**Live:** https://multivac.viostream.xyz

## Features

- Per-app, per-environment version matrix (Dev / Staging / Production)
- Ahead-by counts and diff links between environments
- Branch selector — pick any branch and bulk-deploy all apps to Dev with one click
- Reads the [GitHub Deployments API](https://docs.github.com/en/rest/deployments) directly from the browser
- Auto-refreshes every 2 minutes

## Auth

On first load, Multivac will prompt for a GitHub personal access token. This is stored in `localStorage` and never leaves your browser.

**Required token scopes (classic PAT):** `repo`, `workflow`

Create one at: https://github.com/settings/tokens/new?scopes=repo,workflow&description=multivac

## How it works

Multivac is a single static HTML file. There is no backend. It calls the GitHub API directly from the browser using your PAT.

Deploy data comes from GitHub's Deployments API, which is populated by the `Set deployment description to version` step in the deployinator workflows. Version strings appear once a deploy has run with that step in place.

## Deployment

Deployed to GitHub Pages automatically on every push to `main`. The CNAME points to `multivac.viostream.xyz`.

DNS is managed in terraform-account-components — the CNAME record for `multivac.viostream.xyz` should point to `viostream.github.io`.

## Development

It's one HTML file. Open it in a browser.

```sh
cd ~/dev/multivac
python3 -m http.server 8742
open http://localhost:8742
```
