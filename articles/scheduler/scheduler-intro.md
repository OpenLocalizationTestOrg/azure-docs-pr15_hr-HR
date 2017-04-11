<properties
 pageTitle="Što je raspored Azure? | Microsoft Azure"
 description="Azure raspored omogućuje deklarativno opisuju akcije da biste pokrenuli u oblak. Datoteka, a zatim rasporede i automatsku tih akcija."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Što je raspored Azure?

Azure raspored omogućuje deklarativno opisuju akcije da biste pokrenuli u oblak. Datoteka, a zatim rasporede i automatsku tih akcija.  Planer to pomoću [portala za Azure](scheduler-get-started-portal.md), kod, [REST API -JA](https://msdn.microsoft.com/library/mt629143.aspx)ili Azure PowerShell.

Raspored stvara održava te poziva zakazani rad.  Raspored hostira sve radnih opterećenja ili pokrenuti kod. IT samo kod _poziva_ hostirate negdje drugdje – u Azure, informacije o lokalnom ili s nekog drugog davatelja usluga. Poziva putem HTTP, HTTPS, red čekanja za pohranu, reda bus servisa ili temu bus servisa.

Raspored raspored [zadataka](scheduler-concepts-terms.md), čuva povijest posla izvođenja rezultata da jedan možete pregledati i deterministically i pouzdano Zakazuje radnih opterećenja će se pokrenuti. Azure WebJobs (dio značajka web-aplikacije u aplikacije servisa za Azure) i druge Azure zakazivanje mogućnosti koristite raspored u pozadini. [Raspored REST API -JA](https://msdn.microsoft.com/library/mt629143.aspx) omogućuje upravljanje komunikacije za ove akcije. Kao takve, raspored podržava [složene analize i naprednih ponavljanja](scheduler-advanced-complexity.md) jednostavno.

Postoji nekoliko scenarija koje sami posuđivati korištenje raspored. Ako, na primjer:

+ _Ponavljanje PRIMJERNU:_ Povremeno prikupljanje podataka iz Twitter u sažetak sadržaja.
+ _Dnevno održavanje:_ Dnevni Čišćenje zapisnika, izvođenja sigurnosne kopije te druge zadatke održavanja. Na primjer, administrator možda odlučite sigurnosnu kopiju baze podataka na 1 12.00 podne svakodnevno za sljedećih devet mjeseci.

Raspored omogućuje stvaranje, ažuriranje, brisanje, prikaz i upravljanje zadacima i [posao zbirke](scheduler-concepts-terms.md) programski putem skripti i na portalu.

## <a name="see-also"></a>Vidi također

 [Azure koncepata raspored, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Kako izraditi složene analize i naprednih ponavljanja s Azure raspored](scheduler-advanced-complexity.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure referenca cmdleta ljuske PowerShell za raspored](scheduler-powershell-reference.md)

 [Azure visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Azure ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)

 [Azure izlaznu provjeru autentičnosti rasporeda](scheduler-outbound-authentication.md)
