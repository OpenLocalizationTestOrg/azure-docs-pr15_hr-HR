<properties
    pageTitle="Aplikacije servisa za Azure i njegov utjecaj na postojećim Azure servisima"
    description="U članku se objašnjava kako nove aplikacije servisa za Azure i njegove značajke utjecati na postojeće usluge u Azure."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Aplikacije servisa za Azure i postojeće Azure usluge

U ovom se članku navode promjene na postojećim Azure servisima kao dio promjena dohvaćali zajedno nekoliko Azure usluga [Aplikacije servisa za Azure](https://azure.microsoft.com/services/app-service/), novi integrirani nuditi.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Pregled

[Aplikacije servisa za Azure](https://azure.microsoft.com/services/app-service/) je servis za nove i jedinstveni oblaka koja omogućuje inženjerima omogućuje stvaranje web- a mobilne aplikacije za bilo koju platformu i bilo kojeg uređaja. Aplikacije servisa za je integrirano rješenje namijenjenu pojednostavniti koji se ponavljaju kodiranje funkcije, Integracija s enterprise i SaaS sustavi i tijekom sastanka vaše potrebe za sigurnost, pouzdanosti i skalabilnost automatizacija poslovnih procesa.

Aplikacije servisa za objedinjuje sljedećih postojeće Azure servisa – [web-mjesta](https://azure.microsoft.com/services/websites/), [Mobile](https://azure.microsoft.com/services/mobile-services/)i [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) u jedan spojeni servis, prilikom dodavanja Napredna nove mogućnosti.  Aplikacije servisa omogućuje vam za hostiranje sljedeće aplikacije:

-   Web-aplikacije
-   Mobilne aplikacije
-   API aplikacije
-   Logika aplikacije

U sljedećoj tablici objašnjeno kako postojeću mapu servisa Azure na servis aplikacije i vrste aplikacije dostupne u njoj.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Postojeći servisi za Azure</th>
<th align="left", style="width:10%">Aplikacije servisa za Azure</th>
<th align="left", style="width:80%">Što se promijenilo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure web-mjesta</td>
<td align="left">Web-aplikacije</td>
<td align="left"><li>Azure vaše web-aplikacije servisa za isključivo ograničeno je na promjena naziva web-mjesta na web-aplikacije.
<p><li>Sve svoje postojeće instance web-mjesta koja su sada Web Apps u aplikacije servisa.</p>
<p><li>Možete pristupiti postojeće web-mjesta putem <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Portala za Azure</a>, gdje ćete pronaći sve svoje postojeće web-mjesta u odjeljku <em>Web-aplikacije</em>.</p>
<p><li><em>Web-Hosting planiranje</em> sada je <em>Plan za aplikaciju servisa</em>. U <em>Aplikaciju servisa planiranje</em> možete hostirati bilo koju aplikaciju vrstu aplikacije servisa, kao što je aplikacija Web, Mobile, logika ili API-JA.</p>
<p><li>Servis Web Azure aplikacija je Općenito dostupnost.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Dodatne informacije o web-aplikacije</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobilne usluge</td>
<td align="left">Mobilne aplikacije</td>
<td align="left"><p><li>Mobilne usluge i dalje biti dostupni kao samostalan servis i zadržavanje potpuno podržane.</p>
<p><li>Mobilne aplikacije je vrstu aplikacije na servis za aplikacije koji integrira sve funkcije mobilne usluge i još mnogo toga.</p>
<p><li>Jednostavno je <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrirati iz mobilnih usluga mobilne aplikacije</a>.</p>
<p><li>Kao dio aplikacije servisa za mobilne aplikacije se nove mogućnosti osim mobilne usluge, kao što su Integracija s lokalnog poslužitelja i sustavi SaaS pripremna slobodnih, WebJobs, mogućnosti bolje skaliranja i drugo.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Dodatne informacije o aplikacijama Mobile</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API aplikacije</td>
<td align="left">
<p><li>API aplikacija je nova vrsta aplikaciju u aplikacije servisa za koji vam omogućuje stvaranje i korištenje API-ji u oblaku.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Dodatne informacije o aplikacijama API</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logika aplikacije</td>
<td align="left">
<p><li>Logika aplikacija je nova vrsta aplikaciju u aplikacije servisa koji omogućuje jednostavno automatizirali poslovni proces.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Dodatne informacije o aplikacijama logike</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk Services</td>
<td align="left">Aplikacija BizTalk API-JA</td>
<td align="left">
<li><p>BizTalk servise i dalje biti dostupni kao samostalan servis i zadržavanje potpuno podržane.</p>
<li><p>Mogućnosti usluge BizTalk integrirati u aplikacije servisa za kao API aplikacije Omogućavanje korisnicima integraciju enterprise aplikacija i scenarija integracije B2B pomoću vrsta aplikaciju u aplikacije servisa za</p>
<li><p>S aplikacijama logike možete odmah automatizacija poslovnih procesa pomoću vizualni dizajn iskustvo da biste stvorili tijekova rada</p></td>
</tr>
</tbody>
</table>

Da biste saznali više, posjetite [dokumentaciji o servisu aplikacije](https://azure.microsoft.com/documentation/services/app-service/).
