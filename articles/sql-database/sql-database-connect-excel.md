<properties
    pageTitle="Povezivanje programa Excel s bazom podataka SQL | Microsoft Azure"
    description="Saznajte kako u programu Microsoft Excel povezivanje s bazom podataka Azure SQL u oblaku. Uvoz podataka u programu Excel za istraživanje podataka i izvješćivanje o pogreškama."
    services="sql-database"
    keywords="povezivanje programa excel na sql, uvoz podataka u excel"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Praktični vodič SQL baze podataka: povezivanje programa Excel s bazom podataka Azure SQL i stvaranje izvješća

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Saznajte kako se povezati programa Excel s bazom podataka SQL u oblaku da biste mogli uvesti podatke i stvarati tablice i grafikone na temelju vrijednosti u bazi podataka. U ovom ćete praktičnom vodiču će Postavljanje veze između programa Excel i bazu podataka, spremite datoteku u kojoj su pohranjeni podaci i informacije o vezi za Excel, a zatim stvaranje zaokretnog grafikona iz vrijednosti baze podataka.

Morat ćete bazi podataka sustava SQL Azure prije nego što počnete. Ako ga nemate, potražite u članku [Stvaranje prve baze podataka SQL](sql-database-get-started.md) da biste dobili bazu podataka s oglednim podacima prema gore i pokretanje u nekoliko minuta. U ovom se članku ćete oglednih podataka u Excel uvezete iz članak, ali možete pratiti slične korake vlastitim podacima.

Trebat ćete kopiji programa Excel. U ovom se članku koristi [Microsoft Excel 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Povezivanje programa Excel s bazom podataka SQL i stvaranje odc datoteke

1.  Da biste povezali Excel SQL baze podataka, otvorite Excel i stvorite novu radnu knjigu ili otvorite postojeću radnu knjigu programa Excel.

2.  Na traci izbornika na vrhu stranice kliknite **podataka**, kliknite **Iz drugih izvora**, a zatim **Iz sustava SQL Server**.

    ![Odaberite izvor podataka: Excel povezivanje s bazom podataka SQL.](./media/sql-database-connect-excel/excel_data_source.png)

    Otvorit će se čarobnjak za povezivanje s podacima.

3.  U dijaloškom okviru **za povezivanje s poslužiteljem baze podataka** unesite baze podataka SQL **naziv poslužitelja** koji želite povezati u obrascu <*naziv poslužitelja*>**. database.windows.net**. Na primjer, **adworkserver.database.windows.net**.

4.  U odjeljku **vjerodajnice za prijavu**, kliknite **koristi sljedeće korisničko ime i lozinku**, unesite **Korisničko ime** i **lozinku** postavili za poslužitelj baze podataka SQL kad ste stvorili, a zatim kliknite **Dalje**.

    ![Upišite poslužitelja naziva i prijava vjerodajnice](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] Ovisno o mrežnom okruženju, nećete moći povezati ili izgubiti vezu ako poslužitelj baze podataka SQL ne dopušta promet s IP adrese klijenta. Idite na [portal za Azure](https://portal.azure.com/), kliknite SQL Server, kliknite na poslužitelju, kliknite vatrozid u odjeljku postavke pa dodajte IP adrese klijenta. Detalje potražite u članku [kako konfigurirati postavke vatrozida](sql-database-configure-firewall-settings.md) .

5. U dijaloškom okviru **Odaberite bazu podataka i tablicu** odaberite bazu podataka koju želite raditi s popisa, a zatim tablice ili prikaze kojima želite raditi (smo odabrali **vGetAllCategories**), a zatim kliknite **Dalje**.

    ![Odaberite bazu podataka i tablicu.](./media/sql-database-connect-excel/select-database-and-table.png)

    Otvorit će se dijaloški okvir **Spremanje datoteke za povezivanje podataka** koje unijeti informacije o veze (*.odc) datoteke baze podataka sustava Office koji Excel koristi. Možete ostaviti zadane postavke ili Prilagodba odabira.

6. Možete ostaviti zadane vrijednosti, ali imajte na umu posebno **Naziv datoteke** . **Opis**, **Neslužbeni naziv**i **Ključne riječi za pretraživanje** pomoći ćete i drugi korisnici ne zaboravite što ste se povezujete s i potražite vezu. Kliknite **Uvijek pokušaj koristiti ovu datoteku da biste osvježili podatke** ako želite podatke o vezi spremljene u odc datoteku tako da ga može ažurirati kada s njim povezati, a zatim kliknite **Završi**.

    ![Spremanje odc datoteku](./media/sql-database-connect-excel/save-odc-file.png)

    Pojavit će se dijaloški okvir **Uvoz podataka** .

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Uvoz podataka u programu Excel i stvaranje zaokretnog grafikona
Sad kad ste uspostaviti vezu, a da biste stvorili datoteku s informacijama podataka i veze, čitate da biste uvezli podatke.

1. U dijaloškom okviru **Uvoz podataka** kliknite željenu mogućnost za prikazivanje podataka na radnom listu, a zatim kliknite **u redu**. Ne možemo odabrali **zaokretnog grafikona**. Možete odabrati i da biste stvorili **novi radni list** ili **Dodaj ove podatke u podatkovni model**. Dodatne informacije o podatkovnim modelima potražite u članku [stvaranje podatkovnog modela u programu Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Kliknite **Svojstva** da biste istražili informacije o odc datoteku koju ste stvorili u prethodnom koraku, a da biste odabrali mogućnosti osvježavanja podataka.

    ![Odabir oblika podataka u programu Excel](./media/sql-database-connect-excel/import-data.png)

    Na radnom listu je prazna Zaokretna tablica i grafikona.

8. U odjeljku **Polja zaokretne tablice**, odaberite sve-okvire za polja koja želite prikazati.

    ![Konfiguriranje baze podataka izvješća.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Ako se želite povezati drugih radnih knjiga programa Excel i radni listovi u bazu podataka, kliknite **Podaci**, kliknite **veze**, kliknite **Dodaj**, odaberite vezu koju ste stvorili s popisa i zatim kliknite **Otvori**.
> ![Otvorite vezu iz druge radne knjige](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Daljnji koraci

- Dodatne upute za [Povezivanje s bazom podataka SQL s SQL Server Management Studio](sql-database-connect-query-ssms.md) za napredno postavljanje upita i analize.
- Saznajte više o prednostima koje nudi [elastic grupe](sql-database-elastic-pool.md).
- Saznajte kako [stvoriti web-aplikacije koje se povezuje s bazom podataka sustava SQL na pozadinske](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
