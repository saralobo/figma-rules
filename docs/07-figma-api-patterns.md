# Figma API & AI Agent Patterns — Automating Design with Code

> The Figma Plugin API is how AI agents (Claude, Cursor, MCP) interact with your designs.
> Understanding these patterns ensures agents build screens correctly the FIRST time.

**Sources**: [Figma Plugin API Docs](https://www.figma.com/plugin-docs/), [Figma MCP Server](https://developers.figma.com/docs/figma-mcp-server/), [Figma REST API](https://www.figma.com/developers/api)

---

## TWO APIs, DIFFERENT PURPOSES

| API | Access | Use Case |
|---|---|---|
| **Plugin API** | Runs inside Figma (plugins, MCP) | Create/edit nodes, set properties, manage styles |
| **REST API** | External HTTP calls | Read file data, export images, get comments |

> AI agents primarily use the **Plugin API** through the Figma MCP server.

---

## CRITICAL ORDER OF OPERATIONS

The #1 source of errors when building screens programmatically:

```
1. Create the parent frame
2. Configure auto-layout on the parent
3. Create the child element
4. Append child to parent         ← MUST happen before step 5
5. Set FILL sizing on child       ← Only works AFTER appending
6. Set other child properties
```

### The FILL Rule (Repeat Until Memorized):
> `layoutSizingHorizontal = 'FILL'` can ONLY be set on a node that is ALREADY a direct child of an auto-layout frame.
> Setting it before appending the child to the parent will cause an error.

### Correct:
```javascript
const parent = figma.createFrame();
parent.layoutMode = 'VERTICAL';
parent.primaryAxisSizingMode = 'AUTO';
parent.counterAxisSizingMode = 'FIXED';
parent.resize(393, 100);

const child = figma.createFrame();
parent.appendChild(child);           // Step 1: Add to parent
child.layoutSizingHorizontal = 'FILL'; // Step 2: NOW set FILL
child.layoutSizingVertical = 'HUG';
```

### Incorrect:
```javascript
const child = figma.createFrame();
child.layoutSizingHorizontal = 'FILL'; // ERROR: Not a child of auto-layout yet
parent.appendChild(child);
```

---

## FRAME CREATION PATTERNS

### Screen Frame:
```javascript
const screen = figma.createFrame();
screen.name = 'Screen/Home/Default';
screen.resize(393, 852);
screen.layoutMode = 'VERTICAL';
screen.primaryAxisSizingMode = 'FIXED';
screen.counterAxisSizingMode = 'FIXED';
screen.itemSpacing = 0;
screen.paddingTop = 0;
screen.paddingBottom = 0;
screen.paddingLeft = 0;
screen.paddingRight = 0;
screen.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
```

### Header:
```javascript
const header = figma.createFrame();
header.name = 'Section/Header';
header.layoutMode = 'HORIZONTAL';
header.counterAxisAlignItems = 'CENTER';
header.primaryAxisAlignItems = 'SPACE_BETWEEN';
header.resize(393, 56);
header.paddingLeft = 16;
header.paddingRight = 16;
header.fills = [];

screen.appendChild(header);
header.layoutSizingHorizontal = 'FILL';
// height stays FIXED at 56
```

### Content Area (FILL × FILL):
```javascript
const content = figma.createFrame();
content.name = 'Section/Content';
content.layoutMode = 'VERTICAL';
content.itemSpacing = 24;
content.paddingTop = 16;
content.paddingBottom = 16;
content.paddingLeft = 16;
content.paddingRight = 16;
content.fills = [];

screen.appendChild(content);
content.layoutSizingHorizontal = 'FILL';
content.layoutSizingVertical = 'FILL';  // Absorbs remaining height
```

### Card:
```javascript
const card = figma.createFrame();
card.name = 'Card/Product';
card.layoutMode = 'VERTICAL';
card.itemSpacing = 8;
card.paddingTop = 16;
card.paddingRight = 16;
card.paddingBottom = 16;
card.paddingLeft = 16;
card.cornerRadius = 12;
card.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
card.effects = [{
  type: 'DROP_SHADOW',
  color: { r: 0, g: 0, b: 0, a: 0.08 },
  offset: { x: 0, y: 2 },
  radius: 8,
  visible: true,
  blendMode: 'NORMAL'
}];

content.appendChild(card);
card.layoutSizingHorizontal = 'FILL';
card.layoutSizingVertical = 'HUG';
```

---

## TEXT CREATION

### Load Font BEFORE Setting Characters:
```javascript
const text = figma.createText();
await figma.loadFontAsync({ family: 'Inter', style: 'Regular' });
text.characters = 'Hello World';
text.fontSize = 16;
text.lineHeight = { value: 24, unit: 'PIXELS' };
text.fills = [{ type: 'SOLID', color: { r: 0.13, g: 0.13, b: 0.13 } }];

parent.appendChild(text);
text.layoutSizingHorizontal = 'FILL';
```

### Common Font Loading:
```javascript
await figma.loadFontAsync({ family: 'Inter', style: 'Regular' });
await figma.loadFontAsync({ family: 'Inter', style: 'Medium' });
await figma.loadFontAsync({ family: 'Inter', style: 'Semi Bold' });
await figma.loadFontAsync({ family: 'Inter', style: 'Bold' });
```

### Text Hierarchy:

| Role | Font | Size | Weight | Line Height |
|---|---|---|---|---|
| Display | Inter | 32-40px | Bold | 1.2x |
| Heading 1 | Inter | 24-28px | Bold | 1.3x |
| Heading 2 | Inter | 20-22px | Semi Bold | 1.3x |
| Heading 3 | Inter | 18px | Semi Bold | 1.4x |
| Body Large | Inter | 16px | Regular | 1.5x (24px) |
| Body | Inter | 14px | Regular | 1.5x (20px) |
| Caption | Inter | 12px | Regular | 1.4x (16px) |
| Overline | Inter | 10-11px | Medium | 1.4x |

---

## COMPONENT INSTANTIATION

### Finding Components:
```javascript
const results = figma.currentPage.findAll(node => 
  node.type === 'COMPONENT' && node.name.includes('Button')
);
```

### Creating Instances:
```javascript
const component = figma.currentPage.findOne(node => 
  node.type === 'COMPONENT' && node.name === 'Button/Primary/Default'
);

if (component) {
  const instance = component.createInstance();
  parent.appendChild(instance);
  instance.layoutSizingHorizontal = 'FILL';
}
```

### Setting Instance Properties:
```javascript
instance.setProperties({
  'label#1234:0': 'Click Me',    // Text property
  'showIcon#5678:0': true,        // Boolean property
});
```

> Property IDs are session-specific. ALWAYS search for components at the start of each session.

---

## SECTIONS AND PAGE ORGANIZATION

### Creating Organized Layouts:
```javascript
const section = figma.createSection();
section.name = 'Flow/Onboarding';
section.x = 0;
section.y = 0;

const screen1 = createScreen('Welcome');
screen1.x = 0;
screen1.y = 0;
section.appendChild(screen1);

const screen2 = createScreen('SignUp');
screen2.x = 450; // 393px width + 57px gap
screen2.y = 0;
section.appendChild(screen2);
```

### Spacing Between Screens:
- Horizontal gap: **40-60px** between screens in the same flow
- Vertical gap: **100-200px** between different flows
- Use Sections to group related screens

---

## COLOR HANDLING

### Figma Uses 0-1 Range (Not 0-255):
```javascript
// Converting hex to Figma color:
function hexToFigmaColor(hex) {
  const r = parseInt(hex.slice(1, 3), 16) / 255;
  const g = parseInt(hex.slice(3, 5), 16) / 255;
  const b = parseInt(hex.slice(5, 7), 16) / 255;
  return { r, g, b };
}

// Usage:
frame.fills = [{ 
  type: 'SOLID', 
  color: hexToFigmaColor('#0066FF') 
}];

// With opacity:
frame.fills = [{ 
  type: 'SOLID', 
  color: hexToFigmaColor('#000000'),
  opacity: 0.5
}];

// Transparent (no fill):
frame.fills = [];
```

### Common Colors (Pre-converted):

| Color | Hex | Figma RGB |
|---|---|---|
| White | #FFFFFF | `{ r: 1, g: 1, b: 1 }` |
| Black | #000000 | `{ r: 0, g: 0, b: 0 }` |
| Gray 100 | #F5F5F5 | `{ r: 0.96, g: 0.96, b: 0.96 }` |
| Gray 400 | #BDBDBD | `{ r: 0.74, g: 0.74, b: 0.74 }` |
| Gray 900 | #212121 | `{ r: 0.13, g: 0.13, b: 0.13 }` |
| Blue 500 | #0066FF | `{ r: 0, g: 0.4, b: 1 }` |
| Green 500 | #00A650 | `{ r: 0, g: 0.65, b: 0.31 }` |
| Red 500 | #F23D4F | `{ r: 0.95, g: 0.24, b: 0.31 }` |

---

## ERROR-PRONE PATTERNS & FIXES

| Error | Cause | Fix |
|---|---|---|
| `FILL can only be set on children of auto-layout` | Setting FILL before appendChild | Append first, then set FILL |
| `Cannot read property of undefined` | Font not loaded before setting text | `await figma.loadFontAsync()` first |
| `Node not found` | Stale component ID from previous session | Re-search components every session |
| Frame appears at 0,0 with weird size | Didn't set position and dimensions | Always set x, y, and resize() |
| Text appears as "..." | Text overflow, frame too narrow | Set FILL or resize frame wider |
| Colors look wrong | Using 0-255 values instead of 0-1 | Divide by 255 |
| Auto-layout not working | Missing `layoutMode` | Set `layoutMode = 'VERTICAL'` or `'HORIZONTAL'` |
| Children overlap | No `itemSpacing` set | Set `itemSpacing` between children |

---

## BATCH OPERATIONS (PERFORMANCE)

When creating multiple elements, batch operations for performance:

```javascript
// Good: Load all fonts once at the start
await Promise.all([
  figma.loadFontAsync({ family: 'Inter', style: 'Regular' }),
  figma.loadFontAsync({ family: 'Inter', style: 'Bold' }),
  figma.loadFontAsync({ family: 'Inter', style: 'Semi Bold' }),
]);

// Good: Create all elements, then position
const frames = [];
for (let i = 0; i < 5; i++) {
  const frame = figma.createFrame();
  frame.name = `Card/${i}`;
  frames.push(frame);
}
frames.forEach((f, i) => {
  parent.appendChild(f);
  f.layoutSizingHorizontal = 'FILL';
});
```

---

## COMPLETE SCREEN TEMPLATE

A full screen built with all the correct patterns:

```javascript
// 1. Load fonts
await figma.loadFontAsync({ family: 'Inter', style: 'Regular' });
await figma.loadFontAsync({ family: 'Inter', style: 'Semi Bold' });
await figma.loadFontAsync({ family: 'Inter', style: 'Bold' });

// 2. Screen frame
const screen = figma.createFrame();
screen.name = 'Screen/Home/Default';
screen.resize(393, 852);
screen.layoutMode = 'VERTICAL';
screen.primaryAxisSizingMode = 'FIXED';
screen.counterAxisSizingMode = 'FIXED';
screen.itemSpacing = 0;
screen.fills = [{ type: 'SOLID', color: { r: 0.96, g: 0.96, b: 0.96 } }];

// 3. Header
const header = figma.createFrame();
header.name = 'Section/Header';
header.layoutMode = 'HORIZONTAL';
header.counterAxisAlignItems = 'CENTER';
header.primaryAxisAlignItems = 'SPACE_BETWEEN';
header.resize(393, 56);
header.paddingLeft = 16;
header.paddingRight = 16;
header.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];

screen.appendChild(header);
header.layoutSizingHorizontal = 'FILL';

const title = figma.createText();
title.characters = 'Home';
title.fontSize = 20;
title.fontName = { family: 'Inter', style: 'Semi Bold' };
title.fills = [{ type: 'SOLID', color: { r: 0.13, g: 0.13, b: 0.13 } }];
header.appendChild(title);

// 4. Content
const content = figma.createFrame();
content.name = 'Section/Content';
content.layoutMode = 'VERTICAL';
content.itemSpacing = 24;
content.paddingTop = 16;
content.paddingBottom = 16;
content.paddingLeft = 16;
content.paddingRight = 16;
content.fills = [];

screen.appendChild(content);
content.layoutSizingHorizontal = 'FILL';
content.layoutSizingVertical = 'FILL';

// 5. Card inside content
const card = figma.createFrame();
card.name = 'Card/Welcome';
card.layoutMode = 'VERTICAL';
card.itemSpacing = 8;
card.paddingTop = 16;
card.paddingRight = 16;
card.paddingBottom = 16;
card.paddingLeft = 16;
card.cornerRadius = 12;
card.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];

content.appendChild(card);
card.layoutSizingHorizontal = 'FILL';
card.layoutSizingVertical = 'HUG';

const cardTitle = figma.createText();
cardTitle.characters = 'Welcome Back';
cardTitle.fontSize = 18;
cardTitle.fontName = { family: 'Inter', style: 'Semi Bold' };
cardTitle.fills = [{ type: 'SOLID', color: { r: 0.13, g: 0.13, b: 0.13 } }];
card.appendChild(cardTitle);
cardTitle.layoutSizingHorizontal = 'FILL';

const cardDesc = figma.createText();
cardDesc.characters = 'Here is your daily summary';
cardDesc.fontSize = 14;
cardDesc.fills = [{ type: 'SOLID', color: { r: 0.47, g: 0.47, b: 0.47 } }];
card.appendChild(cardDesc);
cardDesc.layoutSizingHorizontal = 'FILL';

// 6. Bottom Navigation
const bottomNav = figma.createFrame();
bottomNav.name = 'Section/BottomNav';
bottomNav.layoutMode = 'HORIZONTAL';
bottomNav.counterAxisAlignItems = 'CENTER';
bottomNav.primaryAxisAlignItems = 'SPACE_BETWEEN';
bottomNav.resize(393, 56);
bottomNav.paddingLeft = 24;
bottomNav.paddingRight = 24;
bottomNav.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];

screen.appendChild(bottomNav);
bottomNav.layoutSizingHorizontal = 'FILL';

const navItems = ['Home', 'Search', 'Cart', 'Profile'];
for (const label of navItems) {
  const navItem = figma.createText();
  navItem.characters = label;
  navItem.fontSize = 12;
  navItem.textAlignHorizontal = 'CENTER';
  navItem.fills = [{ type: 'SOLID', color: { r: 0.47, g: 0.47, b: 0.47 } }];
  bottomNav.appendChild(navItem);
  navItem.layoutSizingHorizontal = 'FILL';
}

figma.currentPage.appendChild(screen);
figma.viewport.scrollAndZoomIntoView([screen]);
```

---

## MCP-SPECIFIC PATTERNS

### Using figma_execute:
- Code runs inside Figma's sandbox
- Always use `await` for async operations
- Font loading is async and MUST be awaited
- Return results for verification

### Using figma_take_screenshot:
- Call AFTER creating/modifying elements
- Use to verify visual result
- Iterate if the result doesn't match expectations

### Using figma_search_components:
- Call at the START of every session
- Component IDs are session-specific (they change between sessions)
- Cache results for the session but NEVER across sessions

---

## API PATTERNS CHECKLIST

- [ ] Fonts loaded before setting text characters?
- [ ] FILL sizing set AFTER appending to auto-layout parent?
- [ ] Colors using 0-1 range (not 0-255)?
- [ ] Components searched fresh at session start?
- [ ] Screen frame set to FIXED sizing mode?
- [ ] Content area set to FILL × FILL?
- [ ] All frames have explicit names following naming convention?
- [ ] Screenshot taken after creation to verify result?
- [ ] Error handling for missing components?
- [ ] Batch font loading at the start?