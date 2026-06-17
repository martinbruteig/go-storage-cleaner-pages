# go-storage-cleaner-pages

Standalone static compliance site for **GO Storage Cleaner** (App Store privacy policy and support pages). Published by Aksjefokus AS. No framework, no build step, no analytics, no cookies, no tracking, no ads, no backend — plain HTML/CSS, deliberately boring.

## Final App Store URLs

- Marketing URL: **https://gostoragecleaner.app/**
- Privacy Policy: **https://gostoragecleaner.app/privacy/**
- Support: **https://gostoragecleaner.app/support/**

Use these in App Store Connect (App Information → Marketing URL & Support URL, App Privacy → Privacy Policy URL).

**Primary domain: `gostoragecleaner.app`.** The two `.com` domains redirect to it:
`gostoragecleaner.com` → `.app` and `go-storage-cleaner.com` → `.app`. Domains are
registered at **Namecheap**. (Aksjefokus AS remains the legal publisher; the product
no longer uses any `aksjefokus.no` domain.)

The `/privacy` and `/support` pages carry `<meta name="robots" content="noindex, nofollow">` — public but unlisted. The marketing landing page (`/`) is **indexable** so it can be found; remove that distinction by adding the same meta tag to `index.html` if you'd rather keep it unlisted too.

## Structure

```
index.html           marketing landing page (brand, pitch, features, safety)
landing.css          styles for the landing page only
assets/              brand images (app icon, splash)
robots.txt           allows crawling; points to the sitemap
sitemap.xml          lists the marketing landing page only
privacy/index.html   privacy policy
support/index.html   support page
styles.css           shared styles for /privacy and /support
```

Absolute URLs (`https://gostoragecleaner.app/...`) live in `index.html`
(`canonical`, `og:url`, `og:image`), `robots.txt` and `sitemap.xml`. If the site ever
moves to a new domain, find-and-replace that host across those files and update the
App Store Connect URLs.

Directory-based `index.html` files give clean URLs (`/privacy`, `/support`) on any static host.

## Deploy — Cloudflare Pages (recommended)

1. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git**.
2. Select the `martinbruteig/go-storage-cleaner-pages` repository.
3. Build settings: **Framework preset: None**, build command: *(empty)*, output directory: `/` (root).
4. Deploy. The site gets a `*.pages.dev` URL immediately; every push to `master` redeploys.

## Deploy — Vercel (alternative)

1. Vercel dashboard → **Add New → Project** → import `go-storage-cleaner-pages`.
2. Framework preset: **Other**. No build command, output directory: root.
3. Deploy.

## Point the domains at the site (Namecheap → Cloudflare Pages)

Domains are at **Namecheap**; the site is hosted on **Cloudflare Pages**. Apex domains
(`gostoragecleaner.app`) work most cleanly when the zone's nameservers are on Cloudflare.

**1. Primary — `gostoragecleaner.app`:**
   1. Add `gostoragecleaner.app` as a site in the Cloudflare dashboard (free plan).
   2. At Namecheap → Domain → **Nameservers → Custom DNS**, set the two nameservers
      Cloudflare gives you. Wait for it to go active.
   3. In the Pages project → **Custom domains → Set up a custom domain** → `gostoragecleaner.app`.
      Cloudflare creates the records automatically.

**2. Redirects — both `.com` domains → `.app`:** add each `.com` as a Cloudflare site
   (nameservers moved as above), then create a **Redirect Rule** (or Bulk Redirect):
   `gostoragecleaner.com/*` → `https://gostoragecleaner.app/$1` (301), and the same for
   `go-storage-cleaner.com/*`. (Namecheap's built-in URL Redirect also works if you keep
   the `.com` DNS at Namecheap.)

After DNS is live, verify (all should return 200, and the `.com` ones should 301 → `.app`):

```
curl -I https://gostoragecleaner.app/
curl -I https://gostoragecleaner.app/privacy/
curl -I https://gostoragecleaner.app/support/
curl -I https://gostoragecleaner.com/        # expect 301 -> gostoragecleaner.app
curl -I https://go-storage-cleaner.com/       # expect 301 -> gostoragecleaner.app
```

---

GO Storage Cleaner is an unofficial companion utility. It is not affiliated with, endorsed by, or sponsored by Niantic, Nintendo, The Pokémon Company, Game Freak, or Pokémon GO.
