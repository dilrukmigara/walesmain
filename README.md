# Wales Higher Education Centre

React/Vinext website with a persistent password-protected admin at `/admin`.

## Content management
- Teachers: add/edit/remove, publish or hide, photo, teaching GIF/video, qualifications, biography, special notes.
- Classes: weekday, start/end, grade, hall, active state and notes; overlapping hall or teacher schedules are rejected.
- Halls: add/rename/remove unused halls, photos, captions, capacity, descriptions and location Google Maps links.
- Popup ads: image, text, link, enable/disable, priority, start/end in Asia/Colombo and session/visit frequency.
- Site settings: cover, logo, institution name, tagline, phone, email and social links.
- Save & publish commits all pending changes atomically. Revision checks prevent lost edits from another tab. Public pages re-fetch once a minute and when returning to a tab.

## Runtime
D1 binding `DB` stores site content, hashed session tokens and rate limiting. R2 binding `BUCKET` stores uploads. Runtime secret `ADMIN_PASSWORD` is configured in Sites, never in source or client code. Admin uses an 8-hour HttpOnly Secure SameSite=Strict cookie, origin checks and server-side authentication for every write.

Images: JPG, PNG, WebP, GIF up to 8 MB. Clips: GIF up to 8 MB; MP4/WebM up to 25 MB. Removing a photo removes its content reference; uploaded objects are retained to avoid breaking existing references.

The original six sample teacher profiles remain labelled as samples until replaced or removed in the admin. Initial photo cover is AI-generated. Map positions remain schematic G upper, R middle-right, B lower-left. GA6 is deduplicated.

## Validation
Build through the Sites build helper. Run `node tests/admin.integration.mjs` for isolated D1/R2 integration tests; these do not alter hosted data. Source entry points are `client/PublicSite.jsx`, `client/Admin.jsx`, `server/api.js`, and `server/validation.js`. Database changes use append-only Drizzle migrations.
