# jayfriesen.com on EmDash + Cloudflare

This rebuilds the Wix site at jayfriesen.com in [EmDash](https://github.com/emdash-cms/emdash). It runs on Cloudflare Workers, with D1 holding the content and R2 holding the images. It started from EmDash's official Cloudflare marketing template.

## Pages

| Page | URL | Content |
|---|---|---|
| Home | `/` | Hero photo, contact buttons, FOH / A1 / Immersive, photos |
| Live | `/live` | Touring and live FOH credits, show posters, testimonials, an Inquire button |
| The Fallout Shelter | `/falloutshelter` | Copy for the 7.1.4 Atmos room, client quotes, photos |
| Privacy Policy | `/privacy-policy` | The existing policy text |

Every page is made of blocks that you can edit in the admin at `/_emdash/admin`. The block types are Hero, Features, Text, Photo gallery, Testimonials and FAQ.

A page you create in the admin gets its URL automatically from its slug. To show it in the nav or footer, add it to a menu under **Menus**.

Images: on the first setup, EmDash copies every photo from the current Wix site into your R2 bucket. After that the site no longer depends on Wix. This only works while the Wix site is still live, so **set this site up before you cancel Wix.**

## Deploy (one time)

### Option A: GitHub + Cloudflare (no terminal)
1. Create a new GitHub repository and upload the contents of this folder.
2. In the Cloudflare dashboard, go to **Workers & Pages → Create → Import a repository** and pick that repo.
   - Build command: `pnpm run build`
   - Deploy command: `pnpm wrangler deploy`
3. On the first deploy, Cloudflare creates the `jayfriesen` D1 database and the `jayfriesen-media` R2 bucket.
4. Open `https://<your-worker>.workers.dev/_emdash/admin` and complete the setup wizard. This applies the seed, which loads all the pages and copies the images.
5. Check the site. Then, in the Worker's settings, go to **Domains & Routes** and add `jayfriesen.com` and `www.jayfriesen.com`. For that, the domain's DNS needs to be on Cloudflare. If it's still registered with Wix, move the nameservers or transfer the domain to Cloudflare first.

### Option B: from your computer
```bash
pnpm install
pnpm wrangler login
pnpm run deploy
```
Then follow steps 4–5 above.

## Local development
```bash
pnpm install
pnpm dev     # http://localhost:4321 and /_emdash/admin
```

## Notes
- Contact is by email (jr@jayfriesen.com) and phone links. Like the Wix site, there's no form.
- The Wix pages Photography, Members, Coming Soon and "Copy of Home" were empty or unused, so they aren't included.
- The theme is in `src/styles/theme.css`: warm amber on near-black in dark mode, off-white in light mode, set in Archivo. Visitors can switch light and dark mode in the footer.
