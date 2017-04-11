<properties
    pageTitle="Azure državne dokumentaciju | Microsoft Azure"
    description="Ovo omogućuje comparision značajki i upute na razvoj aplikacija za državne ustanove Azure"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure državne baze podataka

##  <a name="sql-database"></a>SQL baze podataka

Pogledajte<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoftov centar za sigurnost za modul baze podataka SQL</a> i [Javne dokumentaciju baze podataka SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/) Dodatne upute o konfiguracija vidljivost metapodataka i najbolje prakse za zaštitu.

### <a name="variations"></a>Varijacije

Baze podataka SQL V12 načelu dostupan je u državne Azure.

Adresa za SQL Server Azure u Azure državne razlikuje se:

Vrsta servisa|Azure javnosti|Azure državne ustanove
---|---|---
SQL baze podataka|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Razmatranja

Sljedeće informacije služi za identifikaciju Azure državne granicu za Azure SQL:

| Regulated/kontrolirati podataka dopušteno | Regulated/kontrolirati podataka nije dopuštena |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Svi podaci spremaju i obuhvaćene sustava Microsoft Azure SQL mogu sadržavati državne Azure regulated podatke. Morate koristiti Alati baze podataka za prijenos podataka državne Azure regulated podataka. | Azure SQL metapodataka nije dopušteno sadrže izvoz upravlja. Ovaj metapodataka uključuje sve podatke o konfiguraciji koji unijeli prilikom stvaranja i održavanja proizvoda za pohranu.  Unesite regulated/kontrolirati podatke u sljedeća polja: naziv, naziv pretplate, grupa resursa, naziv poslužitelja, prijavu administrator poslužitelja, nazivi implementaciju, nazive resursa, oznake resursa za bazu podataka

## <a name="azure-redis-cache"></a>Azure Redis predmemorije

Detalje na taj servis i kako je koristiti potražite u članku [Azure Redis predmemorije javno dokumentacije](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Varijacije

URL-ovi za pristup i upravljanje Azure Redis predmemorije u Azure državne razlikuju:

Vrsta servisa|Azure javnosti|Azure državne ustanove
---|---|---
Predmemorija krajnje točke|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Portal za Azure|https://portal.Azure.com|https://portal.Azure.us

>[AZURE.NOTE] Sve skripte i kod mora računa za odgovarajući krajnje točke i okruženja. Dodatne informacije potražite [u](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)članku povezivanje Azure državne oblakom.


### <a name="considerations"></a>Razmatranja

Sljedeće informacije služi za identifikaciju granicu Azure državne Azure predmemoriju Redis:

| Regulated/kontrolirati podataka dopušteno | Regulated/kontrolirati podataka nije dopuštena |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Svi podaci spremaju i obrađuju u predmemoriji Redis Azure mogu sadržavati državne Azure regulated podatke. | Azure Redis predmemorije metapodataka nije dopušteno sadrže izvoz upravlja. Unesite regulated/kontrolirati podatke u sljedeća polja: predmemoriju ime, naziv VNET, naziv pretplate, grupa resursa, oznake resursa, Redis svojstva.  

##  <a name="next-steps"></a>Daljnji koraci

Za dodatne podatke i ažuriranja pretplatite se na <a href="https://blogs.msdn.microsoft.com/azuregov/">državne Blog o programu Microsoft Azure.</a>
