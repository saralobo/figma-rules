# File Organization — Structuring Figma Files for Scale

> A well-organized Figma file is the difference between a design system that scales and one that collapses under its own weight.

**Sources**: [Figma Developer Docs](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/), [Structuring Large-Scale Figma Design Systems (2025)](https://medium.com/@claus.nisslmueller/structuring-and-splitting-large-scale-figma-design-systems-a-2025-master-guide-for-scalable-c1c3a7dabb0e), Figma Best Practices

---

## PAGE STRUCTURE

Every Figma file should have a clear page hierarchy. Pages are the highest level of organization.

### Recommended Page Order:

| Page | Purpose | Icon |
|---|---|---|
| **Cover** | File thumbnail, project name, status, owner | 📋 |
| **Screens** | Final screen designs organized by flow | 📱 |
| **Components** | Local components (if not using external library) | 🧩 |
| **Patterns** | Reusable screen patterns/templates | 📐 |
| **Explorations** | Work-in-progress, alternatives, experiments | 🔬 |
| **Archive** | Deprecated screens, old versions (don't delete) | 📦 |

### Page Naming Convention:

```
✅ Good:
  📋 Cover
  📱 Screens — Onboarding
  📱 Screens — Home
  📱 Screens — Settings
  🧩 Components
  📦 Archive

❌ Bad:
  Page 1
  Page 2
  New page
  Copy of Screens
```

### Rules:
- ONE flow per page (don't mix unrelated screens on the same page)
- Use emoji prefixes for visual scanning (optional but recommended)
- Keep Cover page as the first page (it becomes the file thumbnail)
- Archive instead of deleting — you never know when you'll need old work

---

## FRAME ORGANIZATION WITHIN A PAGE

### Reading Direction:
Arrange frames **left to right**, **top to bottom** — like reading a book.

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│ Step 1  │  │ Step 2  │  │ Step 3  │
│ Splash  │→ │ Login   │→ │ Home    │
└─────────┘  └─────────┘  └─────────┘
                              ↓
┌─────────┐  ┌─────────┐  ┌─────────┐
│ Step 6  │  │ Step 5  │  │ Step 4  │
│ Confirm │← │ Review  │← │ Detail  │
└─────────┘  └─────────┘  └─────────┘
```

### Frame Spacing:
- **Between related screens**: 100px
- **Between flow sections**: 200-300px
- **Between unrelated groups**: 500px+

### Screen Naming:
```
✅ Good:
  Screen/Onboarding/Welcome
  Screen/Onboarding/CreateAccount
  Screen/Home/Default
  Screen/Home/Loading
  Screen/Settings/Profile

❌ Bad:
  Frame 1
  iPhone 14 Pro - 1
  Screen copy 2
  Final FINAL v3
```

---

## WHEN TO SPLIT INTO MULTIPLE FILES

### Single File (Small Project):
- 1-2 designers
- < 50 screens
- < 20 components
- File loads in < 5 seconds

### Multiple Files (Large Project):
- 3+ designers working simultaneously
- 50+ screens
- Performance issues (slow loading, lag)
- Separate concerns (design system library vs product screens)

### Recommended Split:

| File | Contents |
|---|---|
| **[Project] Design System** | All components, styles, variables, tokens |
| **[Project] Screens — Feature A** | Screens for Feature A flow |
| **[Project] Screens — Feature B** | Screens for Feature B flow |
| **[Project] Explorations** | All WIP, experiments, alternatives |

### Rules for Multi-File:
- Design system file is the **single source of truth** for components
- Screen files **consume** components from the library (never duplicate)
- Use consistent naming across files
- Publish the design system as a **team library**

---

## COVER PAGE TEMPLATE

Every file should have a Cover page with:

```
┌─────────────────────────────────────────┐
│                                         │
│   PROJECT NAME                          │
│   Brief description of the project      │
│                                         │
│   Status: 🟢 In Progress / 🟡 Review / 🔴 Archived
│   Owner: Designer Name                  │
│   Last Updated: Feb 2026                │
│   Platform: iOS / Android / Web         │
│                                         │
│   Links:                                │
│   - Jira/Linear board                   │
│   - Confluence/Notion docs              │
│   - Design system library               │
│                                         │
└─────────────────────────────────────────┘
```

---

## SECTION ORGANIZATION (WITHIN A PAGE)

Use **Sections** (Figma native feature) to group related frames:

### When to use Sections:
- Group all screens of a single flow
- Group all states of a single screen (Default, Loading, Error, Empty)
- Group component variants during exploration

### Section Naming:
```
Flow/Onboarding
Flow/Checkout
States/Home
States/Profile
Exploration/Navigation Alternatives
```

---

## ANNOTATIONS AND DOCUMENTATION

From [Figma's own recommendations](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/):

> Use annotations to convey design intent that's hard to capture from visuals alone — like how something should behave, align, or respond.

### When to annotate:
- Scroll behavior (what sticks, what scrolls)
- Animation/transition intent
- Conditional logic (show X when Y)
- Edge cases the developer needs to handle
- Responsive behavior at different breakpoints

### Where to annotate:
- Use Figma's native annotation feature (Dev Mode)
- Or place annotation frames adjacent to screens with arrows pointing to specific elements

---

## VERSION CONTROL IN FIGMA

### Branch Strategy:
- **Main** branch = source of truth, always production-ready
- **Feature branches** = one per feature or exploration
- Name branches: `feature/new-onboarding`, `fix/button-states`, `explore/dark-mode`

### When to branch:
- Major feature redesigns
- Experimental changes that might not ship
- When multiple designers work on the same file simultaneously

### When NOT to branch:
- Small fixes (just edit main directly)
- Adding new screens that don't affect existing ones

---

## FILE ORGANIZATION CHECKLIST

- [ ] Does the file have a Cover page with project info?
- [ ] Are pages named descriptively (not "Page 1")?
- [ ] Is there one flow per page?
- [ ] Are frames arranged left-to-right, top-to-bottom?
- [ ] Are all frames named with a clear convention?
- [ ] Are deprecated screens archived (not deleted)?
- [ ] Is the design system in a separate published library?
- [ ] Are sections used to group related screens?
- [ ] Are there annotations for non-obvious behavior?
- [ ] Is the file performant (loads in < 5 seconds)?