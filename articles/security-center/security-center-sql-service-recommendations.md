<properties
   pageTitle="Zaštita servisa Azure SQL u centru za sigurnost Azure | Microsoft Azure"
   description="Dokument adrese preporuke u centru za sigurnost Azure koje olakšavaju zaštita servisa Azure SQL i ostati razgovora u skladu s sigurnosna pravila."
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

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Zaštita servisa Azure SQL u centru za sigurnost Azure

Centar za sigurnost Azure analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke za koje će vas voditi kroz postupak konfiguriranja potrebnih kontrola.  Preporuke primijeniti na vrste Azure resursa: virtualnim strojevima (VMs), povezivanje s mrežom, SQL i aplikacije.

U ovom se članku adrese preporuke koji se odnose na servis Azure SQL.  Azure SQL preporuke centar za servisnu oko omogućivanja nadzor Azure SQL poslužitelja i baze podataka i omogućivanje šifriranja za baze podataka SQL.  Pomoću tablice ispod kao referenca pomoću kojih se objašnjava dostupan servis za preporuke za SQL i što svaki od njih će učiniti ako ga primijeniti.

## <a name="available-sql-service-recommendations"></a>Dostupne preporuke servisa SQL

|Preporuka|Opis|
|-----|-----|
|[Omogućivanje poslužitelja SQL nadzora](security-center-enable-auditing-on-sql-servers.md)|Preporučuje uključivanje nadzora za Azure SQL poslužitelja (servisa Azure SQL samo ne sadrži SQL koji se izvode na virtualnim strojevima).|
|[Omogućivanje baze podataka SQL nadzora](security-center-enable-auditing-on-sql-databases.md)|Preporučuje uključivanje nadzora za baze podataka Azure SQL (servisa Azure SQL samo ne sadrži SQL koji se izvode na virtualnim strojevima).|
|[Omogućivanje prozirne šifriranje podataka na baze podataka SQL](security-center-enable-transparent-data-encryption.md)|Preporučuje se da omogućite šifriranje za baze podataka SQL (samo za Azure SQL servis).|

## <a name="see-also"></a>Vidi također

Da biste saznali više o preporuke koji se odnose na druge vrste Azure resursa, pogledajte sljedeće:

- [Zaštita virtualnim računalima sustava u centru za sigurnost Azure](security-center-virtual-machine-recommendations.md)
- [Zaštita vaše aplikacije u Centar za sigurnost Azure](security-center-application-recommendations.md)
- [Zaštita mreže u centru za sigurnost Azure](security-center-network-recommendations.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
