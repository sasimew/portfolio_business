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

## Production deploy note

The normal backend deploy command excludes `home/`, so it will not publish the Home redesign:

```bash
rsync -avz \
  --exclude '.env' \
  --exclude 'env' \
  --exclude '.htpasswd' \
  --exclude '.git/' \
  --exclude '.DS_Store' \
  --exclude '__pycache__/' \
  --exclude 'data/' \
  --exclude 'export/' \
  --exclude 'home/' \
  ~/Desktop/crypto_bot/ root@<server-ip>:/home/crypto_bot/
```

Use that command for the existing app deploy only. For Home, deploy the public static files separately so dashboard, bot, API, logs, data, and secrets stay out of the upload.

```bash
rsync -avz \
  --exclude '.git/' \
  --exclude '.DS_Store' \
  /Users/sasi/Desktop/crypto_bot/home/home.html \
  /Users/sasi/Desktop/crypto_bot/home/index.html \
  /Users/sasi/Desktop/crypto_bot/home/DEPLOY_HOME.md \
  /Users/sasi/Desktop/crypto_bot/home/assets \
  root@<server-ip>:/home/crypto_bot/home/
```

If the web server serves `/home.html` or `/index.html` from another document root, change only the destination path. Do not remove the backend deploy excludes unless the deploy target is meant to receive the Home folder.

Restart the running service after deploy using the project's normal restart command on the server, for example:

```bash
ssh root@<server-ip>
cd /home/crypto_bot
docker compose restart
```

If the server uses the older Compose command, use `docker-compose restart` instead.

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
