# Constraints & Responsiveness — Adaptive Screens and Components

> A screen that only works at one exact size is not a screen — it's a picture.
> Every component, section, and screen must adapt gracefully when viewport dimensions change.

**Sources**: [Figma Help Center](https://help.figma.com/hc/en-us/articles/360040451373), [Figma Developer Docs](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/)

---

## TWO SYSTEMS: CONSTRAINTS vs AUTO-LAYOUT

| System | When to Use | How It Works |
|---|---|---|
| **Constraints** | Elements inside a frame WITHOUT auto-layout | Pin edges or scale relative to parent |
| **Auto-Layout Sizing** | Elements inside an auto-layout frame | FILL, HUG, or FIXED behavior |

> **Auto-layout is the PRIMARY tool for responsiveness.**
> Constraints are for overlays, absolute-positioned elements, or legacy frames.

---

## CONSTRAINTS (Pin & Scale)

### Horizontal Constraints:

| Constraint | Behavior | Use Case |
|---|---|---|
| **Left** | Maintains distance from left edge | Default. Text, content aligned left |
| **Right** | Maintains distance from right edge | Action buttons, trailing icons |
| **Left & Right** | Stretches — maintains distance from BOTH edges | Full-width elements (search bars, dividers) |
| **Center** | Stays centered horizontally | Centered modals, centered icons |
| **Scale** | Scales proportionally with parent width | Images, decorative elements |

### Vertical Constraints:

| Constraint | Behavior | Use Case |
|---|---|---|
| **Top** | Maintains distance from top edge | Default. Headers, top content |
| **Bottom** | Maintains distance from bottom edge | Bottom nav, FAB, sticky CTA |
| **Top & Bottom** | Stretches — maintains distance from BOTH edges | Scrollable content area |
| **Center** | Stays centered vertically | Centered modals, loading states |
| **Scale** | Scales proportionally with parent height | Decorative backgrounds |

### Constraint Map for Common Elements:

| Element | Horizontal | Vertical | Why |
|---|---|---|---|
| Status bar | Left & Right | Top | Full width, pinned top |
| Header/App bar | Left & Right | Top | Full width, pinned top |
| Bottom navigation | Left & Right | Bottom | Full width, pinned bottom |
| Bottom CTA bar | Left & Right | Bottom | Full width, pinned bottom |
| Content area | Left & Right | Top & Bottom | Fills remaining space |
| FAB | Right | Bottom | Fixed size, bottom-right |
| Back button | Left | Top | Pinned top-left |
| Action icons (header) | Right | Top | Pinned top-right |
| Centered title | Center | Top | Centered, pinned top |
| Modal overlay | Left & Right | Top & Bottom | Covers entire parent |
| Modal card | Center | Center | Stays centered |
| Toast/Snackbar | Left & Right | Bottom | Full width, pinned bottom |
| Background image | Scale | Scale | Scales proportionally |

---

## MIN & MAX DIMENSIONS

Figma supports minimum and maximum width/height on frames to prevent elements from becoming too small or too large.

| Property | Use Case | Example |
|---|---|---|
| **minWidth** | Prevent buttons/cards from collapsing | Button min 120px |
| **maxWidth** | Prevent text from stretching too wide | Text max 600px |
| **minHeight** | Prevent sections from disappearing | Card min 80px |
| **maxHeight** | Limit expandable areas | Dropdown max 320px |

### Common Values:

| Component | minWidth | maxWidth | minHeight | maxHeight |
|---|---|---|---|---|
| Button (inline) | 80px | — | — | — |
| Button (full-width) | — | — | 44px | — |
| Input field | — | — | 44px | — |
| Card | — | 480px | 72px | — |
| Modal | 280px | 360px | — | 600px |
| Bottom sheet | — | — | 200px | 85% screen |
| Text paragraph | — | 600px | — | — |
| Dropdown list | — | — | — | 320px |
| Badge | 20px | — | 20px | 20px |
| Chip/Tag | 48px | 200px | 32px | 32px |

---

## RESPONSIVE COMPONENT PATTERNS

### Pattern 1: Stretch & Squish (Most Common)

Icon stays FIXED, text FILLS remaining space:

```
At 393px: [Icon 40px] [Title and Description............]
At 320px: [Icon 40px] [Title and Desc...]
At 428px: [Icon 40px] [Title and Full Description Here..]
```

### Pattern 2: Stack & Reflow (Grid)

Cards wrap to new rows on narrower screens:

```
At mobile (393px):        At tablet (768px):
┌───────────────┐      ┌───────┐┌───────┐
│   Card A      │      │ Card A││ Card B│
├───────────────┤      └───────┘└───────┘
│   Card B      │      ┌───────┐
├───────────────┤      │ Card C│
│   Card C      │      └───────┘
└───────────────┘
```

Use auto-layout with Wrap + minWidth on cards.

### Pattern 3: Fixed Anchors + Flexible Middle

```
┌───────────────────────────────────┐
│  [←]    Screen Title        [⚙️]   │
│  FIXED   FILL              FIXED   │
└───────────────────────────────────┘
```

### Pattern 4: Equal Distribution

All nav items set to FILL for equal width:

```
┌───────────────────────────────────┐
│  Home   Search   Cart   Profile     │
│  FILL    FILL    FILL    FILL       │
│  (25%)  (25%)   (25%)   (25%)      │
└───────────────────────────────────┘
```

---

## SCROLL BEHAVIOR

### Scroll Zones:

```
┌───────────────────────────────────┐
│  Header (FIXED, NO scroll)       │  ← Always visible
├───────────────────────────────────┤
│                                   │
│  Content (FILL, SCROLL vertical) │  ← Scrolls
│                                   │
│  ┌───────────────────────────┐    │
│  │ Carousel (SCROLL horizontal) │    │  ← Nested scroll
│  └───────────────────────────┘    │
│                                   │
├───────────────────────────────────┤
│  Bottom CTA (FIXED, NO scroll)   │  ← Always visible
├───────────────────────────────────┤
│  Bottom Nav (FIXED, NO scroll)   │  ← Always visible
└───────────────────────────────────┘
```

### Rules:
- Fixed elements (header, bottom nav, CTA bar) should NEVER scroll
- Only the Content area scrolls
- Set `clipsContent = true` on scrollable frames
- Screen frame must be FIXED height to force scroll behavior

---

## ABSOLUTE POSITION WITHIN AUTO-LAYOUT

For elements that need to float above auto-layout flow (badges, indicators, overlapping decorations):

- Set `layoutPositioning = 'ABSOLUTE'` on the child
- Then use constraints to position it
- The element opts out of auto-layout flow

### When to Use:
- Badge on an avatar or icon
- Online status indicator dot
- Notification count on nav item
- Floating elements that overlap content

### Rule:
> Use absolute positioning SPARINGLY — only for overlapping decorations. Main content should ALWAYS use auto-layout flow.

---

## RESPONSIVE TESTING PROTOCOL

### Step 1: Resize Width
Resize frame from 393px → 375px → 320px
- Check: text cut off? elements overlap? cards collapse?

### Step 2: Resize Height
Resize frame from 852px → 667px → 915px
- Check: Content area shrinks/grows? header/footer visible?

### Step 3: Content Stress Test
- Add very long text to titles/descriptions
- Add very short text (1-2 chars)
- Check: elements handle both extremes?

### Step 4: Device Frame Test

| Device | Width | Height | Check |
|---|---|---|---|
| iPhone SE | 375px | 667px | Smallest common |
| iPhone 14 | 390px | 844px | Common |
| iPhone 15 Pro Max | 393px | 852px | Design target |
| Samsung Galaxy S24 | 360px | 780px | Narrow Android |
| Pixel 8 | 412px | 915px | Tall Android |

---

## COMMON MISTAKES

| Mistake | Problem | Fix |
|---|---|---|
| Screen height HUG | Screen grows endlessly, no scroll | Set to FIXED |
| Content area FIXED height | Doesn't adapt to screen size | Set to FILL |
| All children FIXED width | Nothing adapts when parent resizes | Use FILL |
| Bottom nav inside scroll | Scrolls away! | Place as direct screen child |
| No overflow on content | Content beyond viewport invisible | Set overflowDirection VERTICAL |
| FILL on non-auto-layout child | Error or silently ignored | Enable auto-layout on parent first |
| Section with FIXED height | Doesn't grow with content | Set to HUG |
| Nav items HUG width | Unequal, off-center | Set all to FILL |

---

## SAFE AREAS

### iOS:
- **Status bar**: 47px (Dynamic Island) / 44px (Notch) / 20px (no notch)
- **Home indicator**: 34px at the bottom
- **Navigation bar**: 44px height

### Android:
- **Status bar**: 24dp
- **Navigation bar**: 48dp (gesture/3-button)

### Rule:
> NEVER place interactive elements in the safe area. Content can scroll behind the status bar, but buttons CANNOT.

---

## RESPONSIVENESS CHECKLIST

### Screen-Level:
- [ ] Screen frame is FIXED width × FIXED height?
- [ ] Header is FILL width × FIXED height?
- [ ] Content area is FILL width × FILL height?
- [ ] Bottom nav/CTA is FILL width × FIXED height?
- [ ] Content area has overflow scroll?
- [ ] Screen works at 852px AND 667px height?

### Component-Level:
- [ ] Icons and avatars are FIXED × FIXED?
- [ ] Cards are FILL width × HUG height?
- [ ] Full-width buttons are FILL width × FIXED height?
- [ ] Text elements are FILL width × HUG height?
- [ ] Min/max dimensions set where needed?

### Stress Tests:
- [ ] Resized to 320px width — nothing breaks?
- [ ] Resized to 428px width — nothing looks empty?
- [ ] Long text — truncates or wraps correctly?
- [ ] Short text — doesn't collapse?