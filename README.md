# NEXUS — Digital Studio

A stunning Vue.js portfolio/agency website with auto-deploy to Vercel on every VS Code save.

## Tech Stack
- **Framework**: Vue.js 3 + Vue Router 4
- **Deployment**: Vercel (auto-deploys on push)
- **CI/CD**: GitHub + Run on Save VS Code extension
- **Fonts**: Bebas Neue, DM Sans, JetBrains Mono

## Pages
- **Home** — Hero, stats, marquee, services list, work grid, testimonials carousel, CTA
- **Work** — Filterable project gallery with hover effects
- **Services** — Accordion service detail, process steps, pricing tiers
- **Contact** — Validated form, live world clocks, social links

## Quick Start

```bash
npm install
npm run serve      # Local dev on http://localhost:8080
npm run build      # Production build to /dist
```

## Deploy to Vercel

1. Push to GitHub
2. Import repo at vercel.com
3. Framework: Vue.js | Output: dist | Build: npm run build
4. Deploy ✅

## Auto-Deploy on Save (VS Code)

Install the **Run on Save** extension (emeraldwalk.RunOnSave).
The `.vscode/settings.json` is already configured — every save auto-commits and pushes to GitHub → Vercel rebuilds automatically.

## Project Structure

```
src/
├── assets/global.css       Global styles + CSS variables
├── router/index.js         Vue Router config
├── views/
│   ├── HomeView.vue        Home page (hero + sections)
│   ├── WorkView.vue        Filterable project gallery
│   ├── ServicesView.vue    Services + pricing
│   └── ContactView.vue     Contact form + world clocks
└── App.vue                 Root + navbar + footer + cursor
```
