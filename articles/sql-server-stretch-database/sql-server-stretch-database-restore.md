<properties
    pageTitle="Vraćanje rastezanje omogućeno baze podataka | Microsoft Azure"
    description="Saznajte kako vratiti rastezanje\-omogućeno baze podataka."
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
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Vraćanje rastezanje omogućeno baze podataka

Vraćanje sigurnosnu kopiju baze podataka kada je to potrebno da biste vratili iz razne vrste pogrešaka, pogrešaka i disasters.

Dodatne informacije o sigurnosne kopije potražite u članku [sigurnosne kopije rastezanje omogućeno baze podataka](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Sigurnosno kopiranje je samo jedan dio dovršeno visoke dostupnosti i poslovnog continuity rješenja. Dodatne informacije o visoke dostupnosti potražite u članku [Visoka dostupnost rješenja](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>Vraćanje podataka sustava SQL Server
Da biste vratili iz hardverske pogreške ili oštećenja, vratite na rastezanje\-omogućeno iz sigurnosne kopije baze podataka SQL Server. Možete nastaviti koristiti metode vraćanja sustava SQL Server koju trenutno koristite. Dodatne informacije potražite u članku [Vraćanje i pregled za oporavak](https://msdn.microsoft.com/library/ms191253.aspx).

Nakon Vraćanje baze podataka SQL Server, morate pokrenuti pohranjena procedura **sys.sp_rda_reauthorize_db** ponovno uspostaviti vezu između sustava rastezanje\-omogućena baze podataka SQL Server i udaljena baza podataka Azure. Dodatne informacije potražite u članku [obnavljanje veze između baze podataka SQL Server i udaljena baza podataka Azure](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Vraćanje udaljeni podaci za Azure

### <a name="recover-a-live-azure-database"></a>Oporavak uživo Azure baze podataka
Rastezanje baze podataka SQL Server na Azure snimke svih podataka u stvarnom vremenu barem svaka 8 sati servisa pomoću Azure prostora za pohranu snimke. Ove snimke održavaju sedam dana. Omogućuje vratiti podatke u neku od najmanje 21 točke u vremenu pada u zadnjih sedam dana do vrijeme kada je potrebno zadnje snimke.

Da biste vratili bazu podataka uživo Azure ranije točke u vremenu pomoću portala za Azure, učinite sljedeće.

1. Prijavite se na portal sustava Azure.
2. Na lijevoj strani zaslona odaberite **PREGLEDAJ** , a zatim odaberite **Baze podataka SQL**.
3. Otvorite bazu podataka i odaberite ga.
4. Pri vrhu plohu baze podataka, kliknite **Vrati**.
5. Unesite novi **Naziv baze podataka**, odaberite **Vrati točke** , a zatim kliknite **Stvori**.
6. Postupak vraćanja baze podataka započet će i moguće nadzirati pomoću **obavijesti**.

### <a name="recover-a-deleted-azure-database"></a>Oporavak izbrisane Azure baze podataka
Servis rastezanje baze podataka za SQL Server Azure stvara snimku baze podataka da bi se baze podataka se prekine i zadržava sedam dana. Kada se to dogodi, više ne zadržava snimke iz baze podataka uživo. Omogućuje vam vraćanje izbrisanih baze podataka točke kad je izbrisana.

Da biste vratili izbrisanu Azure bazu podataka točke kad je izbrisana pomoću portala za Azure, učinite sljedeće.

1. Prijavite se na portal sustava Azure.
2. Na lijevoj strani zaslona odaberite **PREGLEDAJ** , a zatim **SQL poslužitelja**.
3. Idite na poslužitelju i odaberite ga.
4. Pomaknite se do odjeljka operacije Elektronička ploča vašeg poslužitelja, kliknite pločicu **Izbrisane baze podataka** .
5. Odaberite izbrisanu bazu podataka koju želite vratiti.
5. Unesite novi **Naziv baze podataka** , a zatim kliknite **Stvori**.
6. Postupak vraćanja baze podataka započet će i moguće nadzirati pomoću **obavijesti**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Vraćanje veze između baze podataka SQL Server i udaljena baza podataka za Azure

1.  Ako namjeravate povezati vraćene Azure bazu pod drugim nazivom ili u nekoj drugoj regiji, pokrenite pohranjena procedura [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) da biste prekinuli prethodnu Azure bazu podataka.  

2.  Pokretanje pohranjena procedura [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) povezati lokalne rastezanje\-omogućeno bazu podataka za Azure baze podataka.  

    -   Navedite postojeće baze podataka iz djelokruga vjerodajnica kao u sysname ili na varchar\(128\) vrijednost. \(Nemojte koristiti varchar\(Maks\).\) Možete potražiti naziv vjerodajnica u prikazu **sys.database\_iz djelokruga\_vjerodajnice**.  

    -   Odredite želite li stvoriti kopiju udaljeni podaci i povezivanje s Kopiraj (preporučeno).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Vidi također

[Upravljanje i rješavanje problema s rastezanje baze podataka](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sigurnosno kopiranje i vraćanje baze podataka SQL Server](https://msdn.microsoft.com/library/ms187048.aspx)
