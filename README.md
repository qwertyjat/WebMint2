# WebMint — Website

A complete, static, no-framework website for the WebMint web design agency. Plain HTML, CSS, and vanilla JavaScript — no build step required.

## 1. Project structure

```
WebMint/
├── index.html, about.html, services.html, portfolio.html, pricing.html,
│   process.html, blog.html, faq.html, contact.html,
│   privacy-policy.html, terms.html, cookie-policy.html, 404.html
├── css/
│   ├── variables.css     ← all colors, fonts, spacing (edit this to re-theme)
│   ├── style.css         ← layout & components
│   ├── responsive.css    ← breakpoints
│   └── animations.css    ← keyframes & scroll-reveal
├── js/
│   ├── config.js         ← ALL editable text: name, phone, email, hero copy…
│   ├── navigation.js     ← loads header/footer, builds nav, theme toggle
│   ├── main.js            ← page loader + renders services/portfolio/pricing/testimonials/blog from JSON
│   ├── animations.js      ← scroll reveals, counters, hero terminal
│   ├── faq.js              ← FAQ accordion (reads data/faq.json)
│   └── contact.js         ← form validation + pluggable send service
├── data/
│   ├── services.json, portfolio.json, testimonials.json, faq.json, pricing.json
├── partials/
│   ├── header.html, footer.html   ← shared nav & footer markup (loaded via fetch)
├── icons/sprite.svg      ← all icons, one file
├── images/, assets/       ← replace placeholder images here
├── robots.txt, sitemap.xml
```

Every page shares the same header and footer — they live in `partials/` and are injected by `js/navigation.js`, so there is no duplicated markup to keep in sync.

## 2. The ONE place to edit things

**`js/config.js`** is the single source of truth for:

| To change...            | Edit this key in `js/config.js`        |
|--------------------------|------------------------------------------|
| Website / brand name     | `brand.name`, `brand.logoText`            |
| Phone number              | `contact.phoneDisplay` and `contact.phoneRaw` |
| WhatsApp number           | `contact.whatsappNumber` and `social.whatsapp` |
| Email                     | `contact.email`                           |
| Telegram                  | `contact.telegramUrl`                     |
| Address                   | `contact.address`                         |
| Business hours            | `hours.weekdays`, `hours.weekend`         |
| Social links               | `social.*`                                |
| Hero headline/subhead/CTAs | `hero.*`                                  |
| Footer text & columns      | `footer.*`                                |
| Nav menu items              | `nav` (array)                             |

Changing your phone number, email, or WhatsApp number anywhere on the site (nav, footer, contact page, WhatsApp floating button) means editing **one line each** in this file.

**`css/variables.css`** is the single source of truth for colors, fonts, spacing, and radius. Change `--c-mint` to re-brand the entire accent color across every page.

**`data/*.json`** are the single source of truth for content lists:
- `services.json` — services shown on the home page and the full Services page
- `portfolio.json` — portfolio/projects grid
- `pricing.json` — pricing cards
- `testimonials.json` — testimonial cards
- `faq.json` — FAQ accordion (grouped by category)

To add, remove, or edit a service, project, price, testimonial, or FAQ, edit the matching `.json` file only — no HTML or JS changes needed.

## 3. Replacing images

Drop real photos into `images/portfolio/`, `images/testimonials/`, etc., matching the `image`/`avatar` paths already referenced in the JSON files, then update `main.js`'s render functions to use `<img>` tags instead of the current placeholder blocks if you want photos instead of the gradient placeholders. Replace `images/logo.svg` with your own logo and point `brand.logoImage` at it if you'd rather use an image logo than the text logo.

## 4. Changing colors

