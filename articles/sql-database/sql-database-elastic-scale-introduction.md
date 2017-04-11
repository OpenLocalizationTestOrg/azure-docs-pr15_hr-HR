<properties
    pageTitle="Skaliranja izlaz s bazom podataka SQL Azure | Microsoft Azure"
    description="Softver kao razvojnim inženjerima servisa (SaaS) možete jednostavno stvoriti elastic, skalabilni baze podataka u oblak pomoću tih alata"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Promjena veličine odgovor s bazom podataka SQL Azure

Jednostavno mogu mijenjati veličinu izvan baze podataka Azure SQL pomoću alata za **Elastic baze podataka** . Ove alata i značajki omogućuju korištenje resursa gotovo neograničen baze podataka baze podataka **SQL Azure** za stvaranje rješenja za transakcijskih radnih opterećenja i osobito softver kao aplikacije servisa (SaaS). Elastic značajke bazi podataka sastoje se od sljedećeg:

* [Biblioteka klijentski elastic baze podataka](sql-database-elastic-database-client-library.md): klijentska biblioteka je značajka koja vam omogućuje stvaranje i održavanje sharded baze podataka.  Potražite u članku [Početak rada s alatima Elastic baze podataka](sql-database-elastic-scale-get-started.md).
* [Elastic alata baze podataka podijeljene cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md): premješta podatke između sharded baza podataka. To je korisno za premještanje podataka iz više klijentske baze podataka s jednog klijenta bazom podataka (ili obratno). Potražite u članku [Elastic baze podataka vodič alata za podjelu cirkularnog pisma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Poslovi elastic baze podataka](sql-database-elastic-jobs-overview.md) (pretpregled): upravljanje velikim brojem baze podataka Azure SQL pomoću zadataka. Jednostavno izvođenje administratora operacija poput promjene sheme, upravljanje vjerodajnicama, ažuriranja podataka referenca, zbirka podataka o performansama ili klijenta (Kupac) telemetrijskih zbirke pomoću zadataka.
* [Upit elastic baze podataka](sql-database-elastic-query-overview.md) (pretpregled): omogućuje vam da biste pokrenuli Transact-SQL upit koji obuhvaća više baza podataka. Time se omogućuje povezivanje alate za izvješćivanje kao što je Excel, PowerBI, Tableau itd.
* [Elastic transakcije](sql-database-elastic-transactions-overview.md): Ova značajka omogućuje vam pokretanje transakcije koje se protežu na nekoliko baza podataka u bazi podataka SQL Azure. Transakcije elastic baze podataka dostupni su za .NET aplikacije pomoću ADO .NET i integracija s poznato okruženje za programiranje pomoću [klase System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx).

Grafika ispod pokazuje programa arhitekture koja sadrži **značajke Elastic baza podataka** odnosu zbirka baze podataka.

U ovoj grafici boje baze podataka predstavljaju sheme. Baza podataka s istom bojom zajednički koristiti istu shemu.

1. Skup **baze podataka Azure SQL** nalaze se na Azure pomoću sharding arhitektura.
2. **Biblioteka klijentski Elastic baze podataka** koristi se za upravljanje skup shard.
3. Podskup baza podataka spremaju se u **skup Elastic baze podataka**. (Potražite u članku [što je zajedničko područje?](sql-database-elastic-pool.md)).
4. Pokreće zakazani **zadatak Elastic baze podataka** ili ad-hoc T-SQL skripte u odnosu na sve baze podataka.
5. **Alat za podjelu cirkularnog pisma** koristi se za premještanje podataka iz jedne shard na drugi.
6. **Upit Elastic baze podataka** omogućuje sastaviti upit koji obuhvaća sve baze podataka u skupu shard.
7. **Elastic transakcije** omogućuje vam pokretanje transakcije koje se protežu na nekoliko baza podataka. 


![Elastic Alati baze podataka][1]


