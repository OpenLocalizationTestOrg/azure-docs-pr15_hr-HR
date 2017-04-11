<properties
   pageTitle="Azure AD Connect sinkronizacije: objašnjenje arhitektura | Microsoft Azure"
   description="U ovoj se temi opisuju arhitektura Azure AD Connect sinkronizaciju i objašnjava izraze koji se koriste."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect sinkronizacije: objašnjenje arhitektura
U ovoj se temi objašnjava osnovni arhitektura za Azure AD Connect sinkronizaciju. U mnogo se aspekti je slična njegov prethodnike MIIS 2003, ILM 2007 i FIM 2010. Azure AD Connect sinkronizacija je proizvod tih tehnologija. Ako ste upoznati s bilo kojom od tih tehnologija ranije, sadržaja u ovoj se temi bit će vam poznata i. Ako ste novi korisnik sinkronizacije, zatim članak namijenjen vama. Nije no preduvjet znati pojedinosti u ovoj se temi biti uspješno u mijenjanju prilagodbe za sinkronizaciju Azure AD Connect (pod nazivom modula za sinkroniziranje u ovoj temi).

## <a name="architecture"></a>Arhitektura
Modul sinkronizaciju stvara integrirani prikaz objekata pohranjenih u više izvora povezanih podataka i upravlja identiteta informacije u tim izvorima podataka. Integrirano prikaz ovisi o informacije o identitetu dohvaćeni iz izvora povezanih podataka i skup pravila koja određuje način obrade te podatke.

### <a name="connected-data-sources-and-connectors"></a>Povezani izvora podataka i poveznika
Sinkroniziranje modul obrađuje identiteta podatke iz spremišta podataka, kao što je Active Directory ili baze podataka SQL Server. Svaki spremište podataka koje organizira njegove podatke u bazu podataka obliku i koje pruža metode standardne za pristup podacima je potencijalne prijedlog izvora podataka za sinkronizaciju modula. Spremišta podataka koji se sinkroniziraju modula za sinkroniziranje nazivaju se **povežete s mrežom izvora podataka** ili **povezani direktorija** (CD-a).

Modul za sinkronizaciju encapsulates interakciju s izvora povezanih podataka u modulu naziva **poveznik**. Svaka vrsta izvora povezanih podataka ima određene poveznik. Poveznik prevodi traženih operacija u obliku koji možete koristiti izvora povezanih podataka.

Poveznika upućivati pozive API-JA za razmjenu informacija identiteta (čitanje i pisanje) izvora povezanih podataka. Također je moguće dodati prilagođene poveznik pomoću framework extensible povezivanje. Sljedeća ilustracija prikazuje kako poveznik povezuje izvor podataka povezanih s modula za sinkroniziranje.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Možete u bilo kojem smjeru tijek podataka, ali ga nije moguće tijek u oba smjera istodobno. Drugim riječima, poveznika može biti konfigurirano tako da omogućuje protok izvora povezanih podataka za sinkronizaciju modula ili iz modula za sinkroniziranje s izvorom podataka povezanih podataka, ali samo jednu od tih operacija mogu dogoditi na bilo kojem trenutku za jednog objekta i atributa. Smjer mogu biti različiti za različite objekte i za različite atribute.

Da biste konfigurirali poveznika, navedite vrste objekata koje želite sinkronizirati. Određivanje vrste objekata definira djelokrug objekata koji su uključeni u postupku sinkronizacije. Sljedeći je korak da biste odabrali atribute da biste sinkronizirali, koji se naziva s popisa atribut uključivanja. Postavke možete naknadno promijeniti bilo kojem trenutku u odgovoru promjene pravilima tvrtke. Kada koristite čarobnjak za instalaciju Azure AD Connect, umjesto vas konfigurirane ove postavke.

