# Auto-Layout — The Foundation of Every Responsive Design

> If your design doesn't use auto-layout, it's not a design — it's a screenshot.
> Auto-layout is the PRIMARY tool for building responsive, maintainable, and scalable UI in Figma.

**Sources**: [Figma Help Center — Auto Layout Fundamentals](https://help.figma.com/hc/en-us/articles/31351261703063), [Figma Developer Docs](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/), [Figma Best Practices](https://www.figma.com/best-practices/)

---

## ABSOLUTE RULE

> **EVERYTHING uses auto-layout.**
> No manually positioned elements except overlays, decorative elements, and badges.
> If you can use auto-layout, ALWAYS use auto-layout.

---

## THE THREE SIZING MODES

| Mode | Behavior | Mental Model |
|---|---|---|
| **FIXED** | Element has an explicit pixel size that does NOT change | "I am exactly this size, always" |
| **HUG** | Element shrinks to exactly fit its content | "I am as small as my content allows" |
| **FILL** | Element expands to fill all available space in the parent | "I am as large as my parent allows" |

### The Critical FILL Rule:
> `FILL` only works on **direct children** of auto-layout frames.
> NEVER try to set FILL before placing the element inside the auto-layout parent.
> Order of operations: create element → add to parent → THEN set FILL.

---

## SIZING DECISION TABLE

| Component | Horizontal | Vertical | Why |
|---|---|---|---|
| **Screen frame** | FIXED (393px) | FIXED (852px) | The viewport doesn't change |
| **Header** | FILL | FIXED (56px) | Stretches width, fixed height |
| **Content area** | FILL | FILL | Takes ALL remaining space |
| **Bottom nav** | FILL | FIXED (56-83px) | Stretches width, fixed height |
| **Bottom CTA bar** | FILL | HUG | Stretches width, wraps content |
| **Section** | FILL | HUG | Stretches width, grows with content |
| **Card** | FILL | HUG | Stretches width, grows with content |
| **Card in grid (2-col)** | FILL (50%) | HUG | Each fills half the row |
| **Full-width button** | FILL | FIXED (48-56px) | Stretches width, fixed height |
| **Inline button** | HUG | FIXED (44-48px) | Wraps text, fixed height |
| **Text input** | FILL | FIXED (48-56px) | Stretches width, fixed height |
| **Text label** | FILL | HUG | Wraps within parent, grows with lines |
| **Single-line text** | HUG or FILL | HUG | Depends on context |
| **Icon** | FIXED (24px) | FIXED (24px) | Never changes size |
| **Avatar** | FIXED (40-48px) | FIXED (40-48px) | Never changes size |
| **Circular button** | FIXED | FIXED | MUST be fixed on both axes |
| **Badge** | HUG (min 20px) | FIXED (20px) | Grows with number, fixed height |
| **Chip/Tag** | HUG | FIXED (32px) | Grows with text, fixed height |
| **Divider** | FILL | FIXED (1px) | Stretches width, always 1px |
| **Image/Thumbnail** | FILL | FIXED | Stretches width, maintains ratio |
| **Toast** | FILL | HUG | Stretches width, wraps content |

---

## AUTO-LAYOUT DIRECTION

| Direction | When to Use | Examples |
|---|---|---|
| **Vertical** | Content stacks top-to-bottom | Screens, sections, cards, lists, forms |
| **Horizontal** | Content sits side-by-side | Headers, nav bars, button rows, tag lists |
| **Wrap** | Items flow and wrap to next line | Tag clouds, filter chips, grid layouts |

### Rules:
- **Screen frames**: Always VERTICAL
- **Headers/Nav bars**: Always HORIZONTAL
- **Cards**: Usually VERTICAL (image on top, content below)
- **List items**: HORIZONTAL (icon + text + action)
- **Chip/tag groups**: HORIZONTAL with WRAP

---

## SPACING SYSTEM

### The 8px Grid:

All spacing values should be multiples of 4 or 8:

| Token | Value | Typical Use |
|---|---|---|
| xxs | 2px | Fine adjustment (border → icon) |
| xs | 4px | Label → Input, icon → inline text |
| sm | 8px | Between items in the same group |
| md | 12px | Internal padding of chip/badge |
| base | 16px | Button padding, gap between inputs |
| lg | 24px | Card padding, screen side margins |
| xl | 32px | Between screen sections |
| 2xl | 40px | Strong visual separation |
| 3xl | 48px | Between large content blocks |
| 4xl | 64px | Hero space, screen top padding |

### Golden Rule:
> Space BETWEEN groups ≥ 2× space WITHIN a group.
> This creates visual hierarchy through proximity (Gestalt principle).

### Padding vs Gap:

| Property | What It Does | Figma Setting |
|---|---|---|
| **Padding** | Space between the frame edge and its children | `paddingTop`, `paddingRight`, `paddingBottom`, `paddingLeft` |
| **Gap (Item Spacing)** | Space between sibling children | `itemSpacing` |
| **Counter-axis Spacing** | Space between wrapped rows/columns | `counterAxisSpacing` (only with Wrap) |

```
┌──────────────────────────────┐
│  ← paddingLeft              paddingRight →  │
│  ↑ paddingTop                             │
│                                          │
│    ┌───────┐  ← gap →  ┌───────┐     │
│    │ Item1 │           │ Item2 │     │
│    └───────┘           └───────┘     │
│                                          │
│  ↓ paddingBottom                          │
└──────────────────────────────┘
```

---

## ALIGNMENT

### Primary Axis (direction of flow):

| Value | Behavior |
|---|---|
| `MIN` | Children align to start (top or left) |
| `CENTER` | Children center along axis |
| `MAX` | Children align to end (bottom or right) |
| `SPACE_BETWEEN` | Children spread out with equal space between |

### Counter Axis (perpendicular to flow):

| Value | Behavior |
|---|---|
| `MIN` | Children align to start |
| `CENTER` | Children center |
| `MAX` | Children align to end |
| `BASELINE` | Text layers align by text baseline |

### Common Patterns:

| Layout | Primary Axis | Counter Axis |
|---|---|---|
| Header (back + title + action) | `SPACE_BETWEEN` | `CENTER` |
| Card content | `MIN` (top) | `MIN` (left) |
| Centered modal content | `CENTER` | `CENTER` |
| Bottom nav items | `SPACE_BETWEEN` | `CENTER` |
| Form fields stacked | `MIN` (top) | `MIN` (left) |
| CTA button centered | `CENTER` | `CENTER` |

---

## THE RESPONSIVE SCREEN SKELETON

```
Screen (FIXED 393 × 852)
├── Header (FILL × FIXED 56px)
│   ├── BackButton (FIXED 44 × FIXED 44)
│   ├── Title (FILL × HUG)                  ← Absorbs remaining width
│   └── ActionIcon (FIXED 44 × FIXED 44)
│
├── Content (FILL × FILL)                    ← THE KEY: fills remaining height
│   ├── Section/Balance (FILL × HUG)
│   │   ├── Label (FILL × HUG)
│   │   └── Value (FILL × HUG)
│   │
│   ├── Section/Actions (FILL × HUG)
│   │   ├── ActionButton (FIXED 52 × FIXED 52)
│   │   ├── ActionButton (FIXED 52 × FIXED 52)
│   │   ├── ActionButton (FIXED 52 × FIXED 52)
│   │   └── ActionButton (FIXED 52 × FIXED 52)
│   │
│   └── Section/Transactions (FILL × HUG)
│       ├── TransactionItem (FILL × HUG)
│       └── TransactionItem (FILL × HUG)
│
├── CTABar (FILL × HUG)
│   └── PrimaryButton (FILL × FIXED 48px)
│
└── BottomNav (FILL × FIXED 56px)
    ├── NavItem (FILL × HUG)                ← Equal distribution
    ├── NavItem (FILL × HUG)
    ├── NavItem (FILL × HUG)
    └── NavItem (FILL × HUG)
```

### Why Content is FILL × FILL:
> The Content area must use FILL on the vertical axis so it absorbs all remaining height between the Header and the Bottom Nav/CTA. This is what makes the screen work at different heights (667px, 844px, 852px, 915px).

---

## WRAP (GRID LAYOUTS)

Figma's auto-layout supports `Wrap` for creating responsive grid layouts:

```javascript
const grid = figma.createFrame();
grid.layoutMode = 'HORIZONTAL';
grid.layoutWrap = 'WRAP';
grid.itemSpacing = 16;              // Horizontal gap
grid.counterAxisSpacing = 16;       // Vertical gap between rows

// Each card fills proportional width and wraps when too narrow
card.layoutSizingHorizontal = 'FILL';
card.minWidth = 280;               // Triggers wrap
```

### When to Use Wrap:
- Product grids (2-3 columns)
- Filter chip groups
- Tag clouds
- Photo galleries
- Icon grids

---

## NESTED AUTO-LAYOUT

Complex layouts are built by nesting auto-layout frames:

```
Card (VERTICAL auto-layout, padding 16px, gap 12px)
├── ImageContainer (FILL × FIXED 200px)
├── ContentGroup (VERTICAL, gap 8px)
│   ├── Title (FILL × HUG)
│   └── Description (FILL × HUG)
├── MetaRow (HORIZONTAL, gap 8px, SPACE_BETWEEN)
│   ├── Rating (HUG × HUG)
│   └── Price (HUG × HUG)
└── ActionRow (HORIZONTAL, gap 8px)
    ├── SaveButton (HUG × FIXED 36px)
    └── BuyButton (FILL × FIXED 36px)
```

### Rules for Nesting:
- Each level of nesting should have its own auto-layout direction
- Alternate: VERTICAL parent → HORIZONTAL children → VERTICAL grandchildren
- Keep nesting depth reasonable (max 5-6 levels)
- Name every level clearly

---

## COMMON MISTAKES

| Mistake | Problem | Fix |
|---|---|---|
| No auto-layout on frames | Elements don't reflow when content changes | Add auto-layout to everything |
| FILL before adding to parent | Error or silently ignored | Add to parent FIRST, then set FILL |
| Screen height set to HUG | Screen grows to 2000px, no scroll | Set screen height to FIXED |
| All children FIXED width | Nothing adapts when parent resizes | Use FILL for stretchy elements |
| Inconsistent spacing | Design looks amateur | Use the 8px spacing scale |
| Manual positioning inside AL | Breaks auto-layout flow | Remove absolute positioning |
| Forgetting gap between sections | Sections are visually cramped | Set itemSpacing on parent |
| Nav items all HUG width | Unequal widths, off-center | Set all nav items to FILL |

---

## DECISION TREE

```
Is the element a screen/viewport?
├── YES → FIXED width × FIXED height
│
└── NO → Should it stretch to fill its parent?
    ├── YES → Does it need to grow with content?
    │   ├── YES (height grows) → FILL width × HUG height
    │   └── NO (fixed height) → FILL width × FIXED height
    │
    └── NO → Is it a fixed-size element?
        ├── YES (icon, avatar, circle) → FIXED × FIXED
        │
        └── NO → Should it wrap its content?
            ├── YES (inline button, chip) → HUG width × FIXED height
            │
            └── SPECIAL: absorb remaining space?
                └── YES (Content area) → FILL × FILL
```

### Quick Cheat Sheet:
```
Screen        →  FIXED × FIXED   (the viewport)
Header        →  FILL  × FIXED   (stretch width, fixed height)
Content       →  FILL  × FILL    (stretch both, absorb remaining)
Footer/Nav    →  FILL  × FIXED   (stretch width, fixed height)
Card          →  FILL  × HUG     (stretch width, grow with content)
Button (CTA)  →  FILL  × FIXED   (stretch width, fixed height)
Button (inline)→ HUG   × FIXED   (wrap text, fixed height)
Icon          →  FIXED × FIXED   (never changes)
Text          →  FILL  × HUG     (stretch width, grow with lines)
Divider       →  FILL  × FIXED   (stretch width, 1px height)
```

---

## AUTO-LAYOUT CHECKLIST

- [ ] Does every frame use auto-layout?
- [ ] Is the screen frame FIXED width × FIXED height?
- [ ] Is the content area FILL × FILL?
- [ ] Are spacing values from the 8px scale?
- [ ] Is space between groups ≥ 2× space within groups?
- [ ] Is FILL only set AFTER elements are added to parent?
- [ ] Are nav items using FILL for equal distribution?
- [ ] Are icons and avatars FIXED × FIXED?
- [ ] Is nesting depth reasonable (max 5-6 levels)?
- [ ] Does the layout survive resizing at 320px, 393px, and 428px width?