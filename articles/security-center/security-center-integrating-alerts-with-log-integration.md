<properties
   pageTitle="Integriranje upozorenja u centru za sigurnost Azure s Azure zapisnika Integracija (pretpregled) | Microsoft Azure"
   description="Ovaj članak sadrži Uvod u integriranje centar za sigurnost upozorava s Integracija Azure zapisnika."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Integriranje centar za sigurnost upozorava s Azure zapisnika Integracija (pretpregled)

Mnoge postupke za sigurnost i timovima incidenta odgovor oslanjate rješenje informacije o sigurnosti i upravljanje događaja (SIEM) kao početnu točku za triaging i istražuje sigurnosnih upozorenja. Uz integraciju Azure zapisnik sa klijentima možete sinkronizirati centar za sigurnost Azure upozorenja, zajedno s virtualnog računala sigurnosne događaje prikupljene putem Dijagnostika Azure i Azure nadzornih zapisnika s uređajima prijava analitiku ili SIEM rješenja u približno stvarnom vremenu.

Integracija Azure zapisnika radi s HP ArcSight, Splunk, IBM QRadar i drugim korisnicima.

## <a name="what-logs-can-i-integrate"></a>Što se zapisnici možete integrirati?

Azure stvara za svaku uslugu proširenom zapisivanje. Ti zapisnici kategorizirane kao:

- **Kontrola-upravljanje zapisnicima** koje daju uvid u operacije Azure resursima stvaranje, ažuriranje i brisanje.
- **Zapisnici ravnini podataka** koji se ističe u događaje potenciju prilikom korištenja Azure resursa. Primjer je zapisnika događaja sustava Windows – sigurnost i aplikacije zapisnike u virtualnog računala.

Integracija Azure zapisnika trenutno podržava Integracija programa:

- Azure VM zapisnika
- Azure nadzornih zapisnika
- Centar za sigurnost Azure upozorenja

## <a name="install-azure-log-integration"></a>Instalacija Integracija Azure zapisnika

Preuzmite [Azure prijave integracije](https://www.microsoft.com/download/details.aspx?id=53324).

Integracija servisa Azure zapisnika prikuplja telemetrijskih podataka na računalu na kojem je instaliran.  Telemetrijskih podataka prikupljenih je:

- Iznimke koji se pojavljuju tijekom izvođenja Integracija Azure zapisnika
- Metriku o broju upite i događaje obrade
- Statistiku o koje Azlog.exe se koriste mogućnostima naredbenog retka

> [AZURE.NOTE] Možete isključiti skup telemetrijskih podataka poništavanjem tu mogućnost.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integriranje zapisnike nadzora Azure i centar za sigurnost upozorenja

1. Otvorite naredbeni redak i **CD-a** u **c:\Programske datoteke\Microsoft Azure zapisnika integracije**.

2. Pokrenite naredbu **azlog createazureid** da biste stvorili [Azure Active Directory servisa glavnicu](../active-directory/active-directory-application-objects.md) u klijenata za Azure Active Directory (AD) koji hostira Azure pretplate.

    Zatražit će se za prijavu na Azure.

    > [AZURE.NOTE] Morate biti vlasnik pretplate ili Administrator zajednički pretplate.

    Provjera autentičnosti za Azure obavlja putem Azure AD.  Stvaranje glavni servis za integraciju Azure zapisnika će stvoriti Azure AD identitet koji ćete dobiti pristup čitanju iz Azure pretplata.

3. Pokretanje sustava **autorizirali azlog <SubscriptionID> ** naredbu da biste dodijelili pristup Reader u sklopu pretplate glavnicu servisa stvorene u koraku 2. Ako ne navedete **SubscriptionID**, zatim Upravitelj servisa će se dodijeliti ulogu čitač sve pretplate kojima imate pristup.

    > [AZURE.NOTE] Upozorenja možete vidjeti ako pokrenete naredbu **autorizirali** odmah nakon naredbu **createazureid** jer postoji neki latencije između prilikom stvaranja računa Azure AD i kada je račun dostupan za korištenje. Ako Pričekajte otprilike 10 sekundi nakon pokretanja naredbe **createazureid** za izvođenje naredbe **autorizirali** , pa ne vidjet ćete te upozorenja.

4. Provjerite da biste potvrdili da JSON datoteka zapisnika nadzora postoje sljedeće mape:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Provjerite sljedeće mape da biste potvrdili da Centar za sigurnost upozorava postoje u njima:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Pokažite standardni forwarder poveznik za SIEM datoteke u odgovarajuću mapu za pipe podataka na instancu SIEM. Pogledajte [SIEM konfiguracije](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) o vašoj konfiguraciji SIEM.

Ako imate pitanja o integracije zapisnik Azure, pošaljite poruku e-pošte da biste [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o Azure zapisnike nadzora i svojstva definicije, pogledajte:

- [Operacije nadzora s Voditelj resursa](../resource-group-audit.md)
- [Popis upravljanje događaja u pretplatu](https://msdn.microsoft.com/library/azure/dn931934.aspx) - dohvatiti događaje zapisnika nadzora.

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa.
- [Sigurnost Azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.