Da biste izvezli objekti izvora povezanih podataka, na popisu uvrštavanja atributa mora sadržavati najmanje atribute najmanji potreban da biste stvorili vrstu određenog objekta u izvoru podataka povezanih. Na primjer, atribut **sAMAccountName** morate uvrstiti na popisu uvrštavanja atributa da biste izvezli korisnički objekt servisa Active Directory jer se svi objekti korisnika u servisu Active Directory moraju sadržavati atribut **sAMAccountName** definirane. Ponovno Čarobnjak za instalaciju ne tu konfiguraciju umjesto vas.

Ako izvor povezanih podataka koristi strukturnih komponente, kao što su particije ili spremnika za organiziranje objekata, možete ograničiti područja u izvoru povezanih podataka koji se koriste za dani rješenja.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interna struktura prostora za naziv modul sinkronizacije
Prostor za naziv modul cijelu sinkronizaciju sastoji se od dva prostori za spremanje informacija za identitet. Dva prostori su:

- Poveznik razmak (CS)
- Metaverse (MV)

**Poveznik prostor** je pripremna područje koje sadrži reprezentacije određenu objekte iz izvora povezanih podataka i atribute naveden na popisu atributa uključivanja. Modula za sinkroniziranje koristi prostor poveznik da biste odredili što se promijenilo u izvoru podataka povezanih i stupnja dolazne promjene. Modula za sinkroniziranje koristi i razmak poveznik za faze odlazne promjene za izvoz u izvora povezanih podataka. Modul za sinkronizaciju zadržava distinct poveznik prostora kao pripremna područje za svaki poveznik.

Pomoću pripremna područje modula za sinkroniziranje ostaje neovisno o izvora povezanih podataka i ne utječu na njihove dostupnosti i pristupačnost. Zbog toga provodite identiteta informacije u bilo kojem trenutku pomoću podataka u području pripremna. Modul za sinkronizaciju možete zatražiti samo promjene unutar izvora povezanih podataka od zadnji komunikacije sesija prekinuti ili automatske out samo promjene identiteta informacije koje izvora povezanih podataka je još primio, koji smanjuje mrežni promet između modula za sinkroniziranje i izvora povezanih podataka.

Osim toga, modula za sinkroniziranje pohranjuje informacije o statusu o svim objektima faze li prostor poveznik. Primljena nove podatke, modula za sinkroniziranje uvijek procjenjuje li se već sinkronizira podatke.

**Metaverse** je spremište koje sadrži Zbrojeno identiteta podatke iz više izvora povezanih podataka, koja omogućuje jedinstveni prikaz globalni, integrirano svih objekata kombinirane. Objekti Metaverse stvaraju na informacije o identitetu koja se dohvaća iz izvora povezanih podataka i skup pravila koja omogućuju vam prilagodbu sinkronizacija.

Sljedeća ilustracija prikazuje poveznik prostor naziva i naziva metaverse unutar modula za sinkroniziranje.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Sinkronizacija modul identiteta objekata
Objekti u modula za sinkroniziranje su reprezentacije ili objekata u izvoru podataka povezanih ili integrirani prikaza koji se sinkronizirati modul ima od tih objekata. Svaki objekt modul sinkronizaciju mora imati globalno jedinstveni identifikator (GUID). GUID pružaju integritet podataka i eksplicitnih odnosa među objektima.

### <a name="connector-space-objects"></a>Poveznik prostora objekata
Kada modula za sinkroniziranje komunicira s izvora povezanih podataka, čita informacije o identitetu u izvoru podataka povezanih i koristi te podatke za stvaranje hijerarhije objekt identiteta u prostoru poveznik. Ne možete stvarati ni izbrisati te objekte pojedinačno. Međutim, možete izbrisati ručno svi objekti na poveznik razmak.

Svi objekti na prostoru poveznik imaju dva atributa:

- Globalno jedinstveni identifikator (GUID)
- Razlikovni naziv (poznat i kao DN)

