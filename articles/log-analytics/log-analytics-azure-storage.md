<properties
    pageTitle="Prikupljanje podataka Azure prostora za pohranu u pregled prijava analitiku | Microsoft Azure"
    description="Azure resursa možete napisati zapisnika i metrike s računom za Azure prostora za pohranu često korištenjem dijagnostike Azure. Prijava analitiku možete indeksirati podatke i provjerite mogu pretraživati."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Prikupljanje podataka Azure prostora za pohranu u Pregled zapisnika Analytics

Mnoge Azure resurse će moći pisati zapisnika i metrike račun za Azure prostora za pohranu. Prijava analitiku možete zauzeti te podatke i olakšavaju praćenje Azure resurse.

Da biste napisali Azure prostora za pohranu resursa možda koristite Azure Dijagnostika ili ste instalirali svoj način zapisivanje podataka. Ti podaci mogu biti napisani u različitim formatima na neki od sljedećih mjesta:

+ Tablica platforme Azure
+ Blobova platforme Azure
+ EventHub

Prijava analitiku podržava Azure servise koje zapisivanje podataka pomoću [Azure dijagnostičke zapisnike](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Osim toga, prijava analitiku podržava ostale servise koje poslati zapisnike i metriku u različitim oblicima i lokacije.  

>[AZURE.NOTE] Vam se naplatiti Azure podatke tečajeve za pohranu i transakcije kada pošaljete Dijagnostika s računom za pohranu i kada prijava analitiku čita podataka s računa za pohranu.

![Dijagram Azure prostora za pohranu](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Podržani Azure resursi

Prijava analitiku možete prikupiti podatke u sljedećim resursima Azure:

| Vrsta resursa | Zapisnici (dijagnostičkih kategorije) | Zapisnik analize rješenja |
| --------------------------------------- | -------------------------------- | --------------- |
| Aplikacija uvida | Dostupnost <br> Prilagođene događaje <br> Iznimke <br> Zahtjevi za <br> | Uvid računala (pretpregled) |
| Upravljanje API-JA | | *ništa* (Pretpregled) |
| Automatizacija <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (pretpregled) |
| Ključni zbirke ključeva <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (pretpregled) |
| Pristupnik za aplikaciju <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (pretpregled) |
| Mrežni sigurnosne grupe <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (pretpregled) |
| Tkanina servisa                          | ETWEvent <br> Radu događaja <br> Pouzdan glumca događaja <br> Pouzdan usluga| ServiceFabric (pretpregled) |
| Virtualnim strojevima | Linux Syslog <br> Događaja sustava Windows <br> IIS zapisnik <br> Windows ETWEvent | *Ništa* |
| Uloge za web <br> Uloga suradnika | Linux Syslog <br> Događaja sustava Windows <br> IIS zapisnik <br> Windows ETWEvent | *Ništa* |

>[AZURE.NOTE] Za nadzor Azure virtualnim strojevima (Linux i Windows), preporučujemo da instalirate [Prijava analitiku VM nastavak](log-analytics-azure-vm-extension.md). Agenta nudi dublju uvida na virtualnim strojevima nego ako koristite Dijagnostika sastavljene za pohranu.

Pridonesite prioritet dodatne zapisnicima OMS da biste analizirali prema glasovanje na našem [preusmjereni na stranicu](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy).


- Pogledajte [analizirati Azure dijagnostičke zapisnike pomoću zapisnika analize](log-analytics-azure-storage-json.md) dodatne informacije o način prijava analitiku čitanja zapisnike Azure servisa koji podržavaju [Azure dijagnostičkih zapisnika](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md):
  - Azure sigurnog ključa
  - Automatizacija Azure
  - Pristupnik za aplikaciju
  - Mrežni sigurnosne grupe
- Potražite u članku [Korištenje spremište blobova platforme za IIS i spremište tablica za događaje](log-analytics-azure-storage-iis-table.md) da biste saznali više o kako prijava analitiku mogu čitati zapisnike za Azure services taj unos Dijagnostika spremište tablica ili IIS zapisnici mogli unijeti bloba prostor za pohranu, uključujući:
  - Tkanina servisa
  - Uloge za web
  - Uloga suradnika
  - Virtualnim strojevima


Aplikacija uvida se privatni pretpregled i koristi neprekinuti izvoz da biste bloba prostora za pohranu. Da biste pristupili privatne pretpregled, obratite se službi za Microsoftov Account ili se odnositi na informacije o [web-mjesta za povratne informacije](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Daljnji koraci

- [Analiza Azure dijagnostičke zapisnike pomoću zapisnika Analytics](log-analytics-azure-storage-json.md) za čitanje zapisnike Azure services taj unos Dijagnostika sa spremištem blobova u JSON OSNOVNI oblik.
- [Korištenje spremište blobova platforme za IIS i spremište tablica za događaje](log-analytics-azure-storage-iis-table.md) da biste pročitali zapisnike za Azure services taj unos Dijagnostika spremište tablica ili IIS zapisnici mogli unijeti bloba prostora za pohranu.
- [Omogućivanje rješenja](log-analytics-add-solutions.md) za davanje uvid u podatke.
- [Korištenje upita za pretraživanje](log-analytics-log-searches.md) da biste analizirali podatke.
