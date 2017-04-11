<properties
    pageTitle="Azure državne dokumentaciju | Microsoft Azure"
    description="To omogućuje usporedbu značajki i upute na razvoj aplikacija za državne ustanove Azure."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure državne za nadzor i upravljanje njima

U ovom se članku navode za nadzor i upravljanje servisima varijacijama i zahtjevi za Azure državne okruženje.

## <a name="automation"></a>Automatizacija

Automatizacija načelu dostupan je u državne Azure.

### <a name="variations"></a>Varijacije

Sljedeće značajke automatizacije trenutno nisu dostupne u državne Azure.

+ Stvaranje vjerodajnica načelo servis za provjeru autentičnosti

Dodatne informacije potražite u članku [javno dokumentaciju za automatizaciju](../automation/automation-intro.md).

## <a name="log-analytics"></a>Zapisnik Analytics

Prijava analitiku načelu dostupan je u državne Azure.

### <a name="variations"></a>Varijacije

Sljedeće značajke zapisnika analize i rješenja trenutno nisu dostupne u državne Azure.

+ Rješenja koja su u pretpregledu u Microsoft Azure, uključujući:
  - Rješenje praćenja mreže
  - Rješenje ovisnost nadzor aplikacija
  - Rješenja za Office 365
  - Rješenje analize nadogradnje za Windows 10
  - Aplikacija uvida rješenja
  - Rješenja Azure umrežavanje Analytics
  - Rješenja Azure Automatizacija analize
  - Ključ sigurnog analize rješenja
+ Rješenja i značajki koje zahtijevaju ažuriranja lokalnog softver, uključujući:
  - Integracija s programom sustava centra operacije Upravitelj 2016 (podržani starijim verzijama programa Operations Manager)
  - Grupa računala od upravitelja konfiguracije centar sustava
  - Plošni koncentrator rješenja
+ Značajke koje su u pretpregledu u javnom Azure, uključujući:
  - Izvoz podataka za Power BI
+ Azure mjernih podataka i Azure dijagnostiku
+ Operacije paket za upravljanje mobilnim aplikacijama

URL-ovi za analizu zapisnika razlikuju Azure državne:

| Azure javnosti | Azure državne ustanove | Bilješke |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Prijava analitiku portal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Prikupljanje podataka API-JA](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent komunikacije – [Konfiguriranje postavki vatrozida](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent komunikacije – [Konfiguriranje postavki vatrozida](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent komunikacije – [Konfiguriranje postavki vatrozida](../log-analytics/log-analytics-proxy-firewall.md) |


Sljedeće značajke prijava analitiku drugačije ponašaju Azure državne:

+ Agent za Windows mora se preuzeti iz [zapisnika analize portal](https://oms.microsoft.us) za državne ustanove Azure.
+ Da biste povezali poslužitelju za upravljanje sustava centar za komponentu Operations Manager prijava analitiku, morate da biste preuzeli i uvoz ažurirane upravljanje paketi.
  1. Preuzmite i spremanje [ažurira upravljanje paketi](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Raspakiraj datoteku koju ste preuzeli.
  3. Paketi za upravljanje uvesti Operations Manager. Informacije o tome kako uvesti paket za upravljanje s diska, potražite [u](http://technet.microsoft.com/library/hh212691.aspx) članku uvoz paketa za upravljanje Upravitelj operacije na web-mjestu Microsoft TechNet.
  4. Da biste povezali Operations Manager prijava analitiku, slijedite korake u [Povezivanje prijava analitiku u komponente Operations Manager](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Najčešća pitanja

+ Može li se pri migraciji podataka iz zapisnika analize u Microsoft Azure za državne ustanove Azure?
  - ne. Nije moguće da biste premjestili podatke ili radnog prostora iz Microsoft Azure državne Azure.
+ Možete se prebaciti između Microsoft Azure i Azure državne radnih prostora na portalu operacije upravljanja paket zapisnika analize?
  - ne. Portalima za Microsoft Azure i Azure državne odvojene i zajedničko korištenje informacija.

Dodatne informacije potražite u članku [Prijava analitiku javno dokumentacije](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije i ažuriranja, pretplatite se na <a href="https://blogs.msdn.microsoft.com/azuregov/">državne Blog o programu Microsoft Azure.</a>