Ako izvora povezanih podataka dodjeljuje jedinstveni atribut objekta, zatim objekata u prostoru poveznik također može imati atribut sidra. Atribut sidro služi kao jedinstveni identifikator objekta u izvora povezanih podataka. Modul za sinkronizaciju koristi u sidro da biste pronašli odgovarajući prikaz taj objekt u izvoru podataka povezanih. Modul za sinkronizaciju pretpostavlja da sidra objekta nikad ne mijenja tijekom života objekt.

Mnoge poveznika koristiti poznate Jedinstveni identifikator za generiranje programa sidrenja automatski za svaki objekt prilikom uvoza. Ako, na primjer, poveznik za Active Directory koristi atribut **objectGUID** programa sidra. Za izvora povezanih podataka koji omogućuju jasno definirani Jedinstveni identifikator, možete odrediti sidro generacije kao dio konfiguracija Connector.

U tom slučaju na sidro ugrađeni iz jednog ili više jedinstvenih atribute objekta upišite, ni promjene, a to jedinstveno označava objekta u prostoru connector (na primjer, broj zaposlenika ili korisničkog ID-JA).

Objekt programa poveznik prostor može biti nešto od sljedećeg:

- Pripremna objekta
- Rezervirano mjesto

### <a name="staging-objects"></a>Pripremna objekata
Pripremna objekt predstavlja instancu vrste određenu objekata iz izvora povezanih podataka. Pripremna objekt osim GUID i Razlikovni naziv, uvijek ima vrijednost koja je navedena je vrsta objekta.

Pripremna objekte koji su uvijek uvezeni imati vrijednost za atribut sidrenja. Pripremna objekata koji upravo dodijeljen po modula za sinkroniziranje i upravo stvoren u izvoru podataka povezanih imati vrijednost za atribut sidrenja.

Pripremna objekata izvesti i trenutne vrijednosti atributa tvrtke i informacije potrebne za izvođenje postupka sinkronizacije modula za sinkroniziranje. Informacije o radu sa servisom sadrži oznake koje označavaju vrstu ažuriranja koja je kopirana bez postavljanja pripremna objekta. Ako pripremna objekt primila nove identiteta podatke iz izvora povezanih podataka koji još nije obrađen, objekt označit će se kao **na čekanju uvoz**. Ako pripremna objekt ima nove informacije identiteta koji nije još izvezena je izvor povezanih podataka, označene kao **na čekanju izvoz**.

Pripremna objekt može biti za uvoz ili izvoz objekta. Modul za sinkronizaciju stvara objekt uvoza pomoću objekta dobivene iz izvora povezanih podataka. Kad modula za sinkroniziranje prima informacije o postojanje novi objekt koji odgovara jednoj od odabranih u poveznik vrsta objekta, on stvara objekt uvoza prostor poveznik kao prikaz objekta u izvoru povezanih podataka.

Sljedeća ilustracija prikazuje uvoz objekta koji predstavlja objekta u izvora povezanih podataka.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Modula za sinkroniziranje stvara objekta izvoz pomoću informacija objekta u na metaverse. Izvoz objekata izvoze se izvora povezanih podataka tijekom sljedećeg sesije komunikacije. Iz perspektive modula za sinkroniziranje, izvoz objekata ne postoje u izvoru podataka povezanih još. Zbog toga atributa sidrenja za izvoz objekt nije dostupna. Nakon što ga prima objekt iz modula za sinkroniziranje, izvora povezanih podataka stvara jedinstvenu vrijednost atributa sidra objekta.

Sljedeća ilustracija prikazuje kako će se objekt izvoza stvoren pomoću identiteta informacije u na metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Modul za sinkronizaciju potvrđuje izvoz objekta prema reimporting objekta iz izvora povezanih podataka. Izvoz objekti postaju Uvoz objekata kada ih modula za sinkroniziranje primi tijekom sljedećeg uvoza iz tog izvora povezanih podataka.

### <a name="placeholders"></a>Rezervirana mjesta
Modul za sinkronizaciju koristi paušalni prostor naziva za pohranu objekte. Međutim, neke izvora povezanih podataka kao što su Active Directory pomoću hijerarhijskih prostora za naziv. Za pretvaranje informacije iz hijerarhijski polja naziva u plošnu prostor naziva, modula za sinkroniziranje koristi rezerviranih mjesta da biste sačuvali hijerarhije.

