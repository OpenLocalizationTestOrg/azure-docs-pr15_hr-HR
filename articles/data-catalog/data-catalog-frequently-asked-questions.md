<properties
   pageTitle="Najčešća pitanja o Azure katalog podataka | Microsoft Azure"
   description="Najčešća pitanja o Azure katalog podataka, uključujući mogućnosti za otkrivanje izvora podataka, primjedbe i upravljanje."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Najčešća pitanja o Azure kataloga podataka

U ovom se članku navedeni odgovori na najčešća pitanja povezane sa servisom Microsoft **Azure kataloga podataka** .

## <a name="q-what-is-azure-data-catalog"></a>P: što je katalog podataka Azure?

A: katalog podataka za Microsoft Azure je servis za potpuno upravljane smješten u oblak Microsoft Azure koje služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Azure katalog podataka omogućuje mogućnosti koja omogućuje bilo koji korisnik – s analitičar analitičar podataka da biste podatke fizičari programerima – da biste registrirali, otkrivanje, razumijevanje i korištenje izvora podataka.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>P: koje kupca izazove ne katalog podataka Azure riješiti?

Azure katalog podataka adrese pitanja i odgovora te otkrivanje izvora podataka "tamno data" dopuštanjem korisnicima da biste otkrili i razumijevanje enterprise izvora podataka.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>P: tko su ciljne skupine za katalog podataka Azure?

Azure katalog podataka pruža mogućnosti za Tehnički i nisu tehničke prirode korisnika, uključujući:

- Razvojni inženjeri podataka, BI i analize profesionalce: koji su odgovorni za proizvodnju podataka i analitiku sadržaja da ga drugi mogu zauzeti
-   Administratori podataka: Osobe koje imaju znanja o podatke, što znači i kako je namijenjen koja će se koristiti i koju svrhu
- Korisnici podataka: Osobama koje je potrebno da biste mogli jednostavno otkrivanje, razumijevanje i povezivanje s podacima potrebno za obavljanje svojim posla pomoću alata za njihov odabir
- Središnje IT: one kojima su potrebne radi jednostavnijeg pronalaženja stotine izvora podataka za poslovne korisnike, a kojima su potrebne da biste zadržali nadzor nad kako se koristi podatke i tko

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>P: što je regija dostupnost katalog podataka Azure?

Servisa Azure katalog podataka trenutno dostupne su u centre za sljedeće podatke:

- Zapad SAD-a
- Istočni SAD-a
- Europa Zapad
- Sjeverna Europa
- Istok Australija
- Jugoistočne Azije

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>P: koja su ograničenja broja resursima podataka u katalog podataka Azure?

Besplatni izdanje programa Azure kataloga podataka ograničeno je na imovinu 5000 registriranih podataka.

Standardni izdanje programa Azure kataloga podataka podržava do 100 000 imovine registriranih podataka.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>P: koje su podržane vrste podataka sustava izvor i resursa?

Pogledajte [DSC katalog podataka](data-catalog-dsr.md) za popis trenutno podržanih izvora podataka.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>P: kako traženje podrške za drugi izvor podataka?

