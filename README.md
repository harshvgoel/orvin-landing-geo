# Orvin Labs â€” orvinlabs.com

Single-page marketing site. Plain HTML, inlined CSS, ~40 lines of vanilla JS.
No build step, no frameworks, no external CSS or JS libraries. Every word of
content is in the raw HTML source; nothing that matters is injected by
JavaScript.

Deployment target: GitHub Pages with the custom domain `orvinlabs.com`.

---

## Files

| File | What it is |
| --- | --- |
| `index.html` | The landing page. Dark theme by default, light under `prefers-color-scheme: light`. All CSS inlined in one `<style>` block, JSON-LD `@graph` in `<head>`, one small inline `<script>` at the end. All decorative graphics are inline SVG â€” no image requests. |
| `ai-crawlers/index.html` | Reference page: the four kinds of AI user agent and the current robots.txt token for each operator. Every fact verified against the operator's own docs on 2026-10-03 and linked in a Sources list. `TechArticle` + `FAQPage` + `BreadcrumbList` schema. |
| `ai-traffic-in-ga4/index.html` | How-to: why AI sessions land in Referral and Direct, and the retroactive GA4 custom channel group that separates them. `TechArticle` + `HowTo` + `FAQPage` + `BreadcrumbList` schema. |
| `robots.txt` | Explicitly allows every major AI and search crawler. Nothing is disallowed. Points to the sitemap. |
| `sitemap.xml` | Lists `/`, `/llms.txt` and `/llms-full.txt`. |
| `llms.txt` | llmstxt.org-format summary: what we do, services and prices, who we work with, key facts, contact. Plain declarative sentences an AI can quote. |
| `llms-full.txt` | The complete page content as clean markdown. |
| `CNAME` | Contains `orvinlabs.com`. Required by GitHub Pages for the custom domain. |
| `favicon.svg` | 32px favicon, referenced from `<head>`. |
| `logo.svg` | 512px vector mark. |
| `logo.png` | 512Ã—512 raster mark. Referenced as `logo` in the Organization JSON-LD. |
| `apple-touch-icon.png` | 180Ã—180 home-screen icon. |
| `og.png` | 1200Ã—630 Open Graph / Twitter card image. |
| `README.md` | This file. |

### Brand assets

All logo assets are the **real Orvin Labs mark**, derived from `orangelogo.svg`
and the supplied brand kit. Nothing on the site is a placeholder logo any more.

- **Header, all three pages:** the full mark + wordmark lockup, inlined as SVG
  with the wordmark outlined to paths. Exact brand typography, no webfont, no
  network request, themes with `currentColor`.
- **Footer and hero graphic:** the mark alone, filled with `var(--accent)`.
- `favicon.svg`, `logo.svg`, `logo.png`, `apple-touch-icon.png`, `og.png`:
  all regenerated from the real mark.

The wordmark is outlined, so the typeface is not named anywhere in the files and
no font is loaded. If you ever want that typeface for headings too, it has to be
self-hosted as a subset woff2 — do not add a Google Fonts link, which would break
the no-external-CSS and page-weight rules.
---

## Replace before deploying

### 0. Keep the reference pages accurate

`ai-crawlers/index.html` states a verification date in its own body text and in
`dateModified`. Crawler tokens change â€” Anthropic's current set (`ClaudeBot`,
`Claude-User`, `Claude-SearchBot`) replaced the older `Claude-Web` and
`anthropic-ai`, and OpenAI added `OAI-AdsBot`. **Re-check the five linked source
docs quarterly**, update the table, and move both dates. A reference page selling
technical accuracy is worse than useless once it is stale.

`robots.txt` is grouped by operator with the same verification date in a comment.
Update the two together.

### 1. Calendly — done

All four CTAs point at `https://calendly.com/harsh-orvinlabs/20min`. Every one is
a real `<a href>`, so they work with JavaScript disabled and crawlers see a
genuine link.

Verified behaviour: **zero requests to `assets.calendly.com` on page load**. On
the first CTA click the handler calls `preventDefault`, injects `widget.css` and
`widget.js`, then opens `Calendly.initPopupWidget`. Subsequent clicks reuse the
already-loaded assets — confirmed by request count staying at 2 across clicks on
different CTAs.

If you change the scheduling URL, replace all four occurrences. It must start with
`https://` — the handler only intercepts absolute http(s) URLs, and a relative
value would resolve against orvinlabs.com and 404.

