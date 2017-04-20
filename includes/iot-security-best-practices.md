# <a name="internet-of-things-security-best-practices"></a>Najbolji postupci za Internet stvari sigurnosti

Zbog sigurnosti programa Internet od stvari (IoT) Infrastruktura zahtijeva stroge sigurnost u dubina strategije. Ovaj strategije zahtijeva sigurne podataka oblaka, zaštitio integritet podataka u tranzitu putem javnog Interneta i sigurno Dodjela uređaji. Svaki sloj izgrađuje veće sigurnosti kontrolu u cjelokupni infrastrukture.

## <a name="secure-an-iot-infrastructure"></a>Sigurne Infrastruktura IoT

Ovaj sigurnosni u dubina strategije možete razvila i izvršeno aktivni sudjelovanje uključenih proizvodnje, razvoj i uvođenje IoT uređaji i Infrastruktura različite reproduktori. Sljedeće je podroban opis te reproduktori.  

- **Proizvođač hardvera IoT/integrator**: obično su proizvođači hardvera IoT uvodi, integrators Montaža hardver iz različitih proizvođača ili dobavljači davanja hardver za IoT implementacije proizvedeno ili integrirani drugi Dobavljači.
- **IoT projektant rešenja**: Razvojni inženjer rješenja obično vrši razvoj IoT rješenja. Ovog programera možda dio interni tima ili integrator sustava (SI) specializing u ovu aktivnost. IoT projektant rešenja možete razviti razne komponente rješenja IoT ispočetka, integrirati razne komponente off-the-shelf ili Otvori izvor ili prihvaćaju konfiguriranih rješenja s pomoćne adaptacijski.
- **Deployer IoT rješenja**: nakon an IoT rešenje je razvijeno, treba uvesti u polju. To uključuje uvođenja hardver, interconnection uređaji i uvođenje rješenja hardverske uređaje ili oblaka.
- **Operator rješenje IoT**: nakon IoT rješenje uvedeno, zahtijeva Dugoročne operacije, nadzor nadogradnji i održavanje. To možete učiniti interni tima koji se sastoji od specijalistima tehnologija informacije, operacije hardver i održavanja timovima i domene specijalistima tko pratiti ispravan ponašanje cjelokupnog Infrastruktura IoT.

Odjeljci koji slijede sadrže Najbolji postupci za svaku od tih reproduktori pomoći za razvoj, uvođenje i raditi sigurne Infrastruktura IoT.

## <a name="iot-hardware-manufacturerintegrator"></a>Proizvođač hardvera IoT/integrator

Sljedeće su Najbolji postupci za proizvođače hardvera IoT i integrators hardvera.

- **Opseg hardver da biste minimalne preduvjete**: dizajna hardver treba uključivati minimalne značajke potreban za operaciju hardver i ništa više. Primjer je uključiti USB priključke samo ako je potrebno za operaciju uređaj. Dodatne značajke otvorite uređaj vektori neželjene napada koji trebaju se izbjegavati.
- **Provjerite hardver mijenjati dokaz**: izgraditi mehanizme za otkrivanje fizičke neovlašteno mijenjanje, poput otvaranja naslovnice uređaj ili uklanjanje dijela uređaj. Te mijenjati signale može biti dio toka podataka prenijeti oblak nije upozorenja operatore ovih događaja.
- **Oko sigurne hardver izgradnju**: Ako TPR dopušta, izgraditi sigurnosne značajke poput pohranu sigurne i šifrirane ili pokretanje funkcionalnost temelji na Trusted Platform Module (TPM). Provjerite te značajke uređaji više sigurne i zaštitu cjelokupnog Infrastruktura IoT.
- **Provjerite nadogradnji sigurne**: firmver nadogradnji tijekom vijeka uređaj su inevitable. Gradi uređaji s sigurne putove za nadogradnje i kriptografskim kontrolu verzije firmvera Dopusti će uređaj biti sigurno tijekom i nakon nadogradnje.

## <a name="iot-solution-developer"></a>Razvojni inženjer rješenja IoT

Najbolji postupci za IoT rješenje programeri su:

- **Slijedite sigurne software development methodology**: razvoj sigurne softver zahtijeva tlo gore misle o sigurnosti od početka projekta sve do njegove implementaciju i testiranje uvođenja. Izbori platforme, jezika i alati su sve Lobirane s ovom methodology. Životni ciklus razvoj sigurnost Microsoft pruža detaljne pristup za izgradnju sigurne softvera.
- **Odaberite Otvori izvor softver s kupcima**: otvaranje izvora softver pruža priliku za brzo razvoj rješenja. Prilikom odabira si Otvori izvor softvera, razmotrite razine aktivnosti zajednice za svaku komponentu Otvori izvor. Aktivni zajednice osigurava podržan je softver i problemi su otkrivene i adresirana. Umjesto toga, prikrivanju i neaktivne Otvori izvor softver možda nije podržana i problemi vjerojatno neće biti moguće otkriti.
- **Integrate s kupcima**: na granicu biblioteke i API-postoji mnogo softver sigurnosne pogreške. Možda još uvijek dostupne putem API sloj funkcionalnost koja možda nije potreban za trenutni uvođenja. Da biste osigurali cjelokupnog sigurnosti, svakako provjerite sva sučelja komponente integrirani u tijeku za sigurnosne pogreške.      

