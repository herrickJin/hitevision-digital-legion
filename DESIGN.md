---
name: "Warm Cream — Digital Legion OS"
colors:
  # Foundation
  surface: "#f7f4ed"
  surface-dim: "#eceae4"
  surface-bright: "#fcfbf8"
  # Text hierarchy (all derived from charcoal #1c1c1c via opacity)
  on-surface: "#1c1c1c"
  on-surface-variant: "#5f5f5d"
  on-surface-muted: "rgba(28,28,28,0.82)"
  # Borders
  outline: "#eceae4"
  outline-variant: "rgba(28,28,28,0.4)"
  # Interactive
  primary: "#1c1c1c"
  on-primary: "#fcfbf8"
  secondary: "#f7f4ed"
  on-secondary: "#1c1c1c"
  # Status (warm-muted to maintain palette coherence)
  tertiary: "#c4a882"
  error: "#c45b5b"
  success: "#6b9a6b"
  warning: "#c4a04e"
  # Focus
  focus-ring: "rgba(59,130,246,0.5)"
  focus-shadow: "rgba(0,0,0,0.1) 0px 4px 12px"
typography:
  font-family: "'Camera Plain Variable', 'Inter', ui-sans-serif, system-ui, sans-serif"
  display:
    font-size: "60px"
    font-weight: "600"
    line-height: "1.10"
    letter-spacing: "-1.5px"
  headline:
    font-size: "48px"
    font-weight: "600"
    line-height: "1.00"
    letter-spacing: "-1.2px"
  title:
    font-size: "20px"
    font-weight: "600"
    line-height: "1.25"
    letter-spacing: "normal"
  body:
    font-size: "16px"
    font-weight: "400"
    line-height: "1.50"
    letter-spacing: "normal"
  label:
    font-size: "14px"
    font-weight: "400"
    line-height: "1.50"
    letter-spacing: "normal"
  caption:
    font-size: "12px"
    font-weight: "400"
    line-height: "1.50"
    letter-spacing: "normal"
rounded:
  sm: "4px"
  DEFAULT: "6px"
  md: "8px"
  lg: "12px"
  xl: "16px"
  full: "9999px"
spacing:
  xs: "4px"
  sm: "8px"
  DEFAULT: "16px"
  md: "24px"
  lg: "32px"
  xl: "48px"
  2xl: "64px"
  3xl: "96px"
components:
  button-primary:
    background: "{colors.primary}"
    color: "{colors.on-primary}"
    padding: "8px 16px"
    border-radius: "{rounded.DEFAULT}"
    border: "none"
    box-shadow: "rgba(255,255,255,0.2) 0px 0.5px 0px 0px inset, rgba(0,0,0,0.2) 0px 0px 0px 0.5px inset, rgba(0,0,0,0.05) 0px 1px 2px 0px"
    active-opacity: "0.8"
    focus-shadow: "{colors.focus-shadow}"
  button-outline:
    background: "transparent"
    color: "{colors.on-surface}"
    padding: "8px 16px"
    border-radius: "{rounded.DEFAULT}"
    border: "1px solid {colors.outline-variant}"
    focus-shadow: "{colors.focus-shadow}"
  button-ghost:
    background: "{colors.surface}"
    color: "{colors.on-surface}"
    padding: "8px 16px"
    border-radius: "{rounded.DEFAULT}"
    border: "none"
  button-pill:
    background: "{colors.surface}"
    color: "{colors.on-surface}"
    border-radius: "{rounded.full}"
    border: "none"
    default-opacity: "0.5"
    active-opacity: "0.8"
  card:
    background: "{colors.surface}"
    border: "1px solid {colors.outline}"
    border-radius: "{rounded.lg}"
    box-shadow: "none"
  card-featured:
    background: "{colors.surface}"
    border: "1px solid {colors.outline}"
    border-radius: "{rounded.xl}"
    box-shadow: "none"
  input:
    background: "{colors.surface}"
    color: "{colors.on-surface}"
    border: "1px solid {colors.outline}"
    border-radius: "{rounded.DEFAULT}"
    focus-ring: "{colors.focus-ring}"
    placeholder: "{colors.on-surface-variant}"
  sidebar:
    background: "{colors.surface}"
    border-right: "1px solid {colors.outline}"
    width: "260px"
    item-padding: "10px 16px"
    item-radius: "{rounded.md}"
    active-background: "rgba(28,28,28,0.04)"
  badge:
    padding: "2px 10px"
    border-radius: "{rounded.full}"
    font-size: "{typography.caption.font-size}"
    font-weight: "500"
  status-dot:
    width: "8px"
    height: "8px"
    border-radius: "50%"
  issue-card:
    background: "{colors.surface}"
    border: "1px solid {colors.outline}"
    border-radius: "{rounded.md}"
    padding: "{spacing.DEFAULT}"
  avatar:
    width: "32px"
    height: "32px"
    border-radius: "50%"
    border: "1px solid {colors.outline}"
