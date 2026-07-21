# CQA Email Template

A reusable, product-spotlight HTML email template, originally built for Exotel's CQA (Call Quality Audit) product's "Interaction Deep Dive" announcement. Built as a self-contained, table-based HTML email (inline styles, MSO/Outlook conditionals) so it renders consistently across email clients — no build step required.

## Structure

- `project/Email Template.html` — **the master template.** Fixed layout/styling with `{{handlebars}}` placeholders for anything that changes per send (copy, links, images). Not meant to be opened directly in a browser — the placeholders will show as literal text until rendered.
- `project/template-data.example.json` — every placeholder the template expects, filled with the original CQA "Interaction Deep Dive" content, as a reference for what each variable should contain.
- `project/examples/cqa-interaction-deep-dive.html` — the original CQA email, fully rendered (no placeholders), for a quick visual preview:
  ```bash
  open "project/examples/cqa-interaction-deep-dive.html"
  ```
- `project/assets/hero-banner.png` — hero image from the original CQA send.
- `project/uploads/` — reference material from the original design handoff.
- `project/CQA Interaction Deep Dive Email.html` — a generated bundler preview shell from the original Claude Design export; not part of the editable source.

## Using the template for a new email

1. Copy `project/template-data.example.json` and fill in values for the new send (headline, steps, CTA links, etc.).
2. Render `project/Email Template.html` through your merge-tag engine of choice (SendGrid dynamic templates, Handlebars, Customer.io, etc.), passing in that data.
3. The five numbered "steps" rows are fixed slots (`step_1_*` through `step_5_*`) — if a send needs fewer, delete the unused `<tr>` blocks in the steps timeline table; the icons/colors per row are structural and don't need to change.
4. `hero_image_url` should point to a hosted image (not inline base64) — keeps the template lightweight and reusable across sends.

## Placeholders

| Variable | Used for |
|---|---|
| `email_title` | `<title>` tag |
| `preheader_text` | hidden inbox-preview snippet |
| `eyebrow_label` | small label above the headline |
| `headline` / `subheadline` | top color band copy |
| `hero_image_url` / `hero_image_alt` | header image |
| `recipient_name` | greeting |
| `intro_html` | intro paragraph (may contain inline `<a>`/`<strong>`) |
| `section_heading` | heading above the steps list |
| `step_1_title` … `step_5_title` | step titles |
| `step_1_description` … `step_5_description` | step body text |
| `callout_html` | highlighted callout box |
| `cta_primary_label` / `cta_primary_url` | solid CTA button |
| `cta_secondary_label` / `cta_secondary_url` | outlined CTA button |
| `footer_disclaimer` | unsubscribe/preferences line |
| `company_address` | footer address line |

## Design system

Icons are inlined as SVG, sourced from `@phosphor-icons/core` (the icon set used under the hood by `@exotel-npm-dev/signal-design-system`'s `Icon` component). The primary brand color (`#3949ab`) matches `indigo[600]` from the design system's theme.