Svaki rezervirano mjesto predstavlja komponente (na primjer, je Organizacijska jedinica) objekta hijerarhijski naziv koji se ne uvoze u modula za sinkroniziranje, ali je potrebno za sastavljanje hijerarhijski naziv. Prilikom unosa praznine stvorio reference u izvoru podataka povezanih objekata koji su pripremna objekata u prostoru poveznik.

Modul za sinkronizaciju koristi i rezervirana mjesta za pohranu referencirani objekata koji još nisu uvezeni. Na primjer, ako je sinkronizacija je konfiguriran tako da obuhvaća atribut upravitelja za objekt *Abbie Spencer* i primljene vrijednost je objekt koji nije još uvezeni kao što su *CN = Lee Sperry, CN = korisnici, Kontroler = fabrikam, Kontroler = com*, Upravitelj su pohranjeni kao rezervirana mjesta na prostoru poveznik. Ako je objekt upravitelja kasnije uvoza objekt rezerviranog mjesta biti izbrisana pripremna objekt koji predstavlja Upravitelj.

### <a name="metaverse-objects"></a>Metaverse objekata
Metaverse objekt sadrži Zbrojeno prikaz te modula za sinkroniziranje sadrži pripremna objekata u prostoru poveznik. Modul za sinkronizaciju stvara metaverse objekata pomoću informacija u Uvoz objekata. Nekoliko objekata prostora poveznik moguće je povezati s metaverse jedan objekt, ali objekt programa poveznik prostor nije moguće povezati s više od jednog objekta metaverse.

Objekti Metaverse ne može ručno stvoriti ili izbrisati. Modul za sinkronizaciju automatski briše metaverse objekte koji nemaju veza na bilo koji objekt prostora poveznika u prostoru poveznik.

Da biste mapirali objekte unutar izvora povezanih podataka odgovarajuća vrsta objekta unutar na metaverse, modula za sinkroniziranje nudi extensible sheme pomoću unaprijed definiranih skupa vrste objekata i pridruženi atribute. Možete stvoriti nove vrste objekta i atributa metaverse objekte. Atributi može biti jednom vrijednosti ili s više vrijednosti, a vrste atributa može biti nizovi, reference, brojeva i Booleove vrijednosti.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Odnosi između pripremna i metaverse objekata
U modul naziva sinkronizaciju toka podataka omogućena je prema vezu odnos između pripremna i metaverse objekata. Pripremna objekt koji je povezan s objektom metaverse se naziva **pridruženo objekt** (ili **poveznika objekta**). Pripremna objekt koji nije povezan s metaverse objekt se naziva **odvojeno objekt** (ili **disconnector objekta**). Uvjeti pridruženo i odvojeno su Preferirani da ne zbuniti pomoću poveznika odgovoran za uvoz i izvoz podataka iz povezanih imenika.

Rezervirana mjesta nikad povezani s metaverse objekta

Spojena objekta sastoji se od pripremna objekta i njegov povezane odnos jedan metaverse objekt. Spojena objekti se koriste za sinkroniziranje vrijednosti atributa između objekt programa poveznik prostora i metaverse objekt.

Kada pripremna objekt postane spojena objekta tijekom sinkronizacije, atribute tijeka možete između pripremna objekta i metaverse objekt. Atribut tijek je Dvosmjeran i konfiguriran pomoću pravila atribut uvoza i izvoza atribut pravila.

Objekt programa jedan poveznik prostor se može povezati samo jedan objekt metaverse. Međutim, svaki objekt metaverse može povezati s više objekata poveznik prostor u istom ili u različite poveznik razmaka, kao što je prikazano na sljedećoj slici.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Povezani odnos između pripremna objekta i metaverse objekt je stalni, a možete ukloniti jedino putem pravila koji navedete.

