<properties 
    pageTitle="Konfiguriranje aktivni replikacije zemlj za baze podataka SQL Azure pomoću komponente PowerShell | Microsoft Azure" 
    description="Konfiguriranje aktivni zemlj replikacijom za baze podataka SQL Azure pomoću komponente PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Konfiguriranje zemlj replikacije za baze podataka Azure SQL pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Pregled](sql-database-geo-replication-overview.md)
- [Portal za Azure](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

U ovom se članku objašnjava konfiguriranje aktivni replikacije zemlj za baze podataka SQL pomoću komponente PowerShell.

Da biste započeli prebacivanje pomoću komponente PowerShell potražite u članku [pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću komponente PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Sada je dostupan za sve baze podataka u sve razine servisa Active zemlj. – replikacije (čitljivi secondaries). U travnju 2017 će povučena koje nisu čitljiv sekundarne vrsta i postojeće baze podataka koje nisu čitljiv automatski nadogradit će se čitati secondaries.



Da biste konfigurirali aktivni replikacije zemlj pomoću komponente PowerShell, potrebno je sljedeće:

- Azure pretplate. 
- Baze podataka Azure SQL - primarni bazu podataka koju želite za replikaciju.
- Azure PowerShell 1.0 ili novije. Možete preuzeti i instalirati modula Azure PowerShell po sljedeće [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfiguriranje vjerodajnice i odaberite pretplate

Najprije morate uspostaviti pristupa na račun za Azure pa pokrenite PowerShell, a zatim pokrenite sljedeći cmdlet. Na zaslonu za prijavu unesite isto e-pošte i lozinku koje koristite za prijavu na portal za Azure.


    Login-AzureRmAccount

Nakon uspješnog prijave vidjet ćete neke informacije na zaslonu koja sadrži ste se prijavili pomoću ID-a i Azure pretplate kojima imate pristup.


### <a name="select-your-azure-subscription"></a>Odaberite pretplatu Azure

Da biste odabrali pretplata vam je potrebna vaša pretplata ID-a. Id pretplate možete kopirati iz informacija prikazanih u prethodnom koraku, ili ako imate više pretplata, a potrebno više detalja pokrenite cmdlet **Get-AzureRmSubscription** i kopirajte željene pretplate podatke iz sa skupom rezultata. Sljedeći cmdlet koristi Id pretplate da biste postavili trenutne pretplate:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Nakon uspješnog pokretanja **Odaberite AzureRmSubscription** se vraćate na PowerShell upita.


## <a name="add-secondary-database"></a>Dodavanje sekundarni baze podataka


Na sljedeći način stvoriti novu bazu podataka sekundarni u zemlj replikacijom partnerstvo.  
  
Da biste omogućili sekundarni morate biti vlasnik pretplate ili suradnika kao. 

Možete koristiti **Novo AzureRmSqlDatabaseSecondary** cmdlet za dodavanje sekundarni baze podataka na poslužitelju partnera s lokalnom bazom podataka na poslužitelju na kojem ste povezani (primarni baze podataka). 

Ovaj cmdlet zamjenjuje **Start AzureSqlDatabaseCopy** s parametrom **– IsContinuous** .  To će izlaz **AzureRmSqlDatabaseSecondary** objekt koji se mogu koristiti druge cmdleta jasno označavaju određene replikacije vezu. Ovaj cmdlet vratit će se kada se stvara i potpuno seeded sekundarni baze podataka. Ovisno o veličini baze podataka u roku od minuta sati.

Repliciranu bazu podataka na sekundarni poslužitelj će imati isti naziv kao i baza podataka na primarnom poslužitelju i će prema zadanom imaju iste razine servisa. Sekundarni baze podataka može čitati ili nije čitljiv, a može biti jedne baze podataka ili elastic baze podataka. Dodatne informacije potražite u članku [Novo AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) i [Razine servisa](sql-database-service-tiers.md).
Nakon sekundarne se stvara i seeded, podaci će početi replikaciju s primarnom bazom podataka u novu sekundarnu bazu podataka. Koraci u nastavku opisuju kako obaviti taj zadatak pomoću komponente PowerShell da biste stvorili koje nisu čitljiv i čitljiv secondaries, pomoću jedne baze podataka ili elastic baze podataka.

Ako već postoji partnera bazu podataka (na primjer – uslijed prekidanje prethodne odnosa zemlj replikacije) naredbu neće uspjeti.



### <a name="add-a-non-readable-secondary-single-database"></a>Dodavanje nije čitljiv sekundarni (jedan baza podataka)

Sljedeća naredba stvara koje nisu čitljiv sekundarni baze podataka "mydb" s poslužitelja "srv2" u grupu resursa "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Dodavanje čitljiv sekundarni (jedan baza podataka)

Sljedeća naredba stvara čitljiv sekundarni baze podataka "mydb" s poslužitelja "srv2" u grupu resursa "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Dodavanje nije čitljiv sekundarni (elastic baza podataka)

Sljedeću naredbu stvara koje nisu čitljiv sekundarni baze podataka "mydb" u elastic baze podataka pod nazivom "ElasticPool1" s poslužitelja "srv2" u grupu resursa "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Dodavanje čitljiv sekundarni (elastic baza podataka)

Sljedeća naredba stvara čitljiv sekundarni baze podataka "mydb" u elastic baze podataka pod nazivom "ElasticPool1" s poslužitelja "srv2" u grupu resursa "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Uklanjanje sekundarne baze podataka

Pomoću cmdleta **Ukloni AzureRmSqlDatabaseSecondary** trajno prekinuti replikacije partnerstvo između sekundarne baze podataka i njegove primarne. Nakon prekida odnos sekundarne baze podataka postaje čitanja i pisanja baze podataka. Ako je prekinuta veza s sekundarne baze podataka naredba je uspjela, ali sekundarne će postati čitanja i pisanja nakon vratit će se povezivanje. Dodatne informacije potražite u članku [Uklanjanje AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) i [Razine servisa](sql-database-service-tiers.md).

Ovaj cmdlet zamjenjuje Zaustavi AzureSqlDatabaseCopy za replikaciju. 

To uklanjanje je ekvivalent prisilnog prekida koji uklanja replikacije vezu, a ostavlja bivši sekundarni kao samostalni baze podataka koji nisu u potpunosti replicirati prije prekida. Sve podatkovne veze će se očistiti bivši primarni i bivši sekundarne, ako ili kada postanu dostupne. Ovaj cmdlet vratit će se kada se Ukloni vezu replikacije. 


Da biste uklonili sekundarne, korisnici moraju imati pristup pisanje primarnih i sekundarnih baze podataka prema RBAC. Potražite u članku Upravljanje pristupom utemeljeno na ulogama detalje.

Sljedeće uklanja vezu replikacije baze podataka pod nazivom "mydb" poslužitelja "srv2" od grupa resursa "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Praćenje konfiguracija zemlj replikacije i stanju sustava

Nadzor poslovi obuhvaćaju nadzor konfiguracije zemlj replikacije i praćenje stanja replikacije podataka.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) može se koristiti za dohvaćanje informacija o vezama prema naprijed replikacije vidljivim u prikazu sys.geo_replication_links kataloga.

Sljedeća naredba dohvaća stanje veze replikacije između primarni baze podataka "mydb" i sekundarni na poslužitelju "srv2" od grupa resursa "rg2".

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o Active zemlj. – replikacije potražite u članku - [Active replikacije zemlj.](sql-database-geo-replication-overview.md)
- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)

