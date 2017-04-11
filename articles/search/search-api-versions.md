<properties
   pageTitle="API verzije Azure pretraživanja | Microsoft Azure | API pretraživanja"
   description="Verzija pravila za Azure pretraživanje REST API-ji i klijentska biblioteka u .NET SDK."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>API verzije u pretraživanju Azure

Azure pretraživanje kumulira out ažuriranje značajki redovito. Ponekad, ali ne uvijek ažuriranja potrebna nam da biste objavili novu verziju naš API da bi se očuvao kompatibilnost sa starijim verzijama. Nova verzija za objavljivanje omogućuje vam da biste odredili kada i kako integrirati ažuriranja servisa za pretraživanje u kodu.

U pravilu, ne možemo pokušajte objaviti nove verzije samo kada je to potrebno, jer on može obuhvaćati neke trud li nadograditi kod pomoću nove verzije API-JA. Ne možemo će objaviti samo novu verziju Ako ipak Trebamo da biste promijenili neke aspekt API na način koji se prelama kompatibilnost sa starijim verzijama. To se može dogoditi zbog rješenja s postojećih značajki ili zbog nove značajke koje se mijenjaju postojeće API površina.

Ne možemo slijedite isti pravilo za SDK ažuriranja. Pretraživanje SDK Azure slijedi pravila [semantičkog rad s verzijama](http://semver.org/) , što znači da je njegova verzija ima tri dijela: glavnih Sporedna te sastavljanje broj (na primjer, 1.1.0). Ne možemo objavit ćemo nove glavne verzije SDK samo u slučaju promjene koje prijeloma kompatibilnost sa starijim verzijama. Značajka ažuriranja nerastavljajuće smo će povećali pomoćnu verziju, a za popravci programskih pogrešaka smo samo povećava verziju Sastavi.

##<a name="snapshot-of-current-versions"></a>Snimka trenutne verzije 

Ispod je snimke trenutne verzije svih programskog sučelja Azure pretraživanje. Roadmaps i druge detalje pronaći ćete u sljedećim odjeljcima ovog dokumenta.

Sučelja|Najnovija glavna verzija|Status
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Općenito dostupan, objavio veljača 2016
[Pretpregled SDK .NET](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0 pretpregled|Pretpregled, objavio kolovoz 2016
[Servis REST API-JA](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015 02 28|Općenito dostupan
[Servis REST API pretpregled](search-api-2015-02-28-preview.md)|2015-02-28-pretpregled|Pretpregled
[Upravljanje REST API-JA](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015 08 19|Općenito dostupan

Za REST API-ji, uključujući na `api-version` na svaki poziv potreban je. To olakšava ciljne određenu verziju, kao što je pretpregled API-JA. Sljedeći primjer prikazuje način `api-version` parametar:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Iako ima svaki zahtjev za `api-version`, preporučujemo da koristite istu verziju za sve zahtjeve za API-JA. Ovo je osobito true kad se nove verzije API predstavljanje atributa ili operacije koje ne prepoznaje prethodne verzije. Miješanje API verzije može imati neočekivane posljedice i moraju se izbjegavati.
> 
Servis REST API-JA i upravljanje REST API-JA su određuju neovisno o međusobno povezani. Sve sličnosti u brojeve verzija je zajednički slučajnih.

Općenito dostupan (ili GA) API-ji mogu se koristiti u radnog i filtrirat ugovore o razini usluge Azure. Pregled verzije imaju eksperimentalne značajke koje nisu uvijek preneseni GA verziju. **Preporučujemo da nikako protiv pomoću pretpregleda API-ji u aplikacijama proizvodnje.**

##<a name="sdk-version-roadmap"></a>Vodič za verziju SDK

U svakoj verziji .NET SDK pronalaze određenu verziju servisa REST API-JA. Značajke su poslednjeg u REST API-JA prvi put, a zatim implementirana u SDK-a.

.NET SDK sada je Općenito dostupan, a zatim je već sugovornike na novu verziju. U sljedećoj su tablici izgleda unaprijed u budućim verzijama SDK tako da imate uvid uskoro tako dalje.

.NET SDK verzija|Verzija REST API-JA|Značajke|ETA
----------------|----------------|--------|---
1.1|2015 02 28|Sintaksa upita Lucene|Veljača 2016
2.0 pretpregled|2015-02-28-pretpregled|Prilagođeni analyzers, blobova platforme Azure i tablice indexers, mapiranja polja ETags|Kolovoz 2016
2.x|Novu verziju GA API-JA|Jednako kao i 2.0 pretpregled|Prijevremeni Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>O verzijama pretpregled i načelu dostupan

Azure pretraživanje uvijek unaprijed izdaje eksperimentalne značajki preko REST API-JA prvi put, zatim kroz verzije predizdanja .NET SDK.

Pregled značajki ne jamči migrirati GA izdanje. Dok značajke u verziji GA uzimaju stabilan i vjerojatno da biste promijenili uz iznimku small kompatibilan rješenja i poboljšanja, pretpregled značajke dostupne su za testiranje i prebacivanje s ciljem prikupljanje povratnih informacija na značajku dizajna i implementaciju. 

Međutim, jer je pretpregled značajke podložne su promjenama, preporučujemo da protiv pisanja koda za proizvodnje koji traje ovisnosti na pregled verzije. Ako koristite stariju verziju pretpregled, preporučujemo da migracije obično dostupnih (GA) verzija. 

Za .NET SDK: vodič za migraciju kod možete pronaći na [nadogradnju .NET SDK](search-dotnet-sdk-migration.md).

Općenito dostupan znači sada je Azure pretraživanje u odjeljku ugovor razini usluge (SLA). Na SLA možete pronaći na [Azure ugovore o razini usluge pretraživanja](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

