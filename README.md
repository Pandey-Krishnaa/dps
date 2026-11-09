# Delhi Premium School, Hardoi — admissions landing page

Static landing page for Delhi Premium School, Hardoi — an independent school teaching the CBSE
curriculum. Classes begin April 2027; admission forms open February 2027.
Single file, no build step, no dependencies. English and Hindi.

## Deploy to GitHub Pages

```bash
cd dps-hardoi-site
git init -b main
git add -A
git commit -m "Delhi Premium School Hardoi — landing page"
gh repo create dps-hardoi-site --public --source=. --push
```

Then turn Pages on: **Settings → Pages → Source: Deploy from a branch → `main` / `(root)` → Save.**

The site appears at `https://<your-username>.github.io/dps-hardoi-site/` in about a minute.

Without the `gh` CLI, create the repo on github.com first, then:

```bash
git remote add origin https://github.com/<your-username>/dps-hardoi-site.git
git push -u origin main
```

## Still to confirm

The site is live. Done: address, phone, email, bus routes, enquiry form (Google Form + Sheet,
with spam protection), pre-launch banner removed, search indexing enabled.

1. **Placeholders.** Remaining dotted-underline items are unconfirmed: dates, campus acreage,
   library stock, first board batch, affiliation wording. Search `index.html` for `class="ph"`.
2. **Logo.** The header mark is a neutral placeholder. Replace it with the school's official artwork.

## Wiring the enquiry form

In `index.html`, fill in the `GFORM` object at the top of the script (form ID plus one
`entry.<id>` per field — get these from the form's **Get pre-filled link**). The page then posts
each enquiry to the Google Form, which writes it to the linked Google Sheet.
To use a different service instead, change `sendToGoogleForm(d)`.

**Spam protection (free).** The page drops submissions that fill the hidden `website` field or
arrive under 3 seconds after load, and allows 3 enquiries per browser per hour. The Google Form
itself enforces required fields, a `[6-9][0-9]{9}` phone pattern and length limits, so direct
posts that skip the page are rejected too. If you change a limit, change it in both places, or
real enquiries can be silently rejected by the form.

`d` is an object with `student`, `grade`, `parent`, `phone`, `email`, `area`, `message`.

Simplest options, no server needed:

- **Google Form** — build a form with matching fields, then POST to its `formResponse` URL with the
  `entry.<id>` names.
- **Formspree / Basin / Getform** — `fetch('https://formspree.io/f/<id>', {method:'POST', ...})`.
- **WhatsApp handoff** — build a `https://wa.me/91XXXXXXXXXX?text=...` link from `d` and open it.
  In this market that usually converts better than a form.

Keep the existing validation and the success panel; only the send step is missing.

## Editing content

- Two languages live side by side. English is the element's own content; Hindi is in its `data-hi`
  attribute. Change both, or the toggle will show a stale translation.
- Input placeholders use `data-hi-placeholder`.
- Validation messages and the confirmation sentence are in the `T` object in the script.
- Colors are tokens in `:root` at the top of the `<style>` block, with dark-mode values repeated in
  two places — the `prefers-color-scheme` block and the `[data-theme="dark"]` block. Change both.

## Custom domain

Add a `CNAME` file containing the bare domain (e.g. `dpshardoi.in`), point a DNS `CNAME` record at
`<your-username>.github.io`, then enable **Enforce HTTPS** in Settings → Pages.
