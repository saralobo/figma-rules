# Variables & Design Tokens — The Semantic Bridge Between Design and Code

> Hard-coded hex colors and magic numbers are the #1 enemy of scalable design.
> Variables and tokens create a shared language between designers and developers.

**Sources**: [Figma Help Center — Variables](https://help.figma.com/hc/en-us/articles/15339657135383), [W3C Design Tokens Standard](https://design-tokens.github.io/community-group/format/), [Figma Dev Docs](https://developers.figma.com/docs/figma-mcp-server/)

---

## WHAT ARE VARIABLES?

Figma Variables store reusable values that can be applied across a design file:

| Variable Type | Stores | Example Use |
|---|---|---|
| **Color** | RGBA values | Background, text, border colors |
| **Number** | Numeric values | Spacing, corner radius, opacity |
| **String** | Text values | Placeholder text, labels |
| **Boolean** | True/False | Visibility toggles |

---

## THREE-TIER TOKEN ARCHITECTURE

### Tier 1: Primitives (Raw Values)

The most basic palette. Pure values with no semantic meaning:

```
Primitives/
├── blue/50    = #EBF5FF
├── blue/100   = #CCE5FF
├── blue/200   = #99CAFF
├── blue/500   = #0066FF
├── blue/900   = #001A40
├── gray/50    = #FAFAFA
├── gray/100   = #F5F5F5
├── gray/900   = #212121
├── green/500  = #00A650
├── red/500    = #F23D4F
├── yellow/500 = #FFF159
└── ...
```

### Tier 2: Semantic Tokens (Purpose)

Map primitives to their meaning in the UI:

```
Semantic/
├── color/
│   ├── background/primary     = {Primitives/white}        → #FFFFFF
│   ├── background/secondary   = {Primitives/gray/50}      → #FAFAFA
│   ├── background/brand       = {Primitives/blue/500}     → #0066FF
│   ├── background/success     = {Primitives/green/50}     → #E8F5E9
│   ├── background/error       = {Primitives/red/50}       → #FFEBEE
│   ├── text/primary           = {Primitives/gray/900}     → #212121
│   ├── text/secondary         = {Primitives/gray/600}     → #757575
│   ├── text/disabled          = {Primitives/gray/400}     → #BDBDBD
│   ├── text/onBrand           = {Primitives/white}        → #FFFFFF
│   ├── text/link              = {Primitives/blue/500}     → #0066FF
│   ├── text/success           = {Primitives/green/700}    → #00873D
│   ├── text/error             = {Primitives/red/600}      → #D32F2F
│   ├── border/default         = {Primitives/gray/200}     → #EEEEEE
│   ├── border/strong          = {Primitives/gray/400}     → #BDBDBD
│   ├── border/brand           = {Primitives/blue/500}     → #0066FF
│   ├── border/error           = {Primitives/red/500}      → #F23D4F
│   ├── surface/card           = {Primitives/white}        → #FFFFFF
│   └── surface/overlay        = {Primitives/black} @ 50%  → rgba(0,0,0,0.5)
│
├── spacing/
│   ├── xxs    = 2
│   ├── xs     = 4
│   ├── sm     = 8
│   ├── md     = 12
│   ├── base   = 16
│   ├── lg     = 24
│   ├── xl     = 32
│   ├── 2xl    = 40
│   ├── 3xl    = 48
│   └── 4xl    = 64
│
├── radius/
│   ├── none   = 0
│   ├── sm     = 4
│   ├── md     = 8
│   ├── lg     = 12
│   ├── xl     = 16
│   ├── 2xl    = 24
│   └── full   = 999
│
└── elevation/
    ├── none   = 0dp
    ├── low    = 2dp   (cards)
    ├── medium = 4dp   (menus, dropdowns)
    ├── high   = 8dp   (modals, dialogs)
    └── top    = 16dp  (navigation drawers)
```

### Tier 3: Component Tokens (Specific)

Map semantic tokens to specific component properties:

```
Component/
├── button/
│   ├── primary/background      = {Semantic/color/background/brand}
│   ├── primary/text            = {Semantic/color/text/onBrand}
│   ├── primary/border          = {Semantic/color/border/brand}
│   ├── secondary/background    = {Semantic/color/background/primary}
│   ├── secondary/text          = {Semantic/color/text/primary}
│   ├── secondary/border        = {Semantic/color/border/default}
│   ├── disabled/background     = {Semantic/color/background/secondary}
│   ├── disabled/text           = {Semantic/color/text/disabled}
│   ├── padding-horizontal      = {Semantic/spacing/base}    → 16
│   ├── padding-vertical        = {Semantic/spacing/sm}      → 8
│   ├── gap                     = {Semantic/spacing/xs}      → 4
│   └── radius                  = {Semantic/radius/md}       → 8
│
├── card/
│   ├── background              = {Semantic/color/surface/card}
│   ├── border                  = {Semantic/color/border/default}
│   ├── padding                 = {Semantic/spacing/base}    → 16
│   ├── gap                     = {Semantic/spacing/sm}      → 8
│   └── radius                  = {Semantic/radius/lg}       → 12
│
└── input/
    ├── background              = {Semantic/color/background/primary}
    ├── border/default          = {Semantic/color/border/default}
    ├── border/focused          = {Semantic/color/border/brand}
    ├── border/error            = {Semantic/color/border/error}
    ├── text                    = {Semantic/color/text/primary}
    ├── placeholder             = {Semantic/color/text/secondary}
    └── radius                  = {Semantic/radius/md}       → 8
```

---

## MODES (THEMING)

Modes allow the SAME variable to have different values in different contexts:

### Light/Dark Mode:

| Token | Light Mode | Dark Mode |
|---|---|---|
| `background/primary` | #FFFFFF | #121212 |
| `background/secondary` | #FAFAFA | #1E1E1E |
| `text/primary` | #212121 | #FFFFFF |
| `text/secondary` | #757575 | #B0B0B0 |
| `border/default` | #EEEEEE | #333333 |
| `surface/card` | #FFFFFF | #1E1E1E |

### Brand Modes:

| Token | Brand A | Brand B |
|---|---|---|
| `background/brand` | #0066FF (Blue) | #00A650 (Green) |
| `text/onBrand` | #FFFFFF | #FFFFFF |

### How to Apply Modes:
1. Create a collection with modes (Light, Dark)
2. Assign variables to the collection
3. Set mode values for each variable
4. Apply modes to frames — all children inherit the mode

---

## COLLECTION ORGANIZATION

### Recommended Collections:

| Collection | Purpose | Modes |
|---|---|---|
| **Primitives** | Raw color palette | None (just values) |
| **Semantic** | Purpose-based tokens | Light, Dark |
| **Spacing** | Spacing scale | None (universal) |
| **Radius** | Corner radius scale | None (universal) |
| **Component** | Component-specific tokens | Light, Dark |

### Naming Inside Collections:
- Use `/` for grouping: `color/background/primary`
- Group by category first: `color/`, `spacing/`, `radius/`
- Then by purpose: `background/`, `text/`, `border/`
- Then by variant: `primary`, `secondary`, `brand`

---

## VARIABLES IN FIGMA API (for AI Agents)

### Reading Variables:
```javascript
const collections = await figma.variables.getLocalVariableCollectionsAsync();
for (const collection of collections) {
  console.log(collection.name, collection.modes);
  for (const id of collection.variableIds) {
    const variable = await figma.variables.getVariableByIdAsync(id);
    console.log(variable.name, variable.resolvedType, variable.valuesByMode);
  }
}
```

### Creating Variables:
```javascript
const collection = figma.variables.createVariableCollection('Semantic');
const lightModeId = collection.modes[0].modeId;
collection.renameMode(lightModeId, 'Light');
const darkModeId = collection.addMode('Dark');

const bgPrimary = figma.variables.createVariable('color/background/primary', collection, 'COLOR');
bgPrimary.setValueForMode(lightModeId, { r: 1, g: 1, b: 1, a: 1 }); // White
bgPrimary.setValueForMode(darkModeId, { r: 0.07, g: 0.07, b: 0.07, a: 1 }); // #121212
```

### Applying Variables:
```javascript
const frame = figma.createFrame();
const bgVar = await figma.variables.getVariableByIdAsync('VariableID:123:456');
frame.setBoundVariable('fills', bgVar.id); // Binds fill to the variable
```

---

## FIGMA STYLES vs VARIABLES

| Feature | Styles | Variables |
|---|---|---|
| Color values | Yes | Yes |
| Numeric values | No | Yes |
| String/Boolean | No | Yes |
| Theming (modes) | No | Yes |
| Aliasing (referencing) | No | Yes |
| Apply to spacing/padding | No | Yes |
| Apply to corner radius | No | Yes |
| Apply to fills/strokes | Yes | Yes |
| Text styles (font, size, weight) | Yes | No |
| Effect styles (shadows, blurs) | Yes | No |

### Migration Strategy:
- **Keep as Styles**: Typography, Effects (shadows, blurs)
- **Move to Variables**: Colors, Spacing, Radius, Opacity
- **Both**: Colors can exist as both (Styles for backward compatibility, Variables for theming)

---

## CONTRAST & ACCESSIBILITY

### Minimum Contrast Ratios (WCAG 2.2):

| Text Type | Minimum Ratio | Example Pairs |
|---|---|---|
| Normal text (<24px) | 4.5:1 | #757575 on #FFFFFF = 4.6:1 |
| Large text (≥24px or ≥18.66px bold) | 3:1 | #999999 on #FFFFFF = 2.85:1 FAIL |
| UI components & icons | 3:1 | Border, icons, focus indicators |
| Decorative | No requirement | Gradients, backgrounds |

### Dark Mode Contrast Tips:
- Don't use pure white (#FFFFFF) on pure black (#000000) — too harsh
- Use off-white (#E0E0E0) on dark gray (#121212)
- Reduce opacity for secondary text rather than using lighter gray
- Test contrast with Figma's built-in contrast checker or the a11y plugin

---

## COMMON MISTAKES

| Mistake | Problem | Fix |
|---|---|---|
| Hard-coded hex colors | Can't theme, inconsistent | Use variables everywhere |
| No semantic layer | Primitives used directly | Add Semantic tier |
| No dark mode values | App doesn't support theming | Add Dark mode to Semantic collection |
| Magic numbers for spacing | Inconsistent, hard to maintain | Use spacing variables (8px scale) |
| Different radius per component | Inconsistent look | Use radius variables |
| Color styles AND variables for same color | Confusion about source of truth | Pick one system, migrate gradually |
| Variables named by appearance | `blue-button` breaks when brand changes | Name by purpose: `background/brand` |
| Too many primitive shades | 50 shades of gray nobody uses | Keep 6-8 shades per hue |

---

## VARIABLES & TOKENS CHECKLIST

- [ ] All colors use variables (no hard-coded hex)?
- [ ] Three-tier architecture: Primitives → Semantic → Component?
- [ ] Semantic tokens named by purpose, not appearance?
- [ ] Light and Dark modes configured?
- [ ] Spacing uses the 8px scale as variables?
- [ ] Corner radius uses variables?
- [ ] Component tokens reference semantic tokens (not primitives)?
- [ ] Contrast ratios meet WCAG 2.2 minimums?
- [ ] Typography remains as Figma Styles (not Variables)?
- [ ] Collections are organized and documented?