# CQA Email Template

A reusable, product-spotlight HTML email template built for Exotel's CQA (Call Quality Audit) product's "Interaction Deep Dive" announcement. Built as a self-contained, table-based HTML email (inline styles, MSO/Outlook conditionals) so it renders consistently across email clients — no build step required.

Two personas are supported out of the box: **supervisor** and **agent**. Both share the exact same base template and design system; only the copy and a couple of content sections differ.

## Structure

- `project/Email Template.html` — **the master template.** Fixed layout/styling with `{{handlebars}}` placeholders for anything that changes per send (copy, links, images). Not meant to be opened directly in a browser — the placeholders will show as literal text until rendered. Has a brand-palette reference comment at the top of the `<head>` — always check it before hand-picking a new color.
- `project/template-data.example.json` — every placeholder the template expects, filled with the **supervisor** send's content, as a reference for what each variable should contain.
- `project/template-data.agent.example.json` — same idea, filled with the **agent** send's content (adds two agent-only fields, see below).
- `project/examples/cqa-interaction-deep-dive.html` — the supervisor email, fully rendered (no placeholders), for a quick visual preview.
- `project/examples/cqa-interaction-deep-dive-agent.html` — the agent email, fully rendered.
- `index.html` — a landing page (deployable as-is on Vercel/any static host) linking both rendered examples.
- `project/assets/` — hero images used by the examples.
- `project/uploads/` — reference material from the original design handoff.
- `project/CQA Interaction Deep Dive Email.html` — a generated bundler preview shell from the original Claude Design export; not part of the editable source.

Preview either example directly:
```bash
open "project/examples/cqa-interaction-deep-dive.html"        # supervisor
open "project/examples/cqa-interaction-deep-dive-agent.html"  # agent
```

## Using the template for a new email

1. Copy whichever `template-data*.example.json` is closer to your send and fill in values (headline, steps, CTA links, etc.).
2. Render `project/Email Template.html` through your merge-tag engine of choice (SendGrid dynamic templates, Handlebars, Customer.io, etc.), passing in that data.
3. The five numbered "steps" rows are fixed slots (`step_1_*` through `step_5_*`) — if a send needs fewer, delete the unused `<tr>` blocks in the steps timeline table. Each step's icon badge color (tint + border) is structural per row position and doesn't need to change; only the SVG glyph inside would change if you need a different icon — keep its `fill` neutral (`#4b5568`), never colored.
4. `hero_image_url` should point to a hosted image (not inline base64) — keeps the template lightweight and reusable across sends.
5. Don't reintroduce numbering (e.g. "1 · Title") in step titles, colored step icon glyphs, bordered/boxed secondary CTA buttons, or a bright-tint callout box — these were deliberately removed/simplified in the current design (see Design system below). If a future request asks to add one of these back, treat it as a deliberate change, not a bug fix.
6. To add a persona-specific explainer section (like the agent template's "What is the Interaction Deep Dive?" block), copy that block's markup from `cqa-interaction-deep-dive-agent.html` — it's not part of the master template since it's optional per-send content, not structural.

## Placeholders

| Variable | Used for |
|---|---|
| `email_title` | `<title>` tag |
| `preheader_text` | hidden inbox-preview snippet |
| `eyebrow_label` | text next to the logo in the header pill badge |
| `headline` / `subheadline` | header band copy |
| `hero_image_url` / `hero_image_alt` | header image |
| `recipient_name` | greeting |
| `intro_html` | intro paragraph (may contain inline `<a>`/`<strong>`) |
| `section_heading` | heading above the steps list |
| `step_1_title` … `step_5_title` | step titles (no numbering prefix) |
| `step_1_description` … `step_5_description` | step body text |
| `cta_heading` | bold heading inside the CTA card (e.g. "Ready to explore?") |
| `cta_description` | supporting line inside the CTA card |
| `cta_primary_label` / `cta_primary_url` | solid pill CTA button |
| `cta_secondary_label` / `cta_secondary_url` | plain text link below the button (not a bordered button) |
| `sign_off` | closing line above the footer (e.g. "Best regards,<br>The CQA Team") |
| `footer_disclaimer` | dark footer bar text |

Agent-only fields (used by `cqa-interaction-deep-dive-agent.html`, not in the master template — see step 6 above): `what_is_heading`, `what_is_html`, `try_it_html`.

## Design system

Icons are inlined as SVG, sourced from `@phosphor-icons/core` (the icon set used under the hood by `@exotel-npm-dev/signal-design-system`'s `Icon` component). The brand primary color (`#394FB6`) matches the CQA product UI's own primary indigo/blue — confirmed against the CQA app's own screenshots (buttons, active nav states, "Create" actions), not picked independently. The full color reference lives as a comment at the top of `project/Email Template.html` — treat it as the source of truth and update it there first if the palette changes.

Structural conventions, in top-to-bottom order:

1. **Header band** — diagonal gradient (`#2D3E8F → #394FB6 → #2E7BD6`, with a solid `bgcolor="#394FB6"` fallback for Outlook), a translucent pill badge (bg `#4C5CC4`) containing the small white logo mark + `eyebrow_label`, then a bold headline and a muted subheadline.
2. **Hero image** — full-width, hosted (not inline base64).
3. **Greeting + intro** — plain body copy.
4. **Steps timeline** — 5 rows, each a small circular icon badge (32px, light tint bg + a light border in the same hue) next to a bold title and description. The glyph inside every badge is always neutral gray (`#4b5568`) — color lives in the badge, not the icon, and titles carry no number prefix.
5. **CTA card** — a single full-bleed band (light lavender gradient, no side margins — flush with the card edges like the header) containing a bold heading, a description line, one solid pill-shaped primary button, and a plain bold text link underneath for the secondary action (not a bordered button).
6. **Sign-off** — a plain paragraph (e.g. "Best regards,<br>The CQA Team").
7. **Footer** — a full-bleed dark navy band (`#1B2236`), centered muted-gray disclaimer text.

Spacing is intentionally tight (reduced paddings/margins throughout) to minimize scroll length in email clients — don't pad things back out unless a specific section genuinely needs more room.