**On the Calendly event itself:** keep the duration at 20 minutes (the copy says
"20 minute call" and "Twenty minutes"), and keep the required invitee question
asking for their website URL. The hero promises live checks before the call; that
field is what makes it deliverable.

### 2. Google Search Console — verify by DNS, not meta tag

The `google-site-verification` meta tag has been **removed**. Verify by DNS TXT
record instead: it covers the whole domain including `www` and every future page,
and it cannot be lost by an HTML edit.

In Search Console pick **Domain** property (not URL prefix), enter `orvinlabs.com`,
and add the TXT record it gives you:

| Type | Host | Value |
| --- | --- | --- |
| TXT | `@` | `google-site-verification=...` paste exactly as given |

Then press Verify. A failure usually just means DNS has not propagated — wait and
retry. Do not re-add the meta tag; one method is enough.
### 3. `sameAs` â€” both arrays are now filled

```json
// Organization
"sameAs": ["https://www.linkedin.com/company/orvin-labs/"]
// Person (#person, Harsh Vardhan Goel)
"sameAs": ["https://www.linkedin.com/in/harsh-vardhan-goel-856446290"]
```

Both are defined on all three pages and must be kept identical across them.
Each also has a matching visible link â€” the company page in the footer, the
personal profile as a `rel="author me"` link in each article byline. `sameAs`
alone is an unverifiable claim; the visible link is the half that corroborates it.

**Add more as they exist.** Each additional controlled profile strengthens entity
resolution. The highest-value next ones for a B2B SaaS consultancy are Crunchbase,
a listing on whichever review platform your category's buyers use (G2, Capterra,
Clutch), and X/GitHub if you use them. Store URLs canonically â€” strip tracking and
view-state query parameters such as `?viewAsMember=true`.

Only list profiles you actually control.

**The `Person` `sameAs` is filled** with the founder's LinkedIn profile, on all
three pages, plus a visible `rel="author me"` link in each article byline.

**The Organization `sameAs` is still empty**, and it is a different job. A
personal profile does not resolve the company. It needs an Orvin Labs *company*
LinkedIn page, and ideally a Crunchbase entry and a listing on a review platform
your category's buyers use. Creating those is not admin â€” it is the first
increment of the entity work the retainer sells.

The `Person` node is defined on all three pages and is the declared `author` of
both reference articles, with a matching visible byline. Keep the three copies
consistent if you edit it.

### 4. Keep these current

- `<lastmod>` in `sitemap.xml` and `"dateModified"` in the WebPage schema are
  both `2026-10-03`. **Update both whenever you change the page.** Freshness is a
  retrieval signal; a stale date is worse than none.
- `foundingDate` is deliberately absent from the Organization schema. Add it only
  when you have the real date â€” a guessed one is a fabricated fact in the exact
  place AI engines treat as authoritative.

---

## Swapping in the real logo

The mark appears in four places:

1. **Header** â€” inline `<svg>` inside `<a class="brand">` in `index.html`.
   Uses `currentColor`, so it inherits the text colour in both themes.
2. **Footer** â€” a second inline `<svg>`, same markup.
3. **Favicon / touch icon / OG image** â€” the standalone files.
4. **`logo.png`** â€” referenced by absolute URL in the Organization JSON-LD.

To swap:

- Replace the two inline `<svg>` blocks with your own path data. Keep
  `fill="currentColor"` / `stroke="currentColor"` so dark mode works, keep
  `role="img"` and the `aria-label`, and keep the `width`/`height` attributes.
- Replace `favicon.svg`, `logo.svg`, `logo.png` (512Ã—512), `apple-touch-icon.png`
  (180Ã—180) and `og.png` (1200Ã—630) with your own files at the same names and
  dimensions. The JSON-LD declares `width: 512, height: 512` for `logo.png` â€”
  update those numbers if you ship a different size.

### Theming

**Dark is the default theme.** `:root` holds the dark token set; the light set
is applied only under `@media (prefers-color-scheme: light)`. A visitor whose OS
is set to light gets the light site, everyone else gets dark. You can force
either one for testing by setting `data-theme="dark"` or `data-theme="light"` on
`<html>`.

### Changing the accent colour

The accent is the brand orange **`#F65E1D`**, taken from `orangelogo.svg`.

**The important finding: white text on `#F65E1D` is only 3.21:1 and fails AA.**
Rather than darkening the brand colour for buttons, the buttons use **near-black
text on the orange fill**, which measures **6.16:1**. That keeps `#F65E1D` exact
everywhere it appears. `--accent-on` is therefore `#0B0A09`, not white — do not
change it back.

