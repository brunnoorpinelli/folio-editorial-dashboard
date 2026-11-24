---
name: folio-editorial-dashboard
description: Generates interactive editorial data visualization websites, scrollytelling reports, and analytics dashboards in the Folio Swiss-editorial design system — complete with 7 WCAG 2.2 AA colour themes, Geist typography, 12-column spined spreads, 10 zero-dependency responsive SVG chart patterns, expandable accessible data tables, keyboard navigation, and client-side CSV export.
---

# Folio Editorial Data Visualization & Dashboard Skill

Use this skill whenever the user asks to generate an interactive data visualization site, annual report, benchmark study, research microsite, scrollytelling narrative, or editorial analytics dashboard based on the **Folio** template.

---

## 1. Design Philosophy & Non-Negotiable Rules

1. **Swiss-Editorial Precision (Zero Border-Radius)**:
   - Use `border-radius: 0` across all buttons, segmented controls, inputs, tables, tooltips, theme drawers, and chart bars.
   - Exceptions are strictly functional: the sliding header pager circle (`.pager-circle`), the half-filled theme glyph (`.band-btn-glyph`), and circular SVG data markers (dumbbell dots, scatter circles, sparkline endpoints).
   - Separate hierarchy with 1px architectural hairlines (`var(--rule)` for secondary dividers, `var(--ink)` for strong horizontal rules and table headers).
2. **Semantic Colour Tokenization (Never Hardcode Hex in Components/JS)**:
   - All UI surfaces, inks, rules, and data series MUST reference CSS custom properties.
   - `--violet`, `--vermilion`, and `--magenta` represent **Data Series A, B, and C** across all 7 themes (not literal colours).
   - Whenever Series A or Series B appears as text on paper, always use `--violet-ink` and `--vermilion-ink` for guaranteed WCAG 2.2 AA (`>= 4.5:1`) contrast.
   - Whenever text sits directly inside a solid data bar/segment, always use `--on-violet`, `--on-vermilion`, and `--on-magenta`.
   - `--white` represents the ink colour on `--band` and dark heatmap cells (`500–600`); in `night-edition`, `--band` is light amber (`#F2B84B`), so `--white` flips to `#121318`.
3. **Zero External Charting Libraries**:
   - Render all charts with crisp, responsive **vanilla SVG** (`viewBox` updated via `ResizeObserver`) or **semantic HTML tables** (`role="grid"`).
   - Keep the entire application self-contained, fast, and remixable with a single structured `DATA` object.
4. **Universal Data Accessibility**:
   - Every visual figure MUST pair with a `.fig-caption` block containing a figure number (`FIG. 01`), descriptive caption, data source in `Geist Mono`, and a working `"Show data table ↓"` toggle button that dynamically renders an accessible HTML `<table>` synchronized to the active filter/sort state.
   - Support full keyboard navigation (arrow keys inside charts/grids, roving `tabindex`, `J`/`K`/`1–0`/`G`/`Escape` global shortcuts) and a shared polite `aria-live` region (`#liveRegion`).
   - Honour `@media (prefers-reduced-motion: reduce)` across all animations, transitions, View Transitions, count-ups, and scroll-scrubbed strokes.
5. **Typography & Tabular Discipline**:
   - Load **Geist** (`wght@300..700`) and **Geist Mono** (`wght@400;500`) from Google Fonts.
   - Apply `font-variant-numeric: lining-nums tabular-nums;` (`.num`) to every KPI, table numeric cell, folio page number, pager link, and tooltip value.

---

## 2. Complete Design Tokens & 7 WCAG 2.2 AA Colour Themes

### 2.1 Pre-Paint Anti-FOUC Theme Script (Place in `<head>` before `<style>`)

