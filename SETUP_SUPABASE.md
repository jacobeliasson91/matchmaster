# Publicera Matchschema via GitHub Pages

När appen ligger på GitHub Pages öppnar alla användare samma webbadress. Matchdata och planändringar lagras i Supabase, så de följer med när ni loggar in från en annan dator. GitHub Pages är bara webbhotellet för HTML-filerna.

## Första gången: konfigurera Supabase

1. Skapa projekt i [Supabase Dashboard](https://supabase.com/dashboard).
2. Kör SQL-filen `supabase/migrations/20261023000000_create_match_schedule.sql` i **SQL Editor**.
3. Skapa användare under **Authentication → Users → Add user**.
4. Lägg SvFF-nyckeln som `SVFF_API_KEY` under **Edge Functions → Secrets**.
5. Skapa Edge Function `svff-games` via **Edge Functions → Deploy a new function → Via Editor**. Klistra in `supabase/functions/svff-games/index.ts`, låt **Verify JWT** vara aktiverat och deploya.
6. Öppna `supabase-config.js` i projektroten. Ersätt `YOUR_PROJECT_REF` med projektets Project URL och `YOUR_SUPABASE_PUBLISHABLE_KEY` med projektets Publishable key (äldre projekt kan kalla den `anon`). Den publika nyckeln får ligga i webbsidan eftersom databasen skyddas av RLS. Lägg aldrig service-role key eller SvFF-nyckeln där.

## Publicera webbappen

1. Lägg projektets filer i ett GitHub-repository och skicka upp ändringarna.
2. I GitHub: öppna **Settings → Pages**.
3. Välj **Deploy from a branch**, välj `main` (eller er standardgren) och mappen `/(root)`, och spara.
4. Vänta tills GitHub visar webbplatsens adress. Den blir typiskt `https://<konto>.github.io/<repository>/`.
5. I Supabase: öppna **Authentication → URL Configuration** och sätt **Site URL** till er GitHub Pages-adress. Lägg även adressen under **Redirect URLs** om Supabase ber om det.

## Använd appen

Öppna GitHub Pages-adressen. Klicka **Importera matcher** och logga in med Supabase-kontot första gången. Importen startar direkt efter inloggningen. Matcherna hämtas från SvFF och sparas i Supabase. Planändringar synkas när ni är inloggade. På en ny dator öppnar ni samma adress och loggar in med samma användare.

`outputs/index.html` är en förhandskopia. GitHub Pages publicerar `index.html` i projektroten tillsammans med `supabase-config.js` och `supabase/`.

## Filer

- `index.html` – appen som GitHub Pages visar.
- `supabase-config.js` – Project URL och publik nyckel.
- `supabase/functions/svff-games/index.ts` – säker SvFF-import.
- `supabase/migrations/20261023000000_create_match_schedule.sql` – tabell och RLS-policy.
- `supabase/config.toml` – inställningar för Edge Function.

Guider: [GitHub Pages](https://docs.github.com/pages) · [Supabase Edge Functions via Dashboard](https://supabase.com/docs/guides/functions/quickstart-dashboard) · [Supabase secrets](https://supabase.com/docs/guides/functions/secrets) · [Row Level Security](https://supabase.com/docs/guides/database/secure-data)