## <a name="why-use-the-tools"></a>Zašto koristiti alate?

Postizanje elasticity i skaliranje za oblak aplikacije je jednostavne za VMs i blobova – jednostavno dodavanje i oduzimanje jedinica ili povećati power. No ona sadrži pojavili test za s praćenjem stanja obradu podataka u relacijskim bazama podataka. Izazove emerged u sljedećim scenarijima:

* I kapacitet za relacijske baze podataka dio svoje radno opterećenje.
* Upravljanje pristupne točke koji se može pojaviti utječe na određeni podskup podataka – kao što je osobito zauzet završetka-klijenta (klijentsko).

Najčešći scenariji kao što su ovi su je adresirana tako da morate uložiti u bazi podataka za opsežne poslužitelje za podršku aplikacije. No tu mogućnost je ograničen u oblaku gdje se događa sve obrada na unaprijed definirane obavješćivanje hardveru. Umjesto toga raspodjela podataka i obradu preko mnoge baze podataka jednak način strukturiranim (skaliranje iz uzorak naziva "sharding") alternativa tradicionalni pristupa skaliranje gore i trošak i elasticity.

## <a name="horizontal-and-vertical-scaling"></a>Vodoravna i okomita skaliranja

Na slici u nastavku prikazuje vodoravne i okomite dimenzije skaliranja koje su osnovna načina elastic baze podataka možete skalirana.

![Vodoravna i okomita Scaleout][2]

Vodoravno skaliranje se odnosi na dodavanjem ili uklanjanjem baze podataka da biste prilagodili kapaciteta ili ukupne performanse. Ovo se zove "skaliranje". Sharding, u kojem je particije podataka u cijeloj zbirci jednak način strukturirane baza podataka, je na uobičajeni način implementaciju vodoravno skaliranje.  

Okomito skaliranje se odnosi na ćelije rastućim razinom performansi pojedinačne baze podataka ili – to je također poznato kao "skaliranje prema gore."

Većina oblaka skaliranje aplikacija za baze podataka će koristiti kombinacijom tih dvaju strategije. Ako, na primjer, softver kao servisne aplikacije možda dopustili baze podataka za svaki Kraj-klijentima Povećaj ili Smanji resurse tako da povećavaju koristi vodoravno skaliranje Dodjela novim klijentima i okomito skaliranje.

* Vodoravno skaliranje se upravlja pomoću [Biblioteka klijentski Elastic baze podataka](sql-database-elastic-database-client-library.md).

* Okomito skaliranje se postiže pomoću cmdleta ljuske PowerShell Azure da biste promijenili sloju servisa ili potvrđivanjem baze podataka u elastic grupe aplikacija.

## <a name="sharding"></a>Sharding

*Sharding* je postupak za velike količine podataka jednak način strukturiranim raspodijelite broj nezavisnih baze podataka. To je osobito popularne s razvojnim inženjerima oblaka stvaranje softver kao usluge (SAAS) ponude za krajnji korisnici i tvrtke. Ove krajnji korisnici često nazivaju se "klijenata". Sharding može biti potrebni za bilo koji broj razloga:  

* Ukupni iznos podataka je prevelika da stane u ograničenja jedne baze podataka
* Transakcije propusnost cjelokupan radno opterećenje premašuje mogućnosti jedne baze podataka
* Klijenti možda ćete morati fizičke odvajanja međusobno, tako da su vam potrebne zasebnim bazama podataka za svaki klijent
* Različite odjeljke baze podataka možda morati nalaze u različitim geographies za usklađenost, performanse ili Geopolitički razloga.

U drugim situacijama, kao što je ingestion podataka iz raspodijeljeno uređaji sharding može koristiti za popunjavanje skupa baze podataka koje su organizirane dostavljanju. Na primjer, zasebnu bazu podataka možete namjenski svakom dana ili tjedana. U tom slučaju ključ sharding može biti cijeli broj koji predstavlja datum (postoje u svim recima sharded tablica) i upiti Dohvaćanje informacija za raspon datuma moraju usmjerili aplikacije podskup baza podataka koji prekriva dotičnog raspon.

