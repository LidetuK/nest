# Aspio SE1 → SE2 · Phase 1 · Nest

Slidev proposal deck for the Booking Availability Backend (Nest).

## Local

```bash
npm install
npm run dev
```

- Slideshow: http://localhost:3033/
- Presenter: http://localhost:3033/presenter/

## Build

```bash
npm run build
```

Static output lands in `dist/` (used by Vercel).

## Deploy (Vercel)

1. Import this GitHub repo in [Vercel](https://vercel.com/new)
2. Framework: Other · Build: `npm run build` · Output: `dist`
3. Deploy — `vercel.json` already sets rewrites for SPA routing
