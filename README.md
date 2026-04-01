# personal-website-astro

**Astro static personal site with JSON-driven content; Netlify-ready.**

Edit copy and lists in **`src/content/site.json`**. Layout and styles live under `src/` and `public/`.

## Develop

```bash
npm install
npm run dev
```

Build: `npm run build` → `dist/`.

## Deploy (Netlify)

| Setting | Value |
|--------|--------|
| Build command | `npm run build` |
| Publish directory | `dist` |

Contact form: **Netlify Forms** (see `data-netlify` on the contact form in `src/pages/index.astro`).

## Publish to GitHub

Sign in: `gh auth login`, then from this folder:

```bash
gh repo create personal-website-astro --public --source=. --remote=origin --push \
  --description "Astro static personal site with JSON-driven content; Netlify-ready."
```

If the empty repo already exists:

```bash
git remote add origin https://github.com/YOUR_USERNAME/personal-website-astro.git
git push -u origin main
```
