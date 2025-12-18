# White wines
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average 100pt score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Wines/' + bottleName + '"')
		.where(p => p.fileClass === "Wine Tasting")
		.array();
	if (tastings.length === 0) return "-";
	const total = tastings.reduce((sum, t) => sum + (t["100pt Scale"] ?? 0), 0);
	return (total / tastings.length).toFixed(0);
}

const pages = dv.pages('"1_Wines"')
	.filter(p => p.Type === "White wine");

// Create rows with calculated scores
const rows = await Promise.all(pages.map(async p => {
	const avgScore = getAverageScore(dv, p.file.name);
	return [
		p.Label,
		p.file.link,
		p.Winemaker,
		p.WineName,
		p.Variety,
		p.Vintage,
		p["Country-Region"],
		await f(dv, p, "Stars", {options: {showAddField: true}}),
		avgScore,
		p.ValueForMoney,
		await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
		p.PurchaseSource,
		p.Price,
		await f(dv, p, "Buy", {options: {alwaysOn: true, showAddField: true}})
	];
}));

// Sort by average score (descending)
rows.sort((a, b) => {
	const scoreA = a[8] === "-" ? 0 : parseFloat(a[8]);
	const scoreB = b[8] === "-" ? 0 : parseFloat(b[8]);
	return scoreB - scoreA;
});

dv.table(["Label","Wine","Winemaker", "Name", "Variety", "Vintage", "Country-Region", "Stars", "Avg Score", "Value for Money", "Inventory", "Purchase Source", "Price", "Buy"], rows);

```

---
# Red wines
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average 100pt score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Wines/' + bottleName + '"')
		.where(p => p.fileClass === "Wine Tasting")
		.array();
	if (tastings.length === 0) return "-";
	const total = tastings.reduce((sum, t) => sum + (t["100pt Scale"] ?? 0), 0);
	return (total / tastings.length).toFixed(0);
}

const pages = dv.pages('"1_Wines"')
	.filter(p => p.Type === "Red wine");

// Create rows with calculated scores
const rows = await Promise.all(pages.map(async p => {
	const avgScore = getAverageScore(dv, p.file.name);
	return [
		p.Label,
		p.file.link,
		p.Winemaker,
		p.WineName,
		p.Variety,
		p.Vintage,
		p["Country-Region"],
		await f(dv, p, "Stars", {options: {showAddField: true}}),
		avgScore,
		p.ValueForMoney,
		await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
		p.PurchaseSource,
		p.Price,
		await f(dv, p, "Buy", {options: {alwaysOn: true, showAddField: true}})
	];
}));

// Sort by average score (descending)
rows.sort((a, b) => {
	const scoreA = a[8] === "-" ? 0 : parseFloat(a[8]);
	const scoreB = b[8] === "-" ? 0 : parseFloat(b[8]);
	return scoreB - scoreA;
});

dv.table(["Label","Wine","Winemaker", "Name", "Variety", "Vintage", "Country-Region", "Stars", "Avg Score", "Value for Money", "Inventory", "Purchase Source", "Price", "Buy"], rows);

```
