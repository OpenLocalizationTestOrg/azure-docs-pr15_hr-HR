<properties
   pageTitle="Upute za stvaranje zahtjev za podršku možete za SQL Data Warehouse | Microsoft Azure"
   description="Kako stvoriti zahtjev za podršku možete u Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Upute za stvaranje zahtjev za podršku možete za SQL Data Warehouse
 
Ako imate probleme s vašeg SQL Data Warehouse ponovno stvorite na zahtjev za podršku možete tako da se naš tim može vam pomoći.

## <a name="create-a-support-ticket"></a>Stvaranje zahtjev za podršku možete

1. Otvorite [portal za Azure][].

2. Na početnom zaslonu kliknite pločicu **Pomoć + podrška** .

    ![Pomoć + podrška](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Na stranici + plohu za podršku, kliknite **Stvori zahtjev za podršku**.

    ![Novi zahtjev za podršku](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Odaberite **Vrsta zahtjeva**.

    ![Vrsta zahtjeva](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Prema zadanim postavkama svaki SQL server (npr. myserver.database.windows.net) ima **Kvote DTU** 45,000. U ovom kvote je jednostavno ograničenje sigurnost. Stvaranje zahtjev za podršku možete i odabirom *kvote* kao vrstu zahtjev možete povećati kvotu. Da biste izračunali vaše DTU mora, pomnožite s 7.5 Ukupno [DWU][] potrebne. Na primjer, želite li hostira dva DW6000s na jednu SQL server, a zatim treba zahtjev DTU ograničenja od 90,000.  Trenutni DTU potrošnje iz Elektronička ploča poslužitelja SQL možete pogledati na portalu. Baza podataka za privremeno i nepotvrđen pauzirano Brojanje prema DTU kvote. 

5. Odaberite **pretplatu** koju hostira baze podataka pomoću koje su prijavljivanja problema.

    ![Pretplate](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Odaberite **SQL Data Warehouse** kao resurs.

    ![Resurs](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Odaberite vaše [Azure podršku planu][].

    - Podrška za **naplatu, upravljanje kvota i pretplate** dostupna je na svim razinama podrška.
    - Podrška za **prijelom popravak** se odvija pomoću [za razvojne inženjere][], [Standardno][], [Professional Izravni][] ili [Najbolji][] podrška. Prijelom otklanjanje problema su problemi iskustvom tako da korisnici prilikom korištenja Azure kojoj se nalazi pametnije očekivanja da Microsoft uzrok problema.
    - **Mentoring za razvojne inženjere** i **savjeta services** dostupne su na [Professional direktne][] i [Najbolji][] razine podrške tvrtke. 
    
    Ako imate Premier podršku planu, možete prijaviti i SQL Data Warehouse probleme na [mrežni portal Microsoft Premier][]vezane uz.  Potražite u članku [Azure podržava tarife][Azure podršku planu] da biste saznali više o tarifama za podršku različite uključujući opseg vremena odgovor, cijene, itd.  Najčešća pitanja o Azure podršci potražite [Azure podržava najčešća pitanja][].  

    ![Planiranje podrške](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Odaberite željenu **vrstu problema** i **kategorije**.

    ![Kategoriju vrsta problema](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Opišite problem i odaberite željenu razinu utjecaj tvrtke.

    ![Opis problema](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. **Podaci za kontakt** za ovaj zahtjev za podršku možete će biti unaprijed ispunjeni. Ako je potrebno ažurirati to.

    ![Podaci za kontakt](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Kliknite **Stvori** da biste poslali zahtjev za podršku.


## <a name="monitor-a-support-ticket"></a>Praćenje zahtjev za podršku možete

Nakon što ste poslali zahtjev za podršku, Azure podršku će od vas zatražiti. Da biste provjerili status zahtjeva i detalja, kliknite **Upravljanje zahtjeva za podršku** na nadzornoj ploči.

![Provjera statusa](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Ostali resursi

Osim toga, možete se povezati sa SQL Data Warehouse zajednicom na [Stogu prelijevanje][] ili na [forumu Azure SQL podataka skladištu MSDN][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Portal za Azure]: https://portal.azure.com/
[Planiranje podrške za Azure]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Za razvojne inženjere]: https://azure.microsoft.com/support/plans/developer/  
[Standardna]: https://azure.microsoft.com/support/plans/standard/  
[Profesionalni izravno]: https://azure.microsoft.com/support/plans/prodirect/  
[Vrhunska]: https://azure.microsoft.com/support/plans/premier/  
[Podrška za Azure najčešća pitanja]: https://azure.microsoft.com/support/faq/
[Portal za Lync online Microsoft Premier]: https://premier.microsoft.com/
[Preljev stoga]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Forum za MSDN skladištu podataka za SQL Azure]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

