# Layer Naming — Semantic Names for Every Element

> If your layers panel looks like "Frame 847 > Group 12 > Rectangle 39", you've already lost.
> Naming is the foundation of maintainability, searchability, and AI-assisted design.

**Sources**: [Figma Developer Docs](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/), [Designilo — Naming Things in Figma (2025)](https://designilo.com/2025/07/11/naming-things-in-figma-best-practices-for-layers-components-and-tokens/), [Figma Best Practices](https://www.figma.com/best-practices/component-architecture/)

---

## WHY NAMING MATTERS

Good naming directly affects:
- **Searchability** — find any element instantly with Cmd+F
- **AI/MCP compatibility** — agents understand `CTA_Button` but not `Rectangle 39`
- **Developer handoff** — devs see layer names in inspect/Dev Mode
- **Team collaboration** — anyone can understand your file without a walkthrough
- **Design system clarity** — components are easier to maintain
- **Code generation** — Figma MCP generates better code from semantic names

---

## THE GOLDEN RULE

> **Every layer name should describe WHAT it is, not HOW it looks.**

```
✅ CTA_Button      ❌ Blue Rectangle
✅ ProductImage    ❌ Rectangle 12
✅ SearchInput     ❌ Frame 847
✅ UserAvatar      ❌ Ellipse 3
✅ PriceLabel      ❌ Text copy 2
```

---

## LAYER NAMING BY TYPE

### Frames (Containers)

Use the **slash convention** for hierarchy:

| Layer Type | Naming Pattern | Example |
|---|---|---|
| Screen | `Screen/FlowName/StateName` | `Screen/Onboarding/Welcome` |
| Section | `Section/SectionName` | `Section/Header`, `Section/HeroBanner` |
| Row | `Row/Description` | `Row/ActionButtons`, `Row/Stats` |
| Card | `Card/Type` | `Card/Product`, `Card/Transaction` |
| Modal | `Modal/Purpose` | `Modal/Confirmation`, `Modal/Error` |
| BottomSheet | `BottomSheet/Purpose` | `BottomSheet/Filters` |

### Components

Follow the **Type / Function / Variant** hierarchy:

```
Button/Primary/Default
Button/Primary/Pressed
Button/Secondary/Disabled
Input/TextField/Default
Input/TextField/Focused
Input/TextField/Error
Icon/24/ArrowLeft
Icon/24/Search
Avatar/Size-48
Avatar/Size-32
Badge/Notification
Chip/Filter/Selected
```

### Text Layers

Name by content role, not the actual text:

| Purpose | Name | NOT |
|---|---|---|
| Page heading | `Text/Heading` | "Welcome back" |
| Body content | `Text/Body` | "Lorem ipsum..." |
| Button label | `Text/ButtonLabel` | "Continue" |
| Price | `Text/Price` | "R$ 29,90" |
| Caption | `Text/Caption` | "Updated 2 hours ago" |
| Input label | `Text/InputLabel` | "Email" |
| Error message | `Text/Error` | "Invalid email" |

### Shape Layers

| Purpose | Name | NOT |
|---|---|---|
| Background | `Background` | "Rectangle 1" |
| Divider | `Divider` | "Line 3" |
| Shadow overlay | `Overlay/Shadow` | "Rectangle 47" |
| Image placeholder | `ImagePlaceholder` | "Rectangle 12" |
| Status indicator | `Indicator/Online` | "Ellipse 5" |

### Icons

Always use the icon name, never the shape:

```
Icon/Search
Icon/ArrowBack
Icon/Close
Icon/Settings
Icon/Notification
```

With size: `Icon/24/Search`, `Icon/16/Close`

---

## NAMING CONVENTIONS

### Casing:

| Convention | Example | When to Use |
|---|---|---|
| **PascalCase** | `ProductCard`, `SearchInput` | Components, frames |
| **Title Case** | `Primary Button`, `Hero Section` | Layer labels in UI |
| **camelCase** | `productCard`, `searchInput` | Variables, tokens |
| **kebab-case** | `product-card`, `search-input` | CSS/code references |
| **dot.notation** | `color.text.primary` | Design tokens |

Pick ONE convention for each context and be consistent.

### Slash Convention (/):

The forward slash `/` creates hierarchy in Figma's assets panel:

```
Button/Primary/Large    → Assets panel shows:
                            └─ Button
                                └─ Primary
                                    └─ Large
```

Use it for:
- Component organization
- Screen grouping
- Icon categorization

---

## VARIANT PROPERTY NAMING

When creating component variants, name properties clearly:

| Property | Values | Example |
|---|---|---|
| **State** | Default, Hover, Pressed, Focused, Disabled | `State=Default` |
| **Size** | Small, Medium, Large | `Size=Large` |
| **Type** | Primary, Secondary, Tertiary, Ghost | `Type=Primary` |
| **HasIcon** | True, False | `HasIcon=True` |
| **Layout** | Horizontal, Vertical | `Layout=Horizontal` |
| **Theme** | Light, Dark | `Theme=Dark` |

### Rules:
- Use Title Case for property names
- Use Title Case for values
- Boolean properties: use `Has`, `Show`, `Is` prefix (`HasIcon`, `ShowBadge`, `IsActive`)
- Keep property names short but clear

---

## TOKEN & VARIABLE NAMING

Design tokens should use **dot notation** following this structure:

```
category.property.variant
```

### Examples:

| Token | Value | Category |
|---|---|---|
| `color.text.primary` | #1A1A1A | Color |
| `color.text.secondary` | #6B6B6B | Color |
| `color.bg.surface` | #FFFFFF | Color |
| `color.bg.elevated` | #F5F5F5 | Color |
| `color.action.primary` | #007AFF | Color |
| `color.error.default` | #FF3B30 | Color |
| `spacing.xs` | 4px | Spacing |
| `spacing.sm` | 8px | Spacing |
| `spacing.md` | 16px | Spacing |
| `spacing.lg` | 24px | Spacing |
| `radius.sm` | 4px | Radius |
| `radius.md` | 8px | Radius |
| `radius.lg` | 16px | Radius |
| `radius.full` | 9999px | Radius |
| `font.heading.xl` | 32/40 Bold | Typography |
| `font.body.md` | 16/24 Regular | Typography |

### Rules:
- Always use semantic names (`color.text.primary`) not visual names (`color.blue.500`)
- Semantic naming enables theming — `color.bg.surface` can be white in light mode and dark gray in dark mode
- Use consistent scales: `xs`, `sm`, `md`, `lg`, `xl`, `2xl`
- Mirror your code token system for 1:1 handoff

---

## GROUPING: FRAME vs GROUP vs SECTION

| Container | When to Use | Auto-Layout? |
|---|---|---|
| **Frame** | ALWAYS the default choice. Any container that holds other elements. | Yes |
| **Group** | Only for grouping decorative/visual elements that don't need layout (e.g., an illustration made of shapes). | No |
| **Section** | Organizing frames on the canvas (like folders on a page). NOT for UI layout. | No |

### Rules:
- **Default to Frame** for everything inside your UI
- **Group** is almost never needed in modern Figma — auto-layout frames handle everything
- **Section** is for canvas organization, not UI structure
- Name all of them semantically (never leave as "Group 1" or "Section 1")

---

## ANTI-PATTERNS TO AVOID

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| `Frame 1234` | Nobody knows what it is | Name by purpose |
| `Group 7` | No semantic meaning | Convert to Frame, add name |
| `Rectangle 42` | Not searchable | Name by role: `Background`, `Divider` |
| `Copy of Button` | Clutters assets panel | Rename or delete |
| `FINAL_v2_REAL_final` | Chaos | Use Figma branches or archive page |
| `Blue Button` | Breaks when color changes | Name by function: `Button/Primary` |
| `btn_prim_d_lg` | Unreadable abbreviations | Use full words: `Button/Primary/Large` |
| Mixing conventions | `Button-primary` + `CardTitle` + `search_input` | Pick ONE convention |

---

## IMPACT ON AI/MCP CODE GENERATION

From [Figma's official documentation](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/):

> Replace default names (`Frame1268`, `Group5`) with intent-driven ones like `CardContainer`, `ProductImage`, or `CTA_Button`. This helps the model understand what it's working with, and what functionality it should have.

When AI tools (like Figma MCP, Claude, Cursor) read your file:
- `ProductCard` → AI generates a proper card component
- `Frame 847` → AI generates a generic div with no semantic meaning
- `SearchInput` → AI generates an input with search functionality
- `Rectangle 12` → AI generates an empty box

---

## NAMING CHECKLIST

- [ ] Are all frames named semantically (no "Frame 1234")?
- [ ] Are all text layers named by role (not by content)?
- [ ] Are all shapes named by purpose (not by shape type)?
- [ ] Are components following the Type/Function/Variant pattern?
- [ ] Are variant properties using clear names with Title Case?
- [ ] Are design tokens using dot.notation?
- [ ] Is there a single naming convention across the file?
- [ ] Are groups converted to frames where auto-layout applies?
- [ ] Would a new team member understand every layer name?
- [ ] Would an AI agent generate good code from these names?