Odvojenom objekt je pripremna objekt koji nije povezan s bilo kojeg metaverse objekta. Atribut vrijednosti odvojenom objekta nisu obrađuju bilo koji dodatno unutar na metaverse. Vrijednosti atributa odgovarajuće objekta u izvoru povezanih podataka se ne ažuriraju prema modula za sinkroniziranje.

Pomoću odvojenom objekata pohrane podataka identiteta u modula za sinkroniziranje i obrade kasnije. Zadržavanje pripremna objekt kao odvojenom objekt u prostoru poveznika ima brojne prednosti. Budući da sustav već je kopirana bez postavljanja potrebne informacije o taj objekt, nije potrebno za stvaranje hijerarhije objekta ponovno tijekom sljedećeg uvoza iz izvora povezanih podataka. Na taj način modula za sinkroniziranje uvijek ima dovršeno snimke izvora povezanih podataka čak i ako nema trenutnu vezu s izvorom povezanih podataka. Odvojenom objekti se može pretvoriti u spojene objekte i obratno, ovisno o pravila koji navedete.

Objekt uvoza stvara se kao odvojenom objekt. Izvoz objekt mora biti spojena objekta. Logika sustava nameće ovo pravilo i briše svaki izvoz objekt koji nije spojena objekta.

## <a name="sync-engine-identity-management-process"></a>Upravljanje proces za sinkronizaciju modula identiteta
Postupak upravljanja identiteta određuje kako se podaci za identitet ažurirat između različitih povezanih izvora podataka. Upravljanje identitetom dogodit će se tri procesa:

- Uvoz
- Sinkronizacija
- Izvoz

Tijekom postupka uvoza modula za sinkroniziranje vrednuje identiteta podatke o ulaznom iz izvora povezanih podataka. Kada se otkriju promjene, ga stvara novi pripremna objekti ili ažurira postojeći pripremna objekata u prostoru poveznik za sinkronizaciju.

Tijekom sinkronizacije modula za sinkroniziranje ažurira metaverse u skladu s promjenama koje su se pojavile u prostoru poveznika i prilagođava prostor poveznika u skladu s promjenama koje su se pojavile u na metaverse.

Tijekom postupka izvoza modula za sinkroniziranje ih gura out promjene kopirana koji bez su postavljanja na pripremna objekte i koji su označena čekanju izvoz.

Sljedeća slika prikazuje gdje svakog procesa odvija se u obliku tokova identiteta informacije iz jednog izvora povezanih podataka u drugu.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Postupak uvoza
Tijekom postupka uvoza modula za sinkroniziranje vrednuje ažuriranja podataka identitet. Modul za sinkronizaciju uspoređuje identiteta dobivene iz izvora povezanih podataka s informacije o objekt programa pripremna identitetu i određuje hoće li pripremna objekt potrebna ažuriranja. Ako je potrebno da biste ažurirali pripremna objekt novim podacima, pripremna objekt označit će se kao na čekanju uvoz.

Po pripremna objekata u razmak poveznik za sinkronizaciju modula za sinkroniziranje možete obraditi samo identiteta podatke koji su se promijenili. Ovaj postupak pruža sljedeće prednosti:

- **Učinkovito sinkronizacije**. Količinu podataka koji se obrađuju tijekom sinkronizacije je minimiziran.
- **Učinkovito resynchronization**. Možete promijeniti način sinkronizacije modul obrađuje informacije o identitetu bez ponovnog povezivanja modula za sinkroniziranje s izvorom podataka.
- **Mogućnost Pretpregled sinkronizacije**. Možete pretpregledati sinkronizacije da biste provjerili jesu li vaše pretpostavke o postupku upravljanje identitetom ispravni.

