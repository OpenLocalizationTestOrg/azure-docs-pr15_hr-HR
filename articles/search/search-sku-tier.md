<properties
    pageTitle="Odaberite SKU ili cijene sloju za pretraživanje Azure | Microsoft Azure"
    description="Azure pretraživanja koje se mogu dodjeli pri te SKU-ove: besplatne osnovne i standardne, gdje standardnu dostupna je u raznim konfiguracije resursa i kapaciteta razine."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Odaberite SKU ili cijene sloju za pretraživanje Azure

U odjeljku pretraživanje Azure na [servisu dodijeljeni resursi](search-create-service-portal.md) na određene cijene sloju ili SKU-om. Mogućnosti obuhvaćaju **slobodno**, **osnovne**i **standardne**, gdje **standardne** dostupna je u više konfiguracije i kapaciteta. 

Svrha u ovom se članku će vam pomoći da odaberete sloj. Ako je u sloju kapaciteta Pretvori biti pretihi, morat ćete Dodjela resursa za novog servisa na višu sloju i zatim ponovno učitajte vaše indeksi. Postoji bez nadogradnje iste usluge iz jednog SKU na drugi. 

> [AZURE.NOTE] Nakon što odaberete sloju i [Dodjela resursa za servis za pretraživanje](search-create-service-portal.md), možete povećati replike i particija broji unutar servisa. Upute, pročitajte odjeljak [mjerilo razine resursa za upit i indeksiranja radnih opterećenja](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Kako postići cijene sloju odluka

U odjeljku pretraživanje Azure u sloju određuje kapaciteta, dostupnost značajki. Općenito govoreći, značajke dostupne su na svaku sloju, uključujući značajke. Jednu iznimku je ne podržava indexers u S3 HD.

> [AZURE.TIP] Preporučujemo da Dodjela servisa **slobodno** (jedna po pretplate s nema isteka) tako da njegov uvijek su dostupne za osnovne projekte. Korištenje **slobodno** servis za testiranje i procjenu; Stvaranje drugog servisa za naplatu na sloju **osnovne** i **standardne** za proizvodnju ili veća radnih opterećenja test.

Kapacitet i troškove pokrenut servis otvorite rukom u skladištu. Informacije u ovom članku pomoći da odlučite koji se SKU nudi desnom salda, ali bilo koji od da bi bio koristan, potreban vam je najmanje gruba procjenjuje na sljedeće:

- Broj i veličinu indeksi tek planirate stvoriti
- Broj i veličinu dokumenata da biste prenijeli
- Neke ideja opseg upita, pomoću upita po Second (QPS)

Broj i veličinu važna su jer su Maksimalna ograničenja pristupiti putem teško ograničenje broj indeksa ili dokumente na servis i resursa (prostora za pohranu ili replike) servis koristi. Stvarni ograničenje servisa je ovisno o tome što može vidjeti korištena za prvo: resursa ili objekte.

S procjenjuje u skladištu na sljedeći način trebali biste pojednostavili postupak:

- **Korak 1** Pregledajte SKU Opisi ispod dodatne informacije o dostupnim mogućnostima.
- **Korak 2** Odgovoriti na pitanja u nastavku stigne u preliminarni odluka.
- **Korak 3** Dovršavanje vaša odluka tako da pročitate teško ograničenja prostora za pohranu i cijene.

## <a name="sku-descriptions"></a>Opisi SKU

Sljedeća tablica sadrži opise svake sloju. 

Razina|Primarni scenariji
----|-----------------
**Besplatni**|Zajednički servis, bez troškova koristi za procjenu, istrage i small radnih opterećenja. Budući da je zajednički koristite druge pretplatnike, propusnost upita i indeksiranja mijenja se ovisno o tko je još je pomoću servisa. Kapacitet je small (50 MB ili 3 indeksi s gore 10 000 dokumenti svaki).
**Osnovni**|Mali radnog radnih opterećenja na namjenski hardveru. Iznimno dostupno. Kapacitet je do 3 replike te 1 particija (2 GB).
**S1**|Standardni 1 podržava fleksibilne kombinacije particije (12) i replike (12), koristi za srednje radnog radnih opterećenja na namjenski hardveru. Možete dodijeliti particije i replike u kombinacije podržava maksimalan broj jedinica 36 naplatu pretraživanja. Na ovoj razini su 25 GB i QPS je otprilike 15 upita u sekundi.
**S2**|Standardni 2 pokreće veće radnih opterećenja radnog pomoću iste jedinice 36 pretraživanja kao S1, ali s većim veličine particije i replike. Na ovoj razini su 100 GB i QPS je otprilike 60 upita u sekundi.
**S3**|Standardni 3 pokreće proporcionalno veće radnih opterećenja radnog na višu sustavima završetka u konfiguracije najviše 12 particije ili 12 replike jedinice za pretraživanje u odjeljku 36. Na ovoj razini su 200 GB i QPS je više od 60 upita u sekundi. 
**S3 HD**|Standardni 3 visoke gustoće namijenjen je velik broj manji indeksi. Imate najviše 3 particije na 200 GB. QPS je više od 60 upita u sekundi. 

> [AZURE.NOTE] Replike i particija maximums se naplatiti izgleda kao jedinice za pretraživanje (36 jedinica Maksimalna po servisa), koji nameće donju granicu učinkovitih nego što maksimum podrazumijeva nominalne vrijednosti. Na primjer, da biste koristili najviše 12 replike, nije moguće imate najviše 3 particije (12 * 3 = 36 jedinice). Isto tako, da biste koristili Maksimalna particije, smanjite replike 3. Grafikon na dozvoljenu kombinacije potražite u članku [Skaliranje resursa razine upita i indeksiranja radnih opterećenja u pretraživanju Azure](search-capacity-planning.md) .

## <a name="review-limits-per-tier"></a>Pregled ograničenja po sloju

Sljedeći grafikon je podskup ograničenja od [Ograničenja servisa Azure pretraživanja](search-limits-quotas-capacity.md). Popisuje čimbenika najvjerojatnije utjecati na SKU odluka. Može se odnositi na tom grafikonu prilikom pregleda pitanja u nastavku.

Resurs|Besplatni|Osnovni|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Ugovor o razini usluge (SLA)|Nema <sup>1</sup> |Da |Da  |Da |Da  |Da 
Ograničenja indeksa|3|5|50|200|200|1000 <sup>2</sup>
Ograničenja za dokument|10 000 Ukupno|1 milijuna po servisa|15 milijuna po partition |60 milijuna po partition|120 milijuna po partition |1 milijuna po indeksa
Maksimalna particije|N/D |1 |12  |12 |12|3 <sup>2</sup>
Veličina particije|50 MB ukupno|2 GB po servisa|25 GB po partition |100 GB po particija (maksimalno do 1.2 TB po servisa)|200 GB po particija (maksimalno do 2,4 TB po servisa)|200 GB (maksimalno do 600 GB po servisa)
Maksimalna replike|N/D |3 |12 |12 |12|12
Upiti u sekundi|N/D|~ 3 po replike|~ 15 po replike|~ 60 po replike|> 60 po replike|> 60 po replike

<sup>1</sup> slobodno i pretpregled SKU-ove dobili SLA. SLA provode kada SKU postane načelu dostupan.

<sup>2</sup> S3 i S3 HD se sigurnosno Infrastruktura identične visoke kapaciteta, no svaki od njih je dostigne maksimalno ograničenje na različite načine. S3 pronalaze manji broj vrlo velike indeksa. Kao takve, njegov maksimalno ograničenje je resursa granica (2,4 TB za svaki servis). S3 HD pronalaze velikog broja vrlo malen indeksi. Pri 1000 indeksi S3 HD dosegne njegov ograničenja u obliku ograničenja indeksa. Ako ste korisnik sustava S3 HD koji zahtijeva više od 1000 indeksi, obratite se Microsoft Support informacije o tome da biste nastavili.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Uklonite SKU-ove koji ne zadovoljava preduvjete 

Sljedeća pitanja pojednostavljuju dolaze desnom odluka SKU-om za svoje radno opterećenje.

1. Imate li **Ugovor o razini na usluge (SLA)** preduvjeti? Suzite odluka SKU osnovni ili nisu pretpregleda Standard.
2. **Indeksi koliko** želite obavezan? Jedna od najvećih varijable factoring u odluka SKU jest broj indeksa podržava svaki SKU. Podrška za indeks je izrazito različite stupnjeve u donjem cijene razine. Preduvjeti na broj indeksa može biti primarni Determinanta odluka SKU-om.
3. **Koliko dokumenata** bit će učitana u svaki indeks? Broj i veličinu dokumenata određuje veličinu usmjerenog indeksa. Pod pretpostavkom da možete procjenu predviđene veličinu indeks, možete usporediti taj broj o veličini particija po SKU-om, prošireni prema broju particije potrebnih za spremanje indeksa te veličine. 
4. **Što je učitavanja očekivani upita**? Kada su prepoznata preduvjeti za pohranu, razmislite o radnih opterećenja upita. S2 i oba S3 SKU-ove nude propusnost blizu jednaku vrijednost, ali će SLA preduvjeti pravilo sve Preview SKU-ove. 
5. Ako namjeravate sloju S2 ili S3, odredite hoće li vam potrebne [indexers](search-indexer-overview.md). Indexers još nisu dostupne za sloju S3 HD. Zamjenski pristup je model možete koristiti automatskih ažuriranja indeksa pišu aplikacije kod da biste skupa podataka na indeksa.

Većina kupaca možete pravilo određene SKU ili smanjili na temelju njihove odgovore na pitanja gore. Ako još uvijek niste sigurni koji SKU-om za, objavite pitanja na forumima MSDN ili StackOverflow ili obratite se podršci za Azure Dodatne upute.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Odluke provjere valjanosti: ne se SKU nude dovoljno prostora za pohranu i QPS?

Ponovo posjetite [-servisa i po indeks odjeljka ograničenja servisa](search-limits-quotas-capacity.md) i [cijene stranice](https://azure.microsoft.com/pricing/details/search/) da biste ponovno procjene u odnosu na pretplatu i usluge ograničenja kao posljednji korak. 

Ako su preduvjeti cijenu ili prostora za pohranu iz granica, možda ćete morati refactor radnih opterećenja među većem broju servisa manje (na primjer). Na razini precizniji, nije moguće promijeniti dizajn indeksi da je manji ili pomoću filtara za upite omogućuju učinkovitije.

> [AZURE.NOTE] Preduvjeti za pohranu može biti previše inflated ako sadrže druge podatke. Najbolje dokumenta sadrže samo pretraživati podatke ili metapodataka. Binarni podaci ne mogu pretraživati i treba pohraniti zasebno (možda je Azure tablice ili blob prostora za pohranu) s poljem u indeksu držati reference URL-a s vanjskim podacima. Maksimalna veličina pojedinačnih dokumenata je 16 MB (ili manje ako ste skupno prijenos više dokumenata u jedan zahtjev). Dodatne informacije potražite u članku [ograničenja servisa Azure pretraživanja](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Sljedeći korak

Kada znate što SKU je desno Prilagodi, nastavite na pomoću ovih koraka:

- [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md)
- [Promjena dodijeljeni particije i replike za promjenu veličine na servisu](search-capacity-planning.md)

