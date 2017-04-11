<properties
    pageTitle="SQL baze podataka sa starijim verzijama podržavaju i mijenja IP krajnja točka za ispitivanje | Microsoft Azure"
    description="Saznajte više o podršci za klijente hijerarhijski niži baze podataka SQL-a i IP- a promjene krajnje točke za nadzor."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>Podrška za baze podataka SQL - sa starijim verzijama i IP krajnje promjene za nadzor


[Nadzor](sql-database-auditing-get-started.md) automatski funkcionira s SQL klijenti koje podržavaju TDS preusmjeravanje.


##<a id="subheading-1"></a>Podržava sa starijim verzijama

Bilo kojeg klijenta koji implementira TDS 7,4 moraju također podržava preusmjeravanje. Iznimke to obuhvaćaju JDBC 4.0 u kojem su značajku preusmjeravanje nisu u potpunosti podržane, a Tedious za Node.JS u koje preusmjeravanje nije implementirana.

Za "Starijim verzijama", odnosno koje podršku TDS 7.3 i ispod - poslužiteljem FQDN u vezi niz verzije trebali biste ih mijenjati:

Izvorni poslužitelj FQDN u nizu za povezivanje: <*naziv poslužitelja*>. database.windows.net

Izmijenjeni poslužitelja FQDN u nizu za povezivanje: <*naziv poslužitelja*> .database. **sigurna**. windows.net

Djelomičan popis "Sa starijim verzijama" obuhvaća:

- .NET 4.0 i ispod,
- ODBC 10.0 i ispod.
- JDBC (dok JDBC podržava TDS 7,4, značajka preusmjeravanje TDS nije u potpunosti podržano)
- Zamoran (za Node.JS)

**Napomenu:** Iznad poslužitelj izmjene FDQN može biti korisno i za primjenu pravila nadzor za SQL Server razine bez potrebe za konfiguraciju korak u svaku bazu podataka (Privremene ublažiti).

##<a id="subheading-2"></a>IP krajnjoj točki promjene prilikom omogućivanja nadzora

Napominjemo da kada omogućite nadzor, krajnju točku IP baze podataka će se promijeniti. Ako imate postavke vatrozida za točno, ažurirajte te postavke vatrozida sukladno tome.

Krajnju IP baze podataka ovise o regija baze podataka:

| Područje baze podataka | Mogući IP krajnje točke |
|----------|---------------|
| Sjeverna Kina  | 139.217.29.176, 139.217.28.254 |
| Kina Istok  | 42.159.245.65, 42.159.246.245 |
| Istok Australija  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australija Jugoistok | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Južna Brazil  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Središnje SAD-a  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Istočnoazijski   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Istočni sad 2 | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Istočni SAD-a   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Središnje Indija  | 104.211.98.219, 104.211.103.71 |
| Južna Indija   | 104.211.227.102, 104.211.225.157 |
| Indija Zapad  | 104.211.161.152, 104.211.162.21 |
| Istok Japan   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japan Zapad    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Sjeverna središnje SAD-a  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Sjeverna Europa  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Južna središnje SAD-a  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Jugoistočne Azije  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Europa Zapad  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Zapad SAD-a  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Središnja Kanada  | 13.88.248.106 |
| Istok Kanada  |  40.86.227.82 |