Za svaki objekt s navedenim u poveznik modula za sinkroniziranje najprije pokušava pronaći prikaz objekta u prostoru poveznik poveznika. Modula za sinkroniziranje pregledava sve objekte pripremna poveznik prostor i pokušava pronaći odgovarajuće pripremna objekt koji ima odgovarajuće sidrenja atribut. Ako je postojeći objekt pripremna odgovarajuće sidrenja atribut, modula za sinkroniziranje pokušava pronaći odgovarajuće pripremna objekt s istim nazivom razlikovni.

Kada modula za sinkroniziranje pronađe pripremna objekt koji odgovara po Razlikovni naziv, ali ne po sidro, događa se sljedeće posebno ponašanje:

- Ako je objekt koji se nalazi u prostoru poveznik bez sidro, zatim modula za sinkroniziranje uklanja taj objekt iz poveznik prostora i označava metaverse objekt je povezana s kao **pokušajte ponovno dodjeljivanje na sljedeći sinkronizacija**. Zatim stvara novi objekt uvoz.
- Ako je objekt koji se nalazi u prostoru poveznika programa sidrenja, zatim modula za sinkroniziranje pretpostavlja da taj objekt ili preimenovana je ili izbrisati u direktoriju povezani. Dodjeljuje privremene, novu Razlikovni naziv objekta prostora poveznik tako da možete faze dolazne objekta. Stari objekt zatim postaje **tranzitne**, Čeka se poveznik za uvoz preimenovanje ili brisanje da biste riješili situaciji.

Ako modula za sinkroniziranje pronalazi pripremna objekt koji odgovara objekt koji je naveden u poveznik, određuje kakve promjene da biste primijenili. Ako, na primjer, modula za sinkroniziranje može preimenovati ili izbrisati objekt u izvoru podataka povezanih ili ga može ažurirati vrijednosti atributa objekta.

Pripremna objekte s ažurirane podatke označavaju se kao na čekanju uvoz. Dostupne su različite vrste na čekanju uvozi. Ovisno o rezultatu uvoza pripremna objekta u prostoru poveznik sadrži nešto od sljedećeg na čekanju uvoz vrste:

- **Ništa**. Dostupni su bez promjena na bilo koji od atributa pripremna objekta. Modul za sinkronizaciju označite ovu vrstu čekanju uvoz.
- **Dodavanje**. Pripremna objekt je novi objekt uvoz u prostoru poveznik. Modul za sinkronizaciju zastavicama ovu vrstu čekanju uvoz za dodatnu obradu u na metaverse.
- **Ažuriranje**. Modul za sinkronizaciju pronađe odgovarajući pripremna objekt u prostoru poveznika i zastavicama vrste kao što je čeka uvoz tako da se ažuriranja atribute može biti obuhvaćene na metaverse. Ažuriranja obuhvaćaju Preimenovanje objekta.
- **Brisanje**. Modul za sinkronizaciju pronađe odgovarajući pripremna objekt u prostoru poveznika i zastavicama vrste kao što je na čekanju uvoz tako da se spojene objekt moguće je izbrisati.
- **Brisanje i dodavanje**. Modula za sinkroniziranje pronađe odgovarajući pripremna objekt u prostoru poveznika, ali ne podudaraju se vrste objekata. U ovom slučaju na Izbriši dodavanje izmjene je kopirana bez postavljanja. A Izbriši dodavanje izmjene naznačuje da modula za sinkroniziranje mora biti dovršeno resynchronization objekta jer različite skupove pravila primijeniti na taj objekt kada objekt unesite promjene.

Postavljanje statusa na čekanju uvoza pripremna objekta je moguće znatno smanjiti količinu podataka koji se obrađuju tijekom sinkronizacije s obzirom na to tako da dopušta sustava za obradu samo one objekte koji ste ažurirali podatke.

### <a name="synchronization-process"></a>Sinkronizacija
Sinkronizacija sastoji se od dvaju povezanih procesa:

- Dolazni sinkronizaciju, kada sadržaj na metaverse ažurira se pomoću podataka u prostoru poveznik.
- Izlazni sinkronizaciju, kada se ažurira sadržaj prostora poveznik pomoću podataka na metaverse.

