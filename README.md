# اُڀار ڊجيٽل — UBHAR Digital

**خبر، نظر ۽ آواز عوام جو.**

A Sindhi-first, right-to-left, mobile-first digital newsroom built with **Next.js 15 (App Router) · React 19 · TypeScript · Tailwind CSS v4**, ready to deploy on **Vercel**.

---

## 1. What's included

| Area | Details |
|---|---|
| **Homepage** | Top utility bar (Sindhi + Hijri date, socials, language selector, search) · logo header · sticky red-accent navigation · RTL breaking-news ticker · hero (1 main + 3 secondary) · latest news + most-read sidebar · editorial sections for سنڌ، سياست، پاڪستان، ڪراچي، ضلعي خبرون، دنيا، سماج، تعليم، ثقافت · black video section · opinion/analysis · عوام جو آواز (public voice) · newsletter band · footer |
| **Article page** `/news/[slug]` | Category, headline, summary, author, published + updated time, reading time, featured image or video player, Sindhi-optimised body typography, tags, Facebook/WhatsApp/X/copy-link sharing, comments placeholder, related news, sidebar (most read, latest, newsletter, ad slot) |
| **Category pages** `/category/[slug]` | `latest`, `sindh`, `politics`, `pakistan`, `karachi`, `districts`, `world`, `society`, `education`, `culture`, `sports`, `opinion`, `videos`, `public-voice` — title, description, featured story, list, numbered pagination (`?page=2`), sidebar |
| **Search** | Modal (header icon, `/` or `Ctrl K`) with live results + full `/search?q=` page. Matches Sindhi, Urdu-script and English keywords (Urdu ک ی ہ are normalised to Sindhi ڪ ي ه) |
| **Pages** | About, Contact (validated form, WhatsApp), Advertise With UBHAR, Editorial Policy, Privacy Policy, Terms, branded 404, loading skeleton, error page |
| **Admin** `/admin` | Demo content dashboard: create / edit / delete / publish-unpublish, category, type, author, summary, body, image upload preview, video URL, featured + hero order, breaking flag, breaking-ticker manager, JSON export |
| **SEO** | Per-page titles & descriptions, canonical URLs, Open Graph, X cards, `NewsArticle` / `OpinionNewsArticle` + `VideoObject` JSON-LD, `NewsMediaOrganization` + `WebSite` (search action) schema, breadcrumbs (UI + schema), `sitemap.xml`, `robots.txt`, web manifest, favicons |
| **Performance** | Server components by default; only interactive pieces ship JS · `next/image` (AVIF/WebP, responsive `sizes`, lazy loading, fixed aspect ratios → no layout shift) · inline SVG icons (0 KB icon library) · CSS-only ticker · fonts self-hosted by `next/font` with `display: swap` · click-to-load YouTube/Facebook players |

> **Sample content:** all 55 articles, authors and images are clearly marked placeholders written for this preview. They make no claims about real events. A notice is shown on articles and in the footer while `site.showSampleNotice` is `true` (in `lib/site.ts`) — set it to `false` at launch.

---

## 2. Project structure

```
ubhar-digital/
├── app/
│   ├── layout.tsx              ← <html lang="sd" dir="rtl">, fonts, global metadata
│   ├── globals.css             ← Tailwind v4 + UBHAR design tokens, RTL & Sindhi typography
│   ├── (site)/                 ← public website (shares header, ticker, footer, search)
│   │   ├── layout.tsx
│   │   ├── page.tsx            ← homepage
│   │   ├── news/[slug]/page.tsx
│   │   ├── category/[slug]/page.tsx
│   │   ├── search/page.tsx
│   │   ├── about/ contact/ advertise/ editorial-policy/ privacy-policy/ terms/
│   │   ├── loading.tsx  error.tsx  not-found.tsx
│   ├── admin/                  ← demo dashboard (noindex)
│   ├── api/search|newsletter|contact/route.ts
│   ├── sitemap.ts  robots.ts  manifest.ts  not-found.tsx
│   └── icon.png  apple-icon.png  favicon.ico
├── components/                 ← Header, TopBar, MainNav, MobileNavigation, BreakingNewsTicker,
│                                 HeroNews, NewsCard, CategorySection, VideoSection, VideoNewsCard,
│                                 OpinionSection, MostRead, PublicVoice, Newsletter, Footer,
│                                 SearchModal, SocialShare, Sidebar, Breadcrumbs, Pagination,
│                                 VideoEmbed, ContactForm, Logo, Icon, admin/AdminDashboard …
├── lib/
│   ├── types.ts                ← content model (Article, Category, Author, BreakingItem)
│   ├── content.ts              ← ALL data access goes through here (swap for a CMS)
│   ├── search.ts               ← Sindhi/Urdu/English normalisation + ranking
│   ├── date.ts                 ← Sindhi dates, weekdays, Hijri date, time labels
│   ├── seo.ts                  ← metadata + JSON-LD builders
│   ├── site.ts                 ← site name, slogan, URLs, social links, navigation
│   └── categories.ts
├── data/                       ← local content (JSON): articles, categories, authors, breaking
└── public/
    ├── images/logo/            ← official logo files (cut from your artwork)
    ├── images/news/            ← temporary branded placeholder images
    ├── images/og-default.jpg   ← default social-share image
    └── videos/ubhar-sample.mp4 ← sample uploaded video
```

