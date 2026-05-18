# Design Analysis — Vercel landing (vercel.com)

> Analysis generated with the `anydesign` skill on a **real** capture.
> Date: 2026-05-18
> Analysis emphasis: design system + reconstruction

---

## Source

- **Source type**: URL
- **Path / URL**: `https://vercel.com/`
- **Capture method**: HTML via `WebFetch`, CSS custom properties via
  `scripts/extract_css_vars.py` (808 vars across 6 stylesheets + 2 inline blocks),
  visual via `scripts/capture_site.py` (desktop screenshot 1440×900)
- **Detected limitations**: only desktop viewport captured; network-idle never settled
  (Vercel has constant background traffic) — the screenshot is what had rendered at
  T+45s, which is the full visible page

---

## TL;DR

Editorial, near-monochrome marketing surface from Vercel's Geist design system. White
background, near-black text (`#171717`), black primary buttons. The brand statement is
the multi-stop AI gradient (orange → yellow → green → teal) wrapping the Vercel triangle
in the hero — every other surface is restrained. 808 CSS custom properties extracted —
including a complete `ds-*` palette (10 shades × 8 hues, plus alpha variants), a
4px-based `geist-space-*` scale, and 7 named shadow tiers — give a high-confidence
reconstruction map.

---

## 1. Visual identity

**Personality** (3-5 adjectives): editorial, monochromatic, precise, restrained,
typographically-led

**Mood**: confident and quiet. The page does not shout; the AI gradient does — once, in
the hero — and then disappears. The rest is whitespace and type.

**Detectable stylistic references**: this *is* the Geist aesthetic — the original
reference that "Linear-like / Vercel-like" descriptions point at. Influences: editorial
print typography, Swiss minimalism, "developer documentation" density patterns.

**Information density**: minimalist in the hero, **dense in the footer** (10-column link
matrix). The page swings from breath to library on purpose.

**Implicit positioning**: developers and platform engineers. The AI gradient is the
concession to a broader buyer audience (executives shopping AI tooling) but the
typography, density, and information hierarchy speak to people who deploy code.

**Distinctive visual moves**:
- Dot-grid background pattern in the hero (subtle 4-pixel dots) — a Geist signature.
- The Vercel triangle is overlaid on a soft multi-color gradient — the only chromatic
  moment on the page, so it carries enormous weight.
- Footer typography is all-caps small labels (`GET STARTED`, `BUILD`, `SCALE`) — feels
  like a print magazine masthead.

**Confidence**: ✅ high — consistent across the captured area.

---

## 2. Design System (tokens)

All values below are extracted from CSS custom properties on the live site. The
`--ds-*` prefix is Vercel's Design System namespace; `--geist-*` is the older Geist
namespace (still active for marketing-specific properties). They are **explicit tokens
defined by the authors** — therefore ✅ high confidence by default.

### 2.1 Colors

#### Core surface & text

| Token | Hex | Role | Confidence |
|---|---|---|---|
| `--ds-background-100` | `#FFFFFF` | Base surface | ✅ high |
| `--ds-background-200` | `#FAFAFA` | Subtle alternate surface | ✅ high |
| `--ds-gray-1000` | `#171717` | Primary text, primary button fill | ✅ high |
| `--ds-gray-900` | `#4D4D4D` | Secondary text | ✅ high |
| `--ds-gray-700` | `#8F8F8F` | Tertiary text, icons | ✅ high |
| `--ds-gray-500` | `#C9C9C9` | Subtle borders, disabled | ✅ high |
| `--ds-gray-200` | `#EBEBEB` | Light surface, dividers | ✅ high |
| `--ds-gray-100` | `#F2F2F2` | Section background tint | ✅ high |
| `--ds-black` | `#000000` | True black (rare, foregrounds & icons) | ✅ high |
| `--ds-white` | `#FFFFFF` | True white | ✅ high |

#### Alpha scale (transparent overlays)