Pomoću informacija kopirana bez postavljanja u prostoru poveznik ulaznog sinkronizacija stvara u na metaverse integrirani prikaz podataka pohranjenih u izvora povezanih podataka. Svi objekti pripremna ili samo one s informacijama za uvoz na čekanju se pridružuje, ovisno o konfiguraciji pravila.

Postupak ažuriranja izlaznog sinkronizacije izvesti objekte promjenama metaverse objekte.

Dolazni sinkronizacije stvara integrirani prikaz u metaverse informacije o identitetu primljene iz izvora povezanih podataka. Modul za sinkronizaciju možete obraditi identiteta informacije u bilo kojem trenutku pomoću najnovije informacije identitet koji sadrži izvora povezanih podataka.

**Dolazni sinkronizacije**

Dolazni sinkronizacije obuhvaća sljedeće postupke:

- **Dodjela resursa** (naziva se i **projekcije** ako je važno da biste razlikovali ovog postupka od izlaznog sinkronizacije dodjele resursa). Modul za sinkronizaciju stvara novi metaverse objekt koji se temelji na pripremna objekta i povezuje ih. Dodjela resursa je postupak razini objekta.
- **Uključivanje**. Modul za sinkronizaciju povezuje pripremna objekt postojećeg metaverse objekta. Spoj je postupak razini objekta.
- **Tijek uvoza atribut**. Modul za sinkronizaciju ažurira vrijednosti atributa koji se naziva protok atribut objekta u na metaverse. Uvoz atribut tijek je postupak razina atributa koji zahtijeva vezu između pripremna objekta i metaverse objekt.

Dodjela resursa je samo postupak koji stvara objekata u na metaverse. Dodjela resursa utječe samo Uvoz objekata koji su odvojenom objekte. Prilikom dodjele resursa, modula za sinkroniziranje stvara metaverse objekt koji odgovara vrsti objekta objekta uvoza i uspostavlja veza između oba objekata na taj način stvaranja spojena objekta.

Postupak za uključivanje uspostavlja i vezu između Uvoz objekata i metaverse objekt. Razlika između spoj i dodjeljivanje je postupak za uključivanje zahtijeva da se objekt uvoz povezan s postojećeg metaverse objekta, gdje postupka dodjele stvara novi objekt metaverse.

Modul za sinkronizaciju pokušava uključiti objekt uvoza metaverse objekt pomoću kriterija koji je naveden u konfiguraciji sinkronizacije pravilo.

Tijekom procesa dodjele resursa i pridružiti im modula za sinkroniziranje povezuje odvojenom objekt metaverse objekt, čega se pridružili. Kada se dovrše te operacije razini objekta, modula za sinkroniziranje možete ažurirati vrijednosti atributa povezane metaverse objekta. To se naziva tijek uvoza atribut.

Uvoz atribut protok pojavljuje se na sve Uvoz objekata koji imaju nove podatke, a povezani su s metaverse objekt.

**Izlazni sinkronizacije**

Ažuriranja izlaznog sinkronizacije izvesti objekte kada metaverse objekt promjena, ali neće se izbrisati. Cilj izlaznog sinkronizacije je da biste procijenili li metaverse objekte stupi ažuriranja pripremna objekata u za to predviđen poveznik. U nekim slučajevima promjene možete zatražiti te pripremna ažurirat će se objekti u sve razmake poveznik. Pripremna objekata koje su se promijenile označene kao na čekanju izvoz, a zatim da ih izvesti objekata. Ove Izvoz objekata kasnije pomiču se s izvorom povezanih podataka tijekom postupka izvoza.

Izlazni sinkronizacije sastoji se od tri procesa:

- **Dodjela resursa**
- **Deprovisioning**
- **Izvoz tijek atributa**

Dodjeljivanje i deprovisioning su obje operacije razini objekta. Deprovisioning ovisi o dodjeljivanje jer samo dodjele resursa možete pokrenuti ga. Deprovisioning se pokreće prilikom dodjele resursa uklanja vezu između web-mjesto metaverse objekta i u okvir za izvoz objekt.