```html
<meta name="theme-color" content="#6C50F6">
<script>
  (function () {
    var ids = [
      'lavender-press', 'newsprint', 'night-edition',
      'gazette-mono', 'swiss-poster', 'forest-ochre', 'riso-pastel'
    ];
    var t = 'lavender-press';
    try {
      var s = localStorage.getItem('folio.theme.v1');
      if (ids.indexOf(s) > -1) t = s;
    } catch (e) {}
    document.documentElement.setAttribute('data-theme', t);
  })();
</script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@300..700&family=Geist+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### 2.2 Core Scale, Grid & Motion Tokens

```css
:root {
  /* Derived theme-agnostic tokens */
  --band-hairline: color-mix(in srgb, var(--white) 45%, transparent);
  --band-rule: color-mix(in srgb, var(--white) 25%, transparent);
  --band-progress: color-mix(in srgb, var(--white) 60%, transparent);
  --grid-line: color-mix(in srgb, var(--violet) 15%, transparent);
  --scrim: color-mix(in srgb, var(--ink) 38%, transparent);
  color-scheme: light;

  /* Type families */
  --font-sans: "Geist", "Inter", "Helvetica Neue", Arial, system-ui, sans-serif;
  --font-mono: "Geist Mono", ui-monospace, SFMono-Regular, Menlo, monospace;

  /* Fluid type scale */
  --t-numeral: clamp(96px, 13vw, 232px);
  --t-display: clamp(44px, 5.2vw, 84px);
  --t-h2: clamp(32px, 3.4vw, 54px);
  --t-kpi: clamp(44px, 4.6vw, 76px);
  --t-h3: 24px;
  --t-lede: clamp(18px, 1.5vw, 21px);
  --t-body: 15px;
  --t-label: 13px;
  --t-micro: 11px;

  /* 8pt spacing scale */
  --s-1: 4px;   --s-2: 8px;   --s-3: 16px;  --s-4: 24px;  --s-5: 32px;
  --s-6: 48px;  --s-7: 64px;  --s-8: 96px;  --s-9: 128px; --s-10: 192px;

  /* Grid */
  --gutter: 24px;
  --outer-margin: clamp(20px, 4vw, 64px);
  --container-max: 1440px;

  /* Motion */
  --ease-out: cubic-bezier(0.2, 0, 0, 1);
  --ease-in-out: cubic-bezier(0.7, 0, 0.2, 1);
  --ease-std: cubic-bezier(0.4, 0, 0.2, 1);
  --d-1: 120ms; --d-2: 240ms; --d-3: 600ms; --d-4: 900ms; --d-5: 1200ms;
}

