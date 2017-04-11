<properties
 pageTitle="Ograničenja za raspored i zadanim postavkama"
 description="Ograničenja za raspored i zadanim postavkama"
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
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>Ograničenja za raspored i zadanim postavkama

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Raspored kvote, ograničenja, zadanih vrijednosti i Reguliranje

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>U zaglavlju x ms-zahtjev-id

Svaki zahtjev u odnosu na servis rasporeda vraća zaglavlja odgovora pod nazivom**x ms-zahtjev-id**. Ovo Zaglavlje sadrži neprozirne vrijednost koja služi kao jedinstvena identifikacija zahtjev.

Ako zahtjev za dosljedno ne uspijeva i provjeriti je li zahtjev ispravno formulated, mogu koristiti tu vrijednost za Prijavite pogrešku Microsoftu. U izvješću, obuhvaćaju vrijednost x ms-zahtjev-id, očekivano vrijeme stvorenih zahtjev, identifikator pretplate, zbirke posla i/ili posao, a Vrsta postupka koji pokušaj zahtjev.

## <a name="see-also"></a>Vidi također


 [Što je raspored?](scheduler-intro.md)

 [Azure koncepata raspored, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure referenca cmdleta ljuske PowerShell za raspored](scheduler-powershell-reference.md)

 [Azure visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Azure izlaznu provjeru autentičnosti rasporeda](scheduler-outbound-authentication.md)
