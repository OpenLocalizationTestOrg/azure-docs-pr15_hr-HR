
Kod za sve funkcije u aplikaciji navedeni funkcija nalazi se u korijensku mapu koja sadrži datoteku za konfiguriranje glavnog računala i jednu ili više podmapa, od kojih svaka sadrže kod za zasebnom funkcija, kao što prikazuje sljedeći primjer

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

*Host.json* datoteka sadrži neke konfiguracije specifičnih za izvođenje, a nalazi se u korijenskoj mapi aplikaciju (opis funkcije). Informacije o postavkama koje su dostupne potražite u članku [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) u spremištu wiki WebJobs.Script.

Svaku funkciju sadrži mapu koja sadrži jednu ili više datoteka kod, function.json konfiguraciju i druge ovisnosti.