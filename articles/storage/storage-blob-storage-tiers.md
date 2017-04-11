<properties
    pageTitle="Azure zanimljivih prostor za pohranu blob-ova | Microsoft Azure"
    description="Razine za pohranu za spremište blobova platforme Azure nude trošak učinkovitog prostor za pohranu koji objekt podataka na temelju uzoraka programa access. Odlične za pohranu sloju optimiziran je za podatke koje rjeđe pristupiti."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Spremište blobova platforme Azure: Tipkovne i zanimljivih razine prostora za pohranu

## <a name="overview"></a>Pregled

Azure prostora za pohranu sada nudi dvije razine za pohranu za spremište blobova (objekt spremište) tako da možete spremati podataka najčešće isplativo ovisno o tome kako ga koristiti. Azure **Tipkovni prostora za pohranu sloju** optimiziran je za pohranu podataka koji se često pristupa. Azure **odlične za pohranu sloju** optimiziran je za pohranu podataka koji je diskovni pristupa i long-lived. Podataka u sloju odlične za pohranu možete tolerate malo niže dostupnost, ali i dalje potrebna visoka rok trajanja i slično vrijeme za pristup i propusnost karakteristike kao najnovije podatke. Odlične podaci, malo niže SLA dostupnosti i veće troškove pristup su prihvatljiva gubitke za koliko manji trošak prostora za pohranu.

Danas, podatke pohranjene u oblaku je rast eksponencijalne tempom. Da biste upravljali trošak potrebama širi prostor za pohranu, je korisno za organiziranje podataka koji se temelje na atribute kao što su učestalost okvira pristup i razdoblje zadržavanja planiranog. Podatke pohranjene u oblaku može biti prilično različita pomoću kako ga je generira, obrađeni ili pristupiti putem njegova života. Neki podaci je aktivno pristupiti i mijenjati tijekom njegova života. Neki podaci pristupa vrlo često na početku njegov vijek s pristupom ispuštanje drastično kao starije od podataka. Neki podaci ostaju neaktivnosti u oblak i rijetko, ako ikad pristupili jednom pohranjuju.

Svaki od tih scenarija pristupa podacima navedenih pogodnosti iz isporučujte prilagođena razina pohrane koja je optimizirana za uzorak određenog programa access. Uz Uvod tipkovni i zanimljivih pohranu razine blobova platforme Azure prostora za pohranu sada adrese ta potreba za pohranu isporučujte prilagođena razine s odvojite cijene modela.

## <a name="blob-storage-accounts"></a>Računa spremišta blobova platforme

**Računi za pohranu blob** su specijalizirane prostora za pohranu račune za pohranu nestrukturirane podatke kao BLOB-ova (objekte) u odjeljku pohrana Azure. Pomoću računa za spremište blobova platforme, sada možete između razine tipkovni i zanimljivih prostora za pohranu za pohranu rjeđe pristupa zanimljivih podataka na donjem prostora za pohranu cijena i spremiti češće pristupa najnovije podatke na donjem pristup cijena. Računa spremišta blobova platforme su slične postojeće računi općenite namjene prostora za pohranu i zajedničko korištenje sve sjajno rok trajanja, dostupnost, skalabilnost i performanse značajke koje koristite danas, uključujući 100% dosljednost API-JA za blokiranje blob-ova te dodavanje blob-ova.

> [AZURE.NOTE] Računa spremišta blobova platforme podržava samo blok i dodati blob-ova i stranice blob polja.

Računa spremišta blobova platforme izložiti atribut **Razina pristupa** koji omogućuju da navedete sloju prostora za pohranu kao **Aktivno** ili **zanimljivih** ovisno o podacima pohranjenima u račun. Ako nema promjena u korištenje uzorak podataka, možete prijeći i te razine prostora za pohranu u bilo kojem trenutku.

