<properties
   pageTitle="Omogućivanje šifriranje prozirne podataka u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Omogućiti prozirne šifriranje podataka**."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Omogućivanje šifriranje prozirne podataka u centru za sigurnost Azure

Centar za sigurnost Azure će preporučujemo da omogućite prozirne šifriranje podataka (TDE) na baze podataka SQL ako TDE već nije omogućen. TDE štiti podatke i pomaže vam zadovoljava preduvjete za usklađenost šifriranjem svoje baze podataka, pridruženi sigurnosne kopije i datoteke zapisnika transakcije na ostale, bez promjena u aplikaciji. Dodatne informacije potražite u članku [Prozirne šifriranje podataka s bazom podataka SQL Azure](https://msdn.microsoft.com/library/dn948096).

Ove preporuke odnosi se na servis Azure SQL samo; ne obuhvaća SQL koji se izvode na virtualnim računalima.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **Omogući prozirne šifriranje podataka**.
![Omogućivanje šifriranje prozirne podataka][1]

2. Otvorit će se **Omogućiti prozirne šifriranje podataka na baze podataka SQL** plohu. Odaberite SQL baze podataka da biste omogućili TDE.
![Odaberite SQL DB da biste omogućili TDE][2]
3. Na plohu **šifriranje prozirne podataka** odaberite **Dalje** u odjeljku šifriranje podataka i odaberite **Spremi** gornji na vrpci na plohu.
![Uključivanje TDE][3]

  Kada TDE omogućena na odabranu SQL bazu podataka, **Stanje šifriranja** će se promijeniti **šifrirana**.    

  ![Stanje šifriranja][4]

## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke centar za sigurnost "Omogući prozirne šifriranje podataka." Da biste saznali više o SQL TDE, pogledajte sljedeće:

- [Šifriranje prozirne podataka s bazom podataka Azure SQL](https://msdn.microsoft.com/library/dn948096)
- [Početak rada s prozirnim podataka šifriranja (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
