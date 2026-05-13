# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Single static landing page (pt-BR) selling the ebook **"Claude Code para Desenvolvedores .NET — O Guia Definitivo"** for R$ 97. Sole deliverable is `template/index.html` — one self-contained file with inlined CSS and JS, no build step, no dependencies, no tests.

## Working on the page

- Open `template/index.html` directly in a browser to preview — there is no dev server, no bundler, no package manager.
- All styling lives in the `<style>` block at the top of the file. The design system uses CSS variables under `:root` (Claude brand palette — `--orange #d97757`, `--bg #141413`, etc.) and three Google Fonts: Poppins (headings), Lora (body), JetBrains Mono (code). Keep new styles consistent with those tokens rather than introducing ad-hoc colors/fonts.
- Page sections in order: `#hero`, `#para-quem`, `#o-que-aprender`, `#autor`, `#preco`, `#faq`, footer CTA, `<footer>`. Section anchors are used by in-page CTAs — don't rename anchors without updating every `href="#..."` reference.
- The checkout button currently points to `#checkout`, which is a placeholder anchor (no checkout integration yet). Replace with the real checkout URL when wiring up payments.

## Analytics & conversion tracking (load-bearing)

The page is instrumented for paid-ads attribution. **Do not break this when editing.**

- **GA4** Measurement ID placeholder: `G-XXXXXXXXXX` (two occurrences in `<head>`). Must be replaced before publishing.
- **Meta Pixel** ID placeholder: `XXXXXXXXXXXXXXXXXX` (three occurrences — the `fbq('init', ...)`, the `<noscript>` img URL, both in `<head>`). Must be replaced before publishing.
- Event wiring is in the `<script>` block near the bottom of the file:
  - Every CTA carries a `data-cta="<location>"` attribute (e.g. `nav`, `hero`, `price-box`, `footer-cta`). The script attaches click handlers to all `[data-cta]` elements that fire GA4 `begin_checkout` + Meta `InitiateCheckout`. **New CTAs must include `data-cta`** or they will not be tracked.
  - An `IntersectionObserver` on `#preco` fires GA4 `view_item` + Meta `ViewContent` once when the price section becomes visible. Renaming `#preco` breaks this.
  - Scroll-depth milestones (25/50/75/90%) fire GA4 `scroll_depth`.
  - The product object (`item_id: 'ebook-claude-code'`, `value: 97.00`, `currency: 'BRL'`) is the source of truth for tracking — keep it in sync with the displayed price if it changes.

## Conventions

- Language is pt-BR throughout (`<html lang="pt-BR">`, copy, meta description). New copy should match.
- Accessibility: existing markup uses `aria-label`, `aria-hidden`, semantic `<section>`/`<nav>`/`<footer>`, and visible focus styles. Preserve these when editing.
- Author/contact in the footer is Michel Banagouro · michel@leanwork.com.br.