---

# Warm Cream — Digital Legion OS Design System

A warm, humanist design system inspired by Lovable's parchment-toned aesthetic. Every surface radiates warmth through a deliberate cream palette — not cold white, not beige, but a hand-selected parchment that feels like a well-crafted notebook.

## Brand & Style

Digital Legion OS is an enterprise AI agent orchestration platform. The design language balances **professional gravitas** with **approachable warmth** — this is a tool people use all day, so it must feel like a comfortable workspace, not a clinical dashboard.

**Core personality:** Calm competence, editorial precision, quiet confidence.

## Colors

### The Cream Foundation

The entire system sits on `#f7f4ed` — a warm parchment background. This is the single most important design decision. Never substitute with pure white (`#ffffff`). The cream creates an analog warmth that immediately differentiates the platform from typical SaaS tools.

### Opacity-Driven Gray System

All grays derive from `#1c1c1c` at varying opacity. This is not a traditional gray scale — it's a **unified tonal range**:

| Token | Value | Usage |
|-------|-------|-------|
| `on-surface` | `#1c1c1c` | Headings, primary text, dark surfaces |
| `on-surface-muted` | `rgba(28,28,28,0.82)` | Body copy |
| `on-surface-variant` | `#5f5f5d` | Secondary text, descriptions, captions |
| Subtle hover | `rgba(28,28,28,0.04)` | Hover backgrounds, micro-tints |
| Barely-visible | `rgba(28,28,28,0.03)` | Background depth overlays |

### Border Dual System

- **Passive borders** (`#eceae4`): Card edges, dividers, image outlines — warm, quiet containment
- **Interactive borders** (`rgba(28,28,28,0.4)`): Button outlines, active states — stronger signal

### Status Colors

Keep status colors muted and warm to maintain the neutral palette's coherence. No saturated greens or reds — use `#6b9a6b` for success, `#c45b5b` for error, `#c4a04e` for warning.

## Typography

### Font: Camera Plain Variable (Inter fallback)

Camera Plain Variable is a humanist typeface with slightly rounded terminals and organic curves. If unavailable, Inter provides a clean fallback. The system uses only two weights:

- **400** — Body, UI, links, buttons, captions
- **600** — Headings, emphasis, section titles

Never use weight 700 (bold). 600 is the maximum.

### Headline Compression

Headlines use negative letter-spacing for editorial impact, scaled with size:

| Size | Letter-spacing |
|------|---------------|
| 60px (display) | -1.5px |
| 48px (headline) | -1.2px |
| 36px (sub-headline) | -0.9px |
| 20px and below | normal |

## Layout & Spacing

### Sidebar Navigation

Left sidebar (260px wide) with warm cream background and subtle right border. Navigation items use `10px 16px` padding with `8px` border-radius. Active items get a barely-visible charcoal overlay (`rgba(28,28,28,0.04)`). No background fills, no bold text — the active state is whisper-quiet.

**Navigation sections:**
- Dashboard (home)
- Definition Center (数字员工定义)
- Instance Center (实例管理)
- Role Workbench (角色工作台)
- Issue Collaboration (Issue协作)
- Wiki & Memory (知识治理)
- Settings

### Content Area

Max content width ~1200px. Cards and modules sit on the cream surface with warm borders for containment — never drop-shadows. Section spacing uses 32px–48px gaps for breathing room.

### Page Layout Patterns

**Dashboard:** Grid of status cards (project count, active issues, agent instances) + recent activity feed. Cards use 12px radius, `1px solid #eceae4` border.

**Workbench:** Split layout — left panel with issue queue (list of issue cards, 8px radius), right panel with active workspace. Issues show status dot + title + priority badge.

**Definition Center:** Gallery layout showing digital employee "Soul" prototypes as cards. Each card: avatar + name + status badge + capability tags.

**Issue Detail:** Full-width content area with header (issue title + status + assignee), description, timeline/activity feed, and action panel.

## Elevation & Depth

The depth system is intentionally **flat**. There are no floating cards with dramatic shadows.

| Level | Treatment | Usage |
|-------|-----------|-------|
| Flat | No shadow, cream surface | Page background, most content |
| Bordered | `1px solid #eceae4` | Cards, images, dividers, containers |
| Inset | Multi-layer inset shadow on dark buttons | Primary CTAs — tactile pressed feel |
| Focus | `rgba(0,0,0,0.1) 0px 4px 12px` | Active/focus states — soft warm glow |
| Ring | `rgba(59,130,246,0.5)` 2px ring | Keyboard focus on inputs (accessibility) |

