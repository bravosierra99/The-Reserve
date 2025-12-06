```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

const pages = dv.pages('"1_Wines"')
	.sort((a, b) => (b["TotalScore"] ?? 0) - (a["TotalScore"] ?? 0)); // Descending




dv.table(["Wine","Winemaker", "Name", "Variety", "Vintage", "Appearance", "Aroma", "Taste", "Aftertaste","Overall","Total Score","Tasting note"],
await Promise.all(pages.map(async p => {
      const file = app.vault.getAbstractFileByPath(p.file.path);
	const raw = await app.vault.cachedRead(file);


      const heading = "## Tasting note";
      const regexStart = new RegExp(`^${heading}\\s*$`, "m");
      const startIndex = raw.search(regexStart);
console.log("RAW CONTENT:", raw);
console.log("START INDEX:", startIndex);
console.log("REGEX:", regexStart);

      let notes = "";
      if (startIndex !== -1) {
      console.log("inside if");
        const after = raw.slice(startIndex).split("\n").slice(1).join("\n");
        console.log("AFTER:",after);
        const nextIndex = after.search(/^##\s+/m);
          console.log("Next:",nextIndex);
        notes = nextIndex === -1 ? after : after.slice(0, nextIndex);
        console.log("NOTES2:",notes);
        notes = notes.replace(heading, "").trim();
        console.log("NOTES3:",notes);
      }
      console.log("NOTES:",notes);

	return[
		p.file.link, 
		p.Winemaker,
		p.WineName,
		p.Variety,
		p.Vintage,
		p.Appearance,
		p.Aroma,
		p.Taste,
		p.Aftertaste,
		p.Overall,
		p["Total Score"],
		notes
		];
	})
))

```
