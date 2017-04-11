<properties
   pageTitle="Omogućite nadzor baze podataka SQL Azure centar za sigurnost | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **omogućite nadzor baze podataka SQL**."
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

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Omogućite nadzor baze podataka SQL Azure centar za sigurnost

Centar za sigurnost Azure će Preporučujemo uključivanje nadzora za sve baze podataka SQL ako nadzor nije već omogućena. Nadzor olakšava održavanje usklađenosti s propisima, razumijevanje aktivnosti baze podataka i dobiti uvid u nepodudaranja i anomalies koji ukazuje opasnosti tvrtke ili sumnjivu kršenja sigurnost.

Nakon što uključite nadzor možete konfigurirati postavke prijetnju otkrivanja i poruke e-pošte primanje sigurnosnih upozorenja. Otkrivanje prijetnju otkriva aktivnosti anomalous baze podataka koja označava potencijalnih sigurnosnih rizika u bazu podataka. To vam omogućuje da otkrivanje i odgovaranje na potencijalnih prijetnji kako nastaju.

Ove preporuke odnosi se na servis Azure SQL samo; ne obuhvaća SQL koji se izvode na virtualnim računalima.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **Omogući nadzor na baze podataka SQL**.  Otvorit će se **Omogućiti nadzor baze podataka SQL** plohu.
![Omogućite nadzor baze podataka SQL][1]

2. Odaberite SQL baze podataka da biste omogućili nadzor. Otvorit će se plohu **nadzor i prijetnju otkrivanje** .
![Nadzor i prijetnju otkrivanje][2]
3. Na plohu **nadzor i prijetnju otkrivanje** odaberite **Dalje** u odjeljku **nadzor**.
![Uključivanje nadzor i prijetnju otkrivanje][3]


5. Slijedite korake u [Uvod u SQL baze podataka prijetnju otkrivanje](../sql-database/sql-database-threat-detection-get-started.md) za uključivanje i konfiguriranje prijetnju otkrivanje i konfiguriranje na popisu poruka e-pošte koji će primati sigurnosna upozorenja o otkrivanje anomalous aktivnosti.

## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke centar za sigurnost "Omogućite nadzor baze podataka SQL". Dodatne informacije o zaštiti baze podataka sustava SQL potražite u sljedećim člancima:

- [Zaštita baze podataka sustava SQL](../sql-database/sql-database-security.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
