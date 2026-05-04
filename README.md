# Æsthetic Generator

A static, zero-dependency tool for generating cohesive color palettes and font pairings — with live contrast checking, color tweaking, a practical UI preview, and one-click export.

Built as a single `index.html` file. No build steps, no frameworks, no server required.

---

## Features

### Color Palettes
- Generates from a curated library of **24 named palettes** (4–6 colors each)
- **WCAG AA contrast badges** on every swatch — vs. white and vs. black, pass/fail shown inline
- Click any swatch to copy its HEX value to clipboard

### Color Tweaker
- **Accent Color picker** — override the palette's accent with any color you choose
- **Saturation slider** — from full greyscale (0%) to maximum vibrancy (200%); 100% = unchanged
- **Hue Shift slider** — rotate all hues by −180° to +180° to explore new color temperatures
- Tweaks persist across shuffles so you can keep a structure while changing the colors
- Reset button to restore palette defaults

### Font Pairings
- **15 curated Google Font pairs** — each a fixed heading/body combination chosen for quality
- Fonts are loaded lazily — only the selected pair is ever fetched
- Live typography preview: H1, subheading, paragraph, and button sample

### Palette in Context
- A simulated SaaS landing page that paints itself dynamically with your active palette
- Includes: nav bar, hero section (gradient), feature cards, stats strip
- Uses luminance-based logic to assign dark colors to nav/dark elements and light colors to backgrounds
- Font pairing is applied to headings and body text in the preview

### Export
- **CSS Variables** — copy a `:root { }` block with `--color-1` through `--color-n` plus font family vars
- **JSON** — copy or download a structured object with palette, font names, and per-color contrast ratios

### Favorites
- Save any combination (palette + fonts + tweaks applied) to `localStorage`
- Re-apply or delete saved favorites at any time
- Favorites persist across browser sessions

### UI
- **Light/dark mode toggle** — persists to `localStorage`; preview panel stays intentionally neutral
- Responsive down to 320px
- Keyboard shortcut bar in the header (hidden on mobile)
- Animated swatch transitions with `prefers-reduced-motion` support

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Space` | Shuffle palette and fonts |
| `S` | Save current combination to Favorites |
| `C` | Copy CSS variables |
| `J` | Copy JSON |
| `↑` / `↓` | Increase / decrease saturation by 5% |
| `←` / `→` | Shift hue left / right by 5° |

> Shortcuts are disabled when focus is inside a text input or contenteditable element.

---

## Getting Started

No installation required. Just open the file:

```bash
# Clone or download the repo
git clone https://github.com/your-username/aesthetic-generator.git

# Open directly in your browser
open index.html
```

Or serve it locally:

```bash
# Python 3
python -m http.server 8080

# Node (npx)
npx serve .
```

Then visit `http://localhost:8080`.

---

## Project Structure

```
aesthetic-generator/
└── index.html        # Entire app — markup, styles, and script in one file
└── README.md
```

Everything is self-contained in `index.html`:

- **`<style>`** — CSS custom properties, layout, card system, tweaker, UI preview, responsive rules
- **`<script>`** — All JS: color math, WCAG contrast, font loading, palette rendering, tweaker logic, favorites, export, keyboard shortcuts

---

## How It Works

### Color Tweaking
Colors are converted from HEX → HSL, manipulated (saturation scaled, hue rotated), then converted back to HEX. The accent override replaces the last color in the palette (the designated accent slot) with the user-chosen color without touching the rest.

### UI Preview Color Mapping
The palette is sorted by luminance. Darkest colors are assigned to the nav and dark surfaces; lightest colors go to hero backgrounds; the last palette color (accent) drives CTAs, icons, and highlights. Text colors are chosen automatically by contrast ratio to guarantee legibility.

### Contrast Checking
WCAG 2.1 relative luminance formula is used to compute contrast ratios vs. white (`#FFFFFF`) and black (`#000000`). A badge shows pass (≥ 4.5:1) or fail for AA normal text on each swatch.

### Font Loading
Google Fonts `<link>` elements are injected into `<head>` on demand — only when a font pair is first selected. Each family is loaded at most once per session using a `Set` to track what has already been fetched.

### Storage
| Key | Contents |
|-----|----------|
| `aesthetic.favorites` | JSON array of saved combinations |
| `aesthetic.settings` | `{ theme: "light" \| "dark" }` |

---

## Palette & Font Library

### 24 Curated Palettes
Minimal · Playful · Bold · Terracotta · Forest · Slate · Marigold · Plum · Coastal · Dusk · Sakura · Nordic · Latte · Emerald · Neon Night · Rose Gold · Midnight · Peach Fuzz · Desert · Lagoon · Grape · Copper · Glacier · Charcoal

### 15 Curated Font Pairs

| Heading | Body |
|---------|------|
| Playfair Display | Source Sans 3 |
| Fraunces | Inter |
| Syne | Syne |
| Cormorant | Jost |
| Plus Jakarta Sans | Plus Jakarta Sans |
| Bodoni Moda | DM Sans |
| Libre Baskerville | Libre Franklin |
| Space Grotesk | Space Grotesk |
| Raleway | Mulish |
| Bricolage Grotesque | Literata |
| Epilogue | Epilogue |
| Unbounded | Nunito |
| Tenor Sans | EB Garamond |
| Anton | Barlow |
| Archivo Black | Archivo |

**Total combinations: 24 palettes × 15 font pairs = 360**

---

## Accessibility

- Semantic HTML with ARIA labels, landmarks, and roles throughout
- All interactive elements are keyboard navigable with visible focus rings
- Contrast badges are screen-reader friendly (ratio and pass/fail in `aria-label`)
- Dark/light mode does not affect the generated palette or preview backgrounds
- `prefers-reduced-motion` respected — animations disabled when the user requests it

---

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). Requires:
- CSS custom properties
- `navigator.clipboard` (with `execCommand` fallback for older browsers)
- `localStorage`
- ES6+ (arrow functions, destructuring, `const`/`let`, template literals)

---

## License

MIT — free to use, modify, and distribute.

---

## Credits

- Fonts via [Google Fonts](https://fonts.google.com)
- Color math based on the [WCAG 2.1 specification](https://www.w3.org/TR/WCAG21/)
- UI shell fonts: [DM Sans](https://fonts.google.com/specimen/DM+Sans) + [Instrument Sans](https://fonts.google.com/specimen/Instrument+Sans)