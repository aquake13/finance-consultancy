# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file static landing page for an investment strategy consultancy. Everything — HTML, CSS, and JavaScript — lives in one file: `index.html`. There is no build system, package manager, or external framework.

## Development

Open directly in a browser — no server required:

```powershell
Start-Process "index.html"
```

For live-reload during editing, use VS Code's Live Server extension or any static file server:

```powershell
npx serve .
```

## Architecture

`index.html` is organised into three sequential blocks:

1. **`<style>` (embedded CSS)** — structured top-to-bottom in this order:
   - CSS variables (`--primary`, `--secondary`, `--accent`, `--background`, `--text`, `--light-bg`)
   - Reset and base typography
   - Shared utilities (`.container`, `.btn-*`, `.reveal` scroll animation classes)
   - Per-section styles in document order (nav → hero → benefits → process → testimonials → lead-magnet → contact → faq → cta-banner → footer)
   - Responsive breakpoints at the bottom (`@media` blocks for 1024px, 768px, 480px)

2. **`<body>` (HTML sections)** — nine sections in order, each with a matching `id`:
   `#navbar` → `#hero` → `#benefits` → `#process` → `#testimonials` → `#lead-magnet` → `#contact` → `#faq` → `#cta-banner` → `footer`

3. **`<script>` (embedded JS at bottom of body)** — five self-contained behaviours:
   - Sticky nav shadow (scroll listener toggles `.scrolled`)
   - Mobile menu toggle (hamburger ↔ `nav-links.open`)
   - Smooth scroll for all `a[href^="#"]`
   - Scroll reveal via `IntersectionObserver` (adds `.visible` to `.reveal` elements)
   - FAQ accordion (click toggles `.faq-item.open` + `.faq-answer.open`)
   - Form validation and AJAX submission with native-submit fallback

## Form (FormSubmit)

The enquiry form posts to `https://formsubmit.co/andrewquake13@gmail.com`. To change the recipient, update the `action` attribute on `<form id="enquiry-form">`. Relevant hidden fields already in place: `_subject`, `_captcha`, `_template`.

JS validation runs client-side before fetch — required fields: name, email (regex), phone (regex), investment goal (select). On success the form is hidden and `#form-success` is shown; on network failure `#form-error` appears briefly and a native form submit fires as fallback.

## Design Tokens

All colours, shadows, and radii are CSS variables on `:root`. Change values there to restyle the entire page — never hardcode colour values outside the `:root` block.
