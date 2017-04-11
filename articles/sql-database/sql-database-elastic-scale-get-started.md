<properties 
    pageTitle="Početak rada s alatima elastic baze podataka" 
    description="Osnovni objašnjenje značajka elastic baze podataka za Azure SQL baze podataka da biste pokrenuli aplikaciju uzorka, uključujući jednostavno." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Početak rada s alatima Elastic baze podataka

Ovaj dokument uvodi sučelje za razvojne inženjere tako da pokrenete aplikaciju uzorka. Uzorak stvara jednostavno sharded aplikacije i istražuje ključa mogućnosti Alati elastic baze podataka. Uzorak pokazuje funkcije [klijentska biblioteka elastic baze podataka](sql-database-elastic-database-client-library.md)

Da biste instalirali u biblioteku, idite na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Imajte na umu da se u biblioteku instaliran pomoću aplikacije za uzorak opisanim.

## <a name="prerequisites"></a>Preduvjeti

1. Potreban je Visual Studio 2012 ili noviji s C#. Preuzmite besplatnu verziju na [Visual Studio preuzimanja](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 ili noviji. Najnoviju verziju, pročitajte članak [Instaliranje NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Preuzmite i pokrenite aplikaciju uzorka

Na **Elastic baze podataka pomoću Azure SQL – prvi koraci** probna aplikacija prikazuje najvažnije aspekte razvojno okruženje za sharded aplikacije koje se koriste Azure SQL elastic Alati baze podataka. Usmjerena na slučajeve pomoću ključa za [shard mapiranje upravljanja](sql-database-elastic-scale-shard-map-management.md), [podatke o njima ovisne usmjeravanje](sql-database-elastic-scale-data-dependent-routing.md) i [Ispitivanje više shard](sql-database-elastic-scale-multishard-querying.md). Da biste preuzeli i pokrenuli uzorka, slijedite ove korake: 

1. Otvorite Visual Studio, a zatim odaberite **datoteke -> novo -> projekta**.
2. U dijaloškom okviru kliknite na **mreži**.

    ![Novi projekt > Online][2]
3. Zatim **Visual C#** u odjeljku **uzorka**.

    ![Kliknite Visual C#][3]
4. U okvir za pretraživanje upišite **elastic db** da biste potražili uzorka. Naslov pojavit će se **Elastic Alati baze podataka za SQL Azure – prvi koraci** .

    ![Okvir za pretraživanje][1]
 
5. Odaberite uzorak naziv i mjesto za novi projekt te pritisnite **u redu** da biste stvorili projekta.
6. Otvorite datoteku **app.config** u rješenje za projekt uzorak i slijedite upute u datoteci da biste dodali naziv poslužitelja baze podataka za Azure SQL i informacije o prijavi (korisničko ime i lozinka).
7. Stvaranje i pokretanje aplikacija. Kada se od vas zatraži, dopustite Visual Studio da biste vratili NuGet paketa rješenja. To će preuzmite najnoviju verziju klijentska biblioteka elastic baze podataka iz NuGet.
8. Reproduciranje s drugim mogućnostima da biste saznali više o mogućnostima biblioteke klijenta. Imajte na umu korake aplikacije vodi na konzoli izlazne i slobodno da biste istražili kod u pozadini.

    ![tijeku][4]

Čestitamo – uspješno ugrađena i pokrenite prvu sharded aplikacije pomoću alata za elastic baze podataka na baze podataka SQL Azure. Pogledajte brzi shards stvorenog uzorka putem veze s Visual Studio ili SQL Server Management Studio s poslužiteljem baze podataka za Azure. Uočit ćete novi ogledne shard baze podataka i shard karte Upravitelj baze podataka koje je stvorio uzorka.

> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio bude sinkroniziranim ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Ključni dijelove uzorak koda

1. **Upravljanje Shards i Shard karte**: kod prikazuje kako raditi s shards, raspona, a mapiranja u datoteku **ShardMapManagerSample.cs**. Dodatne informacije o ovoj temi možete pronaći: [Upravljanje Shard karta](http://go.microsoft.com/?linkid=9862595).  
2. **Podaci o njima ovisne usmjeravanje**: usmjeravanje transakcije na desni shard prikazuju se u **DataDependentRoutingSample.cs**. Dodatne informacije potražite u članku [Podataka o njima ovisne usmjeravanja](http://go.microsoft.com/?linkid=9862596). 
3. **Querying preko više Shards**: Querying preko shards pokazuju u datoteci **MultiShardQuerySample.cs**. Dodatne informacije potražite u članku [Više Shard upita](http://go.microsoft.com/?linkid=9862597).
4. **Dodavanje prazne shards**: izračun s iteracijama dodavanje novi prazan shards provodi kod u datoteci **AddNewShardsSample.cs**. Detalje o temi možete pronaći u ovdje: [Upravljanje Shard karta](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Ostalih operacija elastic skala

1. **Podjela postojeće shard**: mogućnost Podijeli shards se odvija pomoću **alata za podjelu cirkularnog pisma**. Možete pronaći dodatne informacije o ovom alatu: [Pregled alata za podjelu cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Spajanje postojeće shards**: Shard spajanja i izvršavaju pomoću **alata za podjelu cirkularnog pisma**. Dodatne informacije potražite u priručniku: [Pregled alata za podjelu cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Trošak

Alati za elastic baze podataka se ne naplaćuju. Alati baze podataka za elastic nametnuti dodatne troškove pri vrhu trošak za Azure Upotreba. 

Ako, na primjer, primjer aplikacije stvara novu bazu podataka. Trošak ovisi o izdanju baze podataka baze podataka SQL Azure odaberete i Azure korištenja aplikacije.

Informacije o cijenama potražite u članku [Cijene Detalji baze podataka za SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o alatima za elastic baze podataka potražite u članku:

* [Karte dokumentaciju Alati za elastic baze podataka](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Primjere koda: 
    -    [Elastic DB s Azure SQL - prvi koraci](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastic DB s Azure SQL - Integracija sa entitet Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Shard Elasticity u centru za skripte](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [Objava Elastic skala](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Kanal 9: [Elastic mjerilo pregled videozapisa](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Forum za raspravu: [forum Azure SQL baze podataka](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Performanse izmjeriti: [mjerača performansi za Upravitelj shard karte](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
