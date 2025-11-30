# Bourbon
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type === "Bourbon" && p.Inventory > 0)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey","Distiller", "Name", "Age", "Proof", "Region-State", "Stars", "Total Score", "Value for Money", "Inventory", "Purchase Source", "Price"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	p.Distiller,
	p.WhiskeyName,
	p.AgeStatement,
	p.Proof,
	p["Region-State"],
	await f(dv, p, "Stars", {options: {showAddField: true}}),
	p["Total Score"],
	p.ValueForMoney,
	await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
	p.PurchaseSource,
	p.Price,
	])
))

```

---
# Rye Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type === "Rye Whiskey" && p.Inventory > 0)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey","Distiller", "Name", "Age", "Proof", "Region-State", "Stars", "Total Score", "Value for Money", "Inventory", "Purchase Source", "Price"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	p.Distiller,
	p.WhiskeyName,
	p.AgeStatement,
	p.Proof,
	p["Region-State"],
	await f(dv, p, "Stars", {options: {showAddField: true}}),
	p["Total Score"],
	p.ValueForMoney,
	await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
	p.PurchaseSource,
	p.Price,
	])
))

```

---
# Tennessee Whiskey
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type === "Tennessee Whiskey" && p.Inventory > 0)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey","Distiller", "Name", "Age", "Proof", "Region-State", "Stars", "Total Score", "Value for Money", "Inventory", "Purchase Source", "Price"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	p.Distiller,
	p.WhiskeyName,
	p.AgeStatement,
	p.Proof,
	p["Region-State"],
	await f(dv, p, "Stars", {options: {showAddField: true}}),
	p["Total Score"],
	p.ValueForMoney,
	await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
	p.PurchaseSource,
	p.Price,
	])
))

```

---
# Other Whiskeys
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Whiskeys"')
	.filter(p => p.Type && !["Bourbon", "Rye Whiskey", "Tennessee Whiskey"].includes(p.Type) && p.Inventory > 0)
	.sort((a, b) => (b["Total Score"] ?? 0) - (a["Total Score"] ?? 0)); // Descending

dv.table(["Bottle","Whiskey","Distiller", "Name", "Type", "Age", "Proof", "Region-State", "Stars", "Total Score", "Value for Money", "Inventory", "Purchase Source", "Price"],
await Promise.all(pages.map(async p => [
	p.BottleImage,
	p.file.link,
	p.Distiller,
	p.WhiskeyName,
	p.Type,
	p.AgeStatement,
	p.Proof,
	p["Region-State"],
	await f(dv, p, "Stars", {options: {showAddField: true}}),
	p["Total Score"],
	p.ValueForMoney,
	await f(dv, p, "Inventory", {options: {alwaysOn: true, showAddField: true}}),
	p.PurchaseSource,
	p.Price,
	])
))

```
