# slidev-theme-hccs

Slidev theme for Hudson County Complete Streets — HCCS green background, white text, logo footer, per-slide QR.

## Install

From a Slidev deck:

```bash
pnpm add -D slidev-theme-hccs
# or, from a local clone:
pnpm add -D /Users/ryan/c/hccs/slidev-theme
```

Then in your `slides.md` headmatter:

```yaml
---
theme: slidev-theme-hccs
colorSchema: dark
footer:
  url: tower.hccs.dev          # link text shown above the logo (omit to hide)
  urlHref: https://tower.hccs.dev    # optional, defaults to https://<url>
  logoHref: https://hudcostreets.org # optional
qr:
  position: bl       # bottom-left, matching the logo's bottom-right
  size: 96
  uppercase: true    # tighter QR (alphanumeric mode) — safe with urlForm: n
  skipSlides: [1]    # skip the cover slide (frontmatter is deck headmatter)
---
```

The per-slide QR code is auto-injected by [`Open-Athena/slidev`][fork]'s built-in
`<SlideQR>` component when any `qr:` block is present in the deck headmatter.

[fork]: https://github.com/Open-Athena/slidev

## Per-slide options

```yaml
---
class: light       # white bg / dark fg variant; logo flips to green
footer: false      # hide the footer on this slide
qr: false          # hide the QR on this slide
---
```

## Layouts

- `cover` — centered title slide
- `section` — centered section break
- `default` — standard content slide
- `two-cols` — `::left::` / `::right::` split

## Develop

```bash
pnpm install
pnpm dev          # opens example.md at http://localhost:3947
pnpm build
```
