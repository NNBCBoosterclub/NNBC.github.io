# NNBC Snack Bar — Migration & Hosting Plan

## Background

The app is a static PWA (no build step) currently hosted on GitHub Pages at
`https://nnbcboosterclub.github.io/NNBC/`. GitHub Pages cannot run server code,
so data that needs to be shared across devices (inventory, orders, receipts)
previously had to be written back to the GitHub repo as JSON via a Personal
Access Token — fragile, slow, and rate-limited.

**Goal:** Replace that architecture with a real backend that supports:
- Shared inventory (stock decrements visible to all users instantly)
- Multi-device order history
- Admin receipt storage with images
- User accounts and profiles with avatars
- Weekly nutrition summaries
- No dedicated server to maintain

---

## Tier Comparison

| Feature | Free (Supabase) | $10/mo | $25/mo |
|---|---|---|---|
| **Hosting** | GitHub Pages (static) | GitHub Pages + Supabase Pro | GitHub Pages + Supabase Pro |
| **Database** | Supabase Free (500 MB) | Supabase Pro (8 GB) | Supabase Pro (8 GB) |
| **Auth** | Supabase Auth (50K MAU) | Supabase Auth (100K MAU) | Supabase Auth (100K MAU) |
| **Realtime** | ✅ Stock sync | ✅ Stock sync | ✅ Stock sync |
| **Storage** | 1 GB (avatars + receipts) | 100 GB | 100 GB |
| **Edge Functions** | 500K invocations/mo | 2M invocations/mo | 2M invocations/mo |
| **OCR (receipt)** | ❌ Manual entry | ✅ Supabase Edge Function + Vision API | ✅ Full automation |
| **Custom domain** | ❌ github.io only | ✅ Custom domain on Pages | ✅ Custom domain + Cloudflare CDN |
| **Backups** | Manual export | Daily auto-backup | Daily auto-backup |
| **Estimated cost** | **$0** | **~$10** | **~$25** |

---

## Tier 1 — Free (Active)

**Stack:** Supabase Free + GitHub Pages

### Upgrade triggers
- Storage approaches 800 MB (avatar/receipt images)
- More than 500 concurrent customers (Supabase free realtime limit)
- Need for automated OCR on receipt photos

### Architecture
```
Browser (GitHub Pages)
    │
    ├── js/db.js  →  Supabase (Free)
    │                   ├── PostgreSQL  (menu_items, orders, profiles, receipts)
    │                   ├── Auth        (email/password)
    │                   ├── Realtime    (Postgres changes → stock sync)
    │                   └── Storage     (avatars, receipt images)
    └── Static assets: HTML, CSS, JS (no build step)
```

---

## Tier 2 — $10/mo

**Stack:** Supabase Pro + GitHub Pages + custom domain

- Upgrade `supabase.com` project to Pro plan (~$25/mo base, or ~$10 with org discount)
- Add custom domain via GitHub Pages settings
- Add receipt OCR: Supabase Edge Function that POSTs image to Google Vision API
  (Vision API free tier: 1K calls/mo; billed at $1.50/K after)

### Downgrade triggers
- Usage stays below free tier limits for 3+ months

---

## Tier 3 — $25/mo

**Stack:** Supabase Pro + GitHub Pages + Cloudflare Workers + Resend email

- Add Cloudflare Workers for edge rate-limiting, Venmo webhook verification
- Add Resend for transactional email (weekly nutrition summary, order receipts)
- Full automated OCR pipeline with itemized product matching

### Downgrade triggers
- Email send volume drops below 100/mo (free Resend tier)
- Edge functions not needed for internal-only tool

---

## Repository Files

| File | Purpose |
|---|---|
| `index.html` | Customer PWA — menu, cart, checkout, auth, profile |
| `admin.html` | Admin panel — products, stock, receipts, orders, status |
| `js/db.js` | Supabase data layer (`window.DB`) — all backend calls |
| `supabase/schema.sql` | One-time database setup script (run in Supabase SQL Editor) |
| `products.json` | Legacy fallback — no longer the source of truth |
| `receipts.json` | Legacy — replaced by Supabase `receipts` table |
| `store-status.json` | Legacy — replaced by Supabase `store_status` table |

---

## Setup Instructions (Tier 1 — Free)

### 1. Create Supabase Project
1. Go to [supabase.com](https://supabase.com) → New Project
2. Choose a region close to your users
3. Save the **Project URL** and **anon public key** from Settings → API

### 2. Run Database Schema
1. In Supabase dashboard → SQL Editor
2. Paste the contents of `supabase/schema.sql` and run it
3. This creates all tables, RLS policies, functions, and seed data

### 3. Create Storage Buckets
In Supabase → Storage:
- Create bucket `avatars` — **Public**
- Create bucket `receipts` — **Private**

### 4. Configure `js/db.js`
Replace the placeholder values at the top of `js/db.js`:
```javascript
const SUPABASE_URL      = "https://YOUR_PROJECT_REF.supabase.co";
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY_HERE";
```

### 5. Deploy to GitHub Pages
Push to `main` branch. GitHub Pages serves the static files; Supabase handles the data.

---

## Migration Protocol (localStorage → Supabase)

Existing localStorage data (orders, products, accounts) is **not** automatically
migrated. Options:

1. **Start fresh** (recommended for internal tool with small user base): existing
   users create new Supabase accounts; admin re-enters any product customizations.

2. **Manual migration**: Export localStorage data from browser console, transform
   to Supabase row format, and INSERT via SQL Editor.

The admin can re-run `initSharedDataFromServer()` at any time to pull the latest
state from Supabase.

---

## Acceptance Tests

- [ ] Customer can browse menu on two different devices and see the same stock levels
- [ ] Cash checkout decrements stock visible on both devices within 5 seconds (Realtime)
- [ ] Venmo checkout opens Venmo with correct amount pre-filled
- [ ] User can register, log in, log out, and log back in
- [ ] Profile page shows order history from Supabase
- [ ] Weekly nutrition totals reflect only the current user's last 7 days
- [ ] Admin can add/edit/delete menu items — changes appear on customer page
- [ ] Admin can upload a receipt and stock quantities update
- [ ] Admin can mark cash orders as paid
- [ ] Store status banner appears on customer page when admin sets "ordered" or "restocked"

---

## Security Notes

- **Anon key is safe to ship in JS** — it is the public role key, not the service role key
- **RLS policies** (in `schema.sql`) prevent cross-user data access for profiles
- **Admin PIN** is hashed with SHA-256 + salt and stored in `sessionStorage` (cleared on browser close)
- **No GitHub PAT** is needed or stored after this migration
- For production hardening: tighten RLS on `orders` and `menu_items` tables to require authenticated users
