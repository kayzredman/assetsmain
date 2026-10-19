# Brand Guide (reference palette)

Colours, type and logo are taken from the Access Bank Ghana onboarding site (https://onboarding.ghana.accessbankplc.com/) **as a guide only**. The page layout of that site is not reused; the Assets Management System has its own structure (left navigation, top bar, data-dense tables and dashboards).

Machine-readable tokens: [`design-tokens.css`](design-tokens.css).

## Colour palette

| Token | Hex | Role | Approx. Pantone (verify) |
|---|---|---|---|
| `--brand-navy` | `#004185` | Logo wordmark, sidebar and header background | 2945 C / 7687 C |
| `--brand-orange` | `#f57e20` | Logo mark, brand accents | 151 C |
| `--primary-500` | `#376cc6` | Buttons, links, active states, charts | 2727 C |
| `--primary-600` | `#2d5ba8` | Hover / pressed | 7686 C |
| `--accent-500` | `#f57f20` | Highlights, attention | 151 C |
| `--accent-600` | `#d46a1a` | Accent hover | 1665 C |
| `--success-500` | `#8aac11` | Healthy / supported / success | 376 C |
| `--success-600` | `#7a9a0f` | Success hover | 377 C |
| `--warning-500` | `#f99c00` | Nearing end-of-life | 144 C |
| `--danger-500` | `#ef4444` | Obsolete / error | 1795 C |
| `--ink-900` | `#0f172a` | Primary text | Black 6 C |
| `--line` | `#e2e8f0` | Borders and dividers | Cool Gray 1 C |
| `--surface-1` | `#f8fafc` | App background | - |

### About the Pantone column

The source site publishes **hex values only**. Pantone references above are the nearest matches judged by eye and are **approximate**. Pantone and hex rarely match exactly. Before anything is printed or an official brand reference is cited, confirm with the official Access Bank brand guidelines or a Pantone swatch book. The screens and the application use the hex values.

## Semantic mapping for this product

| Meaning | Colour | Where it shows |
|---|---|---|
| Supported / healthy | green `--success-500` | Health chips, score bars |
| Nearing EOL | amber `--warning-500` | Health chips, aging timeline |
| Obsolete / unsupported | red `--danger-500` | Health chips, KPI cards |
| Unknown / no data | grey `--ink-300` | Health chips |
| Active status | blue `--primary-500` | Status chips |
| Retired / Decommissioned | grey tones | Status chips |

Never rely on colour alone: every state also has a text label (WCAG 2.1 AA).

## Typography

- Reference font: **Effra** (used on the source site). It is a licensed typeface; we do not have the files, so mockups use the fallback stack `system-ui, -apple-system, "Segoe UI", Roboto, Arial, sans-serif`.
- If Effra files are supplied, drop them in `docs/design/brand/fonts/` and add `@font-face` rules; tokens already list Effra first.
- Scale: 12 / 13 / 14 (body) / 16 / 20 / 24 / 30 px. Weights 400, 500, 600, 700.
- Monospace (tags, IPs, hostnames): `ui-monospace, Menlo, Consolas`.

## Logo

- File: [`access-logo.png`](access-logo.png) (800 x 196 PNG, transparent background): orange diamond mark with a dark-blue "access" wordmark. Favicon: [`favicon-32x32.png`](favicon-32x32.png).
- Sampled logo colours: blue `#004185`, orange `#f57e20`.
- On dark navy backgrounds the blue wordmark is unreadable; mockups place the logo on a white or light panel (login card, sidebar header block). A reversed/white logo version should be requested from the brand owner if a dark header is desired.
- The logo is a trademark of its owner. It is stored here for internal mockups only; confirm approval before using it in production or any external material.
- Keep clear space equal to the diamond height around the logo; do not recolour, stretch or add effects.

## Components (mockup conventions)

- Radius: 6 / 10 / 14 px. Shadows are subtle; borders `--line`.
- Buttons: primary filled `--primary-500`, secondary outline, destructive `--danger-500`.
- Tables: zebra off, row hover `--surface-2`, sticky header, status chips as pills with a dot.
- Focus: 2 px outline in `--primary-500` with 2 px offset; never removed.
- Density: compact default (14 px body) because users work with long lists.
