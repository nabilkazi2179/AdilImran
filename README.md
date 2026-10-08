# AADIL AND IMRAN 40+ CRICKET LEGENDS TOURNAMENT
## Player Registration & Auction App

This is a fresh copy of the cricket registration/auction app, set up for this
new tournament. It does **not** share any data with a previous tournament's
app — different database, different photo storage, different link.

- Player, owner, and auction data is stored in a **Postgres database** (a free
  hosted one from a provider like Supabase or Neon works fine — see setup below).
- Player photos and payment-proof screenshots are stored in **Cloudinary**
  (free tier), in the `legends-tournament/` folder — separate from any
  earlier tournament's Cloudinary folder.
- The app itself runs as one small Node.js web service (no separate database
  server to install — Postgres and Cloudinary are both free cloud services).

---

## What's different from the previous tournament's app

- **Registration cap: 156 players** (was 96).
- **Captain pre-pick only.** Before the auction, the admin can assign **one
  registered player directly to each team as Captain** (fixed price ₹10,000,
  any role) — from the Auction tab's "Captain pick" section. There is no
  separate "Retained" step needed for this. Every other player — including
  future captains-in-waiting, batsmen, bowlers, all-rounders, wicket-keepers —
  goes through the **live auction** as usual.
- **New branding**: title, ID cards, PDFs, and the WhatsApp roster export all
  say "AADIL AND IMRAN 40+ CRICKET LEGENDS TOURNAMENT".
- **Redesigned ID card** — gold/black/red design matching the tournament
  poster, with Name, Role, Location, Mobile, and Age Category all shown at the
  same size in one aligned column.
- Age Category field is unchanged (48 Plus / Under 48), defaulting to Under 48.

Everything else — Register / Teams / Admin / Auction / Owner Login tabs, Admin
vs Super Admin split, duplicate-mobile and duplicate-payment-screenshot
checks, self-service owner bidding, custom team crests, Excel export, and the
self-serve "already registered? get your ID card" lookup — works exactly like
before.

---

## One-time setup

### 1. Create a free Postgres database
Use any free Postgres host — [Neon](https://neon.tech) or
[Supabase](https://supabase.com) both work well. Create a new project/database
and copy its **connection string** (`DATABASE_URL`) — it looks like
`postgres://user:password@host/dbname`. You don't need to create any tables
yourself; the app creates everything it needs automatically the first time it
starts.

### 2. Create a free Cloudinary account
Go to [cloudinary.com](https://cloudinary.com), sign up for the free tier, and
from your dashboard copy three values: **Cloud name**, **API key**, and
**API secret**.

### 3. Install dependencies (only if running locally first, to test)
```
npm install
```

### 4. Set environment variables
The app reads these from the environment (set them in Render's dashboard, or
in a local `.env` file / your terminal if testing locally first):

| Variable | What it is |
|---|---|
| `DATABASE_URL` | Your Postgres connection string from step 1 |
| `CLOUDINARY_CLOUD_NAME` | From your Cloudinary dashboard |
| `CLOUDINARY_API_KEY` | From your Cloudinary dashboard |
| `CLOUDINARY_API_SECRET` | From your Cloudinary dashboard |
| `ADMIN_PASSWORD` | Password for the Admin tab (change from the default!) |
| `SUPERADMIN_PASSWORD` | Password for Super Admin (edit/delete/override cap) |

### 5. Run it
```
npm start
```
You should see `Running at: http://localhost:3000` — open that in a browser
to test locally before deploying.

---

## Deploying so everyone can use it (Render)

1. Push this code to its own new GitHub repository (a separate repo or branch
   from the old tournament's app — don't push over the old one).
2. Go to [render.com](https://render.com), sign in, click **New +** →
   **Blueprint**, and pick this repository. Render reads the included
   `render.yaml` and sets up the web service automatically.
3. Under the service's **Environment** tab, fill in `DATABASE_URL`,
   `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`,
   `ADMIN_PASSWORD`, and `SUPERADMIN_PASSWORD` with the values from setup
   above.
4. Click **Deploy**. Render gives you a new live URL (something like
   `https://legends-tournament-registration.onrender.com`) — that's the new
   link you share with players. It's completely separate from any previous
   tournament's link.
5. Log into the **Admin** tab and use "Upload / Replace QR" to add this
   tournament's payment QR code — it's stored fresh in the new database, so it
   won't carry over from before.

---

## Admin vs Super Admin

- **Admin** — view the roster, export Excel, upload the QR code, run the
  auction (sell/undo/add owners, assign the Captain pick).
- **Super Admin** — everything Admin can do, plus editing/deleting a
  registration and forcing registration open past the 156-player cap.

Change both passwords from the Render environment variables before going
live, and only share the Super Admin password with whoever should be able to
edit/delete records or override the cap.

## Registration cap (156 players)

Registration auto-closes once 156 players are currently registered (deleting
a player frees a slot). Super Admin can force it open past the cap from the
Admin tab if needed.

## Assigning a team's Captain (before the auction)

In the **Auction** tab, after adding all 12 owners/teams, the **Captain pick**
section lists each team with a dropdown of every currently available
registered player (any role). Pick the pre-decided captain for each team and
click **Assign** — it's a direct ₹10,000 assignment, no auction round needed.
Every other player for that team is then bought in the live auction as usual.

## Backing up your data

Your data lives in your Postgres database and Cloudinary account, not in a
local file — back up by exporting the Excel roster regularly from the Admin
tab, and note down your Postgres/Cloudinary credentials somewhere safe.

## Troubleshooting

- **Registration fails with an error message** → the popup shows the exact
  reason (e.g. missing photo, server not reachable, over the cap).
- **Forgot the admin password** → check the `ADMIN_PASSWORD` /
  `SUPERADMIN_PASSWORD` environment variables in your Render dashboard.
- **Photos not uploading** → double-check the three `CLOUDINARY_*` environment
  variables are set correctly.

## Rebrand notes (AADIL AND IMRAN 40+ CRICKET LEGENDS TOURNAMENT)
- Sponsor: SanSam Enterprises (logo: `public/sansam-logo.jpg`). More sponsors: edit the `SPONSORS` array in `public/index.html`.
- Gold & black theme, premium ID card, Live Stage (projector) mode in the Auction tab, team stats on the Teams tab,
  event countdown (set it in the Auction tab > Event details; stored in the `settings` table).
- Default admin passwords changed to `legends@2026` / `legends@Super2026` (env vars / saved passwords still win).

## Update: KonkanTech + sponsors + title font
- KonkanTech logo ("Made by") added to the header, footer, ID card, PDFs, auction screen and ticker (`public/konkantech-logo.jpg`).
- Sponsors now read "SanSam Enterprises & Al Nasr Group" everywhere (Al Nasr Group is text-only until a logo is added).
- "AADIL AND IMRAN" uses the Cinzel Decorative font on the site, ID card and auction screen (PDFs use a serif font).
