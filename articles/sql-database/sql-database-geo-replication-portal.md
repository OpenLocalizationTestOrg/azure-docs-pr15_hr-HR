<properties 
    pageTitle="Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću portala za Azure | Microsoft Azure" 
    description="Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću portala za Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću portala za Azure


> [AZURE.SELECTOR]
- [Pregled](sql-database-geo-replication-overview.md)
- [Portal za Azure](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

U ovom se članku objašnjava konfiguriranje aktivni replikacije zemlj za SQL baze podataka pomoću [portala za Azure](http://portal.azure.com).

Da biste započeli prebacivanje s portala za Azure, potražite u članku [pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Sada je dostupan za sve baze podataka u sve razine servisa Active zemlj. – replikacije (čitljivi secondaries). U 2017 Travanj koje nisu čitljiv sekundarne vrsta će biti povučena iz upotrebe, a postojeće baze podataka koje nisu čitljiv automatski nadogradit će se čitati secondaries.

Da biste konfigurirali zemlj replikacije pomoću portala za Azure, potrebno vam je u sljedećim resursima:

- Baze podataka Azure SQL - primarni bazu podataka koju želite za replikaciju različite zemljopisnom području.

## <a name="add-secondary-database"></a>Dodavanje sekundarni baze podataka

Na sljedeći način stvoriti novu bazu podataka sekundarne u zemlj replikacije partnerstvo.  

Da biste dodali sekundarni, morate biti vlasnik pretplate ili suradnika kao. 

Sekundarni baza podataka će imati isti naziv kao primarni baze podataka i će prema zadanom imaju iste razine servisa. Sekundarni baze podataka može biti jedne baze podataka ili elastic baze podataka. Dodatne informacije potražite u članku [Servis razine](sql-database-service-tiers.md).
Nakon sekundarne se stvara i seeded, podaci će početi replikaciju s primarnom bazom podataka u novu sekundarni bazu podataka. 

> [AZURE.NOTE] Ako već postoji partnera bazu podataka (na primjer – uslijed prekidanje prethodne odnosa zemlj replikacije) neće uspjeti naredbu.

### <a name="add-secondary"></a>Dodavanje sekundarni

1. [Portal za Azure](http://portal.azure.com)otvorite bazu podataka koju želite postaviti za zemlj replikacije.
2. Na stranici baze podataka SQL odaberite **Zemlj replikacije**, a zatim području da biste stvorili sekundarni baze podataka. Možete odabrati sve regije osim regija hosting primarnom bazom podataka, ali preporučuje se [upareni regija](../best-practices-availability-paired-regions.md) .

    ![Konfiguriranje zemlj replikacije](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Odaberite ili konfigurirajte poslužitelj i cijene sloju za sekundarne bazu podataka.

    ![Konfiguriranje sekundarni](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Po želji možete dodati sekundarne baze podataka programa skup elastic baze podataka:

 Da biste stvorili sekundarni baze podataka u zajedničko područje, kliknite **skup Elastic baze podataka** , a zatim odaberite grupu na poslužitelju ciljni. Zajedničko područje mora postojati na poslužitelju ciljni ovaj tijek rada stvaranja zajedničko područje.

6. Kliknite **Stvori** da biste dodali sekundarne.
 
6. Stvara se sekundarne baze podataka i počinje seeding postupak. 
 
    ![Konfiguriranje sekundarni](./media/sql-database-geo-replication-portal/seeding0.png)

7. Po dovršetku postupka seeding sekundarne baze podataka prikazuje njezin status.

    ![seeding dovršeno](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Uklanjanje sekundarne baze podataka

Operacija trajno prekida replikaciju sekundarnu bazu podataka i promijeniti ulogu sekundarne u običnoj bazi podataka za čitanje i pisanje. Ako se prekine veza s sekundarne baze podataka, naredba je uspjela, ali vratit će se sekundarne će postaje čitanja i pisanja tek nakon povezivanja.  

1. [Portal za Azure](http://portal.azure.com)pronađite primarni baze podataka u partnerstvo zemlj replikacije.
2. Na stranici SQL baze podataka odaberite **Zemlj replikacije**.
3. Na popisu **SECONDARIES** odaberite bazu podataka koju želite ukloniti iz partnerstvo zemlj replikacije.
4. Kliknite **Zaustavi replikacije**.

    ![uklanjanje sekundarne](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Kliknite **Zaustavi replikacije** otvara prozora za potvrdu pa kliknite **da** da biste uklonili bazu podataka s partnerstvo zemlj replikacije (postavljeno je na čitanje i pisanje baze podataka nije dio sve replikacije).


## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o Active zemlj. – replikacije potražite u članku - [Active replikacije zemlj.](sql-database-geo-replication-overview.md)
- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)

