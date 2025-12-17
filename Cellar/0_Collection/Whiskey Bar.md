# Bourbon
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return "-";
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return (total / tastings.length).toFixed(1);
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type && p.Type.toLowerCase().includes("bourbon") && p.Inventory > 0);

// Create rows with calculated scores
const rows = await Promise.all(pages.map(async p => {
	const avgScore = getAverageScore(dv, p.file.name);
	return [
		p.BottleImage,
		p.file.link,
		p.Distiller,
		p.WhiskeyName,
		p.AgeStatement,
		p.Proof,
		p["Region-State"],
		await f(dv, p, "Stars", {options: {showAddField: true}}),
		avgScore,
		p.ValueForMoney,
		await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
		p.PurchaseSource,
		p.Price,
	];
}));

// Sort by average score (descending)
rows.sort((a, b) => {
	const scoreA = a[8] === "-" ? 0 : parseFloat(a[8]);
	const scoreB = b[8] === "-" ? 0 : parseFloat(b[8]);
	return scoreB - scoreA;
});

dv.table(["Bottle","Whiskey","Distiller", "Name", "Age", "Proof", "Region-State", "Stars", "Avg Score", "Value for Money", "Inventory", "Purchase Source", "Price"], rows);

```

---
# Rye Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return "-";
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return (total / tastings.length).toFixed(1);
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type === "Rye Whiskey" && p.Inventory > 0);

// Create rows with calculated scores
const rows = await Promise.all(pages.map(async p => {
	const avgScore = getAverageScore(dv, p.file.name);
	return [
		p.BottleImage,
		p.file.link,
		p.Distiller,
		p.WhiskeyName,
		p.AgeStatement,
		p.Proof,
		p["Region-State"],
		await f(dv, p, "Stars", {options: {showAddField: true}}),
		avgScore,
		p.ValueForMoney,
		await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
		p.PurchaseSource,
		p.Price,
	];
}));

// Sort by average score (descending)
rows.sort((a, b) => {
	const scoreA = a[8] === "-" ? 0 : parseFloat(a[8]);
	const scoreB = b[8] === "-" ? 0 : parseFloat(b[8]);
	return scoreB - scoreA;
});

dv.table(["Bottle","Whiskey","Distiller", "Name", "Age", "Proof", "Region-State", "Stars", "Avg Score", "Value for Money", "Inventory", "Purchase Source", "Price"], rows);

```

---
# Tennessee Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return "-";
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return (total / tastings.length).toFixed(1);
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type === "Tennessee Whiskey" && p.Inventory > 0);

// Create rows with calculated scores
const rows = await Promise.all(pages.map(async p => {
	const avgScore = getAverageScore(dv, p.file.name);
	return [
		p.BottleImage,
		p.file.link,
		p.Distiller,
		p.WhiskeyName,
		p.AgeStatement,
		p.Proof,
		p["Region-State"],
		await f(dv, p, "Stars", {options: {showAddField: true}}),
		avgScore,
		p.ValueForMoney,
		await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
		p.PurchaseSource,
		p.Price,
	];
}));

// Sort by average score (descending)
rows.sort((a, b) => {
	const scoreA = a[8] === "-" ? 0 : parseFloat(a[8]);
	const scoreB = b[8] === "-" ? 0 : parseFloat(b[8]);
	return scoreB - scoreA;
});

dv.table(["Bottle","Whiskey","Distiller", "Name", "Age", "Proof", "Region-State", "Stars", "Avg Score", "Value for Money", "Inventory", "Purchase Source", "Price"], rows);

```

---
# Other Whiskeys
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

// Helper function to get average score from tastings
function getAverageScore(dv, bottleName) {
	const tastings = dv.pages('"1_Whiskeys/' + bottleName + '"')
		.where(p => p.fileClass === "Tasting");
	if (tastings.length === 0) return "-";
	const total = tastings.reduce((sum, t) => sum + (t.TotalScore ?? 0), 0);
	return (total / tastings.length).toFixed(1);
}

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.fileClass === "Whiskey" && p.Type && !p.Type.toLowerCase().includes("bourbon") && !["Rye Whiskey", "Tennessee Whiskey"].includes(p.Type) && p.Inventory > 0);

// Create rows with calculated scores
const rows = await Promise.all(pages.map(async p => {
	const avgScore = getAverageScore(dv, p.file.name);
	return [
		p.BottleImage,
		p.file.link,
		p.Distiller,
		p.WhiskeyName,
		p.Type,
		p.AgeStatement,
		p.Proof,
		p["Region-State"],
		await f(dv, p, "Stars", {options: {showAddField: true}}),
		avgScore,
		p.ValueForMoney,
		await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
		p.PurchaseSource,
		p.Price,
	];
}));

// Sort by average score (descending)
rows.sort((a, b) => {
	const scoreA = a[8] === "-" ? 0 : parseFloat(a[8]);
	const scoreB = b[8] === "-" ? 0 : parseFloat(b[8]);
	return scoreB - scoreA;
});

dv.table(["Bottle","Whiskey","Distiller", "Name", "Type", "Age", "Proof", "Region-State", "Stars", "Avg Score", "Value for Money", "Inventory", "Purchase Source", "Price"], rows);

```
