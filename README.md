# Finance Consultancy — Investment Strategy Landing Page

A single-file, production-ready landing page for an investment strategy consultancy. Built with plain HTML5, CSS3, and vanilla JavaScript — no frameworks, no build tools, no dependencies.

**Live site:** https://aquake13.github.io/finance-consultancy/

---

## Features

- **Sticky navigation** with mobile hamburger menu and glass-blur effect
- **Hero section** — full-viewport with Unsplash background, fade-in animations, and trust badges
- **Why Choose Us** — 6 benefit cards with hover lift effects and inline SVG icons
- **Investment Process** — 4-step horizontal timeline (desktop) / vertical stacked (mobile)
- **Testimonials** — 3 cards with initials avatars and star ratings
- **Lead Magnet** — download CTA scrolling to the enquiry form
- **Enquiry Form** — client-side validation, AJAX submission via FormSubmit, success/error states
- **FAQ Accordion** — smooth max-height transition, accessible aria attributes
- **Final CTA Banner** and **Footer** with social links and legal disclaimer
- **Scroll reveal animations** via IntersectionObserver
- Fully responsive — mobile (480px), tablet (768px), desktop (1024px+)

---

## File Structure

```
.
├── index.html                        # Entire site — HTML + CSS + JS in one file
├── .github/
│   └── workflows/
│       └── deploy.yml                # GitHub Actions — auto-deploys to GitHub Pages on push
├── .claude/
│   └── commands/
│       └── updategit.md              # /updategit custom Claude Code command
├── CLAUDE.md                         # Guidance for Claude Code
└── README.md
```

---

## Run Locally

Open `index.html` directly in any browser — no server required:

```powershell
Start-Process "index.html"
```

For live-reload during editing, use VS Code's Live Server extension or:

```powershell
npx serve .
```

---

## Form Setup (FormSubmit)

Enquiry submissions are sent via [FormSubmit](https://formsubmit.co/) with no backend required. To change the recipient email, update the `action` attribute on `<form id="enquiry-form">` in `index.html`:

```html
<form action="https://formsubmit.co/YOUR-EMAIL@example.com" method="POST">
```

The form includes hidden fields for subject, captcha, and template. Client-side validation checks name, email (regex), phone (regex), and investment goal before submission. On success the form is replaced with a confirmation message; on network failure it falls back to a native form submit.

---

## Deployment

Every push to `main` triggers the GitHub Actions workflow at `.github/workflows/deploy.yml`, which deploys the site to GitHub Pages automatically using the official `actions/deploy-pages` action. No build step is needed — the raw files are served directly.

To trigger a manual redeploy, go to **Actions → Deploy to GitHub Pages → Run workflow**.
