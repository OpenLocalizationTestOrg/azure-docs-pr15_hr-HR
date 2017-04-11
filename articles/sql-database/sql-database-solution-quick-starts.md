<properties
   pageTitle="Azure SQL baza podataka rješenja brzog pokretanja | Microsoft Azure"
   description="Dodatne informacije o rješenja Azure SQL baze podataka"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Istražite Azure SQL baza podataka rješenja brzog pokretanja

Ovaj članak sadrži pregled sustava Azure SQL baza podataka rješenja brzog pokretanja. Ove brzi pokreće nalaze se u spremištu uzoraka GitHub SQL Server i ilustrira korištenje baze podataka SQL u potpuno rješenje na temelju scenariji stvarnog života. Jednostavan priručnici korak kojima se ilustrira korištenje određene značajke SQL baze podataka, potražite u članku [vodiči za istraživanje Azure SQL baze podataka](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Pokušajte WingTipTickets pokazni videozapis, a stjecanja Laboratorija

Pokazni videozapis [WingTipTickets za baze podataka SQL Azure](https://github.com/microsoft/wingtiptickets) i stjecanja Laboratorija demonstrirati baze podataka SQL Azure i temeljenih na pretraživanju Azure Ogledni program koji se koristi za prodaju koncert karticama.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Prikupljanje i praćenje podataka o korištenju resursa preko više grupe

[Rješenja brzi početak rada: telemetrijskih Elastic skup pomoću komponente PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) nudi rješenje za prikupljanje i praćenje SQL baze podataka resursa preko više grupe u pretplatu. Kada imate velik broj baza podataka u pretplatu, je naporan praćenje svaki skup elastic zasebno.

Da biste riješili taj problem, možete kombinirati cmdleta ljuske PowerShell baze podataka SQL i T SQL upite za prikupljanje podataka o korištenju resursa iz većeg broja grupe i svoje baze podataka. Omogućuje praćenje i učinkovitije analiziranje korištenje resursa.

Ovaj brzi početak rada nudi skup skripte komponente PowerShell i T SQL upitima uz dokumentaciju na čemu služi rješenje te kako ga implementirate.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Početak rada s Elastic baze podataka u scenariju programa SaaS

 [Rješenja brzi početak rada: Elastic skup prilagođene nadzorne ploče za SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) nudi rješenje za softver-kao-na-rješenje (SaaS) scenarij u kojem se upravlja značajku Elastic baze podataka SQL baze podataka da biste prikazali učinkovit i skalabilni bazu podataka pozadinskog SaaS aplikacije.

U ovom rješenju će voditi kroz implementaciju web-aplikacijama. Aplikacija web omogućuje vizualizacija opterećenje generator opterećenja koji koristi prilagođene nadzorne ploče koje nadopunjuju Azure portal stvorenih elastic bazu podataka.

Ovaj brzi početak rada nudi opterećenje generator i nadzor web app uz u dokumentaciji o čemu služi aplikaciju i kako je koristiti.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Stvaranje baze podataka Azure SQL pomoću koda prvi razvoj i Framework entitet

Video i uzorak u [Kod prvog s novom bazom podataka](https://msdn.microsoft.com/data/jj193542.aspx) omogućuje Uvod u kod prvog razvoj namijenjen za novu bazu podataka. Ovaj scenarij pronalaze baze podataka koja ne postoji, ali koji će se stvoriti tako da prvi kod. Osim toga, scenarij stvara praznu bazu podataka u koji kod prvog dodaje nove tablice.

Kod najprije omogućuje definiranje modelu pomoću po klase C# ili Visual Basic .NET. Neobavezne dodatne konfiguracijske možete izvršiti pomoću atribute klase i svojstva ili pomoću fluent API-JA.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Integriranje Alati Elastic baze podataka u aplikaciju entitet Framework

Ogledna [Biblioteka klijentski Elastic baze podataka s Framework entitet](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) prikazuje promjene koje su vam potrebne da biste aplikaciji entitet Framework za integraciju s [alatima Elastic baze podataka](sql-database-elastic-scale-get-started.md). Fokus je na [shard mapiranje upravljanja](sql-database-elastic-scale-shard-map-management.md) i [usmjeravanje ovisne podataka](sql-database-elastic-scale-data-dependent-routing.md) s entitet Framework kod prvom pristupu za sastavljanje poruke.

U [Kod prvog uzorku nove baze podataka za EF](http://msdn.microsoft.com/data/jj193542.aspx) služi kao našem izvodi primjeru cijeloj ovaj uzorak. Uzorak koda koja se isporučuje se uz ovaj dokument je dio skup alata Elastic baze podataka uzoraka u primjere koda za Visual Studio.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Integriranje Alati Elastic baze podataka s sigurnosti na razini retka

[Složene aplikacije s Alati Elastic baze podataka i sigurnost na razini retka](sql-database-elastic-tools-multi-tenant-row-level-security.md) prikazuje promjene koje morate donijeti entitet Framework aplikaciju za integraciju [Alati Elastic baze podataka](sql-database-elastic-scale-get-started.md) s [sigurnosti na razini retka](https://msdn.microsoft.com/library/dn765131). Ovaj primjer prikazuje kako zajednički koristiti tih tehnologija da biste sastavili aplikacije s iznimno skalabilni podataka sloj koji podržava složene shards.

To se pomoću servisa za ADO.NET SqlClient ili Framework entitet. Ovaj primjer proširuje [Elastic baza podataka biblioteke klijenta s entitet Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) dodavanjem podrška za složene shard baze podataka.
Sastavlja program jednostavan konzole za stvaranje blogova i objave, s četiri klijenata i dvije složene shard baze podataka.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Stvaranje ankete online s aplikacijom Tailspin ankete

Ovaj [Tailspin ankete poslušajte aplikacija](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) je složene web-aplikacije, pod nazivom ankete, koja omogućuje korisnicima stvaranje online ankete. Uzorak adrese neke ključne opasnosti o načinu upravljanja identitetima korisnika u složene aplikacije, uključujući prijave, provjere autentičnosti, autorizacije i aplikacije uloge.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Dodatne informacije o najnovije značajke zaštite u SQL baze podataka s aplikacijom Contoso Clinic pokazni videozapis

[Pokazni videozapis Contoso Clinic aplikacije](https://github.com/Microsoft/azure-sql-security-sample) showcases najnovije značajke zaštite u SQL baze podataka.

## <a name="next-steps"></a>Daljnji koraci

[Istražite vodiči za baze podataka SQL Azure](sql-database-explore-tutorials.md)