On the dark theme the same orange also passes as *text* (6.16:1 on `#0B0A09`), so
one colour covers borders, links and buttons. Only the light theme needs a darker
orange (`#C2410C`, 5.18:1) for link text.

Accent tokens, split by job, in the `:root` (dark) block:

```css
--accent:        #4D7CFE;  /* borders, rules, the "Try this" card, SVG strokes */
--accent-ink:    #8FB0FF;  /* accent used as TEXT â€” must clear 4.5:1 on --bg */
--accent-strong: #2E5FE8;  /* button fill â€” white text must clear 4.5:1 on it */
--accent-hover:  #4472F2;
```

and again in the light block:

```css
--accent:        #1A56DB;
--accent-ink:    #1A4FD0;
--accent-strong: #1A56DB;
--accent-hover:  #123FA6;
```

Measured contrast as shipped:

| Pair | Ratio | Needs |
| --- | --- | --- |
| `--ink` on `--bg` (dark) | 18:1 | 4.5:1 |
| `--ink-2` on `--bg` (dark) | 9.1:1 | 4.5:1 |
| `--ink-3` on `--bg` (dark) | 5.1:1 | 4.5:1 |
| `--accent-ink` on `--bg` (dark) | 9.3:1 | 4.5:1 |
| white on `--accent-strong` (dark) | 5.4:1 | 4.5:1 |
| `--ink-2` on `--bg` (light) | 7.9:1 | 4.5:1 |
| `--ink-3` on `--bg` (light) | 4.7:1 | 4.5:1 |
| white on `--accent-strong` (light) | 6.1:1 | 4.5:1 |

Rules to keep when you substitute your brand colour:

- **The three-token split exists so one brand colour can serve three jobs.**
  Brand colours are usually too dark to read as text on a near-black background
  and too light to carry white button text. Lighten for `--accent-ink`, darken
  for `--accent-strong`, keep the original for `--accent` where it is only ever
  a 1px border or an SVG stroke and contrast rules do not apply.
- `--ink-3` is the tightest pair in both themes. If you shift the neutrals,
  re-check that one first.
- Also update both `theme-color` meta tags and the fill colours inside
  `favicon.svg`, `logo.svg` and the three PNGs.

Check with any contrast checker before shipping. The site sells AI visibility
auditing â€” failing an accessibility check is the same credibility problem as
failing a crawler check.

### Previewing locally

The page is a static file, so `file://` works, but `prefers-color-scheme` and
the `/llms.txt` footer link behave properly only over HTTP. Any static server
will do:

```bash
npx serve .
```

---

## Deploying to GitHub Pages

```bash
cd orvinlabslanding
git init
git add .
git commit -m "Orvin Labs site"
git branch -M main
git remote add origin git@github.com:<your-account>/<your-repo>.git
git push -u origin main
```

Then in the repository on GitHub:

1. **Settings â†’ Pages**
2. **Source**: "Deploy from a branch"
3. **Branch**: `main`, folder `/ (root)`. Save.
4. **Custom domain**: enter `orvinlabs.com`. Save.
   GitHub reads the `CNAME` file, but setting it in the UI is what triggers
   certificate provisioning.
5. Wait for the DNS check to pass, then tick **Enforce HTTPS**. The TLS
   certificate can take up to an hour to issue; the box stays greyed out until
   it does.

No Jekyll processing is needed â€” the files are static and no filename starts
with an underscore. If GitHub ever complains, add an empty `.nojekyll` file.

### DNS records

