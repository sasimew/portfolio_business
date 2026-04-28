# Home deploy and GitHub push

This folder is the public-safe Home portfolio site. Keep Home work inside `home/` and do not copy files from dashboard, bot, API, logs, data exports, `.env`, keys, tokens, or secrets.

## Local preview

Open the static file directly:

```bash
open /Users/sasi/Desktop/crypto_bot/home/home.html
```

Or run a local static server from this folder:

```bash
cd /Users/sasi/Desktop/crypto_bot/home
python3 -m http.server 8080
```

Then open:

```text
http://localhost:8080/home.html
```

## Safety check before commit

Run these checks from `home/` before committing:

```bash
git status --short
find . -type f \( -name ".env" -o -name "*.log" -o -name "*key*" -o -name "*token*" -o -name "*secret*" \)
grep -RInE "api[_-]?key|secret|token|password|passwd|BEGIN (RSA|OPENSSH|PRIVATE) KEY|ghp_[A-Za-z0-9_]+" .
```

Expected result: only public Home files are staged, and the secret scan returns no real credentials.

## One-command push flow

Run from the `home/` folder. This publishes only the Home folder contents to `portfolio_business`.

```bash
cd /Users/sasi/Desktop/crypto_bot/home
git init
git branch -M main
git remote remove origin 2>/dev/null || true
git remote add origin https://github.com/sasimew/portfolio_business.git
git add home.html DEPLOY_HOME.md assets/business-hero.png
git commit -m "Redesign public home portfolio page"
git push -u origin main
```

If the remote already has history and this folder is the intended full content for the repository, use a reviewed force-with-lease push only after confirming the remote branch state:

```bash
git fetch origin main
git push --force-with-lease origin main
```

## Public file policy

- Allowed: `home.html`, `DEPLOY_HOME.md`, and assets in `assets/`.
- Not allowed: `.env`, logs, exports, dashboard files, bot/API code, credentials, private config, or generated operational data.
- Do not paste tokens into commands that may be saved in shell history or docs.
