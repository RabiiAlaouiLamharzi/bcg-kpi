# Figma Design Best Practices — Storage Manager Dashboard

**Context:** Middle manager dashboard for monitoring enterprise data storage KPIs (BCG-sourced data, 2021–2025 forecast period).

---

## 1. Design System Foundation

### 1.1 Color Tokens
Define all colors as Figma **Local Variables** (not hardcoded), organized by semantic role:

| Token | Role | Value |
|-------|------|-------|
| `bg/surface-0` | Page background | `#F2F1ED` |
| `bg/surface-1` | Card / panel | `#FFFFFF` |
| `bg/surface-2` | Subtle fill, row hover | `#EBEAE5` |
| `border/default` | Card borders | `#C9C6BE` |
| `border/subtle` | Table row dividers | `#E0DDD8` |
| `text/primary` | Headlines, values | `#1A1A1A` |
| `text/secondary` | Labels, subtitles | `#6B6B6B` |
| `text/muted` | Supporting copy | `#A8A8A8` |
| `brand/primary` | Green, active states | `#177A47` |
| `brand/primary-light` | Green tints, badges | `#E8F5EE` |
| `status/danger` | Critical alerts | `#B91C1C` |
| `status/warn` | Caution indicators | `#B45309` |
| `status/block` | Block storage accent | `#1A4F91` |
| `status/file` | File storage accent | `#5B4FCF` |

**Best practice:** Use **color modes** (Light / Dark) on these variables from the start, even if only Light ships first. This makes a dark variant trivial later.

---

### 1.2 Type Scale
Use **two typefaces** maximum. Assign each a clear role:

| Typeface | Role | Usage |
|----------|------|-------|
| Libre Baskerville | Display / serif | Page titles, card titles, KPI values |
| IBM Plex Sans | Body / UI | Labels, body text, buttons, navigation |

**Type ramp (to define as Figma Text Styles):**

| Style name | Font | Size | Weight | Leading | Use |
|------------|------|------|--------|---------|-----|
| `display/page-title` | Baskerville | 27 | Bold | 1.15 | Dashboard H1 |
| `display/card-title` | Baskerville | 14 | Bold | 1.3 | Chart & section titles |
| `display/kpi-value` | Baskerville | 31 | Bold | 1.0 | Large metric numbers |
| `ui/eyebrow` | IBM Plex | 11 | 500 | 1.2 | Section labels, uppercase |
| `ui/body` | IBM Plex | 13 | 400 | 1.55 | Table text, card body |
| `ui/body-sm` | IBM Plex | 12 | 300 | 1.65 | Insight descriptions |
| `ui/label` | IBM Plex | 10 | 600 | 1.2 | All-caps column headers |
| `ui/caption` | IBM Plex | 11 | 300 | 1.4 | Subtitles, axis labels |

**Best practice:** Never use more than 4–5 distinct sizes on a single screen. Hierarchy is achieved through weight and color contrast, not font size alone.

---

### 1.3 Spacing Grid (8pt)
All spacing, padding, and gap values should be **multiples of 4** (prefer 8).

| Token | Value | Used for |
|-------|-------|---------|
| `space/2` | 4px | Icon gap, tight label spacing |
| `space/3` | 8px | Compact padding, badge padding |
| `space/4` | 12px | Row padding, small gap |
| `space/5` | 16px | Card internal padding |
| `space/6` | 20px | Card padding (preferred) |
| `space/7` | 24px | Section padding |
| `space/8` | 32px | Between major sections |
| `space/9` | 40px | Main content gutter |

---

## 2. Layout & Grid

### 2.1 Dashboard Layout Structure
```
┌──────────────────────────────────────────────────┐
│  Sidebar (232px fixed)  │  Main content (fluid)  │
│  - Logo                 │  - Topbar               │
│  - Nav groups           │  - Alert banner         │
│  - User profile         │  - KPI row (4-col)      │
│                         │  - Chart rows           │
│                         │  - Insight cards (3-col)│
│                         │  - Table                │
└──────────────────────────────────────────────────┘
```

**Key layout rules:**
- Sidebar: fixed width (232px), 100vh sticky. Never collapses in this design (no mobile target for a manager dashboard).
- Main content grid: `40px` horizontal padding, `36px` top padding, `48px` bottom padding.
- KPI row: 4 equal columns with 14px gap.
- Charts: 2-column or 3-column, never more on one row (cognitive load).
- Section spacing: `28px` between major groups.

### 2.2 Figma Auto Layout Rules
- **Every card** should be an Auto Layout frame (vertical, hug contents).
- **KPI grid** row: horizontal Auto Layout, 14px gap, fill container.
- **Section header** row: horizontal Auto Layout, 12px gap, `::after` line achieved via a frame with `fill` and height 1px.
- Use **Constraints** (left + right, or scale) for all elements inside flexible containers.

---

## 3. Component Design

