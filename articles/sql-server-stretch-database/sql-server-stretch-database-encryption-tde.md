<properties
   pageTitle="Omogućivanje šifriranje prozirne podataka (TDE) za SQL Server rastezanje bazu podataka na Azure | Microsoft Azure"
   description="Omogućivanje šifriranje prozirne podataka (TDE) za SQL Server rastezanje bazu podataka na Azure"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Omogućivanje šifriranje prozirne podataka (TDE) za rastezanje baze podataka na Azure
> [AZURE.SELECTOR]
- [Portal za Azure](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Šifriranje prozirne podataka (TDE) pomaže u zaštiti prijetnju zlonamjerni aktivnosti izvođenjem u stvarnom vremenu šifriranje i dešifriranje baze podataka, pridruženi sigurnosne kopije i datoteke zapisnika transakcije na ostale bez promjene u aplikaciju.

TDE šifrira prostora za pohranu cijelu bazu podataka pomoću ključa simetričnu naziva ključa za šifriranje baze podataka. Ključ za šifriranje baze podataka zaštićen certifikata ugrađene poslužitelja. Potvrda ugrađene poslužitelja nije jedinstvena za svaki Azure poslužitelj. Microsoft automatski zakreće ove potvrde barem svaka 90 dana. Opći opis TDE potražite u članku [Prozirne šifriranje podataka (TDE)].

##<a name="enabling-encryption"></a>Omogućivanje šifriranja

Da biste omogućili TDE za programa Azure bazu podataka koja je spremanje podataka migrirati iz baze podataka s omogućenim rastezanje SQL Server, učinite sljedeće:

1. Otvorite bazu podataka na [portal za Azure](https://portal.azure.com)
2. U plohu baze podataka, kliknite gumb **Postavke**
3. Odaberite mogućnost **šifriranje prozirne podataka**![][1]
4. Odaberite postavku **na** , a zatim odaberite **Spremi**
![][2]


##<a name="disabling-encryption"></a>Onemogućivanje šifriranje

Da biste onemogućili TDE za programa Azure bazu podataka koja je spremanje podataka migrirati iz baze podataka s omogućenim rastezanje SQL Server, učinite sljedeće:

1. Otvorite bazu podataka na [portal za Azure](https://portal.azure.com)
2. U plohu baze podataka, kliknite gumb **Postavke**
3. Odaberite mogućnost **šifriranje prozirne podataka**
4. Odaberite postavku **Isključeno** , a zatim odaberite **Spremi**




<!--Anchors-->
[Šifriranje prozirne podataka (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
