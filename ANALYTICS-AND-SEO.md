# Analytics, UTM tags, and showing up in Google

Canonical site: **https://andreaboon.netlify.app/**

This file answers: (1) Google discoverability, (2) Google Analytics 4, (3) UTM parameters for resume vs LinkedIn, (4) LLMs and other tools.

---

## 1. Why the site might not appear in Google yet (and what to do)

**Google does not guarantee position** (e.g. “right after LinkedIn”). Ranking depends on crawl/index, query, competition, and your site’s signals. You can still make indexing **likely** and **faster**.

### Already in this repo (after deploy)

- **`robots.txt`** — allows crawlers and points to the sitemap.
- **`sitemap-index.xml`** (generated at build via `@astrojs/sitemap`) — lists your pages for Google.
- **`<link rel="canonical">`** — one preferred URL for the homepage.
- **Open Graph + Twitter Card meta** — better snippets when links are shared (indirectly helps discovery).
- **`Person` JSON-LD** (in `Base.astro` from `site.json` → `seo.person`) — structured data about you and `sameAs` (LinkedIn, GitHub, MentorCruise).

### Your checklist (manual)

1. **Google Search Console** — [search.google.com/search-console](https://search.google.com/search-console)  
   - Add property for `https://andreaboon.netlify.app` (URL-prefix).  
   - Verify (HTML file, meta tag, or DNS).  
   - Submit **Sitemap**: `https://andreaboon.netlify.app/sitemap-index.xml`  
   - Use **URL Inspection** → “Request indexing” for the homepage after major updates.

2. **Named queries** — Search for `Andrea Boonyarungsrit`, `Penphob Boonyarungsrit`, etc. Google may weight **LinkedIn** higher for people’s names; a personal domain or more inbound links over time helps.

3. **Backlinks** — LinkedIn, MentorCruise, GitHub README, conference bios, PDF resume, and articles that link to your site all help crawlers and ranking.

4. **Patience** — First indexing can take days to weeks; keep the site live and avoid blocking crawlers.

---

## 2. Google Analytics 4 (GA4)

### Create the property

1. In [Google Analytics](https://analytics.google.com/), create a **GA4** property for your site.  
2. Create a **Web** data stream for `https://andreaboon.netlify.app`.  
3. Copy the **Measurement ID** (format `G-XXXXXXXXXX`).

### Wire it to Netlify (recommended)

Do **not** commit the ID if you treat it as sensitive (optional). Use an environment variable:

| Netlify → Site → Environment variables | Value |
|----------------------------------------|--------|
| `PUBLIC_GA_MEASUREMENT_ID` | `G-XXXXXXXXXX` |

The layout **`src/layouts/Base.astro`** loads the gtag snippet **only** when `PUBLIC_GA_MEASUREMENT_ID` is set and starts with `G-`.

Redeploy after saving the variable.

### Local testing

```bash
PUBLIC_GA_MEASUREMENT_ID=G-XXXXXXXXXX npm run dev
```

Use GA4 **Realtime** to confirm hits.

### Privacy / compliance

If you have visitors in the EU/UK/CA, you may need a **privacy policy** and (depending on jurisdiction) consent before analytics. That is legal advice territory—use a template or counsel if unsure.

---

## 3. UTM parameters — resume vs LinkedIn vs everything else

**UTMs are query strings you add to your site URL** so GA4 (and some other tools) can attribute sessions to a **source** you chose.

They do **not** change your site; they only tag the inbound URL.

Standard parameters:

| Parameter | Meaning | Examples |
|-----------|---------|----------|
| `utm_source` | Who sent the traffic | `linkedin`, `resume`, `mentorcruise` |
| `utm_medium` | Type of link | `profile`, `pdf`, `email` |
| `utm_campaign` | Your label | `portfolio_2026`, `job_search` |

### Copy-paste examples (same page, different attribution)

Use these **where you paste the link** (LinkedIn Featured, resume PDF, email signature, etc.), not inside Astro unless you want a default.

**LinkedIn profile / Featured**

```text
https://andreaboon.netlify.app/?utm_source=linkedin&utm_medium=profile&utm_campaign=portfolio
```

**Resume PDF (footer or “Portfolio” line)**

```text
https://andreaboon.netlify.app/?utm_source=resume&utm_medium=pdf&utm_campaign=job_search_2026
```

**MentorCruise / ADPList profile “website”**

```text
https://andreaboon.netlify.app/?utm_source=mentorcruise&utm_medium=profile&utm_campaign=mentorship
```

**Cold outbound email**

```text
https://andreaboon.netlify.app/?utm_source=email&utm_medium=personal&utm_campaign=outreach
```

In GA4: **Reports → Acquisition → Traffic acquisition** (and **Explorations**) — filter or break down by `session_source` / `session_campaign` (UTM-driven dimensions appear after data is collected).

**Tip:** Keep a small table of URLs you use in each channel and reuse the same UTMs so trends are comparable.

---

## 4. LLMs, “agents,” and other browsers

- **Traditional search (Google, Bing)** — Covered by sitemap, Search Console, canonical, and links.  
- **Chatbots / LLMs** — There is **no single “submit URL to ChatGPT”** flow. Models may use Bing grounding, training data, or crawls you don’t control. The practical levers are the same: **clear public site**, **consistent name and `sameAs` links**, **mentions on LinkedIn and elsewhere**, and **time**.  
- **Other browsers** — Same HTML; no extra step beyond normal hosting and HTTPS (Netlify provides HTTPS).

---

## Config reference

| Item | Location |
|------|----------|
| Canonical + OG + Person schema | `src/layouts/Base.astro`, `src/content/site.json` → `seo` |
| Sitemap | `@astrojs/sitemap` in `astro.config.mjs` |
| `robots.txt` | `public/robots.txt` |
| GA4 Measurement ID | Netlify env `PUBLIC_GA_MEASUREMENT_ID` |
