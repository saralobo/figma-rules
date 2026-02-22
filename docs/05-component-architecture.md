# Component Architecture — Building Scalable Design Systems

> Components are the atoms of your design system. Built wrong, they become tech debt.
> Built right, they make every future screen 10x faster.

**Sources**: [Figma Help Center — Components](https://help.figma.com/hc/en-us/articles/360038662654), [Figma Best Practices](https://www.figma.com/best-practices/components-styles-and-shared-libraries/), [Figma Dev Docs — Properties](https://developers.figma.com/docs/figma-mcp-server/)

---

## COMPONENT vs INSTANCE

| Concept | Symbol | Description |
|---|---|---|
| **Main Component** | ◇ (diamond) | The source of truth. Edit here, all instances update |
| **Instance** | ◆ (filled diamond) | A copy that inherits from the main component |
| **Detached Instance** | — (no symbol) | AVOID. A broken copy that no longer updates |

### Rules:
- NEVER detach instances. Override properties instead
- If you need to detach, you need a new variant or a new component
- Component main should live in a dedicated library/page, NOT inline

---

## NAMING CONVENTION

### Slash Hierarchy:

Figma uses `/` to create groups in the Assets panel:

```
Button/Primary/Default
Button/Primary/Hover
Button/Primary/Disabled
Button/Secondary/Default
Button/Secondary/Hover
```

This creates:
```
├── Button
│   ├── Primary
│   │   ├── Default
│   │   ├── Hover
│   │   └── Disabled
│   └── Secondary
│       ├── Default
│       └── Hover
```

### Naming Rules:
- Use **PascalCase** for component names: `Button`, `CardProduct`, `NavItem`
- Use **camelCase** for property names: `showIcon`, `hasAction`, `labelText`
- Use **kebab-case** for variant values: `primary`, `secondary`, `extra-large`
- NEVER use spaces in component names
- NEVER start with underscore unless it's a private/internal component: `_BaseInput`

---

## VARIANTS

Variants are different versions of the same component, grouped in a **component set** (purple dashed border).

### Variant Properties:

| Property Type | Description | Example |
|---|---|---|
| **Variant** | Predefined options (dropdown) | `type=primary`, `size=large` |
| **Boolean** | Toggle on/off | `showIcon=true/false` |
| **Instance Swap** | Swap a nested component | `icon=IconSearch` |
| **Text** | Editable text content | `label=Button Text` |

### Property Naming Standards:

| Property | Values | Use |
|---|---|---|
| `type` or `variant` | `primary`, `secondary`, `tertiary`, `ghost` | Visual style |
| `size` | `small`, `medium`, `large` | Size |
| `state` | `default`, `hover`, `pressed`, `focused`, `disabled` | Interaction state |
| `status` | `info`, `success`, `warning`, `error` | Semantic status |
| `showIcon` | `true`, `false` | Toggle icon visibility |
| `showBadge` | `true`, `false` | Toggle badge visibility |
| `showAction` | `true`, `false` | Toggle trailing action |
| `icon` | Instance swap | Choose icon |
| `label` | Text | Button/label text |
| `isSelected` | `true`, `false` | Selected/active state |

---

## COMPONENT ANATOMY

### The Slot Pattern:

A well-built component uses "slots" — areas that can be shown/hidden and swapped:

```
ListItem Component:
┌────────────────────────────────────────┐
│ [LeadingSlot]  [ContentSlot]  [TrailingSlot] │
│  (icon/avatar)  (title+desc)   (action/chevron)│
└────────────────────────────────────────┘
```

### Slot Properties:
- `showLeading` (Boolean) → Toggle leading slot
- `leading` (Instance Swap) → Choose what goes in the slot
- `showTrailing` (Boolean) → Toggle trailing slot
- `trailing` (Instance Swap) → Choose trailing content

---

## COMPONENT HIERARCHY

### Atoms → Molecules → Organisms:

```
Atoms (smallest, no children components):
  Icon, Badge, Divider, Avatar, Tag

Molecules (combine 2-3 atoms):
  Button (icon + label), Input (label + field + error), 
  ListItem (avatar + text + action)

Organisms (combine molecules):
  Card (image + text group + action bar),
  Header (back button + title + actions),
  Form (label + inputs + button)

Templates (combine organisms):
  Screen (header + content sections + nav)
```

### Rules:
- Atoms should have NO nested component instances
- Molecules reference atoms via instance swap
- Organisms reference molecules and atoms
- Templates reference organisms
- NEVER skip hierarchy levels (no atoms directly in templates)

---

## BASE COMPONENTS (PRIVATE)

Use base components (prefixed with `_` or `.`) for internal building blocks:

```
_BaseButton       →  Used inside Button/Primary, Button/Secondary
_BaseInput        →  Used inside TextField, SearchField, PasswordField
_BaseCard         →  Used inside CardProduct, CardTransaction, CardNotification
.IconContainer    →  Used inside IconButton, NavItem, ActionIcon
```

### Rules:
- Prefix with `_` or `.` to hide from the Assets panel publish
- Base components contain the shared structure
- Exposed components add specific configurations
- NEVER use base components directly in screens

---

## COMPONENT PROPERTIES BEST PRACTICES

### Do:
- Use **Boolean properties** for toggling visibility: `showDivider`, `showBadge`
- Use **Instance Swap** for flexible slots: `leadingIcon`, `trailingAction`
- Use **Text properties** for editable content: `titleText`, `descriptionText`
- Use **Variant properties** for predefined styles: `type`, `size`, `state`
- Name properties clearly: `showLeadingIcon` not `sw1`

### Don't:
- Don't create a variant for every state combination (explosion of variants)
- Don't use variant property for simple show/hide (use Boolean)
- Don't expose more than 8-10 properties per component (too complex)
- Don't nest more than 3 levels of component instances

### Property Count Guide:

| Component Complexity | Max Properties | Example |
|---|---|---|
| Simple (atom) | 2-3 | Icon: `name`, `size` |
| Medium (molecule) | 4-6 | Button: `type`, `size`, `state`, `label`, `showIcon`, `icon` |
| Complex (organism) | 6-10 | Card: `type`, `showImage`, `title`, `desc`, `showAction`, `showBadge`, `status` |

---

## VARIANT MATRIX

Organize variants in a grid for visual clarity:

```
                Default    Hover     Pressed   Disabled
Primary          ■          ■          ■          □
Secondary        ■          ■          ■          □
Tertiary         ■          ■          ■          □
Ghost            ■          ■          ■          □
```

### Spacing Between Variants:
- Gap between variants in same row: **16px**
- Gap between variant rows: **16px**
- The component set frame should use auto-layout with Wrap

---

## COMPONENT DOCUMENTATION

Every published component should include:

1. **Description**: What the component is and when to use it
2. **Property descriptions**: What each property does
3. **Link**: URL to design system documentation (Zeroheight, Storybook)

### In Figma:
- Select the main component
- In the right panel, add a description
- Add a documentation link
- For each property, add a description

---

## COMPONENT LIFECYCLE

```
1. DESIGN    →  Create the component in a local file
2. REVIEW    →  Test with real content, edge cases, responsive
3. PUBLISH   →  Move to shared library, publish
4. ADOPT     →  Replace all local instances with library version
5. MAINTAIN  →  Update in library, all instances auto-update
6. DEPRECATE →  Mark as deprecated, provide migration path
7. ARCHIVE   →  Move to archive, don't delete
```

### Deprecation Rules:
- NEVER just delete a component
- Add `[DEPRECATED]` prefix to name
- Add deprecation note in description with migration instructions
- Keep for at least 2 sprints before archiving

---

## LIBRARY ORGANIZATION

### Structure:

```
Design System Library (Figma file)
├── 📋 Cover
├── 🎨 Foundations
│   ├── Colors (all color styles)
│   ├── Typography (all text styles)
│   ├── Elevation (shadow styles)
│   ├── Grid (layout grids)
│   └── Spacing (spacing reference)
├── 🧩 Components
│   ├── Atoms
│   ├── Molecules
│   ├── Organisms
│   └── Templates
├── 📱 Patterns (common screen patterns)
└── 📦 Archive (deprecated components)
```

---

## COMMON MISTAKES

| Mistake | Problem | Fix |
|---|---|---|
| Detaching instances | Loses connection to source | Override properties instead |
| Components inline in screens | Can't reuse, no single source | Move to dedicated page/library |
| No variant properties | Every change = new component | Add type/size/state properties |
| Too many properties | Component is impossible to use | Max 10 properties, split if more |
| No naming convention | Assets panel is a mess | Use PascalCase + slash hierarchy |
| Hardcoded text in component | Can't customize instances | Use text properties |
| No base components | Duplicated structure | Extract common patterns to `_Base` |
| Publishing internal components | Cluttered library | Prefix with `_` or `.` |
| Deleting deprecated components | Breaks all instances | Deprecate → migrate → archive |
| No documentation | Team doesn't know when/how to use | Add description + link |

---

## COMPONENT ARCHITECTURE CHECKLIST

- [ ] Is the component named with PascalCase and slash hierarchy?
- [ ] Does it use auto-layout (not manual positioning)?
- [ ] Are variant properties named clearly with camelCase?
- [ ] Are Boolean properties used for show/hide toggles?
- [ ] Are instance swap properties used for flexible slots?
- [ ] Is the component set organized in a visual grid?
- [ ] Does it handle edge cases (long text, missing icon, empty state)?
- [ ] Is the component documented with description and link?
- [ ] Are internal parts using base components with `_` prefix?
- [ ] Has it been tested at different sizes and with real content?
- [ ] Is it published to the shared library (not local only)?