Open `css/variables.css`. The key tokens:
- `--c-ink` / `--c-paper` — dark/light backgrounds
- `--c-mint` / `--c-mint-deep` — primary accent color
- `--font-display` / `--font-body` / `--font-mono` — typefaces (loaded from Google Fonts in each page's `<head>`)

Every component on the site references these variables, so one edit updates everything.

## 5. Adding a portfolio project

Open `data/portfolio.json` and add an object to the array:

```json
{
  "id": "unique-id",
  "title": "Project Name",
  "category": "Category",
  "description": "One or two sentences about the project.",
  "image": "images/portfolio/your-image.jpg",
  "tags": ["Tag One", "Tag Two"]
}
```

## 6. Changing pricing

Open `data/pricing.json` and edit the `price`, `features`, or `description` fields. Set `"highlighted": true` on exactly one plan to mark it "Most popular."

## 7. The contact form & connecting a real backend

The form on `contact.html` is fully validated client-side (`js/contact.js`) but doesn't send anywhere by default — it runs a demo simulation so you can test the UI.

To connect a real service:

1. Open `js/contact.js`.
2. Find the `services` object — each key (`emailjs`, `formspree`, `netlify`, `node`, `php`, `telegram`, `whatsapp`) has a clearly commented placeholder function.
3. Fill in the one you want (add any required SDK `<script>` tag to `contact.html` if the service needs one, e.g. EmailJS).
4. Change `const ACTIVE_SERVICE = "demo"` near the top of the file to the service name you configured.

No HTML changes are required for any of these integrations.

## 8. Deploying on GitHub Pages

1. Push this folder to a GitHub repository (the contents of `WebMint/`, not a parent folder).
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select your branch (e.g. `main`) and root folder `/`.
4. Save — your site will be live at `https://<username>.github.io/<repo-name>/` within a few minutes.
5. Update the `<link rel="canonical">` and `og:url` values in each page's `<head>`, and the URLs in `sitemap.xml`, to match your real domain.

## 9. Deploying on Netlify

1. Drag and drop the `WebMint` folder onto [app.netlify.com/drop](https://app.netlify.com/drop), or connect the GitHub repo.
2. No build command is needed — this is a static site (leave the build command blank, publish directory `/`).
3. If using **Netlify Forms**, add `data-netlify="true"` and a hidden `<input type="hidden" name="form-name" value="contact">` to the `<form id="contact-form">` in `contact.html`, and set `ACTIVE_SERVICE = "netlify"` in `js/contact.js`.

## 10. Connecting EmailJS

1. Create a free account at [emailjs.com](https://www.emailjs.com/) and set up an email service + template.
2. Add the SDK to `contact.html`, just before `js/contact.js`:
   ```html
   <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser/dist/email.min.js"></script>
   ```
3. In `js/contact.js`, uncomment and fill in the `emailjs()` function with your public key, service ID, and template ID.
4. Set `ACTIVE_SERVICE = "emailjs"`.

## 11. Connecting a Telegram Bot

1. Create a bot via [@BotFather](https://t.me/BotFather) on Telegram and note the bot token.
2. Message your bot once, then find your chat ID (e.g. via `https://api.telegram.org/bot<TOKEN>/getUpdates`).
3. In `js/contact.js`, uncomment and fill in the `telegram()` function with your bot token and chat ID.
4. Set `ACTIVE_SERVICE = "telegram"`.

**Note:** for production use, proxy this through a small backend rather than calling the Telegram API directly from the browser, so your bot token isn't exposed in client-side code.

## 12. Connecting the WhatsApp Business API

The WhatsApp Cloud API requires a private access token, so it must be called from a backend you control — see the commented `whatsapp()` function in `js/contact.js` for the expected request shape once you have a proxy endpoint set up.

## 13. Editing text on any page

Page-specific copy (headings, paragraphs, testimonials text you wrote by hand, legal page content) is edited directly in the relevant `.html` file, since it isn't repeated anywhere else on the site. Anything that appears on more than one page — contact details, nav links, footer — lives in `js/config.js` instead.

---

Built for WebMint. Questions about this codebase can be pointed at whoever set up the project.
