<properties
   pageTitle="Ažuriranje OS verzija u centru za sigurnost Azure | Microsoft Azure"
   description="U ovom se članku objašnjava implementirati preporuke centar za sigurnost Azure **verzija OS -a ažuriranja**."
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

# <a name="update-os-version-in-azure-security-center"></a>Ažuriranje verzija OS-a u centru za sigurnost Azure

Za virtualnim strojevima (VMs) na servise u oblaku, centar za sigurnost Azure će vam se ažurirati na operacijskim sustavima (OS) ako je dostupno noviju verziju.  Samo oblak services uloge web i tempiranja u radnog slobodnih provjeravaju.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **verzija OS -a ažuriranja**.
![Ažuriranje verzija OS-a][1]

2. Otvorit će se plohu **verzija OS -a ažuriranja**. Slijedite korake u ovom plohu da biste ažurirali verzija OS-a.

## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke centar za sigurnost "Ažuriranje OS verzija". Da biste saznali više o servise u oblaku i ažuriranje verzija OS-a za servise u oblaku, pogledajte:

- [Pregled servisa u oblaku](../cloud-services/cloud-services-choose-me.md)
- [Kako ažurirati servis u oblaku](../cloud-services/cloud-services-update-azure-service.md)
- [Konfiguriranje servisa u Oblaku](../cloud-services/cloud-services-how-to-configure-portal.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
