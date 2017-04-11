<properties
   pageTitle="Upravljanje partnerskih rješenja u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument vodit će vas kroz kako centar za sigurnost Azure omogućuje monitora na prvi pogled status stanja partnerskih rješenja integriran s pretplatom Azure."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Nadzor partnerskih rješenja Azure centar za sigurnost

Ovaj dokument vodit će vas kroz statusa stanja partnerskih rješenja u centru za sigurnost Azure.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer. Ovo nije Postupni vodič.

## <a name="monitoring-partner-solutions"></a>Nadzor partnerskih rješenja

Pločicu **partnerskih rješenja** na **Centar za sigurnost** plohu omogućuje monitora na prvi pogled status stanja partnerskih rješenja integriran s pretplatom Azure.
![Pločica partnerskih rješenja][1]

Pločica **partnerskih rješenja** prikazuje broj partnerskih rješenja i status sažetka za tih rješenja.

**STATUS** rješenja partnera mogu biti:

- Zaštićeni (zelena) – postoji problem ne stanja.
- Dobro (crveni) – postoji problem stanje hitnom.
- Zaustavljen (Narančasto) za izvješćivanje o pogreškama - rješenje prestao njegov stanja za izvješćivanje o pogreškama.
- Nepoznato zaštitu status (Narančasto) – stanje rješenje nije poznat trenutno zbog nije uspio postupak dodavanja novog resursa postojeće rješenje.
- Nije prijavljen (sive) - rješenje je prijavio ništa još rješenja u status možda će biti neprijavljenih da ste povezali te i dalje implementacija.

Ako nema rješenja integriran s pretplatom na pločici će navedite da postoje nema rješenja. Odaberite pločicu **partnerskih rješenja** će vam omogućiti da biste otvorili plohu **preporuke** za implementaciju partnerskih rješenja za sigurnost.
![Nema partnerskih rješenja][2]

Da biste pogledali stanja partnerskih rješenja:

1. Odaberite pločicu **partnerskih rješenja** . Otvorit će se na plohu s popisom sustava partnerskih rješenja povezan s centar za sigurnost.
![Partnerskih rješenja][3]

2. Odabir rješenja partnera. U ovom primjeru omogućuje odabir **F5 WAF2** rješenja.  Otvorit će se na plohu prikazuje status rješenja partnera i rješenja povezani resursi. Odaberite **konzolu rješenja** da biste otvorili sučelje za upravljanje partnera za rješenja.
![Detalji o rješenja partnera][4]

3. Vratite se u plohu **F5 WAF2** i odaberite **aplikaciju za vezu**. Otvorit će se plohu **Aplikacije vezu** . Ovdje možete se povezati resursi rješenja partnera.
![Veza resursa za rješenja partnera][5]

## <a name="see-also"></a>Vidi također
U ovom dokumentu su predstavljena pločicu **Partnerskih rješenja** u centru za sigurnost. Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa.
- [Sigurnost Azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
