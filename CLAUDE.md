# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TBC Strat Library is a single-file static website displaying World of Warcraft: The Burning Crusade Classic raid boss strategies. The entire application is contained in `index.html` with inline CSS and JavaScript.

## Running the Project

Open `index.html` directly in a browser, or serve with any static file server:
```bash
python3 -m http.server 8000
# or
npx serve .
```

## Architecture

### Single-File Structure
Everything is in `index.html`:
- **CSS** (lines 9-90): CSS variables for theming, responsive design at 700px breakpoint
- **HTML** (lines 92-116): Header with raid selector, role filter bar, main content container
- **JavaScript** (lines 117-397): Data definitions and rendering logic

### Data Model
Two main data objects drive the application:

**`SPELLS`** (line 118): Dictionary of spell tooltips
```javascript
{ "Spell Name": { t: "Type", d: "Description" } }
```

**`RAIDS`** (lines 137-365): Raid content organized as:
```javascript
{
  raidKey: {
    name: "Raid Name",
    bosses: [{
      name: "Boss Name",
      img: "url",
      icon: "emoji",
      sections: [{
        title: "Phase Title",
        isNote: boolean,
        tips: [{ text: "Strategy text", role: "tank|heal|melee|ranged" }]
      }]
    }]
  }
}
```

### Key Functions
- `S(name)` - Renders spell with hover tooltip
- `M(name)` - Renders mob name with styling
- `render()` - Main render function, rebuilds entire content area
- `toggleBoss(i)` - Accordion expand/collapse
- `applyRoleFilter()` - Shows/hides tips by role

### External Dependencies
- **Wowhead tooltips** (`wow.zamimg.com/widgets/power.js`): Provides rich tooltips for WoW item/spell links
- **Google Fonts**: Cinzel (headings) and Inter (body text)
- **Boss images**: Hosted on sunderarmor.com

## Adding Content

### New Boss
Add to the appropriate raid's `bosses` array:
```javascript
{
  name: "Boss Name",
  img: IMG["Boss Name"],  // Add to IMG object if needed
  icon: "🐉",
  sections: [
    { title: "Phase 1", tips: [{ text: "Strategy here" }] },
    { title: "Notes", isNote: true, tips: [...] }
  ]
}
```

### New Spell
Add to `SPELLS` object:
```javascript
"Spell Name": { t: "Type", d: "Description text" }
```

Use in tips with `${S("Spell Name")}` template literal.

### Role-Specific Tips
Add `role` property to filter by role:
```javascript
{ text: "Tank-specific tip", role: "tank" }  // tank|heal|melee|ranged
```

## Styling Conventions

- Use CSS variables from `:root` for colors
- Role colors: `--role-tank` (blue), `--role-heal` (green), `--role-melee` (red), `--role-ranged` (orange)
- Content is in French
