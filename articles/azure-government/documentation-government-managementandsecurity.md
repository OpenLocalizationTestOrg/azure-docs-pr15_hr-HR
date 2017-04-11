<properties
    pageTitle="Azure državne dokumentaciju | Microsoft Azure"
    description="Ovo omogućuje comparision značajki i upute na razvoj aplikacija za državne ustanove Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Upravljanje Azure državne i sigurnost

## <a name="automation"></a>Automatizacija

Automatizacija načelu dostupan je u državne Azure.

### <a name="variations"></a>Varijacije

Sljedeće značajke automatizacije trenutno nisu dostupne u državne Azure.

+ Stvaranje vjerodajnica načelo servis za provjeru autentičnosti

Dodatne informacije potražite u članku [javno dokumentaciju za automatizaciju](../automation/automation-intro.md).


##  <a name="key-vault"></a>Ključni zbirke ključeva
Detalje na taj servis i kako je koristiti potražite u članku na <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure ključ sigurnog javno dokumentacije.</a>
### <a name="data-considerations"></a>Razmatranja podataka
Sljedeće informacije služi za identifikaciju Azure državne granicu za Azure ključ sigurnog:

| Regulated/kontrolirati podataka dopušteno | Regulated/kontrolirati podataka nije dopuštena |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Svi podaci šifriran pomoću ključa Azure ključ sigurnog može sadržavati Regulated/kontrolirati podataka. | Azure ključ sigurnog metapodataka nije dopušteno sadrže izvoz upravlja. Ovaj metapodataka uključuje sve podatke o konfiguraciji koji unijeli prilikom stvaranja i održavanja zbirke ključeva vaš ključ.  Unesite Regulated/kontrolirati podatke u sljedeća polja: nazive grupa resursa, ključ sigurnog imena, a zatim naziv pretplate |

Ključ sigurnog načelu dostupan je u državne Azure. Kao javno, postoji bez nastavka tako sigurnog ključ je dostupan putem komponente PowerShell i EŽA samo.
## <a name="log-analytics"></a>Zapisnik Analytics
Prijava analitiku načelu dostupan je u državne Azure. 

### <a name="variations"></a>Varijacije

Sljedeće značajke zapisnika analize i rješenja trenutno nisu dostupne u državne Azure. Ovaj popis ažurira kada status značajke / mijenja rješenja.

+ Rješenja koja su u pretpregledu u javnom Azure uključujući:
  - Rješenje praćenja mreže
  - Nadzor ovisnost aplikacije
  - Rješenja za Office 365
  - Rješenje analize nadogradnje za Windows 10
  - Aplikacija uvida
  - Azure s mrežom Analitikom rješenja
  - Azure Automatizacija analize
  - Ključni sigurnog Analytics
+ Rješenja i značajki koje zahtijevaju ažuriranja lokalnog softver, uključujući
  - Integracija s programom sustava centra operacije Upravitelj 2016 (podržani starijim verzijama programa Operations Manager)
  - Grupa računala iz sustava centrirali Upravitelj konfiguracije
  - Plošni koncentrator rješenja
+ Značajke koje su u pretpregledu u javnom Azure, uključujući
  - Izvoz podataka radi PowerBI
+ Azure mjernih podataka i Azure dijagnostiku
+ OMS mobilne aplikacije

URL-ovi za analizu zapisnika razlikuju Azure državne:

| Azure javnosti | Azure državne ustanove | Bilješke |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Prijava analitiku portal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Prikupljanje podataka API-JA](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent komunikacije – [Konfiguriranje postavki vatrozida](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent komunikacije – [Konfiguriranje postavki vatrozida](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent komunikacije – [Konfiguriranje postavki vatrozida](../log-analytics/log-analytics-proxy-firewall.md) |


Sljedeće značajke prijava analitiku imaju različite ponašanje u Azure državne:

+ Agent za Windows mora se preuzeti iz [zapisnika analize portal](https://oms.microsoft.us) za državne ustanove Azure.
+ Da biste povezali poslužitelju za upravljanje sustava centar za komponentu Operations Manager prijava analitiku, ćete morati preuzeti i uvoz ažurirane paketi za upravljanje.
  1. Preuzimanje i spremanje [ažurirati paketi za upravljanje](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Raspakiraj preuzete datoteke
  3. Paketi za upravljanje uvesti Operations Manager. Informacije o tome kako uvesti paket za upravljanje s diska, potražite u temi [Kako uvesti paketa za upravljanje Upravitelj operacije](http://technet.microsoft.com/library/hh212691.aspx) na web-mjestu Microsoft TechNet.
  4. Da biste povezali Operations Manager prijava analitiku, slijedite korake u [Povezivanje prijava analitiku u komponente Operations Manager](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Najčešća pitanja

+ Možete li se pri migraciji podataka iz zapisnika analize u javnim državne Azure za Azure?
  - ne. Nije moguće premještanje podataka ili radni prostor s javnim državne Azure za Azure.
+ Možete se prebaciti između javno radne prostore Azure i državne Azure s portala sustava OMS prijava analitiku?
  - ne. Portalima za javne Azure i Azure državne odvojene i zajedničko korištenje informacija. 

Dodatne informacije potražite u članku [Prijava analitiku javno dokumentacije](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije i ažuriranja, pretplatite se na <a href="https://blogs.msdn.microsoft.com/azuregov/">državne Blog o programu Microsoft Azure.</a>
 
