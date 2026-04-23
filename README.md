# Hunts Point Haulers

Static marketing site for Hunts Point Haulers LLC — interstate freight carrier.

The site is a single-file React app (`index.html`) with images in `uploads/`. React and Babel load from CDN and compile JSX in-browser. No build step.

## Local development

```
npm install
npm start
```

Serves at http://localhost:3000

## Deployment

Deployed on Railway. `npm start` runs `serve` over the static files — Railway injects `PORT`.

Domains (GoDaddy):
- huntspointshaulers.com *(primary — already has a GoDaddy-built site; needs DNS cutover to Railway)*
- huntpointshaulers.com
- huntspointhauler.com

Both `huntspointshaulers.com` and `huntpointshaulers.com` should point to the Railway deployment.

---

## Status

### Done
- Repo initialized, pushed to `github.com/freenow/hunts_point_haulers_website`
- `package.json` + `serve` wired for Railway (`$PORT`-aware)
- Dead code removed: `__bundler_thumbnail` template, `TweaksPanel`, edit-mode `postMessage` hooks, unused `formData` state, the `Hunts Point Haulers.html` wrapper, orphan image
- **Mobile responsive**: hamburger nav + mobile menu (<960px), all grids collapse 4→2→1, headings scale down, form fields stack on phones
- **Footer links wired**: Services column → Services page; Company column routes to App pages; phone uses `tel:`, email uses `mailto:`
- **Business data ported from Luisa's 2026-04-23 drop**: phone `(201) 908-2622` + IVR prompts (press 1 dispatch / 2 accounting); emails `Dispatch@` / `Accounting@` / `jobs@huntspointhaulers.com`; MC# 1686639; DOT# 4322716
- **Positioning shift (Luisa + Crystal-approved)**: "Interstate" → "Tri-State" framing; Est. 2024; stats updated (1MM+ miles, 24/7/365); "Hunts Point Market specialist" claims replaced with "produce transportation specialist" copy across Home / Services / About
- **Reefer hero image** swapped for Luisa's new NYC-skyline-at-sunset reefer truck shot
- **New Customer Setup** block added to Contact page (mailto CTAs for onboarding packet + carrier setup)

### Not done
Quote form still does not submit anywhere. See blocking items below.

---

## OUTSTANDING — needed from the business

### 1. Quote form backend
The form at `ContactPage` validates fields then just shows a success screen — submitted data is discarded (`handleSubmit`, grep for `setSubmitted(true)`).

**Pick one provider** and give me the endpoint URL — I'll wire it in ~5 min:
- Formspree (free 50/mo) — https://formspree.io
- Web3Forms (free, unlimited) — https://web3forms.com
- Getform / Basin / Formspark — equivalent alternatives

### 2. Street / mailing address *(nice-to-have)*
Site currently says only "Tri-State Area Based / Serving NY · NJ · CT" on Contact. If there's a physical HQ address to publish, provide it and I'll slot it into the Contact "Location" block.

### 3. Domain confirmation *(pre-launch check)*
Luisa's emails are at `huntspointhaulers.com` (no 's' after "hauler"). The GoDaddy domains listed above are `huntspointshaulers.com` / `huntpointshaulers.com` / `huntspointhauler.com` — none is an exact match. Before launch, confirm which domain actually hosts the MX records so the mailto links resolve.

### 4. Unverified marketing claims
Confirm:
- `48` States Licensed (stats)
- `1MM+` Miles Annually (stats)
- `$1M per occurrence` insurance (About → Credentials)

---

## Nice-to-have (post-launch)

- Privacy Policy / Terms of Service pages (footer links are currently dead text — index.html:312)
- Real team/founder content on About page (currently generic copy)
- Swap CDN React + in-browser Babel for a Vite build — removes JSX-compile-on-load, faster first paint
- Add favicon + Open Graph / Twitter Card meta tags for link previews
- Add Google Analytics / Plausible / Umami

---

## For a new session picking this up

When the form endpoint is chosen, the assistant should:

1. Read this README first
2. Update `handleSubmit` in `ContactPage` (grep for `setSubmitted(true)`) to `POST` the form data as `application/json` to the endpoint, keep the existing validation, and only flip `setSubmitted(true)` on a successful response. Add a minimal error branch that keeps the form mounted on failure
3. Commit and push — Railway auto-deploys on push to `main`
