<properties
    pageTitle="Nadogradnja V12 za baze podataka SQL Azure pomoću portala za Azure | Microsoft Azure"
    description="U članku se objašnjava kako nadograditi na V12 za baze podataka SQL Azure uključujući nadogradnju Web i poslovnih baza podataka i nadogradnja V11 poslužitelja migracije njegov baze podataka izravno u elastic baze podataka grupe aplikacija pomoću portala za Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Nadogradnja V12 za baze podataka SQL Azure pomoću portala za Azure


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


V12 baze podataka SQL je najnovija verzija tako da je nadogradnja postojeći poslužitelji V12 baze podataka SQL preporučuje.
SQL baza podataka V12 sadrži brojne [prednosti u odnosu na prethodne verzije](sql-database-v12-whats-new.md) uključujući:

- Povećana kompatibilnost sa sustavom SQL Server.
- Poboljšani premium performanse i nove razine performansi.
- [Grupe elastic baze podataka](sql-database-elastic-pool.md).

Ovaj članak sadrži upute za nadogradnju postojeći poslužitelji za V11 SQL baze podataka i bazama podataka na V12 SQL baze podataka.

Tijekom postupka nadogradnje V12 će nadograditi nijedna baza podataka Web- a tvrtke da biste novi sloju servisa tako da su upute za nadogradnju Web i poslovnih baza podataka.

Osim toga, migracije [skup elastic baze podataka](sql-database-elastic-pool.md) može biti troškova učinkovitiji od nadogradnje na pojedinačne performanse razine (cijene razine) za jedan baze podataka. Grupe Pojednostavnite upravljanje bazom podataka jer samo morate upravljati postavkama performansi na resurse umjesto zasebno upravljanje performansama razine pojedinačne baze podataka. Ako imate baze podataka na više poslužitelja preporučuje da ih premjestite u istom poslužitelju i iskoristi umetanja ih u zajedničko područje. Možete jednostavno [automatski Migracija baze podataka s poslužitelja V11 izravno u grupe elastic baze podataka pomoću komponente PowerShell](sql-database-upgrade-server-powershell.md). Portal sustava možete koristiti i za migriranje V11 baze podataka u grupu, ali na portalu već mora imati V12 server da biste stvorili zajedničko područje. Su upute u nastavku ovog članka da biste stvorili na resurse nakon nadogradnje poslužitelju ako imate [bazama podataka koje su prednosti iz skupa](sql-database-elastic-pool-guidance.md).

Imajte na umu vaših baza podataka će ostati u mreži i nastavili raditi tijekom postupka nadogradnje. Vrijeme stvarni prijelaz na novu razinu performanse privremene ispuštanje veza s bazom podataka može dogoditi vrlo malen trajanja koja je obično oko 90 sekundi, ali može biti pet minuta. Ako je vaša aplikacija [tranzitne kvara rukovanje preuskih vezu](sql-database-connectivity-issues.md) pa je dovoljno zaštitili izostavljanje veze na kraju nadogradnje.

Nadogradnja V12 baze podataka SQL nije moguće poništiti. Nakon nadogradnje na poslužitelju ne može biti vratit će V11.

Nakon nadogradnje na V12, [servis sloju preporuke](sql-database-service-tier-advisor.md) i [pitanja vezana uz performanse za elastic skup](sql-database-elastic-pool-guidance.md) odmah neće dostupno dok servis je vrijeme za procjenu sustava radnih opterećenja na novom poslužitelju. V11 poslužitelj preporuke povijest ne primjenjuju se na poslužiteljima V12 tako da ne zadržava.

## <a name="prepare-to-upgrade"></a>Priprema za nadogradnju

