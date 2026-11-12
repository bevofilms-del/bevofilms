# BevoFilms — GitHub / Netlify project

This project keeps the supplied BevoFilms website layout, colors, logo, tabs, search, interpreter list, library and player. The original sample catalog remains in `index.html`. New uploads are stored in a private Telegram channel and their metadata/file IDs are listed in Supabase.

## Project files
- `index.html` — your original BevoFilms site with native video playback and cloud catalog loading.
- `admin.html` — admin login and upload page.
- `bevo-config.js` — public Supabase project URL and anon key.
- `netlify/edge-functions/upload-to-telegram.js` — validates the admin and sends uploads to Telegram.
- `netlify/edge-functions/telegram-file.js` — streams Telegram-hosted videos to the player without exposing the bot token.
- `supabase-schema.sql` — database table and access policies.

## Setup

1. Create a Supabase project.
2. In Supabase SQL Editor, run `supabase-schema.sql`.
3. Create an admin user in Supabase Authentication. Copy that user's UUID and replace `YOUR_ADMIN_USER_UUID` in the SQL policies, then run the policies (or edit/re-run the file).
4. Edit `bevo-config.js` and add your Supabase Project URL and anon/public key. Never put a Supabase service-role key here.
5. Create a Telegram bot with BotFather. Add the bot as an administrator to a private Telegram channel with permission to post videos. Copy the channel ID (usually starts with `-100`).
6. In Netlify Site configuration → Environment variables, add:
   - `TELEGRAM_BOT_TOKEN` = bot token from BotFather
   - `TELEGRAM_CHANNEL_ID` = private channel ID
   - `SUPABASE_URL` = Supabase project URL
   - `SUPABASE_ANON_KEY` = Supabase anon/public key
   - `SUPABASE_ADMIN_UID` = admin user's UUID
7. Push this folder to a GitHub repository.
8. In Netlify, import the GitHub repository and deploy. Netlify is required for the Edge Functions; GitHub Pages alone cannot safely run the Telegram backend.
9. Open `/admin.html`, sign in with the Supabase admin account, and upload a video. Refresh the main site to see the published media.

## Important notes
- Keep the Telegram bot token only in Netlify environment variables. Never paste it into HTML, JavaScript delivered to browsers, GitHub, or chat.
- Standard Telegram Bot API video uploads are uploads can be up to 50 MB, but the standard Bot API download endpoint is limited to 20 MB. This project therefore limits uploads to 20 MB so they can be played back through the website. If you need larger videos, a different Telegram Bot API hosting arrangement or a dedicated video storage service is needed.
- Telegram is the video file store; Supabase stores titles, poster URLs, descriptions and Telegram file IDs.
- The existing sample media in your original site has blank video URLs, so those sample entries still show the existing fallback message until you add actual playable video files.
- The interpreter photos in the original supplied HTML are still present in its initial source. The accompanying PDF says interpreter photos should not appear; if that requirement is still intended, remove those avatar image URLs from the `interpreters` array before publishing.
- Do not upload copyrighted movies unless you have permission to distribute them.
