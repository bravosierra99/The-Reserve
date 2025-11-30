# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Obsidian vault for tracking and managing a personal wine collection. The vault uses a structured metadata system powered by Obsidian plugins to create tasting notes, track inventory, maintain shopping lists, and visualize wine labels in various views.

## Architecture

### Folder Structure

- `WineCellar/` - Main Obsidian vault
  - `0_Collection/` - Collection views and queries (All wines, Cards, Fridge, Shopping Lists)
  - `1_Wines/` - Individual wine tasting notes (one file per wine)
  - `8_FileClass/` - Metadata Menu FileClass definitions (Wine.md, Wine-AWS.md)
  - `9_Templates/` - QuickAdd templates for creating new wine notes
  - `.obsidian/` - Obsidian configuration and plugins

### Core Components

**Metadata System**: Uses Metadata Menu plugin with FileClass definitions in `8_FileClass/Wine.md`. This defines the structured fields for wine notes:
- Basic info: Winemaker, WineName, Vintage, Type, Variety, Country-Region
- Purchase tracking: Price, PurchaseSource, PurchaseLink, Inventory, Buy
- Rating: Stars (select), ValueForMoney (0-5), Points
- Optional scoring: Appearance, Aroma, Taste, Aftertaste, Overall, Total Score (in Wine-AWS variant)
- Media: Label (embedded images)

**Template System**: QuickAdd plugin configured in `.obsidian/plugins/quickadd/data.json` with two templates:
- `Wine` - Uses `9_Templates/Tasting Note.md`
- `Wine-AWS` - Uses `9_Templates/Tasting Note AWS.md` (includes detailed scoring fields)

Both create files in `1_Wines/` with naming format: `{{Winemaker}} - {{WineName}} - {{Vintage}}.md`

**Collection Views**: Dataview and DataviewJS queries in `0_Collection/`:
- `All wines.md` - Tables of all wines sorted by ValueForMoney, separated by type
- `Cards.md` - Card grid view showing wine labels (uses CSS classes: cards, cards-1-1, hide-properties)
- `Fridge.md` - Current inventory (Inventory > 0)
- `Shopping List ShopA.md` - Wines marked for purchase, filtered by shop and split into "Rebuy" (has Label) vs "New wine" (no Label)

### Key Plugins

Required community plugins (configured in `.obsidian/`):
- **Dataview** - Powers all collection views with queries and JavaScript
- **Metadata Menu** - Structured metadata fields with FileClass system
- **QuickAdd** - Template-based wine note creation
- **Paste image rename** - Auto-renames pasted label images to match wine name

Theme: Minimal theme with Style Settings for table formatting and property display

## Working with Wine Notes

### Creating New Wine Notes

Use QuickAdd template (Wine or Wine-AWS) which prompts for:
1. Winemaker name
2. Wine name
3. Vintage

File is created in `1_Wines/` with auto-generated filename and frontmatter.

### Field Conventions

- **Type**: Select from "Red wine", "White wine", "Rose wine", "Champagne"
- **Country-Region**: Uses template format `{{Country}} - {{Region}}`
- **Stars**: Visual rating from "--" to "⭐️⭐️⭐️⭐️⭐️"
- **ValueForMoney**: Numeric 0-5 scale
- **Inventory**: Number of bottles owned (0 = not in collection)
- **Buy**: Quantity to purchase (>0 appears in shopping lists)
- **Label**: Embedded image stored in `labels/` subfolder

### DataviewJS Query Pattern

Collection views use this standard pattern:

```javascript
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;
const pages = dv.pages('"1_Wines"')
  .filter(p => /* conditions */)
  .sort((a, b) => (b["ValueForMoney"] ?? 0) - (a["ValueForMoney"] ?? 0));

dv.table([/* columns */],
  await Promise.all(pages.map(async p => [
    // Use await f(dv, p, "FieldName", {options}) for interactive fields
    // Use p.FieldName for static display
  ]))
)
```

Interactive fields (Stars, Inventory, Buy) use `await f(dv, p, "FieldName", {options: {alwaysOn: true, showAddField: true}})` to enable inline editing.

## Modifying the Vault

### Adding New Wine Fields

1. Edit FileClass definition in `WineCellar/8_FileClass/Wine.md` (or Wine-AWS.md)
2. Add field configuration in the `fields:` array frontmatter
3. Update template in `WineCellar/9_Templates/Tasting Note.md`
4. Update relevant Dataview queries in `0_Collection/` views

### Adding New Collection Views

Create new `.md` file in `0_Collection/` with Dataview or DataviewJS query. Follow existing patterns:
- Filter with `.filter(p => condition)`
- Sort by `.sort((a, b) => ...)`
- Use interactive fields for editable columns
- For card views, add frontmatter with `cssclasses: [cards, cards-1-1, hide-properties]`

### CSS Customization

Custom CSS snippets go in `WineCellar/.obsidian/snippets/`. Current snippet:
- `hide-properties.css` - Hides frontmatter in reading view for card displays

## Git Workflow

Standard git operations. Current tracked changes typically include:
- Wine notes in `1_Wines/`
- Collection view updates in `0_Collection/`
- Plugin configuration changes in `.obsidian/`
- Template modifications in `9_Templates/`

Note: Wine label images in `1_Wines/labels/` subdirectories are binary files.
