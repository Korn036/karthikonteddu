# karthikonteddu.com

The personal-brand site of **Karthik Onteddu** — a single, self-contained web page
intended to be owned and maintained for the long term (a decade or more).

This README is the front door for any **new person or AI agent**: read it first and
you can navigate, edit, secure, and ship the whole project with confidence.

---

## 1. What this is (architecture in one breath)

- **One file does almost everything: [`index.html`](index.html).** HTML structure,
  all CSS (in one `<style>`), and all JavaScript (in one `<script>` "engine room")
  live in that single file. No build step, no framework, no backend.
- **Why single-file?** Durability. Plain static HTML is the most long-lived format
  on the web — it has no dependencies to rot, can be hosted anywhere, and will still
  render in ten years. This is a deliberate choice, not an accident; keep it that way.
- **Hosting:** Cloudflare (static assets, configured in [`wrangler.toml`](wrangler.toml),
  serving the repo root `./`). **Pushing to `main` auto-deploys** the live site.
- **Routing:** a tiny hash router (`#home`, `#books`, …) swaps `.page` sections in and
  out client-side. No server routes.

```
index.html        ← the entire site (structure + styles + scripts)
_headers          ← security headers applied by Cloudflare (CSP, HSTS, etc.)
wrangler.toml     ← Cloudflare deploy config (serves "./")
robots.txt        ← search-engine crawl rules
sitemap.xml       ← search-engine page list
favicon.png · apple-touch-icon.png · og.png   ← icons + social-share image
README.md         ← you are here
```

---

## 2. Map of `index.html` (how to find anything)

The file is organised top-to-bottom. Use your editor's search for the tags in the
**Search for** column — every landmark has a unique, greppable marker.

| Region | What's there | Search for |
| --- | --- | --- |
| `<head>` | title, SEO description, Open Graph/social tags, JSON-LD person schema, fonts | `application/ld+json` |
| Design system | colour tokens (light + dark), fonts, the `--shadow-tile` depth tokens | `1. DESIGN SYSTEM` |
| Styles | base → nav → pages → home → inner pages → **mobile** (sections numbered 1–7) | `2. BASE`, `7. MOBILE` |
| iOS squircle | the superellipse clip-path that makes toolkit icons feel like app icons | `ios-squircle` |
| Pages | HOME · JOURNEY · WRITING · NOW · BOOKS · TOOLKIT · CONNECT | `PAGE: <NAME>` |
| Engine room (JS) | hash routing, scroll reveal, rotating hero phrase, dark mode, count-ups, tic-tac-toe, sparkle/wool easter eggs, GoatCounter analytics | `ENGINE ROOM` |
| Safe config | the bits you (a human) are meant to edit: hero phrases, city, footer quips | `SAFE CONFIG` |

**Editing conventions used throughout the file** (so non-coders can edit safely):

- `✏️ EDIT` — type over the text right there.
- `📋 TEMPLATE` — copy the block between `START` / `END` to add a post, book, app, or chapter.
- `🚫 ENGINE ROOM` — everything below this line is machinery; you never need to touch it.

---

## 3. Common edits

- **Add a book:** search `PAGE: BOOKS`, copy a `┌── BOOK ──┐ … └── END BOOK ──┘`
  block, paste it on top, edit title/author/takeaway. Add `rec` to the class to flag
  a favourite ("✦ my pick").
- **Add a toolkit app:** search `PAGE: TOOLKIT`, copy an `┌── APP ──┐` block, set the
  link + name + one-line why. The logo auto-loads from the app's domain.
- **Add an essay:** search `ESSAY PAGE 01`; duplicate the section + its tile, change the id.
- **Change the rotating hero line / city / quips:** search `SAFE CONFIG`.

---

## 4. Security posture

Security lives in **[`_headers`](_headers)** (read its top comment for a line-by-line
explanation). In short:

- A strict **Content-Security-Policy** allow-lists exactly the few external services
  the site uses (Google Fonts, GoatCounter analytics, logo/cover image hosts, Formspree)
  and blocks everything else — the primary defence against script injection / XSS.
- **HSTS** (force HTTPS), **X-Frame-Options/frame-ancestors** (anti-clickjacking),
  **nosniff**, a tight **Permissions-Policy**, and a sensible **Referrer-Policy**.
- The newsletter form carries a hidden `_gotcha` **honeypot** to deter spam bots.
- There is **no backend and no user-generated content**, so the attack surface is small
  by design. Keep it that way.

**Rule of thumb:** if you add a new external resource (a new image host, script, or
form), add its domain to the matching CSP directive in `_headers`, or it will be
blocked. Verify the live result at <https://securityheaders.com>.

---

## 5. Durability rules (this site is meant to last a decade)

**Do**
- Keep it static and single-file. No build tools, no framework, no CMS for core content.
- Keep URLs stable (`/`, `#now`, `#books` …) so links never break.
- Keep the canonical copy in this repo, plus at least one off-GitHub backup.
- Stay host-portable: because it's static, it can move to Netlify/Vercel/GitHub Pages/
  any bucket in minutes.

**Don't**
- Don't add live/API-dependent widgets (now-playing, GitHub activity) — external APIs
  rot and need secrets a static site shouldn't hold.
- Don't add stateful features (guestbook, comments) casually — each is a backend +
  spam liability.
- **Known fragility to fix:** book covers are hotlinked from Amazon and app logos from
  Google's favicon service. These can break over time — the durable fix is to download
  them into the repo and reference local files. (Tracked as future work.)

---

## 6. Deploy & preview

- **Deploy:** commit and `git push origin main` → Cloudflare rebuilds automatically.
  Commit identity is the GitHub noreply alias (keeps the personal email private).
- **Preview locally:** open `index.html` in a browser, or run any static server
  (e.g. `python -m http.server`) from the repo root to exercise the hash routes.
- **After deploy, confirm it's live:** hard-refresh the site; visible design changes
  (e.g. the squircle toolkit icons) are the quickest "it shipped" tell.
