You are executing the /updategit command. Follow every step below in order. Do not skip any step. Report progress after each step.

---

## Step 1 — Security Scan (MUST pass before anything is pushed)

Scan every file in the working directory for secrets and sensitive data before touching git. Search for ALL of the following patterns:

- Tokens / API keys: strings matching `ghp_`, `gho_`, `github_pat_`, `sk-`, `pk_`, `AIza`, `AKIA`, `xoxb-`, `xoxp-`
- Passwords: any variable or key named `password`, `passwd`, `pwd`, `secret`, `private_key`, `privatekey` with a non-empty value
- Connection strings: `mongodb://`, `postgresql://`, `mysql://`, `redis://` with credentials embedded
- `.env` files committed by mistake
- Private keys: `-----BEGIN RSA PRIVATE KEY-----`, `-----BEGIN OPENSSH PRIVATE KEY-----`
- Hardcoded bearer tokens in source code

Use Grep to scan all files (exclude `.git/` and `node_modules/`).

If ANY secret is found:
- Stop immediately
- Report exactly which file and line contains the issue
- Do NOT proceed to any further step
- Instruct the user to remove the secret and re-run /updategit

If NO secrets are found, report "✓ Security scan passed — no secrets detected" and continue.

---

## Step 2 — Generate / Update README.md

Read `index.html` (and any other source files present) to understand the project, then write a professional `README.md` that includes:

- Project name and one-line description
- Live site URL in the format `https://<github-username>.github.io/<repo-name>/` — derive username and repo from the existing git remote (`git remote get-url origin`)
- Features list (derive from the actual code)
- File/folder structure
- How to run locally
- Form setup instructions (if a form is present)
- Deployment section explaining the GitHub Actions workflow
- No placeholder text — every section must reflect the actual project

Overwrite any existing README.md.

---

## Step 3 — Ensure GitHub Actions Workflow Exists

Check whether `.github/workflows/deploy.yml` exists.

If it does NOT exist, create it with this exact content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v5

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

If it already exists, leave it unchanged.

---

## Step 4 — Commit and Push to GitHub

Run the following git operations:

1. `git add -A` — stage all changes
2. Check `git diff --cached --stat` — if nothing is staged, report "Nothing new to commit" and skip the commit
3. `git commit -m "chore: update site and docs"` — commit with a clean message
4. Determine the correct remote URL. Run `git remote get-url origin` to get the current remote.
5. Ask the user: "Please provide your GitHub Personal Access Token (needs `repo` and `workflow` scopes) so I can push." Wait for the token.
6. Temporarily set the remote to `https://<username>:<token>@github.com/<username>/<repo>.git` — derive username and repo from the existing remote URL.
7. `git push origin main`
8. Immediately reset the remote URL back to `https://github.com/<username>/<repo>.git` — remove the token from the URL. This is mandatory.
9. Confirm the token is no longer in any git config by running `git remote get-url origin` and verifying no `ghp_` or similar string appears.

---

## Step 5 — Enable GitHub Pages (if not already enabled)

Use the GitHub REST API to check and enable GitHub Pages:

```
GET https://api.github.com/repos/<owner>/<repo>/pages
```

If the response is 404 (not enabled), enable it:

```
POST https://api.github.com/repos/<owner>/<repo>/pages
Body: { "build_type": "workflow" }
```

Use `Invoke-RestMethod` in PowerShell with `Authorization: token <token>` header.

If already enabled, skip and report "✓ GitHub Pages already enabled".

---

## Step 6 — Update Repository About / Description

Use the GitHub REST API to update the repository metadata:

```
PATCH https://api.github.com/repos/<owner>/<repo>
Body: {
  "description": "<one-line description derived from README>",
  "homepage": "https://<owner>.github.io/<repo>/",
  "topics": ["landing-page", "html", "css", "javascript", "github-pages"]
}
```

Derive the description from the first paragraph of the README you just wrote. Keep it under 100 characters.

---

## Step 7 — Final Report

Print a summary block:

```
✓ Security scan     — passed
✓ README            — written
✓ Workflow          — present
✓ Code pushed       — main branch
✓ GitHub Pages      — enabled
✓ Repo About        — updated
✓ Token cleanup     — remote URL contains no secrets

🌐 Live site: https://<owner>.github.io/<repo>/
📁 Repo:      https://github.com/<owner>/<repo>
⏱  GitHub Pages will be live within ~60 seconds.
```