Set these at your registrar for `orvinlabs.com`. The apex needs four A records
and four AAAA records (GitHub's anycast addresses), plus a CNAME for `www`.

| Type | Name | Value | TTL |
| --- | --- | --- | --- |
| A | `@` | `185.199.108.153` | 3600 |
| A | `@` | `185.199.109.153` | 3600 |
| A | `@` | `185.199.110.153` | 3600 |
| A | `@` | `185.199.111.153` | 3600 |
| AAAA | `@` | `2606:50c0:8000::153` | 3600 |
| AAAA | `@` | `2606:50c0:8001::153` | 3600 |
| AAAA | `@` | `2606:50c0:8002::153` | 3600 |
| AAAA | `@` | `2606:50c0:8003::153` | 3600 |
| CNAME | `www` | `<your-account>.github.io.` | 3600 |

Notes:

- Do **not** also point `@` at a CNAME. A CNAME at the apex is invalid in plain
  DNS; some registrars offer ALIAS/ANAME flattening, and if you use that, point
  it at `<your-account>.github.io` and drop the A/AAAA records.
- Confirm these addresses against GitHub's current documentation before you
  rely on them â€” GitHub has changed them before.
- Verify propagation: `dig orvinlabs.com +short` and `dig www.orvinlabs.com +short`.
- Add the domain under **Settings â†’ Pages â†’ Verified domains** on your GitHub
  account to prevent takeover if the repo is ever deleted.

### Email

`harsh@orvinlabs.com` needs MX records from whichever mail provider you use.
GitHub Pages does not host email and the records above do not affect it.

---

## Post-deploy verification checklist

Run every one of these. The site's own claims depend on them.

**Serving and DNS**

- [ ] `https://orvinlabs.com` loads over HTTPS with a valid certificate.
- [ ] `http://orvinlabs.com` redirects to HTTPS.
- [ ] `https://www.orvinlabs.com` redirects to the apex.
- [ ] `curl -sI https://orvinlabs.com | head -1` returns `HTTP/2 200`.

**Crawler access â€” the thing you sell**

- [ ] `https://orvinlabs.com/robots.txt` loads as `text/plain` and lists every bot.
- [ ] `curl -s -A "GPTBot" https://orvinlabs.com -o /dev/null -w "%{http_code}\n"` returns `200`.
- [ ] Repeat for `OAI-SearchBot`, `ClaudeBot`, `PerplexityBot`, `Googlebot`, `Bingbot`, `Applebot`.
- [ ] `curl -s https://orvinlabs.com | grep -i "noindex"` returns nothing.
- [ ] No Cloudflare or other CDN bot-fighting rule sits in front of the domain.
      This is the single most common cause of an invisible site, and GitHub
      Pages is not behind one by default â€” keep it that way.

**Content is in the source**

- [ ] `curl -s https://orvinlabs.com | grep "750"` finds the price. Every price,
      timeline and deliverable must appear in `curl` output, not just in a browser.
- [ ] `curl -s https://orvinlabs.com | grep "harsh@orvinlabs.com"` finds the mailto.
- [ ] Disable JavaScript in your browser and reload. The full page renders and
      every "Book a call" link navigates to Calendly.

**Discovery files**

- [ ] `https://orvinlabs.com/llms.txt` loads as plain text.
- [ ] `https://orvinlabs.com/llms-full.txt` loads as plain text.
- [ ] `https://orvinlabs.com/sitemap.xml` loads and validates.

**Structured data**

- [ ] [Rich Results Test](https://search.google.com/test/rich-results) on
      `https://orvinlabs.com` â€” FAQ detected, zero errors.
- [ ] [Schema Markup Validator](https://validator.schema.org/) â€” zero errors
      across Organization, ProfessionalService, WebSite, WebPage, FAQPage and
      BreadcrumbList.
- [ ] The `@id` references resolve: `#organization`, `#logo`, `#website`,
      `#webpage`, `#breadcrumb`, `#offers`.
- [ ] `sameAs` is no longer an empty array.

**Head and sharing**

- [ ] `<title>` is 39 characters and contains "AI Visibility" and "B2B SaaS".
- [ ] Canonical resolves to `https://orvinlabs.com` with no redirect.
- [ ] Paste the URL into LinkedIn and Slack â€” the OG card renders with `og.png`.
- [ ] No `{{PLACEHOLDER}}` tokens remain anywhere:
      `curl -s https://orvinlabs.com | grep "{{"` returns nothing.

**Performance and accessibility**

- [ ] Lighthouse (mobile, incognito): 95+ on Performance, Accessibility, Best
      Practices and SEO.
- [ ] Network tab on first load: nothing from `assets.calendly.com`. It must
      only appear after you click a CTA.
- [ ] Total transfer under 100KB.
- [ ] Tab through the whole page. Skip link appears first, every link and
      button shows a visible focus ring, nothing is reachable but invisible.
- [ ] Check at 320px width â€” no horizontal scroll.
- [ ] Check in light mode (OS appearance â†’ Light) â€” the site flips to the light
      token set, explicit background, all text readable. Dark is the default and
      needs no special check beyond a normal look.
- [ ] Set OS "Reduce motion" and reload â€” no fade or translate anywhere.

**Search Console**

- [ ] Property verified.
- [ ] `sitemap.xml` submitted.
- [ ] Request indexing for `https://orvinlabs.com`.

**Baseline for yourself**

- [ ] Run your own audit on `orvinlabs.com` and record the result. If a
      prospect asks whether you've done this to your own site, you want the
      report, not the assertion.
