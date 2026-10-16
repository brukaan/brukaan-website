# Brukaan website revamp — deployment guide

## What's included
- Responsive studio website with the supplied logo and a visual library of 68 supplied project/interior images.
- Public searchable library at `/library` and project portfolio pages.
- Better Auth email/password authentication at `/sign-in` and `/sign-up`.
- PostgreSQL/Neon-backed project, media and enquiry APIs; authenticated admin workspace at `/admin`.
- Admin project creation/deletion, enquiry status management, media records, and Cloudflare R2 upload endpoint.

## Deploy
1. Push this project to a private GitHub repository.
2. Create a PostgreSQL database (Neon is suitable) and set `DATABASE_URL` in Vercel.
3. Run `docs/database.sql` once against that database using the Neon SQL Editor.
4. Set `BETTER_AUTH_URL` to the canonical production URL (use the same hostname visitors use) and `BETTER_AUTH_SECRET` to a random secret of at least 32 characters.
5. For uploads, create a Cloudflare R2 bucket and API token with object read/write permissions. Attach a public custom domain (recommended) or enable an R2 public URL, then set the four `R2_*` values from `.env.example`. Without R2 configuration, the prebundled 68 images still work; uploads will return a clear configuration error.
6. Deploy on Vercel with the repository's package manager. Build command is `pnpm build` (or `npm run build` if using npm consistently).
7. Visit `/sign-up` to create the first account, sign in, and open `/admin`. After creating the owner account, disable public sign-up by removing the public sign-up page/route or adding an invite-only policy before sharing the site widely. The admin APIs themselves require a signed-in user.
8. Test a contact enquiry, project creation, library listing and (if configured) a file upload on the production deployment.

## Environment
Copy `.env.example` into your local `.env.local` and fill in values. Never commit secrets or `.env.local`.

## Important notes
- The 68 archive images are included under `public/library/001.jpg` through `068.jpg`. The filenames are neutral sequence names because the archive only contained timestamp-based WhatsApp filenames; replace the library captions with approved project names before publishing claims about which project each image depicts.
- The home page and case-study pages still use the curated `lib/site-config.ts` content as the initial editorial content. Project additions made in `/admin` are stored in PostgreSQL; connecting all homepage sections to database-managed content is the next step if you want every page section editable without code.
- For privacy, add a privacy policy and a retention policy for enquiry data before launch. Set up email notifications (SMTP/provider credentials) if you want enquiries forwarded to staff automatically; current enquiries are stored and visible in the admin workspace.
- Verify that all supplied images are approved for public use and optimize large images if the deployment has bandwidth constraints.