## <a name="iot-solution-deployer"></a>Deployer IoT rješenja

Najbolji postupci za IoT rješenje deployers su:

- **Sigurno uvođenje hardver**: IoT implementacije može zahtijevati hardver uveden u nesigurnom mjesta kao javnu razmake ili unsupervised regionalne sheme. U takvim situacijama, provjerite je li hardver uvođenja mijenjati dokaz u najvećoj mogućoj mjeri. Ako USB ili druge priključke dostupne su na hardver, osigurajte da oni su pokriveni sigurno. Mnogi napada vektori možete koristiti kao ulazne točke.
- **Zadrži provjere autentičnosti tipke sigurnom**: tijekom uvođenja, svaki uređaj zahtijeva uređaju ID i pridružene provjere autentičnosti tipke generira servis oblaka. Zadrži tih ključeva fizički sigurnom čak i nakon uvođenja. Bilo koju tipku compromised mogu se po zlonamjernih uređaj maskirati kao postojeći uređaj.

## <a name="iot-solution-operator"></a>Operator rješenje IoT

Najbolji postupci za IoT rješenje operatore su:

- **Redovito ažurirajte sustav**: osigurali su nadograditi na najnovije verzije uređaj operacijske sustave i sve upravljačke programe uređaja. Ako uključite Automatska ažuriranja u Windows 10 (IoT ili drugim JSK) Microsoft zadržava ga ažurni, pružanje sigurne operacijskog sustava za IoT uređaje. Zadržavanje druge operacijske sustave (primjerice Linux) ažurni pomaže osigurati da također nisu zaštićeni protiv zlonamjernog napada.
- **Zaštita protiv zlonamjernih aktivnosti**: Ako dozvoljava operacijski sustav, instalirajte najnovije mogućnosti protuvirusne i antimalware na svaki uređaj operacijski sustav. To može pomoći umanjiti većinu vanjskih prijetnji. Većina Moderna operacijski sustavi od prijetnji možete zaštititi uzimajući odgovarajuće korake.
- **Često nadzora**: nadzor IoT infrastruktura za probleme vezane uz sigurnost je ključ prilikom odgovaranja incidenata sigurnosti. Većina operacijski sustavi pružaju zapisivanje ugrađene događaja koji treba biti često pregledane da provjerite došlo je do bez ugrožavanja sigurnosti. Informacije o reviziji može poslati kao zasebna telemetry strujanje servisa oblak gdje ga mogu analizirati.
- **Fizički zaštiti Infrastruktura IoT**: najgoreg sigurnosnim napadima protiv Infrastruktura IoT su pokrenuti pomoću fizičke pristup uređajima. Jedan važne sigurnost praksa je zaštiti protiv zlonamjernih koristi USB priključke i drugi fizički pristup. Ključ uncovering breaches možda je došlo do zapisivanje fizičke pristupa, kao što je USB priključak koristi. Ponovno, Windows 10 (IoT i drugim JSK) omogućuje detaljne zapisivanje tih događaja.
- **Zaštita oblak vjerodajnice**: oblak koristi za konfiguriranje i operacijski IoT implementacije vjerodajnice za provjeru autentičnosti su eventualno najlakše dobiti pristup i ugroziti IoT sustava. Zaštiti vjerodajnice često promjenom lozinku i ne smije koristiti ove vjerodajnice na javnim računalima.

Mogućnosti za različite uređaje IoT razlikuju. Neki uređaji možda računalima uobičajene radne površine operacijske sustave, a neki uređaji možda pokrenut operacijski sustavi vrlo Svijetli simbol. Najbolji postupci sigurnost opisane prethodno možda primjenjive ti uređaji u različitoj mjeri. Dani, slijediti treba dodatne sigurnosti i uvođenje najbolja iskustva od proizvođača tih uređaja.

Neki uređaji naslijeđenih i ograničenog možda imaju dizajniran posebno za IoT implementacije. Ti uređaji možda nemaju mogućnost šifriranje podataka, povežite se s Internetom ili pružaju dodatne nadzor. U tim slučajevima možete pristupnik Moderna i sigurne polje zbrajanja podataka iz naslijeđenih uređaja i pružaju sigurnosti potrebnu za povezivanje tih uređaja putem Interneta. Polje pristupnici možete pružiti sigurne provjere autentičnosti, Pregovori šifrirane sesije, primka naredbe iz oblaka i mnoge druge sigurnosne značajke.