Dodjeljivanje uvijek aktivira prilikom promjene primjenjuju se na objekte na metaverse. Kada su promjene metaverse objekata, modula za sinkroniziranje izvršava sve sljedeće zadatke u sklopu postupka dodjele resursa:

- Stvaranje spojenih objekata, gdje metaverse objekt povezan novostvorenu izvoz objekta.
- Preimenovanje spojena objekta.
- Disjoin veza između metaverse objekta i pripremnih objekte, stvaranja odvojenom objekta.

Ako dodjeljivanje zahtijeva modul sinkronizaciju da biste stvorili novi objekt poveznika, pripremna objekta s kojim je povezan objekt metaverse uvijek je objekt izvoz jer objekt još ne postoji u izvoru podataka povezanih.

Ako dodjeljivanje zahtijeva modul sinkronizaciju da biste disjoin spojene objekt stvaranja odvojenom objekta, deprovisioning se pokreće. Postupak deprovisioning briše objekt.

Tijekom deprovisioning, brisanje objekta izvoz fizički briše se objekt. Objekt je označena kao **Izbrisane**, što znači da se operacija brisanja je kopirana bez postavljanja na objekt.

Izvoz atribut protok i pojavljuje se tijekom postupka izlaznog sinkronizacije, slično kao tijek uvoza atribut pojavljuje se tijekom ulaznog sinkronizacije. Izvoz atribut protok pojavljuje se samo između metaverse i izvoz objekte koji su povezani.

### <a name="export-process"></a>Postupak izvoza
Tijekom postupka izvoza modula za sinkroniziranje ispituje sve objekte izvoz označene čekanju Izvoz u prostoru poveznika, a zatim šalje ažuriranja izvora povezanih podataka.

Modul za sinkronizaciju možete odrediti uspjeh izvoz, ali potpuno ne može odrediti dovršetka postupka upravljanje identitet. Objekti u izvoru podataka povezanih uvijek možete promijeniti drugi procesi. Jer modula za sinkroniziranje imaju Neprekidna veza s izvorom podataka povezanih, nije dovoljno da bi pretpostavke o svojstva objekta u izvoru povezanih podataka samo na temelju obavijest uspješnog izvoza.

Na primjer, postupak u izvoru podataka povezanih može promijeniti atribute objekta natrag na njihove izvorne vrijednosti (to jest, izvora povezanih podataka može prebrisati vrijednosti odmah nakon podaci se pomiču tako da modula za sinkroniziranje i uspješno primijenjena u izvoru podataka povezanih).

Služi za pohranu modul sinkronizaciju izvoz i uvoz informacije o statusu o svaki pripremna objekt. Ako vrijednosti atributa koji su navedeni na popisu atributa uvrštavanja su se promijenile od zadnjeg izvoza, prostora za pohranu uvoz i izvoz modul za sinkronizaciju omogućuje status da biste brzo pravilno. Modul za sinkronizaciju koristi uvoza da biste potvrdili vrijednosti atributa izvezeni izvor povezanih podataka. Usporedba između uvezenih i izvezene podatke, kao što je prikazano na sljedećoj slici omogućuje modul sinkronizaciju da biste provjerili je li izvoz je uspio ili ako je treba ponoviti.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Na primjer, ako modula za sinkroniziranje izvozi atribut C, koji sadrži vrijednosti 5, izvora povezanih podataka, sprema C = 5 u svojoj memoriji status izvoz. Svaki dodatni Izvoz na ovom objekt rezultata pokušaj izvesti C = 5 izvora povezanih podataka ponovno jer modula za sinkroniziranje pretpostavlja da ta vrijednost nije persistently primijenjen na objekt (to jest, osim ako neku drugu vrijednost uvezeni nedavno izvora povezanih podataka). Izvoz memorije poništen je primljena C = 5 tijekom operacije uvoza na objekt.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
