# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Obsidian vault for tracking and managing a personal wine and whiskey collection. The vault uses a structured metadata system powered by Obsidian plugins to create tasting notes, track inventory, maintain shopping lists, and visualize labels in various views.

**Key Architectural Difference:**
- **Wines**: Single note per wine with integrated product info and tasting notes
- **Whiskeys**: Separated architecture with bottle notes (product info) and multiple tasting notes per bottle

## Architecture

### Folder Structure

- `WineCellar/` - Main Obsidian vault
  - `0_Collection/` - Collection views and queries (wines and whiskeys)
  - `1_Wines/` - Individual wine tasting notes (one file per wine)
  - `1_Whiskeys/` - Whiskey bottles and tastings (nested structure)
    - `BottleName/` - Folder per bottle
      - `BottleName.md` - Bottle note with product info
      - `Tasting-YYYY-MM-DD-TasterName.md` - Individual tasting notes
  - `8_FileClass/` - Metadata Menu FileClass definitions (Wine.md, Whiskey.md, Tasting.md)
  - `9_Templates/` - QuickAdd templates for creating new notes
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

## Working with Whiskey Notes

### Whiskey Architecture

Whiskeys use a **separated architecture** with two FileClasses:
- **Whiskey (Bottle)**: Product information (distiller, proof, age, price, etc.)
- **Tasting**: Individual tasting sessions with per-taster scoring

Each bottle has its own folder in `1_Whiskeys/BottleName/` containing:
- The bottle note (`BottleName.md`)
- Multiple tasting notes (`Tasting-YYYY-MM-DD-TasterName.md`)

### Creating New Whiskey Bottles

Use QuickAdd template "Whiskey Bottle" which prompts for:
1. Distiller name
2. Whiskey name
3. Year

File is created in `1_Whiskeys/BottleName/BottleName.md` with auto-generated filename and frontmatter.

### Creating New Tastings

**Recommended Method**: Use the "Add New Tasting" button on any bottle note. This:
- Creates tasting in the correct folder automatically
- Pre-fills LinkedBottle field
- Prompts for: Date, Taster Name, Days from crack, Fill level
- Opens the new tasting note for scoring and notes

**Alternative**: Use QuickAdd template "Whiskey Tasting" (requires manual folder selection)

### Whiskey Field Conventions

**Bottle Fields:**
- **Type**: Bourbon, Rye Whiskey, Tennessee Whiskey, etc.
- **Proof**: Numeric (0-200, step 0.1)
- **AgeStatement**: Age in years (not used in filename)
- **Year**: Release year (used in filename like Vintage for wine)
- **MashBill**: Grain composition
- **BarrelType**: Barrel aging information
- **BottleOpenedDate**: Date when bottle was opened
- **Stars/ValueForMoney**: Cross-beverage comparison (same as wine)

**Tasting Fields:**
- **Date**: Tasting date
- **TasterName**: Who tasted it
- **DaysFromCrack**: Days since bottle was opened
- **FillLevel**: Bottle fill percentage (0-100)
- **Nose**: Aroma score (0-10, step 0.1)
- **Palate**: Taste score (0-10, step 0.1)
- **Finish**: Aftertaste score (0-10, step 0.1)
- **Overall**: Overall impression (0-10, step 0.1)
- **TotalScore**: Auto-calculated sum (max 40)

### Whiskey Collection Views

Whiskey views calculate **average scores from all tastings**:
- Filter bottles with `p.fileClass === "Whiskey"`
- Get tastings with `dv.pages('"1_Whiskeys/' + bottleName + '"').where(p => p.fileClass === "Tasting")`
- Calculate averages across all tasting scores
- Display "Avg Score" instead of individual scores

### Bottle Note Features

Each bottle note includes:
- **Product Details section**: Editable metadata fields
- **Tasting Notes Summary**: DataviewJS queries showing:
  - Per-taster average scores (grouped by TasterName)
  - Table of all tasting sessions with links
- **Add New Tasting button**: Creates new tasting in one click

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

### Framework vs Data Separation

The repository uses `.gitignore` to separate framework from personal data:

**Tracked (Framework):**
- FileClass definitions (`8_FileClass/`)
- Templates (`9_Templates/`)
- Collection views (`0_Collection/`)
- Plugin configurations (`.obsidian/plugins/`)

**Ignored (Personal Data):**
- Wine tasting notes (`1_Wines/`)
- Whiskey bottles and tastings (`1_Whiskeys/`)

This allows the framework to be shared publicly while keeping personal tasting data private. Users can clone the repo to get the structure without your personal collection.

### Committing Changes

When modifying framework files, commit as usual. Personal data in `1_Wines/` and `1_Whiskeys/` will not be staged automatically.

To commit personal data for backup purposes, use `git add -f` to force-add ignored files.
