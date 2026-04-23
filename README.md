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
- **Footer links wired**: Services column → Services page; Company column routes to About Us / Services / Contact / Get a Quote; phone uses `tel:`, email uses `mailto:`
- **Contact page**: phone / email / emergency line all clickable; email domain corrected `hphaulers.com` → `huntspointshaulers.com`
- **Form success screen** no longer falsely promises a 2-hour callback — now shows call/email fallbacks

### Not done
See blocking items below. Quote form still does not submit anywhere.

---

## OUTSTANDING — needed from the business

All line numbers are in `index.html` at the commit where this README was written. Use Grep if they've shifted.

### 1. Phone numbers (placeholders are 555-01xx fiction range)
**Replace:** `(201) 555-0182` and `tel:2015550182`
**Locations:** index.html:296, 458, 709, 733 + footer display on 296
**Also replace:** `(201) 555-0199` and `tel:2015550199` (24/7 emergency line) at index.html:751

Do a project-wide find-and-replace on both numbers (display string + `tel:` href).

### 2. Email addresses
Domain is now `huntspointshaulers.com` everywhere. The mailboxes must exist (or be GoDaddy aliases/forwarders):
- `dispatch@huntspointshaulers.com` — main dispatch
- `quotes@huntspointshaulers.com` — quote inquiries

**Locations:** index.html:733, 735, 709 (contact page), footer link

Confirm these mailboxes are set up in GoDaddy before launch.

### 3. FMCSA MC# and USDOT#
**Replace:** `MC# 000000` and `DOT# 000000`
**Locations:**
- Footer copyright line — index.html:304
- About page credentials grid — index.html:649 (MC#), 650 (DOT#)

### 4. Full street address
Currently only `Bogota, NJ 07603` / `Bergen County, New Jersey`.
**Locations:** index.html:298 (footer), 743 (contact page)

### 5. Quote form backend
The form at `ContactPage` (index.html:672) validates fields then just shows a success screen — submitted data is discarded (index.html:687-692, `handleSubmit`).

**Pick one provider** and give me the endpoint URL — I'll wire it in ~5 min:
- Formspree (free 50/mo) — https://formspree.io
- Web3Forms (free, unlimited) — https://web3forms.com
- Getform / Basin / Formspark — equivalent alternatives

### 6. Unverified marketing claims
Confirm or correct:
- `Est. 2009` — index.html:357
- `15+` Years Operating — index.html:319 (stats) + 581 (values desc)
- `48` States Licensed — index.html:320
- `500K+` Miles Annually — index.html:321
- `$1M per occurrence` insurance — index.html:651

---

## Nice-to-have (post-launch)

- Privacy Policy / Terms of Service pages (footer links are currently dead text — index.html:312)
- Real team/founder content on About page (currently generic copy)
- Swap CDN React + in-browser Babel for a Vite build — removes JSX-compile-on-load, faster first paint
- Add favicon + Open Graph / Twitter Card meta tags for link previews
- Add Google Analytics / Plausible / Umami

---

## For a new session picking this up

When the business data arrives, paste it into a new conversation in this working directory. The assistant should:

1. Read this README first
2. Apply the data to every location listed above (use Grep to confirm line numbers — they shift)
3. If Formspree endpoint provided, update `handleSubmit` in `ContactPage` (index.html:687) to `POST` the form data as `application/json` to the endpoint, keep the existing validation, and only flip `setSubmitted(true)` on a successful response
4. Commit and push — Railway auto-deploys on push to `main`
