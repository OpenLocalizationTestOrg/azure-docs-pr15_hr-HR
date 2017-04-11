<properties
   pageTitle="Koristite Redgate na podataka platformu Studio da biste učitali podatke u SQL Data Warehouse | Microsoft Azure"
   description="Saznajte kako koristiti Redgate na podataka platformu Studio skladištenje scenarije podataka."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Učitavanje podataka s Redgate podataka platformu Studio

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Tvorničke podataka](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Pomoću ovog praktičnog vodiča pokazuje kako koristiti [Redgate na podataka platformu Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) da biste premjestili sadržaj iz sustava SQL Server na lokaciji Azure SQL Data Warehouse. Podataka platformu Studio primjenjuje najprikladnije kompatibilnošću i optimizacije, pa je najbrži način za početak rada s SQL Data Warehouse.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) je dugogodišnji Microsoft partner koji nudi razne alate za SQL Server. Značajka u podataka platformu Studio je učinio dostupnima slobodno za komercijalne i NEKOMERCIJALNU upotrebu.

## <a name="before-you-begin"></a>Prije početka
### <a name="create-or-identify-resources"></a>Stvaranje i prepoznavanje resursa

Prije pokretanja ovog praktičnog vodiča, morate koristiti:

- **Na lokaciji SQL Server baza podataka**: podatke koje želite uvesti u SQL Data Warehouse mora potjecati iz sustava SQL Server na lokaciji (verzija 2008R2 ili noviji). Studio platformu podataka ne možete uvesti podatke izravno iz baze podataka SQL Azure ili iz tekstne datoteke.
- **Račun za pohranu Azure**: podataka platformu Studio faze podatke u spremište blobova platforme Azure prije učitavanje u SQL Data Warehouse. Račun za pohranu morate koristiti model implementacije "Voditelj resursa" (zadano) umjesto "Klasični" model implementacije. Ako nemate račun za pohranu, Saznajte kako stvoriti račun za pohranu. 
- **SQL Data Warehouse**: ovog praktičnog vodiča premješta podatke iz lokalnog sustava SQL Server SQL Data Warehouse da morate imati podataka skladištu putem Interneta. Ako već nemate skladištu podataka, Saznajte kako stvoriti programa Data Warehouse SQL Azure.

> [AZURE.NOTE] Performanse je poboljšana Ako račun za pohranu i skladištu podataka stvaraju se na istom području.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Korak 1: Prijava u Studio platformu podataka pomoću računa za Azure
Otvorite web-preglednik i idite na [Studio platformu podataka](https://www.dataplatformstudio.com/) web-mjesta. Prijavite se pomoću isti Azure račun koji ste koristili za stvaranje skladištu račun i podataka za pohranu. Ako je vaša adresa e-pošte povezana sa tvrtke ili obrazovne ustanove poslovnog kontakta i Microsoftov račun, obavezno odaberite račun koji ima pristup resurse.

> [AZURE.NOTE] Ako je ovo prvi put pomoću Studio platformu podataka, što trebate dodijeliti dozvolu aplikacija za upravljanje Azure resurse.

## <a name="step-2-start-the-import-wizard"></a>Korak 2: Pokretanje čarobnjaka za uvoz
Na glavnom zaslonu DPS odaberite uvoz Azure SQL Data Warehouse vezu da biste pokrenuli čarobnjak za uvoz.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Korak 3: Instalirati pristupnik za Studio platformu podacima
Da biste povezali bazu podataka sustava SQL Server na lokaciji, morate instalirati pristupnik za DPS. Pristupnik je agent za klijenta koji omogućuje pristup okruženju na lokaciji, izdvaja podatke i prenosi na račun servisa za pohranu. Vaši podaci nikad prolazi kroz Redgate na poslužiteljima. Da biste instalirali pristupnika:

1.  Kliknite vezu za **Stvaranje pristupnika**
2. Preuzmite i instalirajte pristupnika pomoću navedenih instalacijski program

![][2]

> [AZURE.NOTE] Na bilo kojem računalu s pristupom mreži na izvorišne baze podataka SQL Server može instalirati pristupnik. Pristupa baze podataka SQL Server pomoću provjere autentičnosti sustava Windows s vjerodajnicama trenutnog korisnika.

Kad instalirate, status pristupnika se mijenja povezan i možete odabrati sljedeće.

## <a name="step-4-identify-the-source-database"></a>Korak 4: Prepoznavanje izvorišne baze podataka
U tekstni okvir *Unesite naziv poslužitelja* unesite naziv poslužitelja koji hostira baze podataka, a zatim odaberite **Dalje**. Zatim na padajućem izborniku odaberite bazu podataka koju želite uvesti podatke iz.

![][3]

DPS provjerava ima li odabrane baze podataka za tablice želite uvesti. Prema zadanim postavkama DPS uvozi sve tablice u bazi podataka. Možete potvrdite ili poništite tablice proširenjem vezu sve tablice. Odaberite gumb Dalje da biste ubrzali.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Korak 5: Odaberite račun za pohranu za faze podataka
DPS vas za mjesto faze podatke. Odaberite postojeći račun za pohranu iz pretplate, a zatim odaberite **Dalje**.

> [AZURE.NOTE] DPS će stvoriti novi spremnik blob u odabranom prostora za pohranu računa i korištenje različitih mape za svaki uvoz.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Korak 6: Odaberite podataka skladištu
Nakon toga odaberite online [Azure SQL Data Warehouse](http://aka.ms/sqldw) baze podataka da biste uvezli podatke u. Kada odaberete bazu podataka, morate unijeti vjerodajnice za povezivanje s bazom podataka, a zatim odaberite **Dalje**.

![][5]

> [AZURE.NOTE] DPS spaja podatkovne tablice izvora u skladištu podataka. DPS će vas upozoriti ako naziv tablice zahtijeva da biste prebrisali postojeće tablice u skladištu podataka. Možete po želji izbrisati sve postojeće objekte u skladištu podataka po ticking Izbriši sve postojeće objekte prije uvoza.

## <a name="step-7-import-the-data"></a>Korak 7: Uvoz podataka
DPS potvrditi koje želite uvesti podatke. Jednostavno kliknite gumb Start uvoz da biste započeli uvoz podataka.

![][6]

DPS prikazuje vizualizacije koje prikazuju tijek izdvajanje i prijenos podataka iz sustava SQL Server na lokaciji i tijek uvoza u SQL Data Warehouse.

![][7]

Kada se uvoz dovrši, DPS prikazuje sažetak uvoz podataka i promjena izvješća popravaka kompatibilnosti koji se izvode.

![][8]

## <a name="next-steps"></a>Daljnji koraci
Da biste istražili podataka unutar SQL Data Warehouse, najprije prikaz:

- [Upit Azure SQL Data Warehouse (Visual Studio)][]
- [Vizualni prikaz podataka pomoću dodatka Power BI][]

Dodatne informacije o Redgate na podataka platformu Studio:

- [Posjetite početnu stranicu DPS](http://www.dataplatformstudio.com/)
- [Pogledajte pokazni videozapis o DPS na Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Pregled drugi načini migracije i učitavanje podataka u SQL Data Warehouse potražite u članku:

- [Prenesite rješenje SQL Data Warehouse][]
- [Podatke učitali u skladištu podataka za SQL Azure](./sql-data-warehouse-overview-load.md)

Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Upit Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Vizualni prikaz podataka pomoću dodatka Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Prenesite rješenje SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md