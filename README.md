# Study Resource Organizer — Cloudflare Pages + D1

A small responsive study library with create, list, update, delete, search, and filters. It uses Cloudflare Pages static assets, a Pages Function API, and D1 (SQLite).

## Repository layout

```text
public/
  index.html
  assets/style.css
  assets/app.js
functions/api/[[path]].js
schema.sql
wrangler.jsonc
package.json
README.md
```

## Deploy using the Cloudflare dashboard

1. Push the project files to your GitHub repository's `main` branch.
2. In Cloudflare, open **Workers & Pages → Create application → Pages → Connect to Git**.
3. Choose the repository that contains these files.
4. Set production branch to `main`.
5. Set **Root directory** to `/` (repository root) if these files are at the root. If uploaded inside a subfolder, set the root directory to that subfolder.
6. Set framework preset to **None**, build command to `exit 0`, and build output directory to `public`.
7. Create a D1 database named `study-resource-organizer-db` (or choose a different name and update `wrangler.jsonc` if using the CLI).
8. In the Pages project, open **Settings → Functions → D1 database bindings** (the exact label may vary) and add a binding named `DB` pointing to the database you created.
9. Open the D1 database's Console and execute all of `schema.sql`.
10. Redeploy the Pages project, then test the `*.pages.dev` URL.

## Verify deployment

- Open `/api/health`; expected JSON: `{"ok":true,"database":"connected"}`.
- Open `/api/resources`; expected JSON with `resources` array.
- Add, edit, and delete a resource through the interface, then reload to verify persistence.

## CLI option

```bash
npm install
# Update the D1 database_id in wrangler.jsonc with your actual database ID.
npx wrangler pages dev public
npx wrangler d1 execute study-resource-organizer-db --remote --file=./schema.sql
npx wrangler pages deploy public
```

For the CLI route, authenticate using `npx wrangler login` in your own terminal and ensure the configured D1 database ID is correct.

## Important security note

This is a starter/demo app with no authentication or per-user authorization. The API allows anyone who can reach the public site to create, modify, or delete shared resources. Add access control before using it for private or sensitive institutional data.