Vercel ships a parallel **alpha** scale (`--ds-gray-alpha-100` through `-1000`) for
elements that need to sit on photographic or gradient backgrounds. Notable values:
`#0000000D` (5%) to `#000000E8` (91%). Use these — not the solid grays — when overlaying
the hero gradient.

#### Full DS palette (10 shades × 8 hues)

The design system ships a complete numeric scale (100-1000) per hue: `blue`, `red`,
`amber`, `green`, `teal`, `purple`, `pink`, plus the gray ramp above. Pulled verbatim
in `design-tokens.json`. On the landing itself only a small subset is actually used —
most are infrastructure for product UI elsewhere on the site. The ones in evidence on
the marketing surface:

| Token | Hex | Visible role |
|---|---|---|
| `--ds-blue-700` | `#0070F7` | Focus ring color (`--ds-focus-color`) |
| `--ds-green-700` | `#28A948` | Status indicator (system status dot in footer) |

#### Marketing-only accents

| Token | Hex | Use |
|---|---|---|
| `--geist-marketing-gray` | `#FAFBFC` | Marketing-page background tint |
| `--geist-violet` | `#7928CA` | Marketing decorative accent |
| `--geist-cyan` | `#50E3C2` | Marketing decorative accent (visible in hero gradient) |
| `--geist-highlight-pink` | `#FF0080` | Marketing emphasis (used sparingly) |

### 2.2 Typography

- **Sans family** (`--font-sans`): `ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji"`
- **Sans fallback chain** (`--font-sans-fallback`): `"Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", ...`
- **Mono family** (`--font-mono`): `ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New"`
- **Custom**: `--font-pixel-square` references a Geist custom pixel font (`var(--font-geist-pixel-square)`) — likely loaded via `next/font` and not extractable from CSS vars alone

