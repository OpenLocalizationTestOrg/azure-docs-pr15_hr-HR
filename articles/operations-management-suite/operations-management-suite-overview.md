<properties
   pageTitle="Pregled paketa Management (OMS) operacije | Microsoft Azure"
   description="Microsoft operacije upravljanja paketa (OMS) je Microsoftov oblaku IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku.  U ovom se članku prepoznaje u različitim servisima obuhvaćeno OMS i navode veze na detaljne sadržaja."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Što je operacije upravljanja paket (OMS)?

Microsoft operacije upravljanja paket (OMS) je Microsoftov oblaku IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku.  Budući da se OMS implementira kao servis u oblaku, možete odrediti je s radom brzo s minimalnim ulaganja infrastrukture Services.  Nove značajke isporučuju se automatski spremati trenutno održavanje i nadogradite troškove.

Osim pružanja usluge koristan samostalno, OMS možete integrirati s centar sustava komponente kao što su Operations Manager centar sustava da biste proširili postojeće upravljanje ulaganja u oblak.  Centar za sustav i OMS zajednički možete raditi omogućuje potpunu hibridno okruženje za upravljanje.

Sljedeći odjeljci sadrže više razine opis područja drugu vrijednost OMS i servise koje ih provesti.  Može se odnositi na OMS arhitektura pregled različitih komponenata OMS prije pregleda detaljne dokumentaciju za svaki unos.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Uvid i analitiku](media/operations-management-suite-overview/icon-insight-analytics.png) Uvid i analitiku

[Prijava analitiku](http://azure.microsoft.com/documentation/services/log-analytics) omogućuje prikupljanje, povezivanje, pretraživanje i djelovanje na zapisnika i performanse podacima generira operacijski sustavi i aplikacije. Pruža u stvarnom vremenu radu uvide pomoću integrirane pretraživanja i prilagođene nadzornih ploča lako analizirati milijune zapisa preko svih radnih opterećenja i poslužitelji bez obzira na njihov lokacije.

Jednostavno možete dodati rješenja zapisnika analize koje definiraju prikupljeni podaci i logike za njegov analizu.  Rješenja mogu sadržavati dodatna funkcionalnost koja se automatski isporučuje agenata s minimalnim ili bez konfiguracije.  Osim pomoću alata za analizu nudi pojedinačne rješenja, možete izvršiti prilagođene pretrage preko cijeli skup podataka da bi se povezivanje podataka između sustava i aplikacije.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatizacija i kontrola](media/operations-management-suite-overview/icon-automation-control.png) Automatizacija i kontrola

Azure Automatizacija automatizira administratora procesa pomoću [runbooks](../automation/automation-runbook-types.md) koje su na temelju PowerShell i pokretanje u Azure oblaka.  Runbooks možete pristupiti bilo koji proizvod ili uslugu koji se upravlja sa servisom PowerShell uključujući resursa u drugim oblaka kao što su Amazon Web Services (AWS).  Runbooks je moguće izvršiti i na poslužitelju u centru za lokalnih podataka da biste upravljali Lokalni resursi.

Azure Automatizacija pruža mogućnost upravljanja konfiguracije s [PowerShell DSC](../automation/automation-dsc-overview.md).  Stvaranje i upravljanje resursima DSC smješten u Azure i njihove primjene oblaka i lokalni sustavi definiranje i automatski Nametni njihovu konfiguraciju.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Zaštita i oporavak](media/operations-management-suite-overview/icon-protection-recovery.png) Zaštita i oporavak Izrada

[Sigurnosno kopiranje Azure](http://azure.microsoft.com/documentation/services/backup) štiti podatke iz aplikacije, a zadržava godina bez bilo koje veliko slovo ulaganja i s minimalnim operacijski troškovi.  To možete sigurnosno kopirajte podatke iz fizičkih i virtualne poslužitelje Windows osim radnih opterećenja aplikacije kao što je SQL Server i SharePoint.  I korištenja sustava centra podataka zaštitu Manager (DPM) za replikaciju zaštićeni podaci za Azure zalihosti i dugoročnu prostora za pohranu.

[Oporavak web-mjesta Azure](http://azure.microsoft.com/documentation/services/site-recovery) doprinosa svoje poslovanje i Izrada strategije oporavak (BCDR) tako da orchestrating replikacije, prebacivanje i oporavak lokalnog Hyper-V virtualnim strojevima, VMware virtualnim strojevima i fizičke poslužitelji Windows/Linux. Možete replicirati strojeva centar za sekundarne podatkovne ili proširivanje podatkovnog centra tako da ih replikaciju za Azure. Oporavak web-mjesta omogućuje jednostavno prebacivanje i oporavak za radnih opterećenja. Izrada oporavak mehanizme kao što je SQL Server AlwaysOn se integrira i omogućuje oporavak planove za jednostavno prebacivanje radnih opterećenja koji su tiered preko više računala.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS sigurnost i usklađenost](media/operations-management-suite-overview/icon-security-compliance.png) Sigurnost i usklađenost
Sigurnost i usklađenost olakšava prepoznavanje, procijenite i prevladavanje sigurnosni rizik infrastrukture.  Te značajke OMS primjenjuju se više rješenja u zapisnik analize koje analiziraju podaci iz zapisnika i konfiguracija agent Systems kao pomoć u osiguravanje tijeku sigurnost okruženja.

- [Sigurnost i nadzora rješenje](oms-security-getting-started.md ) prikuplja i analizira sigurnosne događaje na upravljanih sustavi za prepoznavanje sumnjivoj aktivnosti.
- [Rješenje Antimalware](log-analytics-malware.md ) izvješća o statusu antimalware zaštite upravljanih sustavima.  
- Rješenje ažuriranja sustava izvodi analizu sigurnosna ažuriranja i druge ažurira upravljanih sustavima tako da se lakše prepoznali sustave koje je obavezna zakrpa.


## <a name="next-steps"></a>Daljnji koraci
- Saznajte više o [zapisnika analize](http://azure.microsoft.com/documentation/services/log-analytics).
- Saznajte više o [Azure automatizaciju](../automation/automation-intro.md).
- Saznajte više o [Azure sigurnosnu kopiju](http://azure.microsoft.com/documentation/services/backup).
- Saznajte više o [oporavak Azure web-mjesta](http://azure.microsoft.com/documentation/services/site-recovery).
