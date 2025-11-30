### Rebuy
```dataview
table link(BottleImage,"100") as Bottle, Distiller, WhiskeyName as Name, Type, AgeStatement as Age, Year, Region-State, Proof, Price, Buy
from "1_Whiskeys"
where Buy > 0 and BottleImage
sort Region-State asc, "Total Score" desc
```

### New Whiskey
```dataview
table link(BottleImage,"100") as Bottle, Distiller, WhiskeyName as Name, Type, AgeStatement as Age, Year, Region-State, Proof, Price, Buy
from "1_Whiskeys"
where Buy > 0 and !BottleImage
sort Region-State asc, "Total Score" desc
```