The actual rendered font on the live page is **Geist Sans** (Vercel's house typeface),
loaded via `next/font` — that's why it doesn't appear directly in `--font-sans` (which
holds the fallback chain).

**Weight scale** (`--font-weight-*`): 100, 300, 400, 500, 600, 700, 800

**Size scale** (`--text-*` Tailwind-aligned):

| Token | rem | px (approx) | Use |
|---|---|---|---|
| `--text-xs` | 0.75 | 12 | Small UI text |
| `--text-sm` | 0.875 | 14 | Body small, footer labels |
| `--text-base` | 1.0 | 16 | Body default |
| `--text-lg` | 1.125 | 18 | Lead paragraphs |
| `--text-xl` | 1.25 | 20 | Subheadings |
| `--text-2xl` | 1.5 | 24 | Section titles |
| `--text-3xl` | 1.875 | 30 | Block titles |
| `--text-5xl` | 3.0 | 48 | Hero title (display) |

**Fluid typography scale** (`--text-fluid-*`) — multiple `clamp()` tokens for responsive
type. Notable: `--text-fluid-32-80` ranging from 2rem to 5rem — likely the hero treatment
at larger viewports.

**Tracking** (`--tracking-*`): `tighter: -0.05em`, `tight: -0.025em`, `normal: 0em` —
heading-tight tracking declared.

**Leading** (`--leading-*`): `tight: 1.25`, `normal: 1.5`, `relaxed: 1.625`.

### 2.3 Spacing

**Base unit**: 4px (`--geist-space: 4px`).

**Geist spacing scale**:

| Token | px | Multiplier |
|---|---|---|
| `--geist-space` | 4 | 1× |
| `--geist-space-2x` | 8 | 2× |
| `--geist-space-3x` | 12 | 3× |
| `--geist-space-4x` | 16 | 4× |
| `--geist-space-6x` | 24 | 6× |
| `--geist-space-8x` | 32 | 8× |
| `--geist-space-10x` | 40 | 10× |
| `--geist-space-16x` | 64 | 16× |
| `--geist-space-24x` | 96 | 24× |
| `--geist-space-32x` | 128 | 32× |
| `--geist-space-48x` | 192 | 48× |
| `--geist-space-64x` | 256 | 64× |

**Negative variants** also declared (`--geist-space-*-negative`) for asymmetric layout
adjustments.

**Page widths**: `--geist-page-width: 1200px` (older Geist) and `--ds-page-width: 1400px`
(newer Design System) coexist — the marketing surface uses the 1200px container.

**Fluid spacing** (`--spacing-fluid-*`): responsive `clamp()` values for vertical rhythm
that scales with viewport.

### 2.4 Radii

| Token | Value | Use |
|---|---|---|
| `--geist-radius` | 6px | Default — buttons, inputs, tags |
| `--geist-marketing-radius` | 8px | Marketing-specific surfaces |
| `--code-block-radius` | 6px | Code blocks |
| `--modal-radius` | 12px | Modals |
| `--radius-3xl` | 1.5rem (24px) | Large container radius (Tailwind alignment) |

### 2.5 Shadows

A complete 7-step elevation system:

| Token | Value (verbatim) |
|---|---|
| `--ds-shadow-2xs` | `0px 1px 1px #0000000a` |
| `--ds-shadow-xs` | `0px 1px 2px #0000000a` |
| `--ds-shadow-small` | `0px 2px 2px #0000000a` |
| `--ds-shadow-medium` | `0px 2px 2px #0000000a, 0px 8px 8px -8px #0000000a` |
| `--ds-shadow-large` | `0px 2px 2px #0000000a, 0px 8px 16px -4px #0000000a` |
| `--ds-shadow-xl` | `0px 1px 1px #00000005, 0px 4px 8px -4px #0000000a, 0px 16px 24px -8px #0000000f` |
| `--ds-shadow-2xl` | `0px 1px 1px #00000005, 0px 8px 16px -4px #0000000a, 0px 24px 32px -8px #0000000f` |

Plus semantic compounds: `--ds-shadow-tooltip`, `--ds-shadow-menu`, `--ds-shadow-modal`,
`--ds-shadow-fullscreen` — each composing the base shadows with a border-stroke shadow.

### 2.6 Borders

- Default: 1px solid (using `--ds-shadow-border` as a 1-pixel-inset shadow, not a real
  border — a Geist trick for subpixel control on Retina screens)
- Focus ring: `--ds-focus-ring: 0 0 0 2px var(--ds-background-100), 0 0 0 4px var(--ds-focus-color)`
  — a 2px white inner ring + 2px blue outer ring (`#0070F7`)

### 2.7 Motion

- `--ease-in`: `cubic-bezier(.4, 0, 1, 1)`
- `--ease-out`: `cubic-bezier(0, 0, .2, 1)`
- `--ease-in-out`: `cubic-bezier(.4, 0, .2, 1)`
- `--ds-motion-timing-swift`: `cubic-bezier(.175, .885, .32, 1.1)` *(slight overshoot)*
- `--default-transition-duration`: `0.15s`
- `--ds-motion-overlay-duration`: `0.3s`
- `--ds-motion-popover-duration`: `0.2s`

### 2.8 Accessibility quick-check

See companion `design-a11y.md`. Summary:
- Primary text on background: **17.93:1** — AAA ✅
- Muted text on background: **8.45:1** — AAA ✅
- CTA label on primary button: **17.93:1** — AAA ✅
- Focus blue on background: **4.51:1** — AA ✅, AAA ❌ *(adequate for focus rings)*

---

## 3. Components Inventory

### Button (primary, observed in hero CTA, "Ready to deploy?" CTA)

- **Fill**: `--ds-gray-1000` (#171717)
- **Label**: `--ds-white` (#FFFFFF)
- **Radius**: `--geist-radius` (6px)
- **Iconography**: optional left-icon (Vercel triangle on "Start Deploying")
- **Padding**: ~24px horizontal, ~10px vertical (visual estimate)
- **Confidence**: ✅ high — observed in 2+ contexts

### Button (secondary / outline, observed as "Get a Demo")

- **Fill**: `--ds-white` / transparent
- **Border**: 1px via `--ds-shadow-border`
- **Label**: `--ds-gray-1000` (#171717)
- **Radius**: `--geist-radius` (6px)
- **Confidence**: ✅ high

### Button (ghost text link, observed as "Talk to an Expert")

- **No fill, no border**
- **Label**: `--ds-gray-1000`
- **Hover**: presumably underline / opacity shift (not captured in static)
- **Confidence**: ⚠️ medium — only one instance captured

### Tag / Pill (observed in "Scale your [Enterprise] without compromising [Security]")

- **Fill**: `--ds-gray-100` (#F2F2F2) very subtle
- **Border**: 1px subtle (via `--ds-shadow-border`)
- **Radius**: `--geist-marketing-radius` (8px) or pill (full-rounded — both visible)
- **Padding**: tight (~6px horizontal, ~2px vertical)
- **Label**: `--ds-gray-1000`
- **Confidence**: ✅ high

### Status indicator (observed in footer)

- **Form**: small filled circle (~8px diameter)
- **Color**: `--ds-green-700` (#28A948) for "operational"
- **Label**: tiny mono-spaced text alongside (likely from `--font-mono`)
- **Confidence**: ✅ high

### Top navigation

- **Layout**: horizontal, left-aligned product/resources/solutions dropdowns, right-aligned
  Sign In / Log In / Sign Up
- **Background**: transparent over white surface
- **Items**: text-only with subtle hover (no visible bg change captured in static)
- **Confidence**: ⚠️ medium — dropdowns not exercised

### Footer link matrix

- **Structure**: 10 columns × variable rows of categorized links
- **Section labels**: all-caps small (`--text-sm` or `--text-xs`), `--ds-gray-700`
- **Links**: regular case, `--ds-gray-1000`
- **Confidence**: ✅ high

---

## 4. Layout & Composition

- **Container**: `--geist-page-width` 1200px (used on the marketing surface)
- **Horizontal padding**: ~24px on desktop
- **Inferable breakpoints**: ❓ low — only desktop captured. Vercel's fluid typography
  scale (`--text-fluid-*` and `--spacing-fluid-*`) implies viewport-aware scaling rather
  than discrete breakpoints, but no responsive evidence in this capture.
- **Vertical rhythm**: hero ≈ 600px tall; section blocks separated by `--geist-space-24x`
  (96px) approximately
- **Composition patterns**:
  - Hero: centered, vertical stack (eyebrow / h1 / sub / CTA-pair / brand graphic)
  - Mid: "Develop with your favorite tools" — three line-items with inline icons
  - Mid: "Scale your X without compromising Y" — large typographic phrase with inline pills
  - CTA: split — primary CTA pair on the left, separate Enterprise pull-out on the right
  - Footer: dense 10-column link matrix + brand mark + status indicator
- **Visual hierarchy**: typography size + weight do nearly all of the work. Color
  contrast is limited to the binary (black on white, occasionally muted).

---

## 5. Reconstruction Notes

### Suggested stack

**Tailwind v4 + the Geist family of npm packages.**

Justification: the extracted CSS uses Tailwind utilities everywhere (`--tw-*` prefixed
properties confirm), plus a layered design-tokens system. The Geist tokens are published
by Vercel as `@vercel/geist` / available via `geist/font` for typography. For
reconstruction outside the Vercel ecosystem, treat the extracted token map as Tailwind
v4 `@theme` directives and load `Geist Sans` via `next/font/google` or `geist/font/sans`.

### Quick wins

- **Typography is one import.** `import { GeistSans } from 'geist/font/sans'` and the
  display surface is faithful.
- **Palette is a copy-paste.** Drop the 80+ DS palette tokens from `design-tokens.json`
  into a Tailwind v4 `@theme` block (or feed via Style Dictionary).
- **Buttons are trivial.** Black fill, white text, 6px radius, ~10/24 padding — three
  Tailwind utilities reproduce the primary CTA exactly.

### Tricky bits

- **The hero gradient is the brand.** Reproducing the orange → green → teal multi-stop
  gradient behind the Vercel triangle needs careful tuning; this is the one piece you
  cannot extract as a token (it's an SVG/CSS asset, not a CSS variable). Get a
  high-resolution capture of just that area and treat it as a custom asset.
- **Dot-grid background.** The hero has a subtle dot pattern overlay — not a CSS variable
  either, likely an SVG. Reproduce as `background-image: radial-gradient(circle, #0000000a 1px, transparent 1px)` with appropriate sizing.
- **Geist Sans is proprietary.** Free for personal/Vercel-deployed projects via the
  `geist/font` npm package, but licensing for non-Vercel commercial use deserves a check.
- **`--ds-shadow-border` instead of real borders.** Vercel uses inset shadows for
  1-pixel divisions, which renders crisper on Retina. Replicate the trick if pixel
  fidelity matters; otherwise standard borders are fine.
- **The custom `--font-pixel-square`** (Geist pixel square) is a display font used in
  rare moments not visible in this capture. If your reconstruction needs it, it'll
  require licensing the Geist font family fully.

### Responsive coverage gotcha

Only the desktop layout was captured. Vercel's site collapses heavily on mobile (the
10-column footer becomes a single accordion). Before assuming breakpoint values,
re-capture with:

```bash
python scripts/capture_site.py https://vercel.com/ \
    --viewports desktop,tablet,mobile
```

### Implicit states to define

- Button hover (visible color shift expected; not captured statically)
- Button disabled
- Link hover (likely underline or opacity)
- Top-nav dropdown open state
- Input focus (use `--ds-focus-ring`)
- Status indicator other states (yellow = degraded, red = down)

### Confidence map

| Layer | Confidence | Why |
|---|---|---|
| Identity | ✅ high | Strong signals across hero + middle + footer |
| Colors | ✅ high | Extracted from declared CSS vars — not inferred |
| Typography | ✅ high (system), ⚠️ medium (rendered font) | Tokens declared; actual Geist Sans loaded via `next/font` (outside CSS scope) |
| Spacing | ✅ high | Full named scale declared |
| Shadows | ✅ high | 7-tier system declared verbatim |
| Components | ⚠️ medium | Captured 5-6 components; product surface (dashboard) would expose many more |
| Layout | ❓ low | Only desktop captured |
| Motion | ⚠️ medium | Easing/duration tokens declared but actual animation behavior not exercised |

---

## 6. Open Questions

- What are the **actual breakpoint values**? `--text-fluid-*` and `--spacing-fluid-*`
  suggest fluid scaling, not discrete breakpoints — but the layout collapses must
  happen somewhere. Need a mobile capture to confirm.
- Is there a **dark mode**? No `prefers-color-scheme` rules were observed in the captured
  CSS, but the alpha gray scale would support one cleanly.
- What are the **hover and focus states** for the primary button? Not exercised in a
  static capture.
- **`--ds-page-width: 1400px` vs `--geist-page-width: 1200px`** — both coexist. Is 1400
  the new container for newly designed sections (e.g., the AI Cloud surface) and 1200
  the legacy marketing default? Marketing surface used 1200 in this capture.
- The hero gradient asset — is it an SVG, a Canvas, or a layered CSS gradient? Not
  determinable from CSS vars; would need to inspect the rendered DOM.

---

## 7. Companion files

- [x] `design-tokens.json` — structured tokens in W3C DTCG format
      (`$value` / `$type`)
- [x] `design-a11y.md` — WCAG 2.1 contrast report
- [x] `capture.png` — desktop screenshot (downsized to 720×1095, 233 KB; original was 1440×2191)
- [ ] `capture.html` — rendered HTML not committed (regenerable, 520 KB)
- [ ] `css-vars-raw.json` — raw 808-var extraction not committed (regenerable, 224 KB)

---

*End of analysis. To go further: refine the analysis with a mobile capture, convert this
document into a Claude Code prompt for a from-scratch rebuild, or compare against another
landing in the Geist aesthetic family (Linear, Resend, Cal.com).*
