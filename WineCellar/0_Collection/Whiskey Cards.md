---
cssclasses:
  - cards
  - cards-1-1
  - hide-properties
---

# Bourbon
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type && p.Type.toLowerCase().includes("bourbon") && p.BottleImage)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```

---
# Rye Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type === "Rye Whiskey" && p.BottleImage)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```

---
# Tennessee Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type === "Tennessee Whiskey" && p.BottleImage)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```

---
# Other Whiskeys
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type && !p.Type.toLowerCase().includes("bourbon") && !["Rye Whiskey", "Tennessee Whiskey"].includes(p.Type) && p.BottleImage)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```
