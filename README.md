# Figma Rules — Best Practices for Scalable Design

A comprehensive collection of Figma best practices, conventions, and rules optimized for both **human designers** and **AI agents** (Claude, Cursor, MCP).

These rules ensure every screen is built with proper auto-layout, naming, responsiveness, componentization, and token architecture — ready for handoff and automation.

---

## Quick Start

### For Designers:
Read the docs in order. Each rule builds on the previous:

1. Start with **File Organization** to structure your project
2. Apply **Layer Naming** to everything you create
3. Use **Auto-Layout** on every frame (no exceptions)
4. Test **Responsiveness** at multiple screen sizes
5. Build **Components** following the architecture guide
6. Set up **Variables & Tokens** for theming

### For AI Agents (Cursor/Claude + Figma MCP):
Read `docs/07-figma-api-patterns.md` first — it contains the critical order of operations and error-prone patterns specific to programmatic design.

---

## Rules Index

| # | Rule | Description | Key Takeaway |
|---|---|---|---|
| 01 | [File Organization](docs/01-file-organization.md) | Page structure, naming, multi-file strategy, cover pages | Every file needs Cover, Screens, Components, Archive |
| 02 | [Layer Naming](docs/02-layer-naming.md) | Semantic naming, grouping, searchability, dev handoff | Name by purpose, not appearance: `Section/Header` not `Frame 42` |
| 03 | [Auto-Layout](docs/03-auto-layout.md) | FILL/HUG/FIXED sizing, spacing, alignment, nesting | EVERYTHING uses auto-layout. No exceptions. |
| 04 | [Constraints & Responsiveness](docs/04-constraints-responsiveness.md) | Pin & scale, min/max, responsive patterns, scroll | Content area = FILL × FILL to absorb remaining space |
| 05 | [Component Architecture](docs/05-component-architecture.md) | Variants, properties, slots, hierarchy, lifecycle | Atoms → Molecules → Organisms. Never detach instances. |
| 06 | [Variables & Tokens](docs/06-variables-tokens.md) | Three-tier tokens, modes, theming, contrast | Primitives → Semantic → Component. No hard-coded values. |
| 07 | [Figma API Patterns](docs/07-figma-api-patterns.md) | Plugin API, MCP patterns, error prevention | Append child FIRST, then set FILL. Always. |

---

## The 7 Absolute Rules

1. **Everything uses auto-layout** — No manually positioned elements
2. **Every layer has a semantic name** — `Section/Header`, not `Frame 237`
3. **Screen = FIXED, Content = FILL × FILL** — The responsive skeleton
4. **No hard-coded colors** — Use variables/tokens everywhere
5. **Never detach instances** — Override properties instead
6. **Space between groups ≥ 2× space within** — Visual hierarchy through proximity
7. **Append to parent BEFORE setting FILL** — The #1 API error prevention rule

---

## Using as Cursor Rules

These docs can be used as Cursor Rules (`.mdc` files) for AI-assisted design:

1. Copy the relevant `.md` files to your project's `.cursor/rules/` folder
2. Rename them with `.mdc` extension
3. Add the Cursor Rules metadata header:

```
---
description: "Auto-layout rules for Figma screen construction"
globs: []
alwaysApply: true
---
```

4. The AI assistant will automatically reference these rules when building screens

---

## Sources & References

- [Figma Help Center](https://help.figma.com/)
- [Figma Best Practices](https://www.figma.com/best-practices/)
- [Figma Plugin API Documentation](https://www.figma.com/plugin-docs/)
- [Figma Developer Documentation](https://developers.figma.com/)
- [Figma MCP Server Docs](https://developers.figma.com/docs/figma-mcp-server/)
- [W3C Design Tokens Community Group](https://design-tokens.github.io/community-group/format/)
- [WCAG 2.2 Guidelines](https://www.w3.org/TR/WCAG22/)
- [Material Design 3](https://m3.material.io/)

---

## Contributing

These rules are maintained by [@saralobo](https://github.com/saralobo). Feel free to open issues or PRs for improvements.

---

## License

MIT — Use freely, attribution appreciated.