:root[data-theme="night-edition"] {
  --scrim: color-mix(in srgb, #000000 62%, transparent);
  color-scheme: dark;
}

@media (min-width: 1280px) { :root { --t-body: 16px; } }
@media (max-width: 767px)  { :root { --gutter: 16px; --t-numeral: clamp(72px, 26vw, 120px); } }
@media (max-width: 479px)  {
  :root {
    --t-display: clamp(34px, 9.5vw, 44px);
    --t-h2: clamp(26px, 7.2vw, 32px);
    --t-kpi: clamp(34px, 9.5vw, 44px);
  }
}
```

### 2.3 All 7 Curated Theme Blocks (Shared by `:root` and `[data-swatch]`)

```css
/* 1. Lavender Press (Default) — violet masthead, lilac stock, vermilion signal */
:root,
:root[data-theme="lavender-press"],
[data-swatch="lavender-press"] {
  --paper: #E8E6EF;        --paper-raised: #F4F3F8;
  --ink: #121116;          --ink-2: #4A4752;          --ink-3: #625F6B;
  --rule: #B8B4C4;         --band: #6C50F6;           --white: #FFFFFF;
  --violet: #7C63FF;       --violet-ink: #5A3BE6;
  --vermilion: #E2492C;    --vermilion-ink: #B5321A;  --magenta: #C04BD9;
  --on-violet: #121116;    --on-vermilion: #121116;   --on-magenta: #121116;
  --violet-100: #E4DFFC;   --violet-200: #C9BEFB;     --violet-300: #A797FC;
  --violet-400: #7C63FF;   --violet-500: #5A3BE6;     --violet-600: #3A22A8;
  --vermilion-tint: #F2A391; --violet-tint: #B9ABFB;  --neutral-data: #CFCBD9;
}

/* 2. Newsprint — warm uncoated stock, black masthead, cobalt and brick inks */
:root[data-theme="newsprint"],
[data-swatch="newsprint"] {
  --paper: #F0EADB;        --paper-raised: #F8F4EA;
  --ink: #1A1714;          --ink-2: #4A4239;          --ink-3: #655B4F;
  --rule: #C9BEA9;         --band: #1A1714;           --white: #F0EADB;
  --violet: #2C4F8C;       --violet-ink: #2C4F8C;
  --vermilion: #B23A1E;    --vermilion-ink: #9E3219;  --magenta: #8C6A1C;
  --on-violet: #F8F4EA;    --on-vermilion: #F8F4EA;   --on-magenta: #F8F4EA;
  --violet-100: #E4E3E0;   --violet-200: #C3CAD6;     --violet-300: #94A5C0;
  --violet-400: #6D84AC;   --violet-500: #2C4F8C;     --violet-600: #1B2F57;
  --vermilion-tint: #E6AE9C; --violet-tint: #A9B8D2;  --neutral-data: #D6CDBB;
}

/* 3. Night Edition — charcoal pages, amber masthead for dark-mode reading */
:root[data-theme="night-edition"],
[data-swatch="night-edition"] {
  --paper: #121318;        --paper-raised: #1C1E25;
  --ink: #ECEAF2;          --ink-2: #B9B6C4;          --ink-3: #9893A3;
  --rule: #3A3C47;         --band: #F2B84B;           --white: #121318;
  --violet: #8FA2FF;       --violet-ink: #A9B6FF;
  --vermilion: #FF7A59;    --vermilion-ink: #FF8F73;  --magenta: #E58AF0;
  --on-violet: #121318;    --on-vermilion: #121318;   --on-magenta: #121318;
  --violet-100: #1E2133;   --violet-200: #283063;     --violet-300: #33408A;
  --violet-400: #3F52A8;   --violet-500: #8FA2FF;     --violet-600: #C7D0FF;
  --vermilion-tint: #6B3A30; --violet-tint: #3B437A;  --neutral-data: #3A3C47;
}

/* 4. Gazette Mono — pure monochrome greyscale editorial print */
:root[data-theme="gazette-mono"],
[data-swatch="gazette-mono"] {
  --paper: #FFFFFF;        --paper-raised: #F4F4F4;
  --ink: #0A0A0A;          --ink-2: #3D3D3D;          --ink-3: #595959;
  --rule: #BDBDBD;         --band: #0A0A0A;           --white: #FFFFFF;
  --violet: #4A4A4A;       --violet-ink: #0A0A0A;
  --vermilion: #8C8C8C;    --vermilion-ink: #4A4A4A;  --magenta: #6B6B6B;
  --on-violet: #FFFFFF;    --on-vermilion: #0A0A0A;   --on-magenta: #FFFFFF;
  --violet-100: #F0F0F0;   --violet-200: #D6D6D6;     --violet-300: #B3B3B3;
  --violet-400: #8C8C8C;   --violet-500: #4A4A4A;     --violet-600: #1F1F1F;
  --vermilion-tint: #CFCFCF; --violet-tint: #A6A6A6;  --neutral-data: #E0E0E0;
}

/* 5. Swiss Poster — International Style red & black with cobalt counterpoint */
:root[data-theme="swiss-poster"],
[data-swatch="swiss-poster"] {
  --paper: #F3F1EC;        --paper-raised: #FBFAF7;
  --ink: #111111;          --ink-2: #3F3F3F;          --ink-3: #5E5E5E;
  --rule: #C4C1BA;         --band: #D7141A;           --white: #FFFFFF;
  --violet: #D7141A;       --violet-ink: #B5101A;
  --vermilion: #1E4FD6;    --vermilion-ink: #1A44B8;  --magenta: #808080;
  --on-violet: #FFFFFF;    --on-vermilion: #FFFFFF;   --on-magenta: #111111;
  --violet-100: #F6E3DF;   --violet-200: #F0BDB5;     --violet-300: #E88A7F;
  --violet-400: #E0564C;   --violet-500: #C21218;     --violet-600: #7E0B10;
  --vermilion-tint: #B5C6F2; --violet-tint: #EFA49C;  --neutral-data: #D9D6CF;
}

/* 6. Forest & Ochre — field-guide greens, ochre and rust on oat stock */
:root[data-theme="forest-ochre"],
[data-swatch="forest-ochre"] {
  --paper: #EAE6D6;        --paper-raised: #F4F1E6;
  --ink: #18201A;          --ink-2: #3F4A40;          --ink-3: #58625A;
  --rule: #BDB89F;         --band: #24432E;           --white: #F4F1E6;
  --violet: #3B7A4B;       --violet-ink: #2C6339;
  --vermilion: #8F5B06;    --vermilion-ink: #835406;  --magenta: #A5432A;
  --on-violet: #F4F1E6;    --on-vermilion: #F4F1E6;   --on-magenta: #F4F1E6;
  --violet-100: #E3E5D3;   --violet-200: #C3D3B6;     --violet-300: #9DBB92;
  --violet-400: #76A06F;   --violet-500: #2F6A3F;     --violet-600: #1B3F26;
  --vermilion-tint: #E2C48B; --violet-tint: #A9C6A3;  --neutral-data: #D3CDB6;
}

/* 7. Riso Pastel — two-drum risograph: fluoro pink and medium blue on cream */
:root[data-theme="riso-pastel"],
[data-swatch="riso-pastel"] {
  --paper: #F7F2E8;        --paper-raised: #FDFAF4;
  --ink: #22203A;          --ink-2: #4C4864;          --ink-3: #625E7A;
  --rule: #D9CFE0;         --band: #D6246E;           --white: #FFFFFF;
  --violet: #0074BE;       --violet-ink: #005E9C;
  --vermilion: #D6246E;    --vermilion-ink: #B01A58;  --magenta: #007F78;
  --on-violet: #FFFFFF;    --on-vermilion: #FFFFFF;   --on-magenta: #FFFFFF;
  --violet-100: #EAF1F7;   --violet-200: #CFE3F2;     --violet-300: #A6CDEB;
  --violet-400: #6FB0E0;   --violet-500: #0067A8;     --violet-600: #00446F;
  --vermilion-tint: #F7B6CF; --violet-tint: #B5D8F2;  --neutral-data: #E3DCEB;
}
```

---

## 3. Page Architecture, Grid & Chrome

### 3.1 Fixed 40px Editorial Header Band (`.band`)
- Fixed at `top: 0`, `height: 40px`, `background: var(--band)`, `color: var(--white)`, `z-index: 200`.
- **Left**: Folio left-page number (e.g., `002`) + fading running section title (`Overview`).
- **Centre** (`>= 1280px`): Compact inline logotype (`brand | Title Year`) separated by a 1px `0.8em` vertical rule.
- **Right**:
  - Desktop (`>= 768px`): Numbered pager (`01`–`10`) with a 20×20 circular indicator (`.pager-circle`) that translates horizontally (`translate3d(idx * 24 + 2, 0, 0)`) and inverts active numeral colour to `var(--band)`.
  - Mobile (`< 768px`): `← Section 01 / 10 →` toggle button opening a full-width `.pager-dropdown` grid menu.
  - Right folio page number (`003`), 1px separator, and `Control panel` button (`aria-haspopup="dialog"`) with a split-circle glyph (`.band-btn-glyph`).
- **Bottom Edge**: 2px `.scroll-progress` bar updated in a single `requestAnimationFrame` scroll loop (`transform: scaleX(p)`).

### 3.2 Editorial Spread System (`.spread`)
- Each section is a `<section class="spread" id="s01" data-sec-index="0" data-reveal>` (`min-height: 100svh`, `padding-top: var(--s-8)`, `padding-bottom: var(--s-7)`, `border-bottom: 1px solid var(--rule)`).
- Inner `.spread-container` (`max-width: 1440px`, `padding: 0 var(--outer-margin)`) holds a 12-column `.spread-grid`.
- **Spined Spreads** (`.spread--spined`): On desktop (`>= 1024px`), render a continuous 1px centre book spine at `left: 50%` using `.spread--spined .spread-grid::before`. `.page-left` spans `1 / 7` (`padding-right: var(--s-3)`), `.page-right` spans `7 / 13` (`padding-left: var(--s-3)`).
- **Alternating Rhythm**: Alternate visual-left / text-right and text-left / visual-right across consecutive spreads. On mobile (`<= 1023px`), apply `.page-headline-first` (`order: -1`) to right-hand text columns so headlines always precede their charts.
- **Masked Line-Reveal Headlines**: Wrap every headline line in `<span class="line"><span class="line-inner">...</span></span>`. Keep `.headline-h2` concise (`max-width: 17ch`, 2–3 lines, `font-weight: 430`, `letter-spacing: -0.028em`). When `IntersectionObserver` adds `.is-in`, lines slide smoothly from `translateY(105%)` to `0` with `80ms` / `160ms` stagger.
- **Two-Column Editorial Prose**: Use `.body-cols` (`grid-template-columns: repeat(2, minmax(0, 1fr))`, collapsing to `1fr` on `< 768px`, `max-width: 42ch` per paragraph) beneath a `1px solid var(--ink)` `.rule-strong`.

### 3.3 Theme Control Panel (`<dialog id="themePanel">`)
- Slide-over right drawer (`width: 400px`, full-width on mobile, `border-left: 1px solid var(--ink)`, `background: var(--paper-raised)`).
- Renders radio options for all 7 themes, each displaying a pure-CSS **88×60 miniature preview card** (`.theme-swatch[data-swatch="<id>"]`) showing the theme's header band, text rule, 3 series bars, and 6-step tonal ramp.
- Uses `document.startViewTransition()` when available (with `.theme-fading` CSS fallback) and persists selection to `localStorage` (`folio.theme.v1`).
- Updates `<meta name="theme-color">` from computed `--band` and dispatches `folio:themechange`.

### 3.4 Global Keyboard Shortcuts
- `J` / `K`: Jump to next / previous spread and focus its heading.
- `1`–`9`, `0`: Jump directly to spreads `01`–`10`.
- `G`: Toggle the fixed 12-column editorial debug grid overlay (`#gridOverlay`).
- `Escape`: Dismiss tooltip or close mobile pager dropdown (native dialog `cancel` closes theme panel).

---

## 4. Catalog of 10 Responsive Chart & Layout Archetypes

Select all 10 spreads for full flagship reports, or pick the subset of archetypes that best matches the user's dataset. Every chart lives inside a dedicated host `<div id="chartXX">` paired with `<div id="capXX">`.

| # | Archetype | Spread Layout | Key Interactions & Visual Mechanics |
|---|---|---|---|
| **01** | **Hero + Unit Matrix Chart + 4-KPI Strip** | Full masthead + 6/6 split + 4-col `.hero-kpi-strip` | 1 SVG `<rect>` per record; segmented control (`All` / dimensions) animates every square via `translate3d` with staggered diagonal delays; count-up KPIs (`data-countup`). |
| **02** | **100% Stacked Bar + Sortable Summary Table** | Spined (Visual left, Text right) | Horizontal 48px SVG bar with staggered leader ticks (`>= 440px`) or compact chip legend (`< 440px`). Paired sortable table with inline 56×4px bar meters; hovering table rows cross-highlights matching squares in Chart 01. |
| **03** | **Animated Dumbbell Comparison Chart** | Spined (Text left, Visual right) | Baseline hollow circle (`fill: var(--paper)`, `stroke: var(--ink)`) vs latest filled dot (`var(--violet)`). Line stroke uses `var(--violet)` for positive delta and `var(--vermilion)` for negative. Segmented sort (`By Latest` / `By Change` / `By Baseline`) smoothly translates row groups via `translate3d`. Stacks vertically below `460px`. |
| **04** | **Signature Rotated-Numeral Vertical KPI Bars** | Spined (Lead stat & prose left, Headline & bars right) | 3 monumental vertical bars (`scaleY` animation) with 90° counter-clockwise `--t-numeral` percentages. Uses a **dual-layer clip-path inversion technique**: base numeral in `var(--ink)` + cloned overlay inside `.rot-bar-clip` (`clip-path: inset(calc((1 - var(--bar-ratio)) * 100%) 0 0 0)`) in `var(--on-violet/vermilion/magenta)` so text cleanly splits contrast across the bar edge. Regional filter chips trigger rolling exit/enter numeral swaps and update the left-column lead stat. |
| **05** | **Scroll-Scrubbed Line + Milestone Explorer** | Spined (Text & detail card left, Charts right) | SVG trajectory line drawn continuously by scroll position (`stroke-dashoffset` scrubbed from `80%` to `40%` viewport top). Below it, an aligned milestone timeline sharing the exact `xScale` places square markers on staggered vertical tiers (`tierMap`) to prevent label overlap, switching to an interactive tap list below `560px`. Clicking or arrow-keying updates `#timelineDetail`. |
| **06** | **Sticky Scrollytelling Stacked Columns** | Spined (Scroll steps left, Sticky chart right) | Right column sticks at `top: 72px`. Left column has 3 narrative `.scrolly-step` buttons (`min-height: 44svh` desktop; segmented tabs on mobile) observed via `IntersectionObserver` (`rootMargin: '-50% 0px -50% 0px'`) to isolate individual series while muting others to `var(--neutral-data)`. Includes series toggle chips (minimum 1 active), `Absolute` vs `Share of total` switch, SVG 45° diagonal `<pattern>` hatching for projected/estimate periods, and vertical hover crosshair with YoY deltas. |
| **07** | **Full-Width Sortable Matrix Heatmap** | Full-width (No spine) | Semantic `<table role="grid">` mapping values into 6 tonal bins (`--violet-100` to `--violet-600`, text flips to `var(--white)` at `>= 65`). Includes sortable column headers, row/column crosshair dimming (`.is-dim`), threshold `<input type="range">` fading sub-threshold cells to `--neutral-data`, 2D arrow-key grid navigation (`Home`/`End` supported), and sticky first column on mobile. |
| **08** | **10×10 Waffle Lattice + Diverging Likert Bars** | Spined (Charts left, Text & 3-stat strip right) | **08A**: Column-major bottom-left 100-cell SVG waffle combining solid fill (`var(--violet)`), 45° SVG hatch (`url(#waffle-hatch)`), and outlined cells (`var(--paper)`), paired with an oversized rolling rotated numeral callout and segment filter chips. **08B**: 5-point diverging stacked bars centred on the midpoint of Neutral (`SD`, `D`, `N`, `A`, `SA`) using `--vermilion`, `--vermilion-tint`, `--neutral-data`, `--violet-tint`, `--violet`, with net disagree/agree side labels and sort toggle (`Original` / `By agreement`). |
| **09** | **Multi-Shape Scatter Plot + Live OLS Regression** | Spined (Text & OLS readouts left, Scatter right) | 5 categories encoded with **both colour AND distinct SVG geometry** (`circle`, `square`, `triangle`, `diamond`, `hollow` ring) for colour-blind resilience. Interactive category chips, min-score range slider, and trend-line checkbox recalculate client-side Ordinary Least Squares (`slope`, `intercept`, `R²`) in real time, updating the dashed regression line and inline prose `[data-slope]`. Supports 2D spatial arrow-key navigation between active points. |
| **10** | **Scenario Summary + Searchable Index Table + CSV** | Spined top + Full-width bottom table (`spread--compact`) | **10A**: Compact 3-bar rotated-numeral summary (`rot-bars-stage--outlook`). **10B**: Full-width sticky-header table with 120ms debounced keyword search, region filter chips, sortable headers, inline bar meters, 96×24 SVG `<polyline>` trend sparklines, empty-state filter reset, and one-click client-side UTF-8 CSV Blob download exporting the currently filtered and sorted rows. |

---

## 5. Core JavaScript Utility Architecture

Structure all client logic inside a single IIFE (`(() => { 'use strict'; ... })()`) with one centralized `const DATA = { ... };` block at the top so the entire site can be remixed by swapping data alone.

### 5.1 Essential Helpers
- `REDUCED_MOTION`: `window.matchMedia('(prefers-reduced-motion: reduce)').matches`
- `fmt(val, dec = 0)`: `Intl.NumberFormat` formatter
- `scaleLinear(d0, d1, r0, r1)`: pure linear scale mapping domain `[d0, d1]` to range `[r0, r1]`
- `el(tag, attrs, children)` & `svg(tag, attrs, children)`: declarative DOM/SVG factories
- `renderFigureCaption(targetId, { fig, caption, source, cols, rows | getRows })`: mounts `.fig-caption` + lazy-rendered expandable `.data-table`, returning `{ refreshTable }`
- `initTooltip()`: viewport-clamped floating `#tt` tooltip positioned near pointer or focused element, auto-hiding on scroll/outside pointerdown, announcing summary via `#liveRegion`
- `countUp(node)`: cubic ease-out `requestAnimationFrame` counter for `[data-countup]` KPIs (instant under `REDUCED_MOTION`)
- `initReveal(callbacks)`: one-shot `IntersectionObserver` (`threshold: 0.20, rootMargin: '0px 0px -10% 0px'`) that adds `.is-in`, fires chart reveal animations, and starts KPI count-ups
- `initScrollLoop(onRegScroll)`: single passive scroll listener throttled by `requestAnimationFrame` updating `#scrollProgress` and scroll-scrubbed line charts

### 5.2 Shared Controls Styling Contract
- **Segmented Controls** (`.seg-group` > `.seg-btn`): `1px solid var(--ink)`, collapsed `-1px` overlapping margins, `height: 32px` (`min-height: 44px` on `pointer: coarse`), inverted active state (`background: var(--ink); color: var(--paper)` on `[aria-pressed="true"]`).
- **Action / Table Toggle Buttons** (`.fig-table-btn`): `1px solid var(--ink)`, transparent background, hover `var(--paper-raised)`, `border-radius: 0`.
- **Inputs & Sliders**: `.text-input` (`1px solid var(--ink)`, `background: var(--paper-raised)`, `border-radius: 0`), `accent-color: var(--violet-ink)` on range sliders and checkboxes.

---

## 6. Workflow for Generating a New Site with This Skill

When asked to build a site using the Folio template:

1. **Extract Domain & Editorial Angle**:
   - Define the publication brand/logotype (`brand | Report Title Year`), headline thesis, kicker labels, and domain-authentic editorial prose (concise, analytical, two-column copy).
   - Sync `<title>`, `<meta name="description">`, OpenGraph, Twitter Card, and JSON-LD `WebApplication` metadata with the new subject.
2. **Design the Structured `DATA` Object**:
   - Create realistic, internally consistent metrics matching the user's topic (or integrate the user's provided CSV/JSON data).
   - Ensure totals, percentages, regional sums, YoY deltas, and sparkline endpoints are mathematically consistent across prose, KPI strips, charts, and expandable data tables.
