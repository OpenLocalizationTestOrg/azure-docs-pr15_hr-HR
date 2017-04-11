## <a name="dynamic-service-plan"></a>Servis za dinamičku Plan

U u dinamičku servisa planiranje, funkcija aplikacija će se dodijeliti s instancom aplikacije (opis funkcije). Ako je potrebno više instanci dodat će se dinamički.
Slučajevima mogu obuhvaćati preko više računalnih resursa, upućivanje svih pogodnosti dostupna Azure infrastrukture. Nadalje, vaš funkcije će se pokrenuti paralelno minimiziranje Ukupno vrijeme potrebno za obradu zahtjeva. Vrijeme izvođenja za svaku funkciju dodaje prema gore u sekundama i pridružuje koji sadrže aplikacija (opis funkcije). S troška driven broj instanci, njihove veličine memorije i ukupno vrijeme izvođenja mjeri u sekundama gigabajta. Ovo je izvrstan mogućnost ako vašim potrebama računalnim Povremeni ili vaše vrijeme za zadatak obično kratkog obliku omogućuje vam da samo platiti za resurse računalnim kada zapravo su koristi.   

### <a name="memory-tier"></a>Razina memorije

Ovisno o funkcija mora možete odabrati memorija potrebni za pokretanje ih u aplikaciji funkcija (spremnik funkcije).
Mogućnosti veličina memorije razlikovati od **128 MB do 1536 MB**. Veličine odabranog memorije odgovara rad skup potrebne za sve funkcije koje su dio aplikacije (opis funkcije). Ako kodu zahtijeva više memorije od odabrane veličine, funkcija instanci aplikacije će biti zatvoren zbog slobodne memorije.

### <a name="scaling"></a>Promjena veličine

Platforme Azure funkcije će rezultirati promet potrebama, ovisno o konfigurirani okidača kada skaliranje prema gore ili dolje. Preciznosti promjene veličine je funkcija aplikacija. U ovom slučaju skaliranje znači dodavanje više instanci aplikacije (opis funkcije). Inversely tijekom promet prema dolje, funkcija aplikacije instanci su onemogućeno naposljetku skaliranje prema dolje na nulu kada nema koristite.  

### <a name="resource-consumption-and-billing"></a>Potrošnje resursa i naplata

U u dinamičnu Dodjela resursa za način rada činiti drukčije od standardne aplikacije servisa za planiranje, stoga modela potrošnje je i drugi dopuštanja u modelu "Plaćanje po korištenju". Potrošnje će se prijavljivati po aplikacije funkcija samo za vrijeme kad je izvršena kod.  
Ga je izračunati množenjem veličina memorije (u GB) tako da ukupnu količinu vremena izvođenja (u sekundama) za sve funkcije izvode u aplikaciji te funkcije. Jedinice potrošnje bit će **s GB (gigabajta sekunde)**.