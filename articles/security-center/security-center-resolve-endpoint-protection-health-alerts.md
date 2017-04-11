<properties
   pageTitle="Razrješavanje krajnjoj točki zaštitu stanja upozorenja u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **upozorenja o stanju riješiti Zaštita na krajnjoj točki**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Razrješavanje krajnjoj točki zaštitu stanja upozorenja u centru za sigurnost Azure

Centar za sigurnost Azure će preporučujemo da riješite otkriven krajnjoj točki zaštitu stanja upozorenja.  Centar za sigurnost omogućuje vam da biste vidjeli virtualnim strojevima (VMs) pogreške Zaštita na krajnjoj točki i koliko pogreške.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer. Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. **Preporuke plohu**odaberite **upozorenja o stanju riješiti Zaštita na krajnjoj točki**.
![Razrješavanje krajnjoj točki zaštitu stanja upozorenja][1]

2. Otvorit će se plohu **Zaštita na krajnjoj točki pogreške** koji navodi VMs pogrešaka i broj neuspjelih za svaki VM. S popisa odaberite na VM.
![Pogreška zaštite krajnje točke][2]

3. Otvorit će se **Popis pogrešaka** plohu za odabrani VM s popisom pogrešaka. Na popisu da biste saznali više, odaberite pogreške. Time se otvara u plohu s informacijama o odabranu neuspjeha.
![Popis pogrešaka][3]
  ![pogreška događaja][4]

## <a name="see-also"></a>Vidi također

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md)– upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md)– Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md)– upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md)– upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md)– traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/)– traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
