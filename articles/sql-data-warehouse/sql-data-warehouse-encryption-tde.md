<properties
   pageTitle="Šifriranje prozirne podataka u SQL Data Warehouse (Portal) | Microsoft Azure"
   description="Šifriranje prozirne podataka (TDE) u skladištu podataka za SQL"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Početak rada s prozirnim podataka šifriranja (TDE) u skladištu podataka za SQL

> [AZURE.SELECTOR]
- [Pregled sigurnosti](sql-data-warehouse-overview-manage-security.md)
- [Provjera autentičnosti](sql-data-warehouse-authentication.md)
- [Šifriranje (Portal)](sql-data-warehouse-encryption-tde.md)
- [Šifriranje (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Potrebne ovlasti

Da biste omogućili prozirne šifriranje podataka (TDE), morate biti administrator ili član uloge dbmanager.

## <a name="enabling-encryption"></a>Omogućivanje šifriranja

Da biste omogućili TDE za SQL Data Warehouse, slijedite ove korake:

1. Otvorite bazu podataka na [portal za Azure](https://portal.azure.com)
2. U plohu baze podataka, kliknite gumb **Postavke**
3. Odaberite mogućnost **šifriranje prozirne podataka**![][1]
4. Odaberite postavku **na**![][2]
5. Odaberite **Spremi**
![][3]  

## <a name="disabling-encryption"></a>Onemogućivanje šifriranje

Da biste onemogućili TDE za SQL Data Warehouse, slijedite korake u nastavku:

1. Otvorite bazu podataka na [portal za Azure](https://portal.azure.com)
2. U plohu baze podataka, kliknite gumb **Postavke**
3. Odaberite mogućnost **šifriranje prozirne podataka**![][1]
4. Odaberite postavku **Isključeno**![][4]
5. Odaberite **Spremi**
![][5]  

## <a name="encryption-dmvs"></a>Šifriranje DMVs

Šifriranje možete potvrditi sa sljedećim DMVs:

- [sys.Databases]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
