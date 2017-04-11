<properties
   pageTitle="Vizualizacija podataka SQL Data Warehouse pomoću dodatka Power BI Microsoft Azure"
   description="Vizualni prikaz podataka za SQL Data Warehouse pomoću dodatka Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Vizualni prikaz podataka pomoću dodatka Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure strojnog učenja](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Pomoću ovog praktičnog vodiča pokazuje kako koristiti Power BI za povezivanje s SQL Data Warehouse i stvoriti nekoliko osnovnih vizualizacija.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Preduvjeti

Da biste se pomicali kroz ovaj Praktični vodič, potrebno je:

- SQL Data Warehouse unaprijed učitani s bazom podataka AdventureWorksDW. Dodjela to, potražite u članku [Stvaranje SQL Data Warehouse][] i odaberite da biste učitali ogledne podatke. Ako već imate podatke skladištu, no ne sadrži ogledne podatke, možete ga [ručno učitavanje ogledne podatke][].


## <a name="1-connect-to-your-database"></a>1. povezivanje s bazom podataka

Da biste otvorili Power BI i povezivanje s bazom podataka AdventureWorksDW:

1. Prijavite se na [portal za Azure][].
2. Kliknite **baze podataka SQL** i odaberite bazu podataka AdventureWorks SQL Data Warehouse.

    ![Pronađite bazu podataka][1]

3. Kliknite gumb Otvori u dodatku Power BI.

    ![Gumb Power BI][2]

4. Prikazat će se sada stranici vezi SQL Data Warehouse prikaz web-adresu baze podataka. Kliknite Dalje.

    ![Veze za Power BI][3]

6. Unesite Azure SQL server korisničko ime i lozinku, a će se potpuno povezati s bazom podataka SQL Data Warehouse.

    ![Power BI za prijavu][4]

7. Kada ste se prijavili u Power BI, kliknite skup podataka AdventureWorksDW na lijevom plohu. Otvorit će se u bazu podataka.

    ![Otvaranje AdventureWorksDW za Power BI][5]



## <a name="2-create-a-report"></a>2. Stvaranje izvješća

Sada ste spremni za korištenje servisa Power BI da biste analizirali AdventureWorksDW oglednim podacima. Da biste izvršili analizu, AdventureWorksDW sadrži prikaz naziva AggregateSales. Ovaj prikaz sadrži nekoliko ključa metriku za analizu prodaje tvrtke.

1. Da biste stvorili karte iznosa prodaje prema poštanski broj, u oknu desnom polja, kliknite prikaz AggregateSales da biste je proširili. Kliknite Stupci poštanski broj i IznosProdaje da biste ih odabrali.

    ![Power BI odaberite AggregateSales][6]

    Power BI automatski prepoznaje Ovo je geografske podatke i vratite na karti umjesto vas.

    ![Power BI karte][7]

2. Ovaj korak stvara trakasti grafikon koji prikazuju iznos prodaje po dobit klijenta. Da biste stvorili to Idi na proširenom AggregateSales prikaz. Kliknite polje u IznosProdaje. Povucite polje dobit klijenta s lijeve strane i ispustite ga u osi.

    ![Power BI odaberite osi][8]

    Trakasti grafikon smo prijeđete preko s lijeve strane.

    ![Power BI traka][9]

3. Ovaj korak stvara linijski grafikon koji prikazuju iznos prodaje po datumu narudžbe. Da biste stvorili to Idi na proširenom AggregateSales prikaz. Kliknite IznosProdaje i DatumNarudžbe. Vizualizacija stupca kliknite ikonu linijski grafikon; Ovo je prvu ikonu u drugom retku u odjeljku vizualizacije.

    ![Power BI odaberite linijski grafikon][10]

    Sada imate izvješće koje prikazuje tri različite vizualizacije podataka.

    ![Power BI retka][11]

Tijek rada možete spremiti u bilo kojem trenutku tako da kliknete karticu **datoteka** , a zatim odaberete **Spremanje**.

## <a name="next-steps"></a>Daljnji koraci
Nakon što smo ste vam dao neko vrijeme da se Toplo prema gore s oglednim podacima, potražite u članku kako [razviti][], [Učitavanje][]ili [migrirati][]. Ili razmotrite sustava [web-mjesto servisa Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[Migriranje]: sql-data-warehouse-overview-migrate.md
[razvoj]: sql-data-warehouse-overview-develop.md
[Učitavanje]: sql-data-warehouse-overview-load.md
[ručno učitavanje oglednih podataka]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Stvaranje SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Portal za Azure]: https://portal.azure.com/
[Power BI web-mjesta]: http://www.powerbi.com/
