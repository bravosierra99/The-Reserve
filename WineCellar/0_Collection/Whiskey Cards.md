---
cssclasses:
  - cards
  - cards-1-1
  - hide-properties
---

# Bourbon
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return 0;
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return total / tastings.length;
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type && p.Type.toLowerCase().includes("bourbon") && p.BottleImage);

// Sort by average score (descending)
const sortedPages = pages.sort((a, b) => getAverageScore(dv, b.file.name) - getAverageScore(dv, a.file.name));

dv.table(["Bottle","Whiskey"],
await Promise.all(sortedPages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```

---
# Rye Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return 0;
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return total / tastings.length;
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type === "Rye Whiskey" && p.BottleImage);

// Sort by average score (descending)
const sortedPages = pages.sort((a, b) => getAverageScore(dv, b.file.name) - getAverageScore(dv, a.file.name));

dv.table(["Bottle","Whiskey"],
await Promise.all(sortedPages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```

---
# Tennessee Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return 0;
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return total / tastings.length;
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type === "Tennessee Whiskey" && p.BottleImage);

// Sort by average score (descending)
const sortedPages = pages.sort((a, b) => getAverageScore(dv, b.file.name) - getAverageScore(dv, a.file.name));

dv.table(["Bottle","Whiskey"],
await Promise.all(sortedPages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```

---
# Other Whiskeys
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return 0;
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return total / tastings.length;
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type && !p.Type.toLowerCase().includes("bourbon") && !["Rye Whiskey", "Tennessee Whiskey"].includes(p.Type) && p.BottleImage);

// Sort by average score (descending)
const sortedPages = pages.sort((a, b) => getAverageScore(dv, b.file.name) - getAverageScore(dv, a.file.name));

dv.table(["Bottle","Whiskey"],
await Promise.all(sortedPages.map(async p => [
	p.BottleImage,
	p.file.link,
	])
))

```
