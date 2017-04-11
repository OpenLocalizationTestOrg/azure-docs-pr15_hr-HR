<properties
   pageTitle="Instalirajte Zaštita na krajnjoj točki u Centar za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Instalirati Zaštita na krajnjoj točki**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Instalirajte Zaštita na krajnjoj točki u Centar za sigurnost Azure

Centar za sigurnost Azure će preporučujemo da vam Dodjela programa antimalware za Azure virtualnim strojevima (VMs) ako antimalware već nije omogućen. U ovom preporuke primjenjuje se samo Windows VMs.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **Instalacija Zaštita na krajnjoj točki**.
![Odaberite Zaštita na krajnjoj točki Instaliraj][1]

2. Otvorit će se **Instalirati Zaštita na krajnjoj točki** plohu prikazom popisa VMs bez antimalware omogućena. S popisa odaberite VMs koji želite instalirati antimalware na, a zatim kliknite **Instaliraj na VMs**.
![Odaberite VMs da biste instalirali antimalware na][2]

3. Otvorit će se plohu **Odaberite Zaštita na krajnjoj točki** omogućuje vam da biste odabrali rješenje antimalware koji želite koristiti. U ovom primjeru odaberimo **Microsoft Antimalware**.
![Odaberite Zaštita na krajnjoj točki][3]

4. Dodatne informacije o rješenje antimalware prikazuju se. Odaberite **Stvori**.
![Stvaranje rješenja antimalware][4]

5. Unesite potrebne konfiguracijske postavke na plohu **Proširenje dodavanje** , a zatim odaberite **u redu**. Dodatne informacije o postavkama konfiguracije potražite u članku [zadane i prilagođene Antimalware konfiguracije](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) je sada aktivan na odabrani VMs.

## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke centar za sigurnost "Instaliraj Zaštita na krajnjoj točki." Dodatne informacije o omogućivanju programa antimalware u Azure potražite u sljedećim člancima:

- [Microsoft Antimalware za servise u Oblaku i virtualnim strojevima](../azure-security-antimalware.md) – upute za implementaciju Microsoft antimalware.

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – Saznajte kako konfigurirati sigurnosna pravila.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – blog traženje članaka o Azure sigurnost i usklađenost

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
