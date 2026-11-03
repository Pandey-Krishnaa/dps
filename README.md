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

## Before this goes public

This is an **approval build**, not a live site.

1. **Placeholders.** Every dotted-underline item on the page is a placeholder: phone, email,
   address, campus acreage, section size, library stock, all dates, bus route counts, and the
   founding-family fee concession. Search `index.html` for `class="ph"` to find them all.
2. **The founding-family fee concession** (waived registration fee, first-year fees held) is a
   commercial promise. Confirm it with the management committee or delete that block.
3. **The enquiry form does not send anything.** Find `// TODO:` in the script and point it at your
   backend, a Google Form, or a CRM. Until then a parent is told they will get a call and nobody
   is notified. See below.
4. **Logo.** The header mark is a neutral placeholder. Replace it with the official DPS artwork
   your franchise agreement permits, and check the footer attribution wording against the contract.
5. **`<meta name="robots" content="noindex, nofollow">`** is in `<head>` so this build stays out of
   search results during approval. **Delete that line when you go live**, or the real site will be
   invisible to Google.
6. **Remove the amber pre-launch banner** (`<div id="setup">` and its `#setup` CSS) before launch.

## Wiring the enquiry form

In `index.html`, find:

```js
// TODO: send `d` to your admissions backend / Google Form / CRM here.
```

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