### 3.1 KPI Card
Structure (top to bottom):
1. **Header row** — label (uppercase, muted) + icon badge (30×30 rounded square, colored fill)
2. **Value** — 31px Baskerville Bold, letter-spacing −1px
3. **Trend badge** — colored pill with directional arrow icon + short label
4. **Description** — 11px IBM Plex Light, 2–3 lines max
5. **Accent bar** — 4px strip, full width, color matches the card's semantic status

**Do:**
- Keep the value to ≤5 characters (e.g. `$37B`, `+10%`, `41%`).
- Use the accent bar color to immediately signal status (green = good, red = critical, amber = watch).
- Align all 4 KPIs in the same height (min-height or fixed height recommended).

**Don't:**
- Put more than one metric value per card.
- Use the same accent color for unrelated categories.

### 3.2 Chart Card
- Always pair a chart with a **descriptive subtitle** (not just a title).
- Include a **legend** inline with the card header, right-aligned — never inside the chart canvas.
- Chart canvas height: `195px` on standard cards (allows 2 rows without scrolling on 1080p screens).
- Add a `border-bottom` separator between the header and chart body to clearly delineate metadata from data.

**Chart type selection:**
| Data type | Recommended chart |
|-----------|------------------|
| Trend over time (1 series) | Line with area fill |
| Trend over time (2+ series) | Multi-line |
| Part-to-whole over time | Stacked bar |
| Two-metric comparison | Grouped bar |
| Single ratio | Donut (max 4 segments) |

### 3.3 Status Pills / Badges
Four variants only:
- **Green** — positive trend, on target
- **Red** — critical, below threshold
- **Amber** — caution, watch closely
- **Neutral/Blue** — informational, no judgment

Pill structure: `icon (9px) + text (11px, 500 weight)`. Padding: `2px 7–8px`. Corner radius: `4px`.

### 3.4 Section Header
```
│ ▌ Section Title ——————————————————————————
```
- Left accent bar: 3px wide, `brand/primary` green.
- Title: `ui/eyebrow` or `display/card-title`.
- Trailing line: 1px horizontal rule in `border/subtle`, filling remaining width.
- This pattern creates clear visual "chapters" without heavy backgrounds.

### 3.5 Alert Banner
- Left border: 4px solid `status/danger` (or relevant status color).
- Full border: 1px `status/danger` at 30% opacity — makes the box feel defined without overwhelming.
- Icon: 15px, vertically centered with first line of text.
- Title: bold, danger color. Body: light weight, slightly lighter shade.
- Use sparingly — maximum 1 alert banner per dashboard view.

### 3.6 Navigation (Sidebar)
- Active state: `border-left: 2px brand/primary` + background `rgba(brand, 14%)` + white text + icon tinted green.
- Hover state: very subtle background `rgba(white, 5%)`.
- Group labels: ALL CAPS, 9px, `text/muted` at 22% opacity — purely structural, not interactive.
- Badge (alert count): red pill, auto-width, `margin-left: auto`.

---

## 4. Data Visualization Principles

### 4.1 The Manager's Rule
Every chart should answer exactly **one question**. Write the question in the subtitle. If you can't write it in under 12 words, the chart is doing too much.

Examples:
- ✅ `"Object is overtaking File by 2025"` — tells the story
- ❌ `"File and Object-Based Worldwide Storage Revenue 2019–2025"` — describes, doesn't guide

### 4.2 Color Consistency
- Assign each storage segment a permanent color and never deviate across charts:
  - Block → `#1A4F91` (blue)
  - File → `#5B4FCF` (purple)
  - Object → `#177A47` (green)
- Public cloud / data volume → amber / red respectively (status-coded, not segment-coded).

### 4.3 Reducing Noise
- Remove gridlines or make them `rgba(0,0,0,0.05)` (barely perceptible).
- Axis tick labels: 10px, `text/muted` color.
- No chart borders. The card border is sufficient.
- Tooltips only — do not label every data point.

### 4.4 Dual-Axis Charts (Gap Chart)
- Use sparingly. Only when two series have fundamentally different scales.
- Color-code both Y-axis labels to match the series — this is the clearest way to communicate which axis belongs to which line.
- Add a visual annotation or shaded region for the "gap" to make it immediately legible.

---

## 5. Data Table Design

### 5.1 Table Anatomy
```
┌──────────────────────────────────────────────────────────┐
│  COLUMN HEADER  │  COLUMN HEADER  │  COLUMN HEADER  ...  │ ← bg/surface-2, 2px border-bottom
├──────────────────────────────────────────────────────────┤
│  Row value       │  Row value      │  Row value      ...  │ ← 1px border-bottom border/subtle
│  Row value       │  ...            │  ...            ...  │
└──────────────────────────────────────────────────────────┘
```

**Rules:**
- Outer border: `1px border/default` on the wrapping container, `8px` border-radius.
- Header row: `bg/surface-2` background, `2px` bottom border in `border/default`.
- Column headers: ALL CAPS, `ui/label` style, `text/secondary` color.
- Data rows: `1px border/subtle` between rows, last row no border.
- Hover row: `bg/surface-2` fill (very subtle).
- First column padding: `20px` left (matches card padding rhythm).
- Never center-align numeric columns in a comparison table — always right-align or left-align consistently.

