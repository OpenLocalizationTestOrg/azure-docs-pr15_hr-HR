<properties
   pageTitle="Raspodijeljeno transakcije putem oblaka baze podataka"
   description="Pregled Elastic transakcije baze podataka Azure SQL baze podataka"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Raspodijeljeno transakcije putem oblaka baze podataka

Elastic transakcije baze podataka SQL Azure baze podataka (SQL DB) omogućuju pokretanje transakcije koje se protežu na više baza podataka u SQL baze podataka. Elastic transakcije baze podataka za SQL DB dostupni su za .NET aplikacije pomoću ADO .NET i integracija s poznato okruženje za programiranje pomoću klase [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) . U biblioteku, pročitajte članak [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Lokalno, takve scenarij obično potrebna radi Microsoft Distributed transakcije koordinator (MSDTC). Budući da MSDTC nije dostupna za platformu-kao-na-servisne aplikacije u Azure, mogućnost usklađivanju raspodijeljeno transakcije sadrži su izravno integrirani SQL DB. Aplikacije možete se povezati s bilo kojeg SQL baze podataka da biste pokrenuli distribuirane transakcija, a jedan baza podataka će proziran koordiniranje distribuirane transakcije, kao što je prikazano na sljedećoj slici. 

  ![Raspodijeljeno transakcije baze podataka SQL Azure pomoću transakcije elastic baze podataka ][1]

## <a name="common-scenarios"></a>Uobičajeni scenariji

Elastic transakcije baze podataka za SQL DB Omogućivanje aplikacije za mijenjanje atomske podataka pohranjenih u nekoliko različitih baze podataka SQL. Pretpregled usredotočuje se na klijentsko razvoj sučelja u C# i .NET. Poslužiteljsko okruženje pomoću T SQL planu je za kasnije.  
Transakcije baze podataka za elastic pronalaze sljedećim scenarijima:

* Aplikacija za više baze podataka u Azure: S ovom scenariju podataka je okomito particije preko nekoliko baza podataka sustava SQL DB tako da se različite vrste podataka koji se nalaze na različite baze podataka. Neki postupci zahtijevaju promjene podataka koji se sinkroniziraju dvije ili više baza podataka. Aplikacija koristi transakcije elastic baze podataka za koordiniranje promjene preko baze podataka i provjerite jesu li atomicity.

* Aplikacija za sharded baze podataka u Azure: S ovom scenariju sloju podataka koristi [Biblioteka klijentski Elastic baze podataka](sql-database-elastic-database-client-library.md) ili samopotpisani sharding za vodoravno particija podatke preko mnoge baze podataka u SQL baze podataka. Jednom slučaju istaknutu koristi je potrebno izvršiti atomske promjene sharded više klijentske aplikacije prilikom promjene obuhvaćati samoposlužni. Zamislite da je na primjer prijenos iz jednog klijenta na drugi, obje residing različite baze podataka. Drugi slučaj još preciznije sharding prema potrebama kapaciteta za veliki klijent koji se obično shodno podrazumijeva da neke atomske operacije nije potrebno proširila nekoliko baza podataka koristi u istom klijentu. Treći slučaj još atomske ažuriranja referentni podatke koji su replicirati preko baze podataka. Atomske, transakcije, operacije duž te retke moguće je koordinirano sada preko nekoliko baze podataka pomoću pretpregleda.
Transakcije elastic baze podataka koristite two-phase potvrdi da bi transakcije atomicity preko baze podataka. To je dobro rješenje za transakcije koje obuhvaćaju manje od 100 baze podataka na vrijeme unutar jedne transakcije. Ta ograničenja se primjenjuju, ali jedno očekivati performanse i postocima uspjeha za elastic transakcije baze podataka da biste se kada premašuju ta ograničenja.


## <a name="installation-and-migration"></a>Instalacija i migracije

Mogućnosti za elastic transakcije baze podataka u SQL DB se odvija pomoću ažuriranja u biblioteke .NET System.Data.dll i System.Transactions.dll. U DLL bibliotekama provjerite je li taj two-phase potvrdi koristi se gdje je potrebno da biste bili sigurni atomicity. Da biste pokrenuli razvoja aplikacije koje koriste transakcije elastic baze podataka, instalirajte [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) ili noviju verziju. Kada se pokrene u starijoj verziji programa .NET framework, transakcija neće uspjeti Promicanje distribuirane transakcije i iznimku se zaokružuje.

Nakon instalacije, možete koristiti raspodijeljeno transakcije API-ji u System.Transactions s vezama na SQL baze podataka. Ako imate postojeće MSDTC aplikacije pomoću tih API-ji, jednostavno ponovno postojeće aplikacija za .NET 4.6 nakon instalacije na 4.6.1 Framework. Ako projekte ciljani .NET 4.6, automatski će se koristiti ažurirani DLL bibliotekama s novom verzijom Framework i raspodijeljeno transakcije API poziva u kombinaciji s vezama na SQL DB sada će uspjeti.

Imajte na umu da transakcije elastic baze podataka ne zahtijeva instalacije MSDTC. Umjesto toga transakcije elastic baze podataka izravno upravlja po, a u SQL baze podataka. To značajno pojednostavljuje oblaka scenariji jer implementacije MSDTC nije potrebno za uporabu distribuirane transakcije baze podataka SQL. Odjeljak 4 detaljno objašnjava kako uvesti transakcije elastic baze podataka i potreban .NET framework zajedno s oblaka aplikacija za Azure.

## <a name="development-experience"></a>Sučelje za razvoj

### <a name="multi-database-applications"></a>Aplikacija za više baze podataka

Sljedeći primjer kod koristi poznato okruženje za programiranje sa .NET System.Transactions. Klase TransactionScope uspostavlja razine okolne transakcija u .NET. (Li "razine okolne transakcija" je onaj koji se nalaze u trenutnoj niti). Sve veze koje se otvara programa u TransactionScope sudjelovati u transakcije. Ako sudjelujete različite baze podataka, transakcije automatski se razinom raspodijeljeno transakcijom. Rezultat transakcije upravlja postavkama opsega da biste dovršili da biste naznačili na potvrdi.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Aplikacija za sharded baze podataka
 
Elastic transakcije baze podataka za SQL DB podržavaju koordinaciju raspodijeljeno transakcije kojemu koristiti metodu OpenConnectionForKey klijenta biblioteke elastic baze podataka da biste otvorili veze za skaliranu out podatke sloju. Razmislite o slučajevima u kojima morate jamči transakcijskih dosljednost promjene preko nekoliko različitih sharding vrijednosti ključa. Veze na shards hosting vrijednosti ključa različite sharding su brokered pomoću OpenConnectionForKey. U slučaju Općenito veze može biti u različitim shards tako da se osiguravanje transakcijskih jamstva zahtijeva raspodijeljeno transakcije. Sljedećim primjerom koda ilustrira takvog. Pretpostavlja se da varijabla pod nazivom shardmap služi za predstavljanje shard mapu iz biblioteke za klijent elastic baze podataka:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.NET instalacije za servise u Oblaku Azure

Azure nudi nekoliko ponude aplikacijama .NET glavnog računala. Usporedba različitih ponude dostupan je u [aplikacije servisa za Azure, servise u Oblaku i virtualnim strojevima usporedbe](../app-service-web/choose-web-site-cloud-service-vm.md). Ako je goste OS na nuditi manji od .NET 4.6.1 potrebne za elastic transakcije, morate nadograditi goste OS 4.6.1. 

Za Azure aplikacije usluge nadogradnje za goste OS trenutno nisu podržani. Za Azure virtualnim strojevima jednostavno prijavite na VM i pokrenite instalacijski program za najnoviji .NET framework. Za servise u Oblaku Azure, morate uključiti instalacije novijom verzijom .NET u zadatke prilikom pokretanja implementaciju sustava. Koncepti i koraci su navedenih u [Instalirati .NET na servis ulozi oblaka](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Imajte na umu instalacijski program za .NET 4.6.1 možda ćete morati dodatnog prostora za pohranu privremene tijekom postupka pokretački na servise u oblaku Azure od instalacijski program za .NET 4.6. Da biste osigurali uspješnu instalaciju, morate povećati Privremena pohrana servisa Azure oblak u datoteci ServiceDefinition.csdef u odjeljku LocalResources i postavki okruženja pokretanje zadatka, kao što prikazuje sljedeći primjer:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transakcije preko više poslužitelja

Transakcije baze podataka za elastic podržani su na različitim logičke poslužiteljima u bazi podataka SQL Azure. Kada transakcije Unakrsna logičke poslužitelja ograničenja, koji sudjeluju poslužitelje najprije morate unijeti u odnosu međusobna komunikacije. Kada je uspostavljena je odnos komunikacije, sve baze podataka u bilo kojem od dva poslužitelja može sudjelovati u elastic transakcija s bazama podataka iz drugih poslužitelja. Za transakcije koje se protežu na više od dva logičke poslužitelja, odnos komunikacije mora biti ovlasti za sve par logičke poslužitelja.

Koristite sljedeće Cmdlete ljuske PowerShell za upravljanje komunikacije poslužitelj za više odnosa za transakcije elastic baze podataka:

* **Novo AzureRmSqlServerCommunicationLink**: pomoću tog cmdleta da biste stvorili novi komunikacije odnos između dvije logičke poslužiteljima baze podataka SQL Azure. Odnos je simetričnu što znači da se oba poslužitelji možete pokrenuti transakcija s drugi poslužitelj.
* **Get-AzureRmSqlServerCommunicationLink**: pomoću tog cmdleta za dohvaćanje postojeće odnose komunikacije i njihova svojstva.
* **Uklanjanje AzureRmSqlServerCommunicationLink**: pomoću tog cmdleta da biste uklonili postojeći odnos komunikacije. 


## <a name="monitoring-transaction-status"></a>Nadzor stanje transakcije

Pomoću upravljanja dinamičnih prikaza (DMVs) u SQL DB monitor stanje i napredak transakcija tijeku elastic baze podataka. Sve DMVs vezane uz transakcija relevantne distribuirane transakcija u SQL baze podataka. Možete pronaći na odgovarajući popis DMVs ovdje: [transakcije povezane dinamički upravljanje prikaza i funkcijama (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Ove DMVs su osobito korisni:

* **sys.dm\_pri\_aktivni\_transakcije**: popis trenutno aktivan transakcije i njihovo stanje. Stupac UOW (radna jedinica) možete prepoznati transakcije različite podređeni koji pripadaju istom raspodijeljeno transakcijom. Sve transakcije unutar iste raspodijeljeno transakcije imaju iste vrijednosti UOW. Potražite u [dokumentaciji DMV](https://msdn.microsoft.com/library/ms174302.aspx) više pojedinosti.
* **sys.dm\_pri\_baze podataka\_transakcije**: pruža dodatne informacije o transakcije, kao što su položaj transakcije u zapisnik. Potražite u [dokumentaciji DMV](https://msdn.microsoft.com/library/ms186957.aspx) više pojedinosti.
* **sys.dm\_pri\_zaključavanja**: daje informacije o zaključavanja koji trenutno zaključale tijeku transakcije. Potražite u [dokumentaciji DMV](https://msdn.microsoft.com/library/ms190345.aspx) više pojedinosti.

## <a name="limitations"></a>Ograničenja 

Sljedeća ograničenja primjenjuju trenutno elastic transakcije baze podataka u SQL baze podataka:

* Podržani su samo transakcije preko baze podataka u SQL baze podataka. Drugi [X / otvorite XA](https://en.wikipedia.org/wiki/X/Open_XA) davatelji resursa i baza podataka izvan SQL DB ne može sudjelovati u transakcije elastic baze podataka. To znači da transakcije elastic baze podataka nije moguće proširila lokalnog sustava SQL Server i baze podataka SQL Azure. Raspodijeljeno transakcije lokalno i dalje koristiti MSDTC. 
* Podržani su samo klijent Koordinirano transakcije iz aplikacije za .NET. Poslužiteljsko podrška za T-SQL kao što su POČETKA TRANSAKCIJE DISTRIBUTED je planiranog, ali još nije dostupno. 
* Podržani su samo baze podataka na V12 za baze podataka SQL Azure.
* Transakcije preko WCF services nisu podržani. Na primjer, imate način za WCF servisa koji se izvršava transakcije. Stavljanja poziva unutar opsega transakcije neće uspjeti kao [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="additional-resources"></a>Dodatni resursi

Još nisu putem mogućnosti elastic baze podataka za aplikacije za Azure? Pogledajte naš [Dokumentaciju karta](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Za pitanja, provjerite stupili nam na [forumu SQL baze podataka](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) i zahtjeve za značajke, dodajte ih na [forum za povratne informacije SQL baze podataka](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



