<properties
   pageTitle="Omogućivanje VM Agent u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Omogućiti VM Agent**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Omogućivanje VM Agent u centru za sigurnost Azure

Agent za VM mora biti instaliran na virtualnim strojevima (VMs) u redoslijedu omogućuje [Prikupljanje podataka](security-center-enable-data-collection.md).  Centar za sigurnost Azure omogućuje vam da biste vidjeli koje VMs zahtijevaju VM Agent i će preporučujemo da omogućite Agent VM na te VMs.

Agent za VM se instalira prema zadanim postavkama za VMs koji su raspoređeni iz trgovine Azure Marketplace. U članku [VM Agent i proširenja – dio 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) pruža informacije o instalaciji VM Agent.


> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer. Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. **Preporuke plohu**odaberite **Omogući VM Agent**.
![Omogućivanje VM Agent][1]

2. Otvorit će se plohu **VM Agent nedostaju ili ne reagira**. U ovom plohu popis VMs koje je potrebno VM Agent. Slijedite upute na plohu da biste instalirali VM agent.
![Agent za VM nedostaje][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