### 5.2 Inline Bar Spark
For the "market share" column, use a `100px` inline bar:
- Background: `bg/surface-2`
- Fill: segment color
- Height: `5px`, `3px` radius
- Pair with the percentage text at `12px text/secondary`

This pattern conveys magnitude without adding a full chart.

---

## 6. Accessibility & Readability

| Requirement | Standard | How to meet it |
|-------------|----------|---------------|
| Text contrast (body) | WCAG AA (4.5:1) | `text/primary` `#1A1A1A` on `#FFFFFF` = 19.5:1 ✓ |
| Text contrast (labels) | WCAG AA (3:1) | `text/secondary` `#6B6B6B` on `#FFFFFF` = 5.9:1 ✓ |
| Status not color-only | WCAG 1.4.1 | Always pair color with an icon (arrow, circle, exclamation) |
| Focus states | Interactive elements | All chips, buttons, nav items need visible focus ring |
| Min touch target | 44×44px | Sidebar nav items, chip filters, buttons |
| Font size minimum | 10px | Chart axis labels are at 10px — acceptable for desktop only |

**Note:** This dashboard is designed for desktop (1280px+ width). Do not attempt to reflow it below 1024px — build a separate mobile summary view if needed.

---

## 7. Figma File Organization

### 7.1 Page Structure
```
Pages:
├── 🎨 Design System
│   ├── Color Tokens
│   ├── Typography
│   ├── Spacing
│   └── Icons
├── 🧩 Components
│   ├── KPI Card
│   ├── Chart Card
│   ├── Insight Card
│   ├── Data Table
│   ├── Navigation
│   ├── Alert Banner
│   └── Chips / Buttons
├── 📐 Layouts
│   └── Dashboard – Full View (1440px)
└── 📝 Annotations
    └── Redlines / Handoff specs
```

### 7.2 Layer Naming Convention
- Use `PascalCase` for component names: `KpiCard`, `ChartCard/Market`
- Use `kebab-case` for sub-layers: `kpi-label`, `kpi-value`, `accent-bar`
- Group related layers: `[Sidebar]`, `[Main Content]`, `[KPI Row]`
- Prefix hidden/helper layers with `_`: `_grid`, `_overlay`

### 7.3 Component Variants
Build KPI Card as a single component with variants:

| Property | Values |
|----------|--------|
| `Status` | `default`, `positive`, `negative`, `warning` |
| `Size` | `default`, `compact` |

Use **boolean properties** for optional elements (e.g., show/hide badge, show/hide description).

---

## 8. KPI Selection Justification (for this dashboard)

The following KPIs were selected based on what a middle manager needs to act on:

| KPI | Why it matters | Actionability |
|-----|----------------|---------------|
| **Total Market Size ($37B)** | Sets the scale of the market the manager operates in | Strategic context |
| **Object Storage Growth (+10%)** | Fastest growing segment — budget reallocation signal | Invest / shift resources |
| **Public Cloud Share (41%)** | Indicates structural shift toward cloud — renegotiation signal | Renegotiate contracts |
| **Capacity Gap (4 pts)** | Data growing 23% vs 19% storage capacity — risk metric | Procure now, don't wait |

**Proposed additional KPIs** (justified):
- **Cost per ZB stored** — As data volume grows 23% CAGR, cost management per unit becomes critical for a manager's operational budget.
- **Cloud vs On-Prem cost delta** — With public cloud at 41% of IT spend, a manager needs to track whether cloud migration is actually saving money.
- **Storage utilization rate** — % of installed capacity in use. If near 80–85%, procurement must begin immediately to avoid outages.

---

## 9. Common Dashboard Design Mistakes to Avoid

1. **Information overload** — More than 6–8 KPIs on a single view causes decision paralysis. This design shows 4 KPIs + 3 insights = clean.
2. **Colorful charts without semantic meaning** — Every color should mean something. Decorative gradients are noise.
3. **Missing the "so what"** — Every chart needs a subtitle that states the implication, not just the data description.
4. **Tiny interactive targets** — Buttons and filters on a management dashboard may be used in meeting rooms on large screens. Keep tap targets generous (min 36px height).
5. **No visual hierarchy** — If everything looks the same size and weight, nothing is important. Use size, weight, and color consistently to rank importance.
6. **Alert overuse** — If there are 5 red alerts, none of them feel urgent. Reserve critical-red for genuine action-required items only.
7. **Chart borders** — Charts don't need borders around the canvas. The card border provides enough containment.
8. **Inconsistent spacing** — Non-8pt spacing values create a "hand-assembled" feel. Enforce the grid strictly in Figma using layout grids and spacing tokens.

---

*Document prepared for Figma design review — Storage Manager Dashboard project. Based on BCG enterprise storage market data (2021 BCG analysis).*