3. **Choose & Compose Spreads**:
   - Always start with **Section 01 (Hero Masthead + Headline + Lede + Interactive Visual + 4-KPI Strip)** and close with a **4-column Editorial Colophon (`<footer class="colophon">`)**.
   - Select between 4 and 10 spreads from the Chart Archetype Catalog above based on the data dimensions available, keeping folio numbers sequential (`002`/`003`, `010`/`011`, etc.) and alternating left/right visual rhythm.
4. **Implement All 7 Themes & Interactive Chrome**:
   - Include the anti-FOUC `<head>` script, all 7 CSS theme blocks, the fixed 40px `.band` header with responsive desktop/mobile pagers, the `<dialog id="themePanel">` drawer with CSS swatches, and `J`/`K`/`1–0`/`G`/`Escape` shortcuts.
5. **Preserve Zero-Dependency Responsive SVG & Accessibility**:
   - Attach `ResizeObserver` to every SVG chart container so charts adapt cleanly from `320px` mobile to `1440px+` ultrawide without label collisions or horizontal page overflow (`overflow-x: clip`).
   - Wire `renderFigureCaption` on every visual chart, `tooltipController` on every interactive mark (`pointerenter`/`pointermove`/`pointerleave`/`focus`/`blur`), and `prefers-reduced-motion` fallbacks.

---

## 7. Pre-Delivery Quality Checklist

- [ ] No hardcoded hex values inside component CSS or JS chart renderers (all tokens route through the 7 theme blocks).
- [ ] All 7 themes switch cleanly via the Control Panel, persist in `localStorage`, and maintain WCAG 2.2 AA contrast (including inverted `--white` on `night-edition` and `--on-*` bar text).
- [ ] Every chart resizes cleanly via `ResizeObserver` and has compact mobile adaptations (`< 460px` / `< 560px` / `< 768px`).
- [ ] Every visual figure has a working `"Show data table ↓"` toggle reflecting live filters and sorts.
- [ ] Keyboard navigation works across header pager, theme dialog (focus trap + Escape), rotated bars, timeline milestones, heatmap grid, and scatter points.
- [ ] CSV download exports the active filtered/sorted dataset cleanly.
- [ ] Reduced-motion users see immediate final states with no disorienting transforms or scroll scrubbing.
