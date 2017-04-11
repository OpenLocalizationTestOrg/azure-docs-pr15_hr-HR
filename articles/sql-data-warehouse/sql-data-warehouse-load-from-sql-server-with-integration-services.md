<properties
   pageTitle="Učitavanje podataka iz sustava SQL Server u Azure SQL podataka skladištu (SSIS) | Microsoft Azure"
   description="Prikazuje kako stvoriti paket sustava SQL Server integraciju servisa (SSIS) za premještanje podataka iz raznih izvora podataka u SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Učitavanje podataka iz sustava SQL Server u Azure SQL podataka skladištu (SSIS)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Stvaranje paketa za SQL Server integraciju servisa (SSIS) da biste učitali podatke iz sustava SQL Server u Azure SQL Data Warehouse. Možete po želji Preustroj, pretvaranje i cleanse podatke kao što je prolazi kroz tijeku SSIS.

U ovom ćete praktičnom vodiču ćete:

- Stvorite novi projekt Integracijskih usluga u Visual Studio.
- Povezivanje s izvorima podataka, uključujući SQL Server (kao što je izvor) i SQL Data Warehouse (kao što je odredište).
- Dizajnirajte SSIS paketa koja učitava podatke iz izvora u odredište.
- Pokrenite SSIS paket da biste učitali podatke.

Pomoću ovog praktičnog vodiča koristi SQL Server kao izvor podataka. SQL Server može imati lokalno ili na Azure virtualnog računala.

## <a name="basic-concepts"></a>Osnovni koncepti

Paket je jedinica posla u SSIS. Srodni paketa grupiraju se u projektima. Stvaranje projekata i dizajn paketa u Visual Studio sa SQL Server Data Tools. Postupka dizajniranja je vizualni proces u kojem ste povucite i ispustite komponente iz alatnog okvira na površinu za dizajniranje, povezati ih i postavljanje njihova svojstva. Nakon što završite vašem paketu, možete po želji njegove implementacije sustava SQL Server za sveobuhvatno upravljanje, praćenje i sigurnost.

## <a name="options-for-loading-data-with-ssis"></a>Mogućnosti za učitavanje podataka s SSIS

SQL Server integraciju servisa (SSIS) je fleksibilne skup alata koji pruža mnoštvo mogućnosti za povezivanje i učitavanja podataka u SQL Data Warehouse.

1. Korištenje programa odredište neto ADO za povezivanje s SQL Data Warehouse. Pomoću ovog praktičnog vodiča koristi se odredište neto ADO jer ima najmanje mogućnosti konfiguracije.
2. Korištenje programa OLE DB odredište za povezivanje s SQL Data Warehouse. Ta mogućnost može vam dati malo bolje performanse od ADO neto odredište.
3. Pomoću zadataka za prijenos blobova platforme Azure faze podatke u spremište blobova platforme Azure. Da biste pokrenuli Polybase skriptu koja učitava podatke u SQL Data Warehouse koristiti SQL izvršavanje SSIS zadataka. Ta mogućnost nudi najbolje performanse tri mogućnosti navedene ovdje. Da biste zadatak prijenos blobova platforme Azure, preuzmite [Microsoft SQL Server 2016 integraciju servisa značajka Pack za Azure][]. Dodatne informacije o Polybase potražite u članku [Vodič za PolyBase][].

## <a name="before-you-start"></a>Prije početka

Da biste se pomicali kroz ovaj Praktični vodič, potrebno je:

1. **SQL Integracijskim uslugama poslužitelja (SSIS)**. SSIS je komponenta korisničkog sučelja sustava SQL Server i potreban je probna verzija ili licenciranu verziju sustava SQL Server. Probna verzija sustava SQL Server 2016 Preview, pročitajte članak [Procjene SQL Server][].
2. **Visual Studio**. Da biste nabavili besplatne Visual Studio 2015 zajednice izdanje, potražite u članku [Visual Studio zajednice][].
3. **SQL Server Data Tools za Visual Studio (SSDT)**. SQL Server Data Tools za Visual Studio 2015, pročitajte članak [Preuzimanje SQL Server podataka Alati (SSDT)][].
4. **Ogledne podatke**. Pomoću ovog praktičnog vodiča koristi ogledne podatke pohranjene u sustavu SQL Server u oglednu bazu podataka AdventureWorks kao izvor podataka u SQL Data Warehouse učitati. Oglednu bazu podataka AdventureWorks, pročitajte članak [AdventureWorks 2014 ogledne baze podataka][].
5. **A SQL Data Warehouse baze podataka i dozvole**. Pomoću ovog praktičnog vodiča povezuje na instancu komponente SQL Data Warehouse i učitava podataka u njega. Morate imati dozvole za stvaranje tablice, a da biste učitali podatke.
6. **Pravila vatrozida**. Morate stvoriti pravila vatrozida na SQL Data Warehouse s IP adresom na lokalnom računalu prije SQL Data Warehouse možete prenijeti podatke.

## <a name="step-1-create-a-new-integration-services-project"></a>Korak 1: Stvorite novi projekt Integracijskih usluga

