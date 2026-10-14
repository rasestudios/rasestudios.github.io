# Rase Studios website (GitHub Pages)

Static developer website for **Rase Studios** and **Word Kingdom: Solitaire / Kelime Krallığı: Solitaire**.
Plain HTML + one stylesheet: no build step, no JavaScript, no cookies, no analytics, no external
fonts/CDNs. All links between pages are relative, so the site works on any domain.

| Path | Page |
|---|---|
| `/` | Rase Studios home (EN + TR) |
| `/word-kingdom/` | Game page (EN + TR, store links) |
| `/word-kingdom/privacy/` | Privacy policy, English |
| `/word-kingdom/gizlilik/` | Gizlilik politikası, Türkçe |
| `/word-kingdom/support/` | Support / FAQ (EN + TR) |
| `/app-ads.txt` | AdMob app-ads.txt (must stay at the root) |
| `/404.html` | Not-found page (served by GitHub Pages) |

The app builds its links from `AppConfig.websiteBase` in `lib/config/app_config.dart`
(currently `https://rasestudios.github.io`), so these paths must not change.

## Publish on GitHub Pages (user site)

1. On GitHub create a **public** repository named exactly `<username>.github.io`
   (for the account `rasestudios` that is `rasestudios.github.io`).
2. Push the **contents** of this `site/` folder to the root of that repository's `main` branch
   (`index.html`, `.nojekyll`, `app-ads.txt` … at the top level, not inside a `site/` folder):

   ```bash
   cd site
   git init -b main
   git add -A
   git commit -m "Publish Rase Studios website"
   git remote add origin https://github.com/<username>/<username>.github.io.git
   git push -u origin main
   ```

   Later updates: copy the changed files into that repository, commit and push.
3. In the repository: **Settings → Pages → Build and deployment → Source: Deploy from a branch →
   Branch: `main` / `/ (root)` → Save**. The first deployment takes a minute or two.
4. Check that these open in a browser:
   `https://<username>.github.io/`, `/word-kingdom/privacy/`, `/word-kingdom/gizlilik/`,
   `/word-kingdom/support/` and `/app-ads.txt`.
5. If the username is not `rasestudios`, set `AppConfig.websiteBase` to
   `https://<username>.github.io` (one line) and update the URLs in `docs/STORE_LISTING.md`.
6. Enter the URLs in the store consoles: privacy policy URL (Play Console + App Store Connect +
   AdMob → Privacy & messaging), support URL (App Store Connect), and the site root as the
   developer website / marketing URL (needed for app-ads.txt verification).

`.nojekyll` tells GitHub Pages to serve the files as they are (no Jekyll processing).
`404.html` uses root-absolute links (`/assets/style.css`) because GitHub serves it for missing
paths at any depth; that works because a user site and a custom domain are both served at `/`.

## app-ads.txt

`app-ads.txt` currently contains only comments. When the AdMob account exists, copy the line from
**AdMob → Apps → View all apps → app-ads.txt**, add it **without** the leading `#`
(`google.com, pub-…, DIRECT, f08c47fec0942fa0`), push, and wait up to 24 hours for AdMob to verify.
The domain must be the same one entered as the developer website in Google Play / App Store.

## Optional: custom domain `rasestudios.com`

Not set up yet (no `CNAME` file on purpose). When you own the domain:

1. Repository **Settings → Pages → Custom domain**: enter `rasestudios.com` and save. GitHub
   commits a `CNAME` file containing `rasestudios.com` to the repository root (or add that file
   yourself, one line, no `https://`). Verifying the domain under your account's
   **Settings → Pages** is recommended to prevent takeovers.
2. At your DNS provider add four **A** records for the apex `rasestudios.com`:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   (optional IPv6 **AAAA**: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`,
   `2606:50c0:8003::153`), and a **CNAME** record `www` → `<username>.github.io`.
3. When the certificate is ready, tick **Enforce HTTPS**.
4. Change `AppConfig.websiteBase` to `https://rasestudios.com`, update the store consoles and
   `docs/STORE_LISTING.md`, and ship an app update. Keep the old GitHub URL working: GitHub
   redirects `<username>.github.io` to the custom domain automatically.

## Editing

- Brand colours come from `lib/core/theme/app_colors.dart`; the stylesheet is `assets/style.css`
  (light and dark themes via `prefers-color-scheme`). Headings use a Latin + Turkish subset of
  Nunito ExtraBold (`assets/fonts/nunito-800.woff`, SIL Open Font License, see `assets/fonts/OFL.txt`).
- Icons (`assets/icon-192.png`, `assets/apple-touch-icon.png`, `assets/favicon-32.png`,
  `favicon.ico`) are resized from `assets/branding/icon_1024.png`.
- Store buttons on `/word-kingdom/` point to `#` until the listings are live (see the
  `TODO(release)` comments in that file).
- When the app changes what data it processes (a new SDK, analytics, accounts …), update **both**
  privacy pages, their “Last updated” date, and `docs/PRIVACY_POLICY.md`.
- Each page has a strict Content-Security-Policy `<meta>` tag (`default-src 'none'`, only local
  styles, images and fonts). Inline `style` attributes and scripts are blocked; use classes in
  `assets/style.css`.
- The privacy text is written from the app's actual behaviour; it is not legal advice. Have it
  reviewed if the business setup changes (company, address, new markets).
