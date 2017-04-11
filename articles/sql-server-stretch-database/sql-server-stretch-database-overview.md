<properties
    pageTitle="Razvlačenje pregled baze podataka | Microsoft Azure"
    description="Saznajte kako rastezanje baze podataka prenosi podatke Hladna proziran i sigurno oblak Microsoft Azure."
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
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Razvlačenje pregled baze podataka

Razvlačenje baze podataka prenosi podatke Hladna proziran i sigurno oblak Microsoft Azure.

Ako samo želite započeti s bazom podataka rastezanje odmah, potražite u članku [Početak rada tako da pokrenete omogućiti baze podataka za korištenje čarobnjaka za rastezanje](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Koje su prednosti rastezanje baze podataka?
Razvlačenje baze podataka pruža sljedeće prednosti:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Omogućuje trošak\-učinkovitih dostupnost Hladna podataka
Razvlačenje Toplo Hladna transakcijskih podataka i dinamički iz sustava SQL Server za Microsoft Azure s bazom podataka sustava SQL Server rastezanje. Za razliku od tipičnog podatkovnog Hladna prostor za pohranu, vaši podaci uvijek je Internetu i dostupan za upit. Možete unijeti više vremenskih crta zadržavanja podataka bez prekidanje banke za velike tablice, kao što su korisnička redoslijed povijest. Prednosti na malom troškova Azure umjesto skaliranje skupi na\-lokalno spremište. Odaberite cijene sloju i konfiguriranje postavki na portalu Azure da biste zadržali kontrolu nad troškove. Promjena veličine prema gore ili dolje po potrebi. Posjetite stranicu [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) detalje.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Nije potreban promjene upita ili aplikacije
Pristup vašim podacima sustava SQL Server jednostavno bez obzira na to jesu li se\-lokalno ili Rastegnuto u oblak.  Postavljanje pravilnika za koji određuje gdje podaci se pohranjuju i SQL Server rukuje premještanje podataka u pozadini. Cijelu tablicu je uvijek online i možete. Pa rastezanje baze podataka ne zahtijeva promjene postojeće upite i aplikacija – mjesto podataka nije potpuno proziran aplikaciji.

### <a name="streamlines-on-premises-data-maintenance"></a>Pojednostavnjuje na\-lokalni održavanje podataka
Smanjivanje na\-lokalni održavanje i prostora za pohranu za vaše podatke. Sigurnosno kopiranje za vaše na\-lokalnim podacima brže izvoditi te da završi unutar prozora Održavanje. Automatsko pokretanje sigurnosne kopije za oblak dio vaše podatke. Vaš na\-lokalno spremište potrebama su znatno smanjiti. Azure prostora za pohranu može biti manji pozivnim od dodavanja na 80%\-lokalni SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Zadržava podataka sigurne čak i tijekom migracije
Uživajte brigu kao rastezanje najvažnije aplikacija sigurno s oblakom. SQL Server uvijek šifrirane nudi šifriranje za podatke u pokretu. Redak razinu sigurnosti (RLS) i druge napredne značajke sigurnosti SQL Server i raditi s bazom podataka rastezanje radi zaštite vaše podatke.

## <a name="what-does-stretch-database-do"></a>Čemu služi rastezanje baze podataka?
Kada omogućite rastezanje baze podataka za SQL Server instanci, baze podataka i barem jedna tablica, Rastegnuto baze podataka tihu započinje migriranja Hladna podataka u Azure.

-   Ako u zasebnu tablicu pohranili Hladna podatke, možete migrirati cijelu tablicu.

-   Ako tablica sadrži najnovije i Hladna podatke, možete odrediti funkcija filter da biste odabrali retke koje želite migrirati.

**Ne morate promijeniti postojeće upite i aplikacije klijenta.** Nastavite pristupiti objedinjenog lokalnih i udaljenih podatke, čak i tijekom migracije podataka. Postoji mali Latencija za udaljene upite, ali samo naiđete na ovom kašnjenje kada upit Hladna podataka.

Ako dođe do pogreške tijekom migracije, **Rastegnuto baze podataka osigurava nema podataka o izgubiti** . Ima i Ponovi logika učiniti veze probleme koji se pojavljuju tijekom migracije. Prikaz za dinamičku upravljanja omogućuje stanje migracije.

**Zaustavite prijenos podataka** otkloniti poteškoće s lokalnog poslužitelja ili Maksimiziraj propusnost mreže dostupna.

![Pregled rastezanje baze podataka][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Baza podataka rastezanje namijenjen vama?
Ako vam na raspolaganju sljedeće naredbe, rastezanje baze podataka može pridonijeti odgovaraju potrebama i rješavanje problema.

|Ako ste odluka maker|Ako se nalazite u DBA|
|------------------------------|-------------------|
|Imam li zadržati transakcijskih podatke na dulje vrijeme.|Veličina Moja tablica će sve dobiti iz kontrole.|
|Ponekad imam upit Hladna podatke.|Korisnicima pretpostavimo da žele pristup Hladna podacima, ali će ga samo rijetko koristiti.|
|Imam aplikacije, uključujući starije aplikacije koje se neće ažurirati.|Imam da biste zadržali kupnjom i dodavanje dodatnog prostora za pohranu.|
|Želim da biste pronašli način da biste spremili novac na prostora za pohranu.|Nije moguće sigurnosno kopiranje ili vraćanje takve velike tablice unutar na SLA.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Kakvu vrstu baze podataka i tablice su ikone za bazu podataka rastezanje?
Razvlačenje baze podataka ciljnih web-mjesta transakcijskih baze podataka s velikim količinama Hladna podataka koji se pohranjuju u malim brojem tablica. U ovim su tablicama mogu sadržavati više od milijarde redaka.

Ako koristite značajku vremenski tablice sustava SQL Server 2016, koristite rastezanje bazu podataka da biste migrirali potpuno ili djelomično povijest povezane tablice cijena\-učinkovitih pohranom na servisu Azure. Dodatne informacije potražite u članku [Upravljanje zadržavanja od povijesne podataka u sustavu određuju vremenski tablica](https://msdn.microsoft.com/library/mt637341.aspx).

Korištenje rastezanje Savjetnik za baze podataka, značajka servisa SQL Server 2016 nadograditi Savjetnik za prepoznavanje baze podataka i tablice za bazu podataka rastezanje. Dodatne informacije potražite u članku [bazama podataka za otkrivanje i tablica za bazu podataka rastezanje](sql-server-stretch-database-identify-databases.md). Da biste saznali više o potencijalne probleme blokiranja, potražite u članku [ograničenja za rastezanje bazu podataka](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Probnu vožnju rastezanje baze podataka
**Testirajte pogon rastezanje baze podataka s oglednu bazu podataka AdventureWorks.** Da biste dobili oglednu bazu podataka AdventureWorks, preuzmite barem datoteke baze podataka i uzorke i skripte datoteke iz [ovdje](https://www.microsoft.com/download/details.aspx?id=49502). Kada se vratite na bazu oglednih podataka instance komponente SQL Server 2016, raspakiraj datoteku uzorka, i otvorite datoteku rastezanje DB uzorka iz mape rastezanje DB. Pokrenite skripte u tu datoteku da biste provjerili prostor koji se koristi podatke prije i nakon što ste omogućili rastezanje bazu podataka, da biste pratili tijek migracije podataka, a da biste potvrdili koji možete nastaviti upita postojeće podatke, a zatim Umetanje novih podataka tijekom i nakon migracije podataka.

## <a name="next-step"></a>Sljedeći korak
**Odredite baze podataka i tablice koje su ikone za rastezanje bazu podataka.** Preuzmite Savjetnik nadogradnje za SQL Server 2016 i pokrenite rastezanje Savjetnik za baze podataka da biste odredili baze podataka i tablice koje su ikone za rastezanje bazu podataka. Razvlačenje Savjetnik za baze podataka određuje i tog problema. Dodatne informacije potražite u članku [bazama podataka za otkrivanje i tablica za bazu podataka rastezanje](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