1. Pokrenite Visual Studio 2015.
2. Na izborniku **datoteka** , odaberite **novi | Project**.
3. Idite na **instaliran | Predlošci | Poslovno obavještavanje | Servisima za integraciju** projekta vrste.
4. Odaberite **projekt Integracijskih usluga**. Unesite vrijednosti za **naziv** i **mjesto**, a zatim odaberite **u redu**.

Visual Studio otvorit će se i stvara novi projekt integraciju servisa (SSIS). Zatim Visual Studio otvorit će se dizajner za jednu SSIS paket (Package.dtsx) u projektu. Pogledajte sljedeće područja zaslona:

- Na lijevoj strani komponenti SSIS **alatnog okvira** .
- U sredini površini za dizajniranje s više kartica. Obično koristi barem kartice **Tijek podataka** i **Upravljanje tijekom** .
- S desne strane, za **Preglednik rješenja** i **Svojstva** okna.

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Korak 2: Stvaranje toka osnovnih podataka

1. Povucite podataka tijek zadatka iz alatnog okvira u Centar za dizajniranje (na kartici **Upravljanje tijekom** ).

    ![][02]

2. Dvokliknite podataka tijek zadatka da biste se prebacili na karticu tijek podataka.
3. Na popisu drugih izvora u alatnom povucite izvora ADO.NET za dizajniranje. Izvor prilagodnika odabran, promijenite njegov naziv izvor **SQL Server** u oknu **Svojstva** .
4. Na popisu drugih odredišta u alatnom povucite na odredište ADO.NET za dizajniranje u odjeljku ADO.NET izvor. Odredište prilagodnika odabran, promijenite njegov naziv **SQL DW** odredište u oknu **Svojstva** .

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Korak 3: Konfiguriranje prilagodnika izvora

1. Dvokliknite prilagodnik izvor otvorite **Uređivač izvornog koda ADO.NET**.

    ![][03]

2. Na kartici **Upravitelja veze** **Uređivač izvornog koda ADO.NET**, kliknite gumb **Novo** pokraj popisa **ADO.NET Upravitelja veze** da biste otvorili dijaloški okvir **Konfiguriranje Upravitelja veze ADO.NET** i stvaranje postavki veze za baze podataka SQL Server s kojeg ovog praktičnog vodiča učitavanja podataka.

    ![][04]

3. U dijaloškom okviru **Konfiguriranje Upravitelja veze ADO.NET** kliknite gumb **Novo** da biste otvorili dijaloški okvir **Upravitelj veze** i stvorite novu podatkovnu vezu.

    ![][05]

4. U dijaloškom okviru **Upravitelj veze** , učinite sljedeće.

    1. **Davatelj usluga**, odaberite davatelja podataka SqlClient.
    2. U odjeljku **naziv poslužitelja**unesite naziv za SQL Server.
    3. U odjeljku **prijaviti poslužitelj** odaberite ili unesite podatke za provjeru autentičnosti.
    4. U odjeljku **Povezivanje s bazom podataka** odaberite oglednu bazu podataka AdventureWorks.
    5. Kliknite **Testiraj vezu**.
    
        ![][06]
    
    6. U dijaloškom okviru koji se izvješća rezultate Testiranje veze, kliknite **u redu** da biste se vratili u dijaloški okvir **Upravitelj veze** .
    7. U dijaloškom okviru **Upravitelj veze** kliknite **u redu** da biste se vratili u dijaloški okvir **Konfiguriranje upravitelja ADO.NET veze** .
 
5. U dijaloškom okviru **Konfiguriranje Upravitelja veze ADO.NET** kliknite **u redu** da biste se vratili na **Uređivač izvornog koda ADO.NET**.
6. U **Uređivač izvornog koda ADO.NET**, na popisu **naziv tablice ili prikazu** odaberite **Sales.SalesOrderDetail** tablice.

    ![][07]

7. Kliknite **Pregled** da biste vidjeli najprije 200 redaka podataka iz izvorišne tablice u dijaloškom okviru **Pretpregled rezultata upita** .

    ![][08]

8. U dijaloškom okviru **Pretpregled rezultata upita** kliknite **Zatvori** da biste se vratili na **Uređivač izvornog koda ADO.NET**.
9. **Uređivač izvornog koda ADO.NET**, kliknite **u redu** da biste dovršili konfiguraciju izvora podataka.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Korak 4: Povezati prilagodnik izvor odredište prilagodnika

1. Odaberite izvor prilagodnika na površini za dizajniranje.
2. Odaberite plava strelicu koja se prenosi s izvora i povucite ga na odredište uređivač dok je poravnat na mjestu.

    ![][10]

    U standardne SSIS paketa pomoću brojnih drugih komponenti iz SSIS alatnog između izvorišnog web-mjesta, a odredište Preustroj, pretvaranje i cleanse podataka kao što je prolazi kroz tijeku SSIS. Da biste zadržali u ovom se primjeru sadržavati jednostavno, ne možemo povezujete izvorišnog web-mjesta izravno na odredište.

## <a name="step-5-configure-the-destination-adapter"></a>Korak 5: Konfiguriranje prilagodnika odredište

