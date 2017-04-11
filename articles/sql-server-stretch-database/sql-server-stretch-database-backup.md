<properties
    pageTitle="Sigurnosno kopiranje rastezanje omogućeno baze podataka | Microsoft Azure"
    description="Upute za stvaranje sigurnosne kopije rastezanje\-omogućeno baze podataka."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Sigurnosno kopiranje baze podataka rastezanje omogućeno

Sigurnosne kopije baze podataka vam pomoći da se može oporaviti iz razne vrste pogrešaka, pogrešaka i disasters.  

-   Imate sigurnosnu kopiju vašeg rastezanje\-omogućeno baza podataka sustava SQL Server.  

-   Microsoft Azure automatski stvara sigurnosnu kopiju udaljeni podaci koji sadrži baze podataka rastezanje migrirati iz sustava SQL Server Azure.  

>    [AZURE.NOTE] Sigurnosno kopiranje je samo jedan dio dovršeno visoke dostupnosti i poslovnog continuity rješenja. Dodatne informacije o visoke dostupnosti potražite u članku [Visoka dostupnost rješenja](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Sigurnosno kopirajte podatke iz sustava SQL Server  

Stvaranje sigurnosne kopije vaše rastezanje\-omogućeno baze podataka sustava SQL Server, možete nastaviti koristiti SQL Server metode sigurnosne kopije koju trenutno koristite. Dodatne informacije potražite u članku [sigurnosno kopiranje i vraćanje sustava SQL Server baze podataka](https://msdn.microsoft.com/library/ms187048.aspx).

Sigurnosno kopiranje baze podataka s omogućenim rastezanje SQL Server sadržavati samo lokalnih podataka i podataka ispunjava uvjete za migraciju u točki u trenutku kada se pokrene sigurnosnu kopiju. \(Uvjete podaci podatke koji još nisu su preneseni, ali će se premjestiti Azure ovisno o postavkama migracije tablice.\) To se naziva **shallow** sigurnosnu kopiju, a ne sadrže podatke već prenijeli Azure.  

## <a name="back-up-your-remote-azure-data"></a>Stvaranje sigurnosne kopije udaljeni podaci za Azure   

Microsoft Azure automatski stvara sigurnosnu kopiju udaljeni podaci koji sadrži baze podataka rastezanje migrirati iz sustava SQL Server Azure.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure smanjuje rizik od gubitka podataka pomoću automatskog sigurnosnog kopiranja  
Servis rastezanje baze podataka za SQL Server Azure štiti udaljena baza podataka s automatsko spremanje snimki barem svaka osam sati. Zadržava svaki snimke sedam dana da vam pruži raspon točke moguće vraćanja.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure smanjuje rizik od gubitka podataka s zemlj\-zalihosti  
Sigurnosne kopije baze podataka za Azure pohranjuju se na zemlj\-suvišnih Azure prostora za pohranu (RA\-GRS) i zbog toga su zemlj\-suvišnih prema zadanim postavkama. Zemlj\-suvišnih prostora za pohranu replicira podataka na sekundarnu regiju koju je stotine milja izvan primarni regija. U primarnih i sekundarnih regijama podataka replicirati će se triput svakog, u zasebnom kvara domene i nadogradnje domene. Provjerava je li vaši podaci durable čak i ako potpuni regionalnih nedostupnosti ili Izrada koji prikazuje jednu od Azure regija nije dostupan.

### <a name="stretchRPO"></a>Razvlačenje baze podataka smanjuje rizik od gubitka podataka za Azure podatke privremeno zadržavanje migriranim redaka
Kada se baza podataka rastezanje migrira uvjete redaka iz sustava SQL Server Azure, zadržava te retke u tablici pripremna za najmanje osam sati. Ako vraćate sigurnosnu kopiju baze podataka Azure, Rastegnuto baze podataka pomoću redaka koji se sprema u tablici pripremna uskladiti SQL Server i Azure baze podataka.

Nakon Vraćanje sigurnosne kopije Azure podataka, morate pokrenuti pohranjena procedura [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) se ponovno povezali s rastezanje\-omogućeno baze podataka SQL Server udaljenom bazom podataka Azure. Kada pokrenete **sys.sp_rda_reauthorize_db**, Rastegnuto baze podataka automatski usklađuje SQL Server i Azure baze podataka.

Da biste povećali broj sati premještene podatke zadržava privremeno rastezanje baze podataka u tablici pripremna, pokrenite pohranjena procedura [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) i navedite broj veći od 8 sati. Da biste odlučili koliko će se zadržavati podaci, razmotrite sljedeće čimbenike:
-   Učestalost automatsko Azure sigurnosno kopiranje (barem svaka osam sati).
-   Naplata nakon problema za prepoznavanje problema i odlučite da biste vratili sigurnosnu kopiju.
-   Trajanje postupak vraćanja Azure.

> [AZURE.NOTE] Povećanje količinu podataka koji se zadržava privremeno rastezanje baze podataka u tablici pripremna povećava razmak na SQL Server.

Da biste provjerili broj sati rastezanje baze podataka trenutno zadržava privremeno u tablici pripremna podataka, pokrenite pohranjena procedura [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Vidi također

[Upravljanje i rješavanje problema s rastezanje baze podataka](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sigurnosno kopiranje i vraćanje baze podataka SQL Server](https://msdn.microsoft.com/library/ms187048.aspx)