Sharding najbolje funkcionira prilikom svakog transakcija u aplikaciji može biti ograničeno na jednu vrijednost odgovarajućeg ključa sharding. Osigurati da sve transakcije biti lokalni određenu bazu podataka.

## <a name="multi-tenant-and-single-tenant"></a>Više klijenta i jednog klijenta

Neke aplikacije pomoću najjednostavniji način stvaranja zasebnu bazu podataka za svaki klijent. Ovo je **Uzorak sharding jednog klijenta** koji omogućuje odvajanja, mogućnost sigurnosnog kopiranja i vraćanja i resursa skaliranje na preciznosti klijentu. S jednog klijenta sharding, svaku bazu podataka je pridružen vrijednost ID-a određene klijenta (ili vrijednost ključa klijenta), ali te tipke ne uvijek biti prisutne u same podatke. Je odgovornosti aplikacije da biste usmjerili svaki zahtjev na odgovarajuće bazi podataka, i to može pojednostavniti klijentska biblioteka.

![Jednog klijenta i više klijenta][4]

Drugi scenariji zajedno pack više klijenata u bazama podataka, umjesto da ih izoliranja u zasebnim bazama podataka. Ovo je uobičajeni **Uzorak više klijentu sharding** – i mogu biti vođene po fact aplikaciju upravlja je velikog broja vrlo malen klijenata. U sharding klijenta s više redaka u tablicama baze podataka sve dizajnirani provesti ključ prepoznavanje ID klijenta ili sharding ključ. Ponovno je razina aplikacije zadužen za usmjeravanje zahtjev na klijentu odgovarajuće bazu podataka, a to može podržavati klijentska biblioteka elastic baze podataka. Uz to, filtar koji su reci svaki klijent možete pristupiti – dodatne informacije potražite u članku [više klijentske aplikacije s Alati elastic baze podataka i sigurnost na razini retka](sql-database-elastic-tools-multi-tenant-row-level-security.md)mogu se sigurnosti na razini retka. Smije podataka između baza podataka možda je potreban s uzorkom sharding više klijentu, a to je olakšano po elastic alata baze podataka podijeljene cirkularnog pisma. Da biste saznali više o dizajn obrazaca za SaaS aplikacije koje se koriste elastic grupe, uvidjeti [dizajna za više klijentske aplikacije SaaS s bazom podataka SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Premještanje podataka iz više na jednom tenancy baze podataka

Prilikom stvaranja aplikacije SaaS je Tipični nudi potencijalnim klijentima probne verzije softvera. U ovom slučaju je učinkovit za korištenje baze podataka za više klijenta za podatke. Međutim, kad je potencijalni klijent postane klijenta, jednog klijenta baza podataka nije bolje jer pruža bolje performanse. Ako je korisnik odabrao stvorili podataka tijekom probnog razdoblja, premještanje podataka iz više klijenta na nove jednostruko klijentske baze podataka pomoću [alata za podjelu cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md) .

## <a name="next-steps"></a>Daljnji koraci

Ogledna aplikacije koji pokazuje klijentska biblioteka, potražite u članku [Početak rada s alatima Elastic Datababase](sql-database-elastic-scale-get-started.md).

Pretvaranje postojeće baze podataka pomoću alata potražite u članku [Migracija postojeće baze podataka da bi skaliranje izlaz](sql-database-elastic-convert-to-use-elastic-tools.md).

Da biste vidjeli specifičnosti skup Elastic baze podataka, u odjeljku [cijena i performanse zahtjevi za grupe aplikacija programa elastic bazu podataka](sql-database-elastic-pool-guidance.md)ili stvorite novi skup s [Praktični vodič](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

