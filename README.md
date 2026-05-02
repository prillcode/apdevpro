# AP Cloud / AP Dev — Marketing Site

Modern static marketing website for a solopreneur software consulting brand.  
Built with **Astro**, **TypeScript**, and **Tailwind CSS**, deployed to **Cloudflare Pages**.

**Domains:**
- `apcloud.pro` / `www.apcloud.pro`
- `apdev.pro` / `www.apdev.pro`

---

## Quick Start

```bash
# Install dependencies
npm install

# Start local dev server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Project Structure

```
src/
├── components/        # Reusable UI components
│   ├── Layout.astro       # Base HTML layout with SEO meta + OG tags
│   ├── Header.astro       # Sticky nav with mobile menu
│   ├── Footer.astro       # Footer with links + domains
│   ├── Hero.astro         # Hero section (reusable)
│   ├── ServiceCard.astro  # Service card with icon + tech badges
│   ├── CaseStudyCard.astro # Case study card with problem/approach/outcome
│   ├── Badge.astro        # Tiny badge component
│   └── CTASection.astro   # Call-to-action section block
├── pages/             # Route pages
│   ├── index.astro        # Home
│   ├── services.astro     # Services
│   ├── work.astro         # Work / Case Studies
│   ├── about.astro        # About
│   └── contact.astro      # Contact (static form)
├── styles/
│   └── global.css         # Tailwind directives + custom utility classes
└── env.d.ts
public/
├── favicon.svg
└── robots.txt
```

## Pages

| Page    | Path         | Description                                          |
|---------|-------------|------------------------------------------------------|
| Home    | `/`         | Hero, services preview, how I help, featured project  |
| Services| `/services` | All service cards, engagement models                  |
| Work    | `/work`     | Case studies with problem/approach/outcome            |
| About   | `/about`    | Bio, focus areas, tech stack, why work with me        |
| Contact | `/contact`  | Static contact form (ready to wire up)                |

## Tech Stack

- **Astro** — Static site generation, zero JS by default
- **TypeScript** — Type safety throughout
- **Tailwind CSS** — Utility-first styling with custom theme
- **`@astrojs/sitemap`** — Automatic sitemap generation
- **`@astrojs/tailwind`** — Tailwind integration for Astro

## Design System

See `tailwind.config.mjs` for the full theme. Key tokens:

| Token | Value |
|-------|-------|
| Background | `navy-950` (#050a14) |
| Card bg | `white/[0.03]` glassmorphism |
| Accent 1 | `electric-500` (#3b82f6) |
| Accent 2 | `cyan-500` (#06b6d4) |
| Accent 3 | `violet-500` (#8b5cf6) |
| Text primary | `slate-100` |
| Text secondary | `slate-400` |
| Font | Inter (sans), JetBrains Mono (mono) |

Custom CSS utilities defined in `src/styles/global.css`:
- `.glass-card` — Frosted glass card base
- `.glass-card-hover` — Hover transition for cards
- `.gradient-text` — Blue-cyan-violet gradient text
- `.gradient-border` — Animated gradient border via pseudo-element
- `.glow-orb` — Ambient colored orbs
- `.bg-grid` — Subtle grid pattern

## Deployment — Cloudflare Pages

### First-time setup

1. Push this repo to GitHub or GitLab
2. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com)
3. Go to **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
4. Select your repo
5. Configure build settings:

   | Setting | Value |
   |---------|-------|
   | **Framework preset** | Astro |
   | **Build command** | `npm run build` |
   | **Build output directory** | `dist` |
   | **Node.js version** | 18+ (or latest LTS) |

6. Click **Save and Deploy**

### Custom Domains

After initial deployment, add custom domains in **Pages** → your project → **Custom domains**:

1. Click **Set up a custom domain**
2. Add each domain one at a time:
   - `apcloud.pro`
   - `www.apcloud.pro`
   - `apdev.pro`
   - `www.apdev.pro`

3. For each domain, Cloudflare will provide DNS records. If your domains are already on Cloudflare, they'll be configured automatically. Otherwise, add the CNAME records at your DNS provider.

4. Wait for SSL certificate provisioning (usually a few minutes).

### Redirect www → apex (or vice versa)

Cloudflare Pages automatically creates a redirect from the `www` subdomain to the apex domain (or vice versa, depending on which you set as primary). You can configure this in **Custom domains** settings.

### Environment Variables

No environment variables are required for the static site. If you later wire up the contact form to a Cloudflare Worker or third-party service, add those keys in **Pages** → **Settings** → **Environment variables**.

## Wiring the Contact Form

The contact form on `/contact` is currently a static placeholder. To make it functional:

### Option A — Formspree (recommended, free tier)

1. Create an account at [formspree.io](https://formspree.io)
2. Create a new form and copy your form endpoint URL
3. In `src/pages/contact.astro`, find the `<form>` element and change:
   ```astro
   action="https://formspree.io/f/YOUR_FORM_ID"
   ```
4. Formspree will forward submissions to your email. No server code needed.

### Option B — Cloudflare Workers + Email Routing

1. Create a new Cloudflare Worker
2. Handle POST requests from the form
3. Use [Cloudflare Email Routing](https://developers.cloudflare.com/email-routing/) or a service like Resend to deliver notifications
4. Update the form `action` to your Worker URL

### Option C — Other backends

The form sends standard `name`, `email`, `projectType`, and `message` fields. Any form backend that accepts POST requests will work (Netlify Forms, Google Forms via POST, custom Express endpoint, etc.).

## Editing Content

All page content lives in `src/pages/*.astro` files. Each page is self-contained — edit the content directly in those files. Shared UI components are in `src/components/`.

### Adding a new page

1. Create `src/pages/new-page.astro`
2. Import and wrap with `<Layout>`:
   ```astro
   ---
   import Layout from "../components/Layout.astro";
   ---
   <Layout title="Page Title" description="Page description.">
     <!-- Your content -->
   </Layout>
   ```
3. Add the nav link in `src/components/Header.astro` and `src/components/Footer.astro`

### Changing colors

Edit the custom colors in `tailwind.config.mjs` under `theme.extend.colors`. The CSS variables and utility classes will update automatically.

## Performance

- Zero JavaScript shipped by default (Astro islands pattern)
- Tailwind purges unused CSS at build time
- Fonts loaded via Google Fonts with `preconnect` for faster delivery
- All pages are static HTML — served directly from CDN edge nodes
- No client-side routing, no hydration overhead

## License

Private — all rights reserved. This is a commercial marketing site.