- **Nadogradnja sve baze podataka Web- a tvrtke**: potražite u članku [Nadogradnja sve baze podataka Web- a tvrtke](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) odjeljak u nastavku ili potražite u članku [nadzor i upravljanje skup elastic baze podataka (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Pregled i obustavi zemlj replikacije**: Ako bazu podataka Azure SQL je konfiguriran za zemlj replikacije treba dokumenta njegov trenutnu konfiguraciju i [Zaustavljanje zemlj replikacije](sql-database-geo-replication-portal.md#remove-secondary-database). Kada Nadogradnja Završi konfigurirajte bazu podataka za zemlj replikaciju.
- **Otvorite sljedeće priključke ako imate klijente na programa Azure VM**: klijentski program povezuje V12 baze podataka SQL tijekom klijent na Azure virtualnog računala (VM), morate otvoriti priključak raspona 11000 11999 i 14000 14999 na na VM. Detalje potražite u članku [priključke za V12 SQL baze podataka](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Pokretanje nadogradnje

1. U Pregledaj [Azure portal](https://portal.azure.com/) na poslužitelju želite li nadograditi tako da odaberete **PREGLEDAJ** > **SQL poslužitelja**, a ne i odaberite želite li nadograditi poslužitelj 2.0.
2. Odaberite **Najnovije ažuriranje SQL baze podataka**, a zatim odaberite **ovaj poslužitelj nadograditi**.

      ![poslužitelj nadogradnje][1]

3. Nadogradnja na poslužitelju na najnovije ažuriranje baze podataka za SQL je trajno i nepovratno. Da biste potvrdili nadogradnje, upišite naziv poslužitelja, a zatim kliknite **u redu**.

## <a name="upgrade-all-web-and-business-databases"></a>Nadogradnja sve Web- a tvrtke baze podataka

Ako je poslužitelj za bazu podataka weba ili tvrtke, morate nadograditi ih. Tijekom postupka nadogradnje V12 baze podataka SQL će se ažurirati sve Web- a tvrtke baze podataka da biste novi sloju servisa.    

Kako bi vam uz nadogradnju, servis baze podataka SQL preporučuje odgovarajući servisa sloju i performanse razinu (cijene sloju) za svaku bazu podataka. Servis preporučuje najbolje sloju radno opterećenje svoje postojeće baze podataka radi analizom povijesne korištenje za bazu podataka.

3. U plohu **nadograditi ovaj poslužitelj** odaberite svaki baze podataka da biste pregledali i odaberite sloju preporučene cijene za nadogradnju. Uvijek možete pregledavati dostupne razine cijena i odaberite onaj koji najbolje odgovara vašem okruženju.


     ![Baza podataka][2]


7. Nakon što kliknete predloženi sloju će se prikazivati s plohu **Odabir vaše cijene sloju** gdje možete odabrati sloj i zatim kliknite gumb **Odaberi** da biste promijenili tu sloju. Odaberite novi sloju za svaku weba ili tvrtke bazu podataka.

    Što je važno je napomenuti da je da baze podataka sustava SQL nije zaključana u bilo kojem određenog servisa sloju ili performanse razinu, tako da se kao preduvjeti za promjenu baze podataka možete jednostavno promijeniti između raspoloživ servis razine i performanse razine. Zapravo, Basic, standardna i baze podataka SQL Premium naplatiti po satu, a imate mogućnost skaliranja svaku bazu podataka prema gore ili dolje 4 vremena u razdoblju 24 sata.

    ![preporuke][6]


Nakon što su sve baze podataka na poslužitelju uvjete ste spremni za pokretanje nadogradnje

## <a name="confirm-the-upgrade"></a>Potvrdite nadogradnje

3. Kada sve baze podataka na poslužitelju ispunjavaju uvjete za nadogradnju ćete morati **Vrsta POSLUŽITELJA ime** da biste potvrdili da želite izvršiti nadogradnju, a zatim kliknite **u redu**.

    ![Provjerite je li nadogradnje][3]


4. Nadogradnja pokreće i prikazuje u tijeku obavijesti. Pokreće postupak nadogradnje. Ovisno o detalje o bazama podataka za određenu nadogradnje na V12 može potrajati. Za to vrijeme sve baze podataka na poslužitelju će ostati u mreži, ali poslužitelj i bazu podataka upravljanja akcije bit će ograničeni.

    ![Nadogradnja je u tijeku][4]

    U trenutku stvarni prijelaz na novu performanse razinu privremene ispuštanje veza s bazom podataka može dogoditi vrlo malen trajanje (obično se mjeri u sekundama). Ako aplikacija ima tranzitne kvara rukovanje (Ponovi logike) za preuskih vezu, a zatim je dovoljno zaštitili izostavljanje veze na kraju nadogradnje.

5. Nakon dovršetka postupka nadogradnje plohu **Najnovije ažuriranje** prikazat će se **omogućeno**.

    ![V12 omogućeno][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Premještanje baze podataka programa skup elastic baze podataka

[Portal za Azure](https://portal.azure.com/) pronađite V12 poslužitelja, a zatim kliknite **Dodaj grupu**.

– ili –

Ako se prikaže sljedeća poruka: **kliknite ovdje da biste pogledali grupe preporučene elastic baze podataka za ovaj poslužitelj**, kliknite da biste jednostavno stvorili grupu koja je optimizirana za vaš poslužitelj baze podataka. Detalje potražite u članku [cijena i performanse zahtjevi za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-guidance.md).

![Dodavanje grupe aplikacija na poslužitelju][7]

Slijedite upute u članku [Stvaranje grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool.md) da biste dovršili stvaranje vaše grupe aplikacija.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Praćenje baze podataka nakon nadogradnje na V12 baze podataka SQL

>[AZURE.IMPORTANT] Nadograditi na najnoviju verziju sustava sustav SQL Server Management Studio (SSMS) da biste iskoristili nove mogućnosti v12. [Preuzimanje SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx).

Nakon nadogradnje, aktivno praćenje baze podataka da biste bili sigurni aplikacije trenutno pokrenuto željene performanse i optimizirati postavke prema potrebi.

Osim nadzirati pojedinačne baze podataka, možete nadzirati elastic baze podataka grupe [Monitor, upravljanje, a veličina je skup elastic baze podataka pomoću portala za Azure](sql-database-elastic-pool-manage-portal.md) ili sa [servisom PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Resursa potrošnje podataka:** Za Basic, standardna i Premium baze podataka resursa potrošnje podataka je dostupan putem u [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV u bazi podataka za korisnika. U ovom DMV nudi blizu stvarnom vremenu potrošnje informacije o resursu na 15 drugi preciznosti prethodne h operacije. Postotak potrošnje DTU za interval je izračunati kao potrošnju maksimalan postotak dimenzije procesora, IO i zapisnika. Evo upita za izračunavanje prosjeka potrošnju postotak DTU putem zadnjem satu.

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Dodatne informacije o praćenju:

- [Baze podataka SQL azure performanse smjernice za jednu baze podataka](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Cijene i performanse zahtjevi za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-guidance.md).
- [Nadzor baze podataka SQL Azure pomoću dinamičke Upravljanje prikazima](sql-database-monitoring-with-dmvs.md)




**Upozorenja:** Postavljanje "Upozorenja" na portalu za Azure da vas obavijesti kada potrošnje DTU nadograđena baze podataka izvršenja određene visoku razinu. Upozorenja baze podataka može biti Postava Azure portalu za različite metriku performanse kao što su DTU, procesora, IO i zapisnika. Otvorite bazu podataka i odaberite **upozorenja pravila** plohu **Postavke** .

Na primjer, možete postaviti upozorenja e-pošte na "DTU postotak" ako prosječnu vrijednost postotak DTU premašuje 75% putem posljednjih 5 minuta. Pogledajte da biste [primali upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) da biste saznali više o konfiguriranju upozorenja.





## <a name="next-steps"></a>Daljnji koraci

- [Traženje grupe aplikacija preporuke i stvaranje zajedničko područje](sql-database-elastic-pool-create-portal.md).
- [Promjena servisa sloju i performanse razine bazu podataka](sql-database-scale-up.md).



## <a name="related-links"></a>Srodne veze

- [Što je novo u V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [Planiranje i Priprema za nadogradnju na V12 baze podataka SQL](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
