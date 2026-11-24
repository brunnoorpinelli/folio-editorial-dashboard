# Folio Editorial Dashboard Skill

An AI agent skill for generating interactive editorial data visualization websites, scrollytelling reports, and analytics dashboards in the **Folio** Swiss-editorial design system.

## Highlights

- **Swiss-Editorial Precision**: Architectural 1px hairlines, zero border-radius discipline, 12-column spined spreads with alternating visual/prose rhythm, and Geist + Geist Mono typography with tabular numerals.
- **7 WCAG 2.2 AA Colour Themes**: Built-in slide-over Control Panel (`<dialog>`) with live CSS miniature swatches, View Transitions, anti-FOUC pre-paint script, and `localStorage` persistence:
  - `lavender-press` (default)
  - `newsprint`
  - `night-edition` (dark mode)
  - `gazette-mono`
  - `swiss-poster`
  - `forest-ochre`
  - `riso-pastel`
- **Zero External Charting Libraries**: 10 responsive, self-contained vanilla SVG and semantic HTML chart archetypes driven by a single centralized `DATA` object and `ResizeObserver`.
- **Universal Accessibility**: Every figure pairs with a live-synced expandable HTML data table (`Show data table ↓`), full keyboard navigation (`J`/`K`, `1`–`0`, `G` grid overlay, `Escape`, 2D arrow-key traversal), polite `aria-live` announcements, and `prefers-reduced-motion` support.
- **Client-Side CSV Export**: Searchable, sortable index table with one-click UTF-8 CSV download reflecting active filters and sort order.

## Chart & Layout Archetypes

| # | Archetype | Key Mechanics |
|---|---|---|
| **01** | Hero + Unit Matrix Chart + 4-KPI Strip | Per-record SVG matrix with staggered `translate3d` regrouping and count-up KPIs |
| **02** | 100% Stacked Bar + Sortable Summary Table | Staggered leader ticks, inline bar meters, and cross-highlighting with Chart 01 |
| **03** | Animated Dumbbell Comparison Chart | Baseline vs. latest dots with animated row reordering (`By Latest` / `By Change` / `By Baseline`) |
| **04** | Rotated-Numeral Vertical KPI Bars | Monumental 90° rotated numerals with dual-layer `clip-path` contrast inversion |
| **05** | Scroll-Scrubbed Line + Milestone Explorer | Scroll-driven SVG stroke reveal paired with a synchronized tiered milestone timeline |
| **06** | Sticky Scrollytelling Stacked Columns | `IntersectionObserver` narrative steps, series toggles, hatched estimates, and YoY crosshair |
| **07** | Full-Width Sortable Matrix Heatmap | 6-bin tonal ramp, crosshair dimming, threshold slider, and 2D keyboard grid navigation |
| **08** | 10×10 Waffle Lattice + Diverging Likert Bars | 100-cell SVG waffle with hatching + 5-point diverging stacked bars centred on Neutral |
| **09** | Multi-Shape Scatter Plot + Live OLS Regression | Colour + distinct geometry encoding with real-time client-side OLS (`slope`, `intercept`, `R²`) |
| **10** | Scenario Summary + Searchable Index Table + CSV | Debounced search, region chips, SVG trend sparklines, and filtered CSV Blob export |

## Installation

### Using `skills` CLI (Recommended)

```bash
# Install globally
npx skills add brunnoorpinelli/folio-editorial-dashboard -g

# Or install in the current project
npx skills add brunnoorpinelli/folio-editorial-dashboard
```

### Manual Installation

Copy `skills/folio-editorial-dashboard/` into the skills directory used by your agent:

- **Antigravity (global)**: `~/.gemini/config/skills/folio-editorial-dashboard/SKILL.md`
- **Gemini CLI (global)**: `~/.gemini/skills/folio-editorial-dashboard/SKILL.md`
- **Claude Code (global)**: `~/.claude/skills/folio-editorial-dashboard/SKILL.md`
- **Project-local (`.agents` standard)**: `.agents/skills/folio-editorial-dashboard/SKILL.md`

## Repository Structure

```text
.
├── skills/
│   └── folio-editorial-dashboard/
│       └── SKILL.md
├── LICENSE
└── README.md
```

## License

[MIT](LICENSE)