**Key principle:** Use borders for containment, not shadows. The only shadow pattern is the inset shadow on dark buttons — a signature detail creating a tactile, pressed-into-surface feeling.

## Shapes

### Border Radius Scale

| Token | Value | Usage |
|-------|-------|-------|
| `sm` | 4px | Small buttons, micro elements |
| `DEFAULT` | 6px | Buttons, inputs, navigation items |
| `md` | 8px | Compact cards, divs, issue cards |
| `lg` | 12px | Standard cards, image containers |
| `xl` | 16px | Large containers, featured sections |
| `full` | 9999px | Pills, badges, icon buttons, avatars, toggles |

Never apply `full` (9999px) radius on rectangular buttons — pills are exclusively for icon buttons, action toggles, badges, and status indicators.

## Components

### Buttons

**Primary (Dark with Inset Shadow)**
The signature button. Charcoal background (#1c1c1c) with multi-layer inset shadow creating a tactile depth. Off-white text (#fcfbf8). 6px radius. Use for primary CTAs like "Create Issue", "Deploy Instance", "Start Agent".

**Outline**
Transparent background with charcoal-40% border. For secondary actions like "Cancel", "Back", "View Details".

**Ghost**
Cream surface background, no border. For tertiary actions, toolbar buttons, inline operations.

**Pill**
Full 9999px radius. Used exclusively for icon buttons, action toggles, status badges, filter chips. Never for text-label buttons.

### Cards

Cards are defined by warm borders, never shadows. Background matches the page cream (#f7f4ed) for seamless integration. Content is contained by the `1px solid #eceae4` edge.

- **Standard card**: 12px radius, used for dashboard modules, definition prototypes
- **Issue card**: 8px radius, compact layout for issue queue items
- **Featured card**: 16px radius, for hero content or featured items

### Status Indicators

Digital Legion OS uses a status machine heavily. Status indicators combine:
- **Status dot** (8px circle) with color-coded fill
- **Badge** (pill shape, 9999px radius) with status label

Status colors (warm-muted):
- Active/Running: `#6b9a6b`
- Pending/Queued: `#c4a04e`
- Error/Failed: `#c45b5b`
- Idle/Completed: `#5f5f5d`

### Sidebar

Fixed left sidebar with cream background. Navigation items are text-only (no icons required but icon+text is acceptable). Active item indicated by subtle background tint, not bold or color change. Sections separated by thin `#eceae4` dividers.

### AI Agent / Chat Interface

When displaying AI agent interactions, use a conversational bubble layout:
- Agent messages: cream background card with `#eceae4` border, left-aligned
- User/system messages: charcoal-3% background, right-aligned
- Input area: full-width with warm border, suggestion pills below

### Data Tables

For listing instances, issues, or definitions:
- Header row: charcoal-4% background, 14px weight 600 labels
- Body rows: cream background, 16px weight 400 text
- Row borders: bottom-only `1px solid #eceae4`
- Row hover: charcoal-3% overlay
- No zebra striping — the cream surface provides enough warmth

## Responsive Behavior

### Breakpoints

| Breakpoint | Width | Layout |
|-----------|-------|--------|
| Mobile | <768px | Sidebar collapses to hamburger, single-column content |
| Tablet | 768–1024px | Sidebar auto-hides, 2-column grids |
| Desktop | 1024–1280px | Full sidebar + multi-column layout |
| Large | >1280px | Maximum content width with generous margins |

### Touch Targets

All interactive elements maintain minimum 44px touch targets. Button padding of `8px 16px` provides comfortable touch areas. Pill-shaped elements naturally create large tap targets.

## Do's and Don'ts

### Do
- Use cream (#f7f4ed) as the universal surface — it's the brand signature
- Derive all grays from #1c1c1c at opacity levels for tonal unity
- Use #eceae4 borders for card containment instead of shadows
- Apply the inset shadow on dark buttons — it's the signature micro-detail
- Use negative letter-spacing on headlines, scaled with size
- Keep status colors muted and warm to maintain palette coherence
- Use full-pill radius only for badges, toggles, and icon buttons
- Apply opacity 0.8 on active/pressed states for tactile feedback

### Don't
- Don't use pure white (#ffffff) as background — ever
- Don't use drop-shadows on cards — borders are the containment mechanism
- Don't introduce saturated accent colors — the palette is warm-neutral
- Don't use font-weight 700 — 600 is the system maximum
- Don't apply 9999px radius on rectangular buttons
- Don't use sharp focus outlines — use soft shadow-based focus
- Don't mix border styles — #eceae4 for passive, rgba(28,28,28,0.4) for interactive
- Don't increase letter-spacing on headlines — they should run tight