Zahtjeve za značajke, a drugi povratne informacije možete poslati u na [forumu Azure kataloga podataka](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>P: Kako započeti rad s katalog podataka Azure?

Najbolje za početak je prema uputama u odjeljku [Uvod u katalog podataka](data-catalog-get-started.md). Ovaj je članak pregled završetka do kraja mogućnosti u servisu.

## <a name="q-how-do-i-register-my-data"></a>P: kako registrirati moje podatke?

Da biste registrirali podataka u katalogu podataka Azure, pokrenite alat za registraciju Azure katalog podataka iz područja "Objavi" portala za Azure kataloga podataka. U aplikaciji objavljivanje katalog podataka Azure prijavite se pomoću istih vjerodajnica koje koristite za pristup portalu Azure kataloga podataka, a zatim odaberite izvor podataka i određene resursi koji želite registrirati.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>P: koje svojstva se izdvajaju podataka imovine registrirane?

Konkretnih svojstava će se razlikovati od izvora podataka za izvor podataka, no Općenito Azure katalog podataka servisa za objavljivanje će izdvojiti sljedeće informacije:

- Naziv resursa
- Vrsta resursa
- Opis resursa
- Nazivi atribut/stupaca
- Atribut i stupca vrste podataka
- Opis atribut i stupca

> [AZURE.IMPORTANT] Registracija imovine podataka pomoću kataloga podataka Azure ne premještanje i kopiranje podataka s oblakom. Registracija imovine iz izvora podataka će kopirati metapodataka na imovinu Azure, no podaci ostaju u postojeće mjesto izvora podataka. Jedina to pravilo je iznimka ako korisnik odabere da biste prenijeli Pretpregled zapisa ili podataka profila prilikom registriranja resursi. Kada uključujući pretpregled, do 20 zapisa koji se kopiraju iz svakog resursa i pohranjuju se kao snimku stanja Azure u katalogu podataka dodatka. Kada uključujući podataka profila, zbrajanja podatke (primjerice veličinu tablice, postotak null vrijednosti po stupcu i minimalne, maksimalne i prosjek vrijednosti za stupce) bit će izračunati i obuhvaćeno metapodaci spremljeni u katalogu.

<br/>

> [AZURE.NOTE] Izvore podataka kao što je SQL Server Analysis Services koje imaju jednostavno prva liga svojstvo **Opis** , katalog podataka Azure aplikacije za objavljivanje će izdvojiti vrijednost tog svojstva. Za SQL Server relacijskim bazama podataka, koji nemaju jednostavno prva liga svojstvo **Opis** katalog podataka Azure objavljivanje aplikacija će izdvojiti vrijednost iz ms_description proširena svojstva za objekte i stupce. Dodatne informacije potražite u članku TechNet [Pomoću proširena svojstva za objekte baze podataka](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>P: koliko treba traje tek registrirani imovine da se prikazuje u katalog podataka Azure?

Kada registrirate imovine Azure katalog podataka mogu postojati u razdoblju od 5 do 10 sekundi prije nego što se pojavljuju na portalu za Azure kataloga podataka.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>P: kako dodavati i obogaćivanje metapodataka za imovinu Moje registrirane podataka?

Najjednostavniji način za prikaz metapodataka registrirani imovine je odaberite imovinu na portalu za Azure katalog podataka, a zatim unesite vrijednosti metapodataka u oknu svojstva ili oknu sheme za odabrani objekt.

Neki metapodaci, kao što su stručnjake i oznake, možete unijeti i tijekom postupka registracije. Vrijednosti navedene u katalog podataka Azure servisa za objavljivanje će se primijeniti na sva sredstva koja se registrira u tom trenutku. Da biste pogledali objekti nedavno registrirana na portalu za dodatne opaske, odaberite gumb **Portal za prikaz** na zadnjem zaslonu Azure katalog podataka aplikacije za objavljivanje.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>P: kako izbrisati objekti registriranih podataka?

Brisanje objekta iz kataloga podataka Azure tako da odaberete objekt na portalu, a zatim kliknete gumb **Izbriši** . To će uklonili metapodatke objekta iz kataloga podataka Azure, ali ne utječu na stvarni temeljnom izvoru podataka.

## <a name="q-what-is-an-expert"></a>P: što je stručnjak?

Stručnjaka je osoba koja ima informirali perspektive o podatkovni objekt. Objekt može imati više stručnjaka. Stručnjaka moraju biti "vlasnik" za neki objekt stručnjak je jednostavno netko tko zna kako podatke i želite koristiti.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>P: kako se zajednički koriste informacije s timom Azure katalog podataka ako mogu se pojaviti problemi?

Korištenje kataloga podataka Azure forum prijavljivanja problema, zajedničko korištenje informacija i postavljati pitanja. Forum možete pronaći na http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>P: Azure katalog podataka funkcionira ova drugog izvora podataka sam zanimati?
Aktivno radimo o dodavanju više izvora podataka za Azure kataloga podataka. Ako je izvor podataka koji biste željeli potražite u članku podržane, ponovno ga prijedlog (ili glasovne podršku na radnom mjestu ako je već je predložio) u na [forumu Azure kataloga podataka](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>P: način Azure katalog podataka povezan s kataloga podataka u dodatku Power BI za Office 365?

Kataloga Azure podataka možete smatrati je proizvod kataloga podataka. Azure katalog podataka nudi slične mogućnosti za objavljivanje izvora podataka i otkrivanje, ali je usmjeren na šira scenariji, a ne ovise o sustavu Office 365. Uskoro nakon katalogu podataka servisa Azure postane načelu dostupan dva katalozi će spojiti u jedan servis.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>P: koje dozvole korisnik mora registrirati imovine pomoću kataloga podataka Azure?

Korisnik koji se pokreće alat za registraciju Azure katalog podataka mora dozvole za izvor podataka koji će dopustiti mu čitanje metapodatke iz izvora. Ako korisnik odabere i da biste dodali pretpregled, korisnik mora i imati dozvole koje je moguće ga je čitati u podatke iz objekata registracije.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>P: Azure katalog podataka bit će dostupan za lokalnu implementaciju kao i?

Azure katalog podataka je neki servis u oblaku, koje možete raditi s oblaka i lokalnih izvora podataka, izlaganja rješenje otkrivanje izvora podataka hibridnog. Trenutno postoje planovi za verziju servisa Azure katalog podataka koji će se izvoditi na lokalni.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>P: mogu smo izdvojiti više / bogatiji metapodatke iz izvora podataka ne možemo registrirati?

Aktivno Radimo na proširiti mogućnosti kataloga podataka Azure. Ako postoji dodatne metapodatke koje želite da biste vidjeli izdvojenih iz izvora podataka prilikom registracije, provjerite predložiti je (ili ga ako je već je predložio glasovanja) u na [forumu Azure kataloga podataka](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). U budućnosti ćemo omogućuju trećim stranama da biste dodali nove vrste izvora podataka putem proširenja API-JA.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>P: kako ograničiti vidljivost imovine registriranih podataka, tako da ih možete otkriti samo određene osobe?

A: Odaberite resursi podataka u katalogu podataka servisa Azure, a zatim kliknite gumb "Preuzimanje vlasništva". Vlasnici imovine podataka u katalogu podataka Azure možete promijeniti postavke vidljivosti, ili omogućivanja sve korisnike kataloga da biste otkrili vlasništvu imovine ili da biste ograničili vidljivost određene korisnike.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>P: kako ažurirati Registracija podataka resursa u koji se mijenja u izvoru podataka odražavaju se u katalogu?

A: da biste ažurirali metapodataka za imovinu podataka koje su već registrirane u katalogu, jednostavno ponovno registrirajte izvor podataka koji sadrži imovinu. Promjene u izvoru podataka, kao što su stupaca koji se dodati ili ukloniti s tablicama ili prikazima, ažurirat će se u katalogu, ali sve primjedbe koje ste dobili od korisnika će biti zadržane.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>P: niste pronašli odgovor pitanje ovdje – što učiniti?

Glavni na iznad da biste na [forumu Azure kataloga podataka](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Postoji postavljanja pitanja tražit će njihove način.
