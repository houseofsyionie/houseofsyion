# HOUSE OF SYION 🎀

One static page (`index.html`) plus a Supabase backend.

## Run it locally (demo mode)
Open `index.html`. With no keys filled in, everything is stored in the browser.
Default admin PIN in demo mode: `2026`. Change it under Admin > Settings.

## Go live with Supabase
1. Create a Supabase project.
2. SQL editor: run `supabase/schema.sql` (tables, row level security, realtime, daily cron).
   Replace `PROJECT_REF` and `SERVICE_ROLE_KEY` in the last statement first.
3. Authentication > Users > Add user: your admin email and a numeric PIN of 6+ digits as the password.
   The PIN you type on the site is that user's password. Turn off public sign-ups.
4. Open `index.html` and fill `window.SYION_CONFIG` at the top:
   `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `ADMIN_EMAIL` (the user from step 3).
5. Host `index.html` on Netlify, Vercel, Cloudflare Pages or GitHub Pages.

The storefront reads public content with the anon key. Orders, passwords and templates are only readable after the admin signs in.

## 3-day expiry email
```
supabase functions deploy expiry-reminder --no-verify-jwt
supabase secrets set RESEND_API_KEY=... ADMIN_EMAIL=you@example.com FROM_EMAIL="House of Syion <you@yourdomain.com>"
```
The cron job in `schema.sql` calls it every day at 09:00 WIB. It emails orders longer than 1 month that end in exactly 3 days, then marks them as reminded.

## Google Sheets export
Without setup, the Export button copies the table (tab separated) and opens a new sheet; paste with Ctrl/Cmd+V.
For one-click export, create a Google Apps Script web app and put its URL in `SHEETS_WEBHOOK`:
```js
function doPost(e){
  const d = JSON.parse(e.postData.contents);
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sh = ss.getSheetByName(d.sheet) || ss.insertSheet(d.sheet);
  sh.clear();
  sh.getRange(1,1,d.rows.length,d.rows[0].length).setValues(d.rows);
  return ContentService.createTextOutput('ok');
}
```
Deploy it as Web app, execute as you, access: anyone.

## Fonts
Dino Care, Cute Dino, Catcut, Eazy Chat, Sobiscuit and Buycat are not on Google Fonts. The CSS lists them first, so they apply when installed or added with `@font-face`. Until then, Chewy, Baloo 2 and Sniglet stand in.