> [AZURE.NOTE] Promjena prostora za pohranu sloju može uzrokovati dodatne troškove. Pogledajte odjeljak [određivanje cijena i naplata](storage-blob-storage-tiers.md#pricing-and-billing) za dodatne pojedinosti.

Primjer korištenja namjene sloju tipkovni prostora za pohranu obuhvaćaju:

- Podaci koji se aktivno koristi ili očekuje moguć (čitanje iz i pisane da biste) često.
- Podaci koji je kopirana bez postavljanja za obradu i usmjerenog migraciju u sloju odlične za pohranu.

Primjer korištenja namjene sloju odlične za pohranu obuhvaćaju:

- Sigurnosna kopija, arhiviranje i skupova Izrada oporavak podataka.
- Stariji medijskog sadržaja nije često odjednom prikazati, ali se očekuje da bi bio dostupan odmah nakon pristupiti.
- Velikih skupova podataka koji se moraju biti pohranjene trošak učinkovito dok još podataka se koji se prikupljaju za buduće obradu. (*npr.*, dugoročno spremanje znanstvene podatke, neobrađenog telemetrijskih podataka iz proizvodnje funkcijom)
- Izvorni (neobrađenog) podaci koji se moraju moraju sačuvati, čak i nakon bude obrađen u konačni upotrebljivosti obrazac. (*npr.*, neobrađenog medijske datoteke nakon prekodiranje u druge oblike zapisa)
- Sukladnost i arhiviranje podataka koje treba nalaziti na dulje vrijeme i hardly ikad pristupiti. (*npr.*, sigurnost kamere materijal, stari X-Rays/MRIs za zdravstvene tvrtke ili ustanove, audiozapisa i transkripti poziva klijenta za financijske usluge)

Dodatne informacije o računima za pohranu potražite u članku [o Azure prostora za pohranu računa](storage-create-storage-account.md) .

Za aplikacije koje je obavezna samo blokirati ili dodati blobova, preporučujemo da putem računa spremišta blobova platforme, da biste iskoristili isporučujte prilagođena cijene model tiered prostora za pohranu. Međutim, uviđamo da to možda neće biti moguće određenim okolnostima gdje općenite namjene prostora za pohranu računi bio način da biste otišli, kao što su:

- Morate koristiti tablica, redove ili datoteka i želite da vaše blob-ova pohranjene u isti račun za pohranu. Napomena ima li bez tehničke prednost pohrani na isti račun koji nije isti pojavljuju sljedeće zajednički koristiti tipke.
- Svejedno morate koristiti model implementacije klasični. Računa spremišta blobova platforme su dostupne samo putem model implementacije Azure Voditelj resursa.
- Morate koristiti blob-Ova stranica. Računa spremišta blobova platforme ne podržavaju blob-Ova stranica. Obično je preporučujemo korištenje bloka blob-ova, osim ako imate određene potrebe za blob-Ova stranica.
- Koristite verziju [Prostora za pohranu servisa REST API -JA](https://msdn.microsoft.com/library/azure/dd894041.aspx) prethodi 2014. 02 14 ili biblioteci klijenta s verzijom koja je manja od 4.x, a ne može nadograditi aplikaciju.

> [AZURE.NOTE] Računa spremišta blobova platforme trenutno podržava većina Azure područja s više da biste počeli pratiti. Možete pronaći ažurirani popis dostupnih područja na stranici [Servisa Azure po regijama](https://azure.microsoft.com/regions/#services) .

## <a name="comparison-between-the-storage-tiers"></a>Usporedba razine prostora za pohranu

U sljedećoj su tablici ističe usporedbe između dvije razine prostora za pohranu:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Razina tipkovni prostora za pohranu</center></strong></td>
    <td><strong><center>Razina odlične za pohranu</center></strong></td
</tr>
<tr>
    <td><strong><center>Dostupnost</center></strong></td>
    <td><center>99.9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Dostupnost<br>(RA GRS čita)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99.9%</center></td>
</tr>
<tr>
    <td><strong><center>Naknade za korištenje</center></strong></td>
    <td><center>Veće troškove prostora za pohranu<br>Manji trošak access i transakcije</center></td>
    <td><center>Manji trošak prostora za pohranu<br>Veće troškove pristupa i transakcije</center></td>
</tr>
<tr>
    <td><strong><center>Objekt Minimalna veličina<center></strong></td>
    <td colspan="2"><center>N/D</center></td>
</tr>
<tr>
    <td><strong><center>Trajanje minimalne prostora za pohranu<center></strong></td>
    <td colspan="2"><center>N/D</center></td>
</tr>
<tr>
    <td><strong><center>Latencija<br>(Vrijeme do prvi bajt)<center></strong></td>
    <td colspan="2"><center>milisekundama</center></td>
</tr>
<tr>
    <td><strong><center>Skalabilnost i performanse ciljnih web-mjesta<center></strong></td>
    <td colspan="2"><center>Jednako kao i račune općenite namjene prostora za pohranu</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] Računa spremišta blobova platforme podržava isti performanse i skalabilnost ciljnih web-mjesta kao računi općenite namjene prostora za pohranu. Dodatne informacije potražite u [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Cijene i naplata

Računa spremišta blobova platforme pomoću novog modela cijene za spremište blobova na temelju sloju prostora za pohranu. Kada koristite račun za spremište blobova platforme, primjenjuju se naplate Imajte na umu sljedeće:

- **Troškovi spremanja**: osim količinu podataka koji su pohranjeni trošak spremanje podataka ovisi o sloju prostora za pohranu. Trošak po gigabajta je donjem za sloju zanimljivih prostora za pohranu od za sloju tipkovni prostora za pohranu.
- **Pristup podacima troškove**: za podatke u sloju odlične za pohranu koji će se naplatiti naknadu po gigabajta podataka programa access čitanja i pisanja.
- **Transakcije troškove**: postoji naknadu po transakcije za oba razine. Međutim, je trošak po transakcije za sloju zanimljivih prostora za pohranu višim od onih za sloju tipkovni prostora za pohranu.
- **Prijenosa podataka zemlj replikacije troškove**: to se odnosi samo na račune s zemlj. – replikacijom konfiguriran, uključujući GRS i RA GRS. Prijenos podataka zemlj replikacije uključuje naknadu po gigabajta.
- **Izlazni podataka prijenos troškove**: prijenosi izlaznog podataka (podataka koji se prenose iz Azure regija) plaćati naplata korištenja propusnosti na temelju po gigabajta, usklađene s računima općenite namjene prostora za pohranu.
- **Promjena prostora za pohranu sloju**: promjena prostora za pohranu sloju iz najbolju da biste tipkovne plaćati naknadu jednako sve podatke postojeći račun za pohranu za svaku prijelaza za čitanje. S druge strane, promjena prostora za pohranu sloju iz tipkovni najbolju bit će besplatne troška.

> [AZURE.NOTE] Da bi korisnicima omogućuje isprobajte novi prostor za pohranu razine i provjeriti funkcionalnost objavu prilikom pokretanja naplatiti za promjenu prostora za pohranu sloju iz najbolju da biste tipkovne će biti waived isključivanje do lipnja 30 2016. Pokretanje srpanj 1st 2016 naplatiti će se primijeniti na sve prijelaze iz najbolju da biste tipkovne. Dodatne informacije o cijenama modela za spremište blobova platforme računa potražite u odjeljku [Cijene za pohranu Azure](https://azure.microsoft.com/pricing/details/storage/) stranice. Dodatne informacije o izlazne podatke naknade za prijenos pročitajte članak stranicu s [Detaljima cijene za prijenos podataka](https://azure.microsoft.com/pricing/details/data-transfers/) .

## <a name="quick-start"></a>Brzi početak rada

U ovom odjeljku ne možemo će demonstrirati pomoću portala za Azure sljedećim scenarijima:

- Upute za stvaranje računa za spremište blobova platforme.
- Kako upravljati računom spremišta blobova platforme.

### <a name="using-the-azure-portal"></a>Pomoću portala za Azure

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Stvaranje računa spremišta blobova platforme pomoću portala za Azure

1. Prijavite se na [portal za Azure](https://portal.azure.com).

2. Na izborniku koncentrator odaberite **Novo** > **podataka + prostor za pohranu** > **računa za pohranu**.

3. Unesite naziv za vaš račun za pohranu.

    Taj naziv mora biti globalno jedinstven. koristi se kao dio URL-a za pristup objekata u račun za pohranu.  

4. Odaberite **Resursima** kao model implementacije.

    Tiered prostora za pohranu može se koristiti samo s resursima za pohranu računa. To je preporučena implementacije model za nove resurse. Dodatne informacije potražite u članku [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md).  

5. Na padajućem popisu vrsta računa odaberite **Spremište blobova platforme**.

    To je mjesto odaberite vrstu računa za pohranu. Prostor za pohranu tiered nije dostupna u općenite namjene prostora za pohranu; je dostupna je samo u vrsta računa spremišta blobova platforme.    

    Imajte na umu da kada odaberete to, sloju performanse postavljeno na standardni. Prostor za pohranu tiered nije dostupna s performansama sloju Premium.

6. Odaberite mogućnost replikacije za račun za pohranu: **LRS**, **GRS**ili **RA GRS**. Zadano je **RA GRS**.

    LRS = lokalno suvišnih prostora za pohranu; GRS = zemlj suvišnih prostora za pohranu (2 regije); RA GRS je pristup za čitanje zemlj suvišnih prostora za pohranu (2 regije s pristupom čitanja sekunde).

    Dodatne informacije o mogućnostima replikacije Azure prostora za pohranu potražite u članku [replikacije Azure prostora za pohranu](storage-redundancy.md).

7. Odaberite sloj desno za pohranu za vaše potrebe: Postavljanje **Razina pristupa** **zanimljivih** ili **Aktivno**. Zadano je **Aktivno**.

8. Odaberite pretplatu u koju želite stvoriti novi račun za pohranu.

9. Odredite novu grupu resursa ili odaberite postojeću grupu resursa. Dodatne informacije o grupama resursa potražite u članku [Pregled upravljanja resursima Azure](../azure-resource-manager/resource-group-overview.md).

10. Odaberite područje za vaš račun za pohranu.

11. Kliknite **Stvori** da biste stvorili račun za pohranu.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Promjena prostora za pohranu sloju računa spremišta blobova platforme pomoću portala za Azure

1. Prijavite se na [portal za Azure](https://portal.azure.com).

2. Da biste došli na račun servisa za pohranu, odaberite sve resurse, a zatim odaberite svoj račun za pohranu.

3. U plohu postavke kliknite **konfiguracije** za prikaz i/ili promijenite konfiguracija računa.

4. Odaberite sloj desno za pohranu za vaše potrebe: Postavljanje **Razina pristupa** **zanimljivih** ili **Aktivno**.

5. Kliknite Spremi pri vrhu na plohu.

> [AZURE.NOTE] Promjena prostora za pohranu sloju može uzrokovati dodatne troškove. Pogledajte odjeljak [određivanje cijena i naplata](storage-blob-storage-tiers.md#pricing-and-billing) za dodatne pojedinosti.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Procjena i migriranje s računima za spremište blobova platforme

Svrha ovaj odjeljak je korisnicima lakšeg prijelaza u putem računa za spremište blobova platforme. Postoje dva scenarija korisnika:

- Imate postojeći račun općenite namjene prostora za pohranu i želite procijeniti promjenu stvorite račun za pohranu Blob s sloju desno za pohranu.
- Ste odlučili pomoću računa za spremište blobova platforme ili već postoji i želite procijeniti li trebali biste koristiti sloju tipkovni ili zanimljivih prostora za pohranu.

U oba slučaja, prvi tvrtke slobode redoslijed je procjenu troškova pohranu i pristup podatke pohranjene u račun za spremište blobova platforme i usporedbu koji trenutno troškove.

### <a name="evaluating-blob-storage-account-tiers"></a>Procjena razine za račun spremište blobova platforme

Da bi se procjene troškova pohranu i pristup podacima koji su spremljeni na računu za spremište blobova platforme, morat ćete rezultirati vaš postojeći obrazac uporabe ili aproksimaciju na očekivani način korištenja uzorkom. Općenito govoreći, želite znati:

- Vaš potrošnje prostora za pohranu – kako količinom podataka koji se pohranjuju se i kako se to promijeniti mjesec?
- Vaše uzorak pristupa za pohranu - koliko je podataka se čitanja i napisali račun (uključujući nove podatke)? Koliko je transakcija koriste se za pristup podacima i vrste transakcije su?

#### <a name="monitoring-existing-storage-accounts"></a>Nadzor postojećih računa za pohranu

Prikupite te podatke i praćenje svoje postojeće račune za pohranu, možete učiniti korištenja analitičkih podataka za pohranu Azure koji izvršava zapisivanje i daje metriku podatke za račun za pohranu.
Prostor za pohranu analize možete spremiti metriku koji sadrže Zbrojeno transakcije Statistika i kapacitet podatke o zahtjevima za servis za pohranu Blob i općenite namjene prostora za pohranu računa kao i računi spremište blobova platforme.
U ovom podaci se pohranjuju u tablicama povezanima u isti račun za pohranu.

Dodatne informacije, pročitajte članak [O metrike pohrane analize](https://msdn.microsoft.com/library/azure/hh343258.aspx) i [Prostora za pohranu analize metriku tablice sheme](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] Računa spremišta blobova platforme Izloži krajnja točka servisa tablice samo za pohranu i pristup podacima metriku za taj račun.

Praćenje potrošnje prostora za pohranu za servis za pohranu Blob, morate omogućiti metriku kapaciteta.
Ovime omogućena, kapacitet podataka naveden dnevnih računa spremišta blobova platforme servisa, a snimljeni kao unos u tablicu napisan na tablicu *$MetricsCapacityBlob* unutar iste račun za pohranu.

Da biste pratili pristup uzorak podataka za servis za pohranu Blob, morate omogućiti svaki sat metriku transakcije na razini API-JA.
Ovime omogućena, po API transakcije su pridružuje svaki sat, a naveden kao unos u tablicu napisan *$MetricsHourPrimaryTransactionsBlob* tablicu unutar iste račun za pohranu. U tablici *$MetricsHourSecondaryTransactionsBlob* zapisa transakcije sekundarne krajnjoj u slučaju računa RA GRS prostora za pohranu.

> [AZURE.NOTE] U slučaju da imate račun za općenite namjene prostora za pohranu u kojem spremljenim blob-Ova stranica i virtualnog računala diskova duž blok i dodavanje blob podataka, postupak procjeni nije primjenjivo. To je zato na raspolaganju su vam nije moguće razlikuje kapaciteta i transakcije metrike koji se temelje na vrsti blobova platforme za samo blokirati, a zatim Dodaj blob-ova koji možete prenijeti na račun spremište blobova platforme.

Da biste dobili dobar usklađivanja podataka potrošnje i pristup uzorak, preporučujemo odaberite razdoblje zadržavanja za metriku koja je predstavnika običnog upotrebe, a Ekstrapoliranje.
Jedan je mogućnost zadržati metriku podatke sedam dana i prikupiti podatke svaki tjedan, za analizu na kraju mjeseca.
Druga mogućnost je zadržati podatke metriku zadnjih 30 dana i prikupljanje i analizirati podatke na kraju razdoblja 30 dana.

Detalje za omogućivanje prikupljanje i pregled podataka metriku, pročitajte članak, [Omogućivanje Azure prostora za pohranu metrika i prikaz podataka metriku](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Spremanje, pristupanju i preuzimanje analize podataka i naplaćivati baš kao i obične korisničkih podataka.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Korištenja korištenje metriku za procjenu troškova

##### <a name="storage-costs"></a>Troškovi spremanja

Zadnju stavku u na kapacitet metriku tablice *$MetricsCapacityBlob* s ključem u retku *"podatke"* prikazuje kapacitet pohrane troše korisničkih podataka.
Zadnju stavku u na kapacitet metriku tablice *$MetricsCapacityBlob* s ključa redak *"analize"* prikazuje kapacitet pohrane troše zapisnike analize.

Ovaj ukupni kapacitet troše oba korisničkih podataka i analize zapisnika (Ako je omogućeno) se zatim može koristiti za procjenu troškova pohrana podataka u račun za pohranu.
Klikanje stanja tijeka rada može se koristiti za procjenu izvodi troškove prostora za pohranu za blokiranje i dodavati blob-ova u računi općenite namjene prostora za pohranu.

##### <a name="transaction-costs"></a>Transakcije troškovi

Zbroj *'TotalBillableRequests'*preko svih stavki za API-JA u tablici metriku transakcije označava ukupan broj transakcije za taj određeni API-JA. *– primjerice*, ukupan broj transakcije *'GetBlob'* u određenom razdoblju može biti izračunat prema zbroju Ukupno zahtjeva za naplatu za sve stavke s ključem retka *' korisnika; GetBlob'*.

Da bi se procjene transakcije trošak računa spremišta blobova platforme morat ćete raščlanjuju transakcije u tri skupine jer oni su cjenovnom drugačije.

- Pisanje transakcije kao što su *"PutBlob"*, *"PutBlock"*, *"PutBlockList"*, *"AppendBlock"*, *"ListBlobs"*, *"ListContainers"*, *"CreateContainer"*, *"SnapshotBlob"*i *"CopyBlob"*.
- Brisanje transakcije kao što su *"DeleteBlob"* i *"DeleteContainer"*.
- Sve ostale transakcije.

Da bi se procjene transakcije troškova za račune općenite namjene prostora za pohranu, morate aggregate sve transakcije bez obzira operacija/API-JA.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Troškovi za prijenos podataka programa access i zemlj replikacije podataka

Tijekom analize prostora za pohranu ne nudi količinu podataka čitanja i zapisan s računom za pohranu, ga možete biti otprilike Procijenjena tako da pogledate tablici metriku transakcije.
Zbroj *'TotalIngress'* preko svih stavki za API-JA u tablici metriku transakcije označava ukupnu količinu podataka ingress u bajtova za taj određeni API-JA.
Na sličan način zbroj *"TotalEgress"* označava ukupni iznos izlazne podatke, u bajtova.

Da bi se procjene troškova podataka programa access za račune za spremište blobova platforme, morat ćete raščlanjuju transakcije u dvije grupe.

- Količinu podataka koji se dohvaćaju s računa za pohranu možete biti Procijenjena tako da pogledate zbroj *'TotalEgress'* prvenstveno operacija *"GetBlob"* i *"CopyBlob"* .
- Količinu podataka napisan na račun za pohranu možete biti Procijenjena tako da pogledate zbroj *'TotalIngress'* za prvenstveno *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* i *'AppendBlock'* operacije.

Trošak zemlj replikacije prijenos podataka za račune za spremište blobova platforme i moguće izračunati pomoću Procjena iznos podaci mogli unijeti u slučaju računa za pohranu GRS ili RA GRS.

> [AZURE.NOTE] Na primjer detaljnije o izračunavanju troškova za korištenje sloju tipkovni ili zanimljivih prostora za pohranu Molimo pogledajte najčešća pitanja vezana uz pod naslovom *"što su aktivno i najbolju razine pristupa i kako treba li određuju koji će se use?"* u [Azure prostora za pohranu cijene stranice](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Migracija postojeće podatke

Račun za spremište blobova platforme je oblici sadrže za pohranu samo blok i dodavati blob-ova. Postojećih računa općenite namjene prostora za pohranu koje omogućuju spremanje tablice, redove, datoteke i diskova kao i blob-ova, ne mogu pretvoriti u računa spremišta blobova platforme. Da biste koristili razine prostora za pohranu, morat ćete stvaranje novog računa spremišta blobova platforme i Migracija postojeće podatke u novostvorenu račune.
Migracija postojeće podatke u Blob prostora za pohranu račune s uređaja za pohranu na lokaciji, davatelji drugog davatelja usluge za pohranu ili postojećih računa općenite namjene prostora za pohranu u Azure možete koristiti na sljedeće načine:

#### <a name="azcopy"></a>AzCopy

AzCopy je Windows naredbenog retka utility namijenjen visokih performansi kopiranje podataka i iz Azure prostora za pohranu. AzCopy možete koristiti da biste kopirali podatke na račun za spremište blobova iz postojećih računa općenite namjene prostora za pohranu ili da biste prenijeli podatke iz lokalnih uređaja za pohranu na račun za spremište blobova platforme.

Dodatne informacije potražite u članku [prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Biblioteka za premještanje podataka

Azure prostora za pohranu podataka premještanje biblioteke za .NET temelji se na okvir za premještanje podataka core koji potencije AzCopy. Operacija prijenos visokih performansi, pouzdano i jednostavno podataka slično AzCopy namijenjen je u biblioteku. Omogućuje da iskoristite puno prednosti se značajke koje nudi AzCopy u aplikaciji nativno bez potrebe za baviti operacijski sustav, a vanjske instance AzCopy za nadzor.

Dodatne informacije potražite u članku [Azure prostora za pohranu podataka premještanje biblioteke za .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API-JA ili klijentska biblioteka

Možete stvoriti prilagođenu aplikaciju za migriranje podataka na račun za pohranu Blob pomoću jednog od biblioteke Azure klijenta i servise za pohranu Azure REST API-JA. Azure prostora za pohranu nudi biblioteke obogaćenog klijenta za više jezika i platforme kao što su .NET, Java, C++, Node.JS, PHP, fonetski zapis i Python. Biblioteka klijentski nude naprednih mogućnosti kao što su pokušaj logike, zapisivanje i paralelno prijenosa. Možete razviti i izravno u odnosu na REST API-JA, koji možete pozvati bilo koji jezik koji vam se čini zahtjeva za HTTP/HTTPS.

Dodatne informacije potražite u članku [Početak rada s spremište blobova platforme Azure](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Blob-ova šifriran pomoću šifriranja klijentsko pohranjivati metapodatke u vezane uz šifriranje spremljena s blob-om. Je apsolutno važnosti da sve kopije mehanizam potrebno je provjeriti se zadržava metapodataka blob, a posebice vezane uz šifriranje metapodatke. Ako kopirate blob-ova bez ti metapodaci, blob sadržaj neće biti veličina ponovno. Dodatne informacije vezane uz vezane uz šifriranje metapodataka potražite u članku [šifriranja na strani klijenta za pohranu Azure](storage-client-side-encryption.md).

## <a name="faqs"></a>Najčešća pitanja

1. **Računi za pohranu i dalje dostupne su postojeće?**

    Da, postojećih računa za pohranu i dalje su dostupni i neće se promijeniti u cijene ili funkcija.  Imate mogućnost da biste odabrali u sloju prostora za pohranu i neće imati tiering mogućnosti u budućnosti.

2. **Zašto i kada započeti putem računa za spremište blobova platforme?**

    Računa spremišta blobova platforme su oblici sadrže za pohranu blob-ova i dopustite nam se predstavlja nove značajke blob usmjereni na. Odsad nadalje, računi spremište blobova platforme su preporučeni način za pohranu blob-ova, kao budućih mogućnosti kao što su hijerarhijski prostora za pohranu i tiering će biti primijenjene na temelju vrstu računa. Međutim, nije do kada želite migrirati prema svojim potrebama tvrtke.

3. **Mogu li pretvoriti postojeći račun za pohranu na račun spremište blobova platforme?**

    ne. Račun za pohranu blob neku drugu vrstu računa za pohranu i morat ćete stvoriti novi i Migracija podataka kao što je opisano iznad.

4. **Li spremati objekte u obje razine prostora za pohranu u jedan račun?**

    Atribut *' pristup sloju '* koji pokazuje sloju prostora za pohranu postavljeno na razini računa, a odnosi se na sve objekte u taj račun. Atribut razina pristupa nije moguće postaviti na razini objekta.

5. **Je li moguće promijeniti sloj prostora za pohranu moj račun spremište blobova platforme?**

    Da. Ćete moći promijeniti sloju prostora za pohranu postavljanjem atribut *"pristup sloju"* na računu za pohranu. Promjena prostora za pohranu sloju primjenjuju se na sve objekte koji su spremljeni na računu. Promjena u prostor za pohranu sloju iz tipkovni najbolju će uzrokovati bilo kakvih troškova pri prijelazu sa najbolju da biste tipkovne će uzrokovati na po GB trošak za sve podatke na računu za čitanje.

6. **Koliko se često promjena prostora za pohranu sloj moj račun spremište blobova platforme?**

    Dok se ne možemo Nametni ograničenje na koliko se često se mogu mijenjati sloju prostora za pohranu, imajte na umu da promjena prostora za pohranu sloju iz najbolju da biste tipkovne će uzrokovati znatnog troškove. Ne preporučujemo često promjena prostora za pohranu sloju.

7. **Će blob-ova u sloju odlične za pohranu se drugačije ponašaju od onih u sloju tipkovni prostora za pohranu?**

    Blob-ova u sloju tipkovni prostora za pohranu imaju isti Latencija kao BLOB-ova u računi općenite namjene prostora za pohranu. Blob-ova u sloju odlične za pohranu imaju slične Latencija (u milisekundama) kao BLOB-ova u računi općenite namjene prostora za pohranu.

    Blob-ova u sloju odlične za pohranu imat će malo niže dostupnost servisa razine (SLA) od blob-ova pohranjene u sloju tipkovni prostora za pohranu. Dodatne informacije potražite u članku [SLA za pohranu](https://azure.microsoft.com/support/legal/sla/storage).

8. **Možete I spremiti blob-Ova stranica i diskova virtualnog računala u računi spremište blobova platforme?**

    Računa spremišta blobova platforme podržava samo blok i dodati blob-ova i stranice blob polja. Azure virtualnog računala diskova se sigurnosno po stranici blob-ova i kao rezultat računa spremišta blobova platforme ne može se koristiti za pohranu diskova virtualnog računala. No moguće je spremiti sigurnosne kopije diskova virtualnog računala kao bloka blob-ova u račun za spremište blobova platforme.

9. **Će morate promijeniti svoje postojeće aplikacije korištenje računa za spremište blobova platforme?**

    Računa spremišta blobova platforme su 100% API usklađene s računima općenite namjene prostora za pohranu za blok i dodavati blob-ova. Pod uvjetom da se aplikacija pomoću blokirati blob-ova ili dodati blob-ova, i koristite verziju 2014. 02 14 [Prostora za pohranu servisa REST API -JA](https://msdn.microsoft.com/library/azure/dd894041.aspx) ili noviji, a zatim aplikacija trebao raditi. Ako koristite stariju verziju protokol, zatim morat ćete ažuriranje aplikacije pomoću nove verzije biste jednostavan rad s obje vrste računa za pohranu. Općenito govoreći, uvijek preporučeno koristiti najnoviju verziju bez obzira na to koji račun za pohranu upišite koristimo.

10. **Hoće li se nalaziti promjene u korisnički doživljaj?**

    Računa spremišta blobova platforme vrlo su slične računi općenite namjene prostora za pohranu za pohranu blok i dodavanje blob-ova i podržava sve ključne značajke programa Azure prostor za pohranu, uključujući visoke rok trajanja i dostupnost, skalabilnost, performanse i sigurnost. Osim značajke i ograničenja određene račune spremište blobova platforme i njegov prostora za pohranu razine koji imaju oblačićima iznad, sve ostalo ostaje isto.

## <a name="next-steps"></a>Daljnji koraci

### <a name="evaluate-blob-storage-accounts"></a>Analiza računa spremišta blobova platforme

[Provjera dostupnosti računa spremišta blobova platforme po regijama](https://azure.microsoft.com/regions/#services)

[Analiza korištenja svoje trenutne račune za pohranu omogućivanjem metrike pohrane Azure](storage-enable-and-view-metrics.md)

[Provjera Blob prostora za pohranu cijene po regijama](https://azure.microsoft.com/pricing/details/storage/)

[Potvrdite podatke prijenosima cijene](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Početak korištenja računa spremišta blobova platforme

[Početak rada s spremište blobova platforme Azure](storage-dotnet-how-to-use-blobs.md)

[Premještanje podataka i iz spremišta Azure](storage-moving-data.md)

[Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)

[Pregledavanje i istraživanje svoje račune za pohranu](http://storageexplorer.com/)
