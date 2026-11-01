# Ledgerwise

A single-page, plain-language guide to how Bitcoin and crypto actually work â€” the
21 million supply cap, the halving schedule, keys and custody, and the risks that
usually get skipped. Built as one self-contained HTML file with no build step,
no dependencies and no server code.

Live features:

- **Live market strip** â€” BTC price, 24h change, total market cap and BTC dominance,
  pulled from CoinGecko's public API, with a graceful fallback when the API is rate-limited.
- **Halving clock** â€” a countdown to the estimated fifth halving at block 1,050,000,
  with a cycle-progress bar and blocks-remaining estimate.
- **Scam lab** â€” an interactive phishing test: four messages, one genuine, with a
  full breakdown of the red flags after you answer.
- **Follow-the-creator popup** â€” an Instagram call-to-action, shown once per session,
  plus a permanent button in the navbar.
- **Social preview card** â€” a 1200x630 Open Graph image plus Twitter/X meta tags, so
  shared links show a proper card instead of a bare URL.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site â€” markup, styles, scripts and favicon in one file |
| `og-image.png` | 1200x630 social preview image (Open Graph / Twitter card) |
| `README.md` | This file |
| `CNAME` | Optional. Only add it when you connect your own domain |

Keep `og-image.png` next to `index.html` in the repo root â€” the meta tags point to
it by relative path.

## Publish it free on GitHub Pages

You need a free GitHub account. No Git experience required â€” everything below
can be done in the browser.

### 1. Create the repository

1. Go to github.com and sign in.
2. Click **+** (top right) â†’ **New repository**.
3. Name it `ledgerwise` (or anything you like).
4. Set visibility to **Public**. GitHub Pages on the free plan only publishes
   from public repositories.
5. Leave "Add a README" unchecked â€” you already have one.
6. Click **Create repository**.

### 2. Upload the files

1. On the new repo page, click **Add file** â†’ **Upload files**.
2. Drag in `index.html`, `og-image.png` and `README.md`.
3. Important: `index.html` must be named exactly that and sit at the **root** of
   the repo, not inside a folder. GitHub Pages looks for that filename by default.
4. Type a commit message like `Add Ledgerwise site` and click **Commit changes**.

### 3. Turn on Pages

1. In the repo, open **Settings** â†’ **Pages** (left sidebar).
2. Under **Build and deployment â†’ Source**, choose **Deploy from a branch**.
3. Set **Branch** to `main` and the folder to `/ (root)`.
4. Click **Save**.
5. Wait 1â€“2 minutes, then refresh the page. A banner appears with your URL:
   `https://<your-username>.github.io/ledgerwise/`

First deploy can take a few minutes. If you see a 404, wait, then reload â€”
or check **Actions** for the `pages build-and-deployment` run.

### 4. Updating the site

Edit `index.html` in the repo (pencil icon â†’ edit â†’ **Commit changes**), or
upload a new copy via **Add file â†’ Upload files**. The site rebuilds itself
within a minute or two. No re-deploy step needed.

## Make the social card work

The preview tags in `index.html` contain a placeholder address,
`https://your-username.github.io/ledgerwise/`. Most social platforms ignore
relative image paths, so these URLs must be absolute and must match your live site.

1. After your site is live, open `index.html` and find the block near the top
   marked `<!-- REPLACE every SITE_URL below ... -->`.
2. Replace **all five** occurrences of `https://your-username.github.io/ledgerwise/`
   with your real URL, keeping the trailing slash. That covers the canonical link, `og:url`,
   `og:image`, `twitter:image` and the JSON-LD `url`.
3. Commit the change.
4. Force a refresh of the cached preview:
   - Facebook / WhatsApp / Instagram: paste the URL into the
     [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) and
     click **Scrape Again**.
   - X / Twitter: use the
     [Card Validator](https://cards-dev.twitter.com/validator).
   - LinkedIn: use the
     [Post Inspector](https://www.linkedin.com/post-inspector/).
   - Telegram and WhatsApp cache aggressively; sending the link to yourself once
     usually shows the stale card, the second send shows the new one.
5. Quick sanity check: if the image loads at
   `https://your-site/og-image.png` in a browser, the path is right.

Meta tags included: `og:title`, `og:description`, `og:image` (with type, width,
height and alt), `og:url`, `og:site_name`, `og:locale`, `twitter:card`
(`summary_large_image`), `twitter:title`, `twitter:description`, `twitter:image`,
plus a canonical link, `theme-color`, author, and WebSite JSON-LD structured data.

## Add a custom domain (optional)

### Free option â€” a developer subdomain

Services like **is-a.dev** give out free subdomains such as `ledgerwise.is-a.dev`.
You register by opening a pull request in their register repository following
their instructions. Once approved:

1. Add a `CNAME` file to the repo root containing just your domain, no `https://`:
   ```
   ledgerwise.is-a.dev
   ```
2. In **Settings â†’ Pages â†’ Custom domain**, enter the same domain and click Save.
3. At your DNS provider, add a `CNAME` record for the subdomain pointing to
   `<your-username>.github.io`.
4. Tick **Enforce HTTPS** once the certificate is issued (can take up to 24 hours).
5. Update the meta tags and JSON-LD to the new domain â€” the social card breaks
   otherwise.

### Paid option â€” your own domain

A `.com` costs roughly Rs 800â€“1,200 per year. That is the only part of this setup
that is not free. For an apex domain, add four `A` records pointing to GitHub's
Pages IPs; for a subdomain, a single `CNAME` to `<your-username>.github.io` is
enough. Then set it in **Settings â†’ Pages** and enable HTTPS â€” GitHub issues the
certificate automatically.

## Editing tips

- All colours live in the `:root` block at the top of the `<style>` tag. Change
  `--amber` to rebrand the whole page in one edit.
- The scam-test content is a single `SCENARIOS` array near the bottom of the
  `<script>` tag â€” add, remove or rewrite cards there.
- The countdown dates are `LAST` and `NEXT` in the halving-clock block. Update
  `NEXT` after a halving happens.
- Text is plain HTML. Section anchors (`#basics`, `#clock`, `#scam`) match both
  the navbar and footer links, so renaming a section means updating both.
- To regenerate or restyle `og-image.png`, keep it at 1200x630 and under ~300 KB
  for fast previews.

## Troubleshooting

| Symptom | Fix |
|---|---|
| 404 after enabling Pages | Wait 2â€“3 minutes, confirm `index.html` is at the repo root, and check the Actions tab |
| Link preview shows no image | Absolute URLs missing or stale cache â€” fix the meta tags, then re-scrape with the debugger links above |
| Price strip says "live feed offline" | CoinGecko's free API is rate-limited; it usually recovers on reload |
| Popup never appears | It shows once per browser session â€” clear site data or open a private window |
| Custom domain shows a certificate error | DNS has not propagated yet; wait, then re-save the custom domain |
| Fonts look different | Google Fonts is blocked or offline; the page falls back to system fonts |

## A note on content

The page is educational only and is not financial advice. Scam examples use
invented domains â€” `coinbase-secure-verify.com`, `ledger-live-sync.com` and
`claim-rewards-eth.xyz` are illustrations, not real sites. Do not register or
visit them expecting to find anything.

## Credit

Built by Vansh Bansal â€” [@vanshbansal_08](https://www.instagram.com/vanshbansal_08?exln=M2NlOWh4eThzZzF0)