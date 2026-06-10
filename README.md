# Finance Consultancy — Investment Strategy Landing Page

A modern, single-file landing page for an investment strategy consultancy. Built with plain HTML5, CSS3, and vanilla JavaScript — no frameworks, no build tools.

**Live site:** https://aquake13.github.io/finance-consultancy/

## Features

- Full-viewport hero with scroll animations
- Six benefit cards with hover effects
- Four-step investment process timeline
- Testimonials section
- Lead magnet download CTA
- Enquiry form with client-side validation (FormSubmit integration)
- FAQ accordion
- Fully responsive — mobile, tablet, desktop

## Structure

Everything lives in a single file:

```
index.html   ← HTML + embedded CSS + embedded JavaScript
```

## Form Setup

Enquiry submissions are sent via [FormSubmit](https://formsubmit.co/) to the configured email. To change the recipient, update the `action` attribute on `<form id="enquiry-form">` in `index.html`:

```html
<form action="https://formsubmit.co/YOUR-EMAIL@example.com" method="POST">
```

## Deployment

The site is deployed automatically to GitHub Pages via GitHub Actions on every push to `main`. The workflow is defined in `.github/workflows/deploy.yml`.

To run locally, open `index.html` directly in a browser — no server required.