---

## 3. Run locally

Requirements: **Node.js 18.18+** (Node 20 or 22 recommended).

```bash
npm install
npm run dev        # http://localhost:3000
```

Production check (do this once before your first deploy):

```bash
npm run build && npm start
npm run typecheck  # optional: TypeScript only
```

Copy `.env.example` to `.env.local` and set `NEXT_PUBLIC_SITE_URL` to your domain (used for canonical URLs, sitemap and share links).

---

## 4. Deploy to Vercel

**Option A — GitHub (recommended)**
1. Push this folder to a new GitHub repository.
2. On vercel.com → **Add New… → Project** → import the repository. Vercel detects Next.js automatically; no settings need changing.
3. Under **Settings → Environment Variables**, add `NEXT_PUBLIC_SITE_URL = https://your-domain` (optional until you have a domain — Vercel's production URL is used automatically).
4. Deploy. Every push to `main` redeploys.
5. Add your domain under **Settings → Domains**.

**Option B — CLI**
```bash
npm i -g vercel
vercel          # preview
vercel --prod   # production
```

---

## 5. The official UBHAR logo

The logo files in `public/images/logo/` were cut directly from the artwork you supplied (`اُڀار | UBHAR` with the red circle), with the dark background made transparent. Nothing was redrawn or stretched — the `Logo` component always keeps the original aspect ratio.

| File | Used in | Size |
|---|---|---|
| `ubhar-logo.png` | header, mobile header, admin, 404 | 365 × 344 |
| `ubhar-logo-tagline.png` | footer, About page (includes "سوال کان شعور تائين") | 391 × 416 |
| `app/icon.png`, `app/apple-icon.png`, `app/favicon.ico` | browser tab, home-screen icon | 512 / 180 / 48 |
| `public/images/og-default.jpg` | default Facebook / WhatsApp / X preview | 1200 × 630 |

**To use your master files instead** (recommended — e.g. a transparent PNG or SVG export from the designer):
1. Replace `public/images/logo/ubhar-logo.png` and `ubhar-logo-tagline.png` with files of the same name.
2. If the proportions differ, update `w` and `h` in `components/Logo.tsx` (`LOGOS` object).
3. Replace `app/icon.png` (512×512), `app/apple-icon.png` (180×180) and `app/favicon.ico`.
4. Optionally design a new `public/images/og-default.jpg` (1200×630).
5. For an SVG, change the paths in `lib/site.ts` (`logo`, `logoWithTagline`) to `.svg`.

---

## 6. Managing and publishing news

### Today: local JSON + demo admin

All content lives in `data/*.json` and is read **only** through `lib/content.ts` — no article is hard-coded in a component.

**Using the dashboard (`/admin`):**
1. Open `https://your-site/admin` (not indexed by search engines).
2. Create, edit, delete or publish/unpublish articles; set *featured* (+ hero order 1–4) and *breaking*; manage the ticker under **بريڪنگ نيوز**.
3. Changes are saved as drafts in that browser. To publish, open **شايع ڪريو / ايڪسپورٽ**, download `articles.json` and `breaking.json`, replace the files in `data/`, put any new images in `public/images/news/`, then commit and push — Vercel redeploys in about a minute.

**Editing JSON directly:** each article follows the `Article` type in `lib/types.ts`:

```jsonc
{
  "id": "a056",
  "slug": "karachi-new-park-opens",          // English URL slug
  "type": "news",                            // news | video | opinion | public-voice
  "category": "karachi",
  "title": "…", "summary": "…",
  "body": [
    "First paragraph…",
    { "type": "h2", "text": "Sub-heading" },
    { "type": "quote", "text": "…", "cite": "Source" }
  ],
  "authorId": "karachi-reporter",
  "image": "/images/news/karachi-park.jpg", "imageAlt": "…", "imageCaption": "…",
  "publishedAt": "2026-11-19T09:30:00+05:00",
  "published": true,
  "featured": true, "heroRank": 1,           // optional: homepage hero
  "breaking": true,                          // optional: adds to ticker
  "video": { "url": "https://www.youtube.com/watch?v=…", "duration": "05:20" },
  "tags": ["ڪراچي", "Karachi", "کراچی"],     // Sindhi + English + Urdu helps search
  "location": "ڪراچي"
}
```

Videos accept **YouTube, Facebook or a direct `.mp4`** URL; third-party players load only when the reader presses play.

### Next step: connect a CMS

Only `lib/content.ts` needs to change. Replace `localSource` with a source that fetches from your CMS and maps results to the types in `lib/types.ts`:

```ts
// lib/content.ts — example with Supabase
import { createClient } from '@supabase/supabase-js';
const supabase = createClient(process.env.SUPABASE_URL!, process.env.SUPABASE_ANON_KEY!);

const supabaseSource: ContentSource = {
  articles: async () => (await supabase.from('articles').select('*').eq('published', true)).data as Article[],
  authors: async () => (await supabase.from('authors').select('*')).data as Author[],
  breaking: async () => (await supabase.from('breaking').select('*')).data as BreakingItem[],
};
const source: ContentSource = supabaseSource;
```

- **Sanity** (best editor experience): create schemas mirroring `Article`, `Author`, `Category`, `BreakingItem`; use `@sanity/client` with GROQ in the same three functions; images come from `cdn.sanity.io` (already allowed in `next.config.mjs`). Sanity Studio then replaces `/admin`.
- **Supabase** (database + storage + auth): tables matching the types; store images in Supabase Storage; make `/admin` save through authenticated API routes instead of `localStorage`, and protect it with Supabase Auth.
- **Headless WordPress / Contentful**: same pattern — fetch, map, return.
- Pages already use `revalidate = 300` (refresh every 5 minutes). For instant updates, add an API route that calls `revalidatePath('/')` from a CMS webhook.

> Before connecting a real backend to `/admin`, add authentication (e.g. Supabase Auth, NextAuth, or Vercel password protection). The current dashboard cannot change the live site, so it is safe as a demo.

### Newsletter & contact forms

Both forms validate on the client **and** server and show proper success/error states. They are placeholders until you connect a service:
- `app/api/newsletter/route.ts` → Mailchimp, Brevo, Resend Audiences, or a Supabase table.
- `app/api/contact/route.ts` → send email with Resend/SMTP or store in a database.

The success messages currently say "(ڊيمو …)" so nobody is misled — edit them once connected.

---

## 7. Customising

- **Social links, email, WhatsApp, address:** `lib/site.ts` (currently placeholder `@UBHARdigital` handles).
- **Navigation:** `mainNav` / `footerNav` in `lib/site.ts`.
- **Categories:** `data/categories.json` + the `CategorySlug` type in `lib/types.ts`.
- **Colours:** `@theme` block at the top of `app/globals.css` (`--color-ink #0B0B0F`, `--color-charcoal #17171C`, `--color-brand #E50920`, `--color-mist #F1F1F3`, `--color-steel #8A8A94`).
- **Fonts:** `app/layout.tsx` — Noto Sans Arabic (headlines/UI), Noto Naskh Arabic (article body), Inter (English & numbers).
- **Images:** the files in `public/images/news/` are temporary on-brand graphics. Replace them with real news photos (16:9, ≥1200 px wide; `next/image` resizes and compresses automatically).

---

## 8. RTL & language notes

- `<html lang="sd" dir="rtl">`; layouts use logical properties (`start`/`end`, `ps`/`pe`) so everything mirrors correctly.
- English words, emails, phone numbers, handles and durations are wrapped in the `.ltr` class (bidi-isolated) so they display correctly inside Sindhi text.
- The ticker scrolls left→right so each Sindhi headline is read from its beginning; it pauses on hover/focus and stops for users who prefer reduced motion.
- **Urdu / English later:** the language selector is in place. Recommended path: move routes under `app/[lang]/…`, add `lang` to `Article`, and set `dir` per language in the root layout.

---

## 9. How this build was verified

This version was produced in an environment without internet access, so `npm install` / `next build` could not be run there. Instead:
- Every page and component was compiled with esbuild and server-rendered with React 19 (with small stand-ins for `next/link`, `next/image`, `next/font`), styled with Tailwind v4, and checked in Chromium at 1440 px, 768 px and 390 px — no horizontal overflow, no console or hydration errors.
- Interactions were tested end-to-end: mobile menu (open / Esc), search modal (Urdu-script query `کراچی` → 6 Sindhi results; empty state), newsletter and contact validation + success, copy-link sharing, video playback, admin create / delete / export.
- A strict TypeScript pass was run over `app/`, `components/` and `lib/`.

👉 **Please run `npm install && npm run build` once locally before deploying** — it is the first build with the real Next.js toolchain. If anything is reported, it should be minor and easy to fix.

---

## 10. Future improvements

1. **Real CMS + authenticated admin** (Sanity Studio or Supabase), image uploads to cloud storage, editorial roles (reporter → editor → publish).
2. **Instant publishing** via CMS webhooks + `revalidatePath` / `revalidateTag`.
3. **Urdu and English editions** (`/ur`, `/en`) with hreflang tags.
4. **Comments** (moderated) — e.g. Supabase table or a hosted service.
5. **Live view counts / trending** from analytics (Vercel Analytics, Plausible or GA4) to drive "وڌيڪ پڙهيل".
6. **Push notifications & PWA offline reading** for breaking news.
7. **Google News / Discover**: News sitemap (`news-sitemap.xml`), Publisher Center registration, AMP-free fast article pages (already lightweight).
8. **Video hub** with YouTube channel sync, playlists and Shorts.
9. **Live coverage pages** (election night, budget day) with auto-updating timelines.
10. **Ad management** (Google Ad Manager slots already have a sidebar placeholder) and sponsored-content labels.
11. **Author profile pages**, tag pages and an archive by date.
12. **Accessibility audit** with Sindhi screen-reader testing; dark mode.

---

© UBHAR Digital — All Rights Reserved
