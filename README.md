# go-storage-cleaner-pages

Standalone static compliance site for **GO Storage Cleaner** (App Store privacy policy and support pages). Published by Aksjefokus AS. No framework, no build step, no analytics, no cookies, no tracking, no ads, no backend — plain HTML/CSS, deliberately boring.

## Final App Store URLs

- Privacy Policy: **https://go-storage-cleaner.aksjefokus.no/privacy**
- Support: **https://go-storage-cleaner.aksjefokus.no/support**

Use these in App Store Connect (App Privacy → Privacy Policy URL, and App Information → Support URL).

All pages carry `<meta name="robots" content="noindex, nofollow">` — public but unlisted.

## Structure

```
index.html           minimal utility page linking to /privacy and /support
privacy/index.html   privacy policy
support/index.html   support page
styles.css           shared styles
```

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

## Point go-storage-cleaner.aksjefokus.no at the site

Since `aksjefokus.no` is already on Cloudflare:

**Cloudflare Pages:** in the Pages project → **Custom domains → Set up a custom domain** → enter `go-storage-cleaner.aksjefokus.no`. Cloudflare creates the CNAME record in the `aksjefokus.no` zone automatically. Done in minutes.

**Vercel:** in the Vercel project → **Settings → Domains** → add `go-storage-cleaner.aksjefokus.no`, then create the CNAME record Vercel shows (`cname.vercel-dns.com`) in the Cloudflare DNS zone for `aksjefokus.no` (set the record to *DNS only*, not proxied).

After DNS is live, verify:

```
curl -I https://go-storage-cleaner.aksjefokus.no/privacy
curl -I https://go-storage-cleaner.aksjefokus.no/support
```

Both should return `200`.

---

GO Storage Cleaner is an unofficial companion utility. It is not affiliated with, endorsed by, or sponsored by Niantic, Nintendo, The Pokémon Company, Game Freak, or Pokémon GO.
