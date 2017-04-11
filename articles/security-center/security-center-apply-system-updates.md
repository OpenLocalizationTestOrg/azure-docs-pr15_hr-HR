<properties
   pageTitle="Primjena ažuriranja sustava u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **primijenili ažuriranja sustava** i **ponovno nakon ažuriranja sustava**."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Primjena ažuriranja sustava u centru za sigurnost Azure

Centar za sigurnost Azure nadzire svakodnevno Windows i Linux virtualnim strojevima (VMs) za operacijski sustav ažuriranja koja nedostaju. Centar za sigurnost dohvaća popis dostupnih sigurnost i kritičnih ažuriranja iz servisa Windows Update ili Windows Server Update Services (WSUS), servis ovisno o tome koji je konfiguriran na Windows VM.  Centar za sigurnost i provjerava ima li najnovija ažuriranja u sustavima Linux. Ako vaš VM nedostaje ažuriranje sustava, centar za sigurnost će preporučujemo da primijenite ažuriranja sustava

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. Odaberite **Primijeni ažuriranja sustava**plohu **preporuke** .
![Primjena ažuriranja sustava][1]

2. Otvorit će se **ažuriranja sustava Primijeni** plohu s popisom VMs nedostaje ažuriranja sustava. Odaberite na VM.
![Odaberite na VM][2]

3. Otvorit će se na plohu prikazom popisa nedostaje ažuriranja za taj VM. Odaberite ažuriranje sustava. U ovom primjeru odaberimo KB3156016.
![Nedostaje sigurnosna ažuriranja][3]

4. Slijedite korake u plohu **Sigurnosno ažuriranje** da biste primijenili nedostaju ažuriranje.
![Sigurnosnog ažuriranja][4]

## <a name="reboot-after-system-updates"></a>Ponovno nakon ažuriranja sustava

5. Vratite se u plohu **preporuke** . Nova stavka generirana nakon primjene ažuriranja sustava naziva **ponovnog pokretanja računala nakon ažuriranja sustava**. Stavka obavještava da morate ponovno pokrenuti VM da biste dovršili postupak primjene ažuriranja sustava.
![Ponovno nakon ažuriranja sustava][5]

6. Odaberite **ponovno nakon ažuriranja sustava**. Otvorit će se **ponovno pokretanje računala čeka se da biste dovršili ažuriranja sustava** plohu s popisom VMs koje ćete morati ponovno pokrenuti da biste dovršili postupak ažuriranja sustava Primijeni.
![Ponovno pokrenite na čekanju][6]

Ponovno pokrenite VM iz Azure da biste dovršili postupak.

## <a name="see-also"></a>Vidi također

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – blog traženje članaka o Azure sigurnost i usklađenost

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