1. Dvokliknite prilagodnik odredište da biste otvorili **Uređivač ADO.NET odredište**.

    ![][11]

2. Na kartici **Upravitelja veze** **ADO.NET odredište uređivača**kliknite gumb **Novo** pokraj popisa **Upravitelja veze** da biste otvorili dijaloški okvir **Konfiguriranje Upravitelja veze ADO.NET** i stvaranje postavki veze za Azure SQL Data Warehouse baze podataka u koju ovog praktičnog vodiča učitavanja podataka.
3. U dijaloškom okviru **Konfiguriranje Upravitelja veze ADO.NET** kliknite gumb **Novo** da biste otvorili dijaloški okvir **Upravitelj veze** i stvorite novu podatkovnu vezu.
4. U dijaloškom okviru **Upravitelj veze** , učinite sljedeće.
    1. **Davatelj usluga**, odaberite davatelja podataka SqlClient.
    2. U odjeljku **naziv poslužitelja**unesite naziv SQL Data Warehouse.
    3. U odjeljku **prijaviti poslužitelj** odaberite **SQL Server koristi provjeru autentičnosti** i unesite podatke za provjeru autentičnosti.
    4. U odjeljku **Povezivanje s bazom podataka** odaberite postojeće baze podataka SQL Data Warehouse.
    5. Kliknite **Testiraj vezu**.
    6. U dijaloškom okviru koji se izvješća rezultate Testiranje veze, kliknite **u redu** da biste se vratili u dijaloški okvir **Upravitelj veze** .
    7. U dijaloškom okviru **Upravitelj veze** kliknite **u redu** da biste se vratili u dijaloški okvir **Konfiguriranje upravitelja ADO.NET veze** .
5. U dijaloškom okviru **Konfiguriranje Upravitelja veze ADO.NET** kliknite **u redu** da biste se vratili **Uređivač ADO.NET odredište**.
6. U **Uređivač ADO.NET odredište**, kliknite **Novo** pokraj **koristi tablicu ili prikaz** popis da biste otvorili dijaloški okvir **Stvaranje tablice** da biste stvorili novi odredišne tablice s popisom stupac koji odgovara izvorišnu tablicu.

    ![][12a]

7. U dijaloški okvir **Stvaranje tablice** , učinite sljedeće.

    1. Promijenite naziv odredišne tablice u **SalesOrderDetail**.
    2. Uklonite stupac **rowguid** . Vrsta podataka **Jedinstveni identifikator koji** nije podržan u SQL Data Warehouse.
    3. Promijenite vrstu podataka u stupcu **LineTotal** u **novca**. **Decimalni** vrsta podataka nije podržan u SQL Data Warehouse. Informacije o podržanih vrsta podataka potražite u članku [Stvaranje TABLICE (Azure SQL Data Warehouse, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Kliknite **u redu** da biste stvorili tablicu i vratili u **Uređivač ADO.NET odredište**.

8. U **Uređivač ADO.NET odredište**, odaberite karticu **mapiranja** da biste vidjeli mapiranje stupaca u izvoru za stupce u odredište.

    ![][13]

9. Kliknite **u redu** da biste dovršili konfiguraciju izvora podataka.

## <a name="step-6-run-the-package-to-load-the-data"></a>Korak 6: Pokrenuli paket da biste učitali podatke

Pokrenite paket klikom na gumb **Start** na alatnoj traci ili odabirom neke od mogućnosti **Pokreni** na izborniku **za ispravljanje pogrešaka** .

Kada paket počne da biste pokrenuli, vidjet ćete žuti kotače adrese e da biste naznačili aktivnosti, kao i broj redaka koji se obrađuju dosad.

![][14]

Kada paket završi, vidjet ćete zelene kvačice da biste naznačili uspjeh, kao i ukupan broj redaka podataka s izvorišnog web-mjesta učitati na odredište.

![][15]

Čestitamo! Da biste učitali podatke u Azure SQL Data Warehouse ste iskoristili uspješno SQL Integracijskim uslugama poslužitelja.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o tijeku SSIS. Počnite ovdje: [Tijek podataka][].
- Saznajte kako ispraviti pogreške i otklanjanje poteškoća s svojih paketa u okruženje za dizajniranje. Počnite ovdje: [Otklanjanje poteškoća s alatima za razvoj paketa][].
- Saznajte kako implementirati SSIS projekata i pakete integraciju servisa poslužitelju ili na drugo mjesto za pohranu. Počnite ovdje: [Implementacija projekata i paketa][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Vodič za PolyBase]: https://msdn.microsoft.com/library/mt143171.aspx
[Preuzimanje sustava SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[Stvaranje TABLICE (Data Warehouse Azure SQL, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Toka podataka]: https://msdn.microsoft.com/library/ms140080.aspx
[Alati za otklanjanje poteškoća za razvoj paketa]: https://msdn.microsoft.com/library/ms137625.aspx
[Uvođenje projekata i paketa]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 servisima za integraciju značajkama za Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server procjene]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio zajednice]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[Ogledne baze podataka AdventureWorks 2014.]: https://msftdbprodsamples.codeplex.com/releases/view/125550
