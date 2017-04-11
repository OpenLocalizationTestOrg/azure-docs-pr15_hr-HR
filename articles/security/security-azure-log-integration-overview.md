<properties
   pageTitle="Uvod u Microsoft Azure zapisnika Integracija | Microsoft Azure"
   description="Saznajte više o Integracija Azure zapisnika, njegovim ključa mogućnostima i kako to funkcionira."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Uvod u Microsoft Azure zapisnika Integracija (pretpregled)

Saznajte više o Integracija Azure zapisnika, njegovim ključa mogućnostima i kako to funkcionira.

## <a name="overview"></a>Pregled

Platforme kao servis (PaaS) i infrastrukture kao Service (IaaS) smješten u Azure generirati veliku količinu podataka u zapisnicima sigurnost. Ti zapisnici sadrži ključne informacije koje možete unijeti obavještavanje i naprednih uvida u kršenja pravila, interne i vanjske prijetnji, usklađenosti s propisima problema i anomalies u mreži, glavno računalo i aktivnosti korisnika.

Integracija Azure zapisnika omogućuje integraciju neobrađenog zapisnika iz Azure resursa u lokalni informacije o sigurnosti i upravljanje događaja (SIEM) sustavima. Integracija Azure zapisnika prikuplja Azure Dijagnostika iz sustava Windows *(WAD)* virtualnih računala, kao i Dijagnostika iz partnerskih rješenja kao što je Web aplikacije vatrozid (WAF). Ta integracija omogućuje objedinjenih nadzorne ploče za sve svoje imovine, lokalnog ili u oblaku, tako da možete zbrajanja, povezivanje, analizirati i upozorenja za sigurnost događaje.

![Integracija Azure zapisnika][1]

## <a name="what-logs-can-i-integrate"></a>Što se zapisnici možete integrirati?

Azure stvara za svaku uslugu Azure proširenom zapisivanje. Ti zapisnici su kategorizirani u dvije glavne vrste:

- **Kontrola-upravljanje zapisnika**, koji se ističe u operacije Azure resursima stvaranje, ažuriranje i brisanje. Primjer ove vrste zapisnika je Azure zapisnika nadzora.
- **Zapisnike ravnini podataka**, koji se ističe u događaje potenciju kao dio korištenje Azure resursa. Primjeri ovu vrstu zapisnika su Windows događaja sustava, sigurnost i aplikacije zapisnike u virtualnog računala.

Integracija Azure zapisnika trenutno podržava integraciju Azure zapisnike nadzora, zapisnika virtualnog računala i centar za sigurnost Azure upozorenja.

Ako imate pitanja o integracije zapisnik Azure, pošaljite poruku e-pošte da biste [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Daljnji koraci

U ovom dokumentu su predstavljena Integracija Azure zapisnika. Da biste saznali više o Azure zapisnika integracije i vrste zapisnika podržane, pogledajte sljedeće:

- [Microsoft Azure zapisnika integracije Azure zapisnika (pretpregled)](https://www.microsoft.com/download/details.aspx?id=53324) – centar za preuzimanje detalje i sistemski preduvjeti i upute za instalaciju vezana uz integraciju Azure zapisnika.
- [Početak rada s Azure zapisnika Integracija](security-azure-log-integration-get-started.md) - ovog praktičnog vodiča vodit će vas kroz instalaciju integraciju Azure zapisnika i integracija zapisnika Azure prostora za pohranu, zapisnike nadzora Azure i upozorenja u centru za sigurnost.
- [Navedeni koraci za konfiguraciju partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ovaj članak na blogu prikazuje kako konfigurirao integraciju Azure zapisnika za rad s partnerskih rješenja Splunk, HP ArcSight i IBM QRadar.
- [Azure zapisnika Integracija često postavljana pitanja](security-azure-log-integration-faq.md) - ova najčešća pitanja vezana uz navedeni odgovori na pitanja o Integracija Azure zapisnika.
- [Centar za sigurnost Integrating upozorenja s Azure prijave Integracija](../security-center/security-center-integrating-alerts-with-log-integration.md) – ovaj dokument objašnjava da biste sinkronizirali upozorenja centar za sigurnost, zajedno s virtualnog računala sigurnosne događaje prikupljene putem Dijagnostika Azure i Azure nadzornih zapisnika prijava analitiku ili SIEM rješenja.
- [Nove značajke za Azure Dijagnostika i zapisnika nadzora Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – ovaj članak na blogu predstavlja zapisnike nadzora Azure i ostale značajke koje olakšavaju dobiti uvida u operacije Azure resurse.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
