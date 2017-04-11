<properties
   pageTitle="Zaštita vaše aplikacije u Centar za sigurnost Azure | Microsoft Azure"
   description="Dokument adrese preporuke u centru za sigurnost Azure koje olakšavaju zaštititi Azure aplikacija, a Ostanite razgovora u skladu s sigurnosna pravila."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-applications-in-azure-security-center"></a>Zaštita vaše aplikacije u Centar za sigurnost Azure

Centar za sigurnost Azure analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke za koje će vas voditi kroz postupak konfiguriranja potrebnih kontrola.  Preporuke primijeniti na vrste Azure resursa: virtualnim strojevima (VMs), povezivanje s mrežom, SQL i aplikacije.

U ovom se članku adrese preporuke koji se odnose na aplikacijama.  Aplikacija centar za preporuke oko implementacije vatrozida aplikaciju za web.  Pomoću tablice ispod kao referenca pomoću kojih se objašnjava preporuke dostupnih aplikacija i što svaki od njih će učiniti ako ga primijeniti.

## <a name="available-application-recommendations"></a>Preporuke dostupnih aplikacija

|Preporuka|Opis|
|-----|-----|
|[Dodavanje aplikacije Vatrozid za web](security-center-add-web-application-firewall.md)|Preporučuje implementacije Vatrozid za aplikaciju web (WAF) za krajnje točke web. Dodavanjem te aplikacije na vaše postojeće WAF implementacijama možete zaštititi više web-aplikacija u centru za sigurnost. WAF aparata (stvorena pomoću modela implementaciju upravljanja resursima) moraju biti implementirano u zasebnom virtualne mreže. WAF aparata (stvorena pomoću modela uvođenje klasičnog) su ograničeni na korištenje mreže sigurnosne grupe. Podrška za ovaj će se proširiti da biste potpuno Prilagođeno implementacije potražite WAF (klasični) u budućnosti.|
|[Dovršavanje zaštite računala](security-center-add-web-application-firewall.md#finalize-application-protection)|Da biste dovršili konfiguraciju u WAF, promet mora biti preusmjereni potražite WAF. Slijedite ove preporuke će dovršiti instalaciju potrebne promjene.|

## <a name="see-also"></a>Vidi također

Da biste saznali više o preporuke koji se odnose na druge vrste Azure resursa, pogledajte sljedeće:

- [Zaštita virtualnim računalima sustava u centru za sigurnost Azure](security-center-virtual-machine-recommendations.md)
- [Zaštita mreže u centru za sigurnost Azure](security-center-network-recommendations.md)
- [Zaštita na servisu Azure SQL u centru za sigurnost Azure](security-center-sql-service-recommendations.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
