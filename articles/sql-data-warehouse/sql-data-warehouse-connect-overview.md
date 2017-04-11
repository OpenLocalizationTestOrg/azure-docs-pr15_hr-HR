<properties
   pageTitle="Povezivanje s Azure SQL Data Warehouse | Microsoft Azure"
   description="Kako pronaći poslužitelja naziva i veze niz za vaše za Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Povezivanje s Data Warehouse Azure SQL

Ovaj članak sadrži povezivale SQL Data Warehouse prvi put.

## <a name="find-your-server-name"></a>Pronalaženje naziva poslužitelja

Prvi korak za povezivanje s SQL Data Warehouse je znati kako pronaći naziv poslužitelja.  Na primjer, naziv poslužitelja u sljedećem primjeru je sample.database.windows.net. Da biste pronašli potpuno kvalificiran naziv:

1. Idite na [portal za Azure][].
2. Kliknite na **baze podataka SQL** 
3. Kliknite bazu podataka koju želite povezati.
4. Pronađite naziv cijelog poslužitelja.

    ![Naziv cijelog poslužitelja][1]

## <a name="supported-drivers-and-connection-strings"></a>Podržani upravljačke programe i nizove veze

Azure SQL Data Warehouse podržava [ADO.NET][], [ODBC][], [PHP][]i [JDBC][]. Kliknite jedan od prethodna upravljačkih programa potražite najnoviju verziju i u dokumentaciji o. Automatsko generiranje niz za povezivanje za upravljački program koji koristite na portalu Azure, možete kliknuti **Prikaži nizu za povezivanje baze podataka** u prethodnom primjeru.  Slijede i nekoliko primjera kako niza za povezivanje izgleda za svaki upravljački program.

> [AZURE.NOTE] Preporučujemo da postavite vremensko ograničenje veze na 300 sekundi da biste omogućili preživi kratki razdoblja nedostupnosti vezu.

### <a name="adonet-connection-string-example"></a>Primjer niza veze za ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Primjer niza za povezivanje ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Primjer niz za povezivanje i

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Primjer niza za povezivanje JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Postavke veze

SQL Data Warehouse Standardizira neke postavke tijekom veze i stvaranje objekata. Te postavke nije moguće nadjačati i obuhvaća:

| Postavljanje baze podataka       | Vrijednost                        |
| :--------------------- | :--------------------------- |
| [POSTAVKOM][]         | UKLJUČENO                           |
| [QUOTED_IDENTIFIERS][] | UKLJUČENO                           |
| [DATEFORMAT][]         | MDG                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Daljnji koraci

Povezivanje i upita s Visual Studio potražite u članku [upit s Visual Studio][]. Da biste saznali više o mogućnostima za provjeru autentičnosti, u odjeljku [Provjera autentičnosti za Azure SQL Data Warehouse][].

<!--Articles-->
[Upit s Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Provjera autentičnosti za Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[POSTAVKOM]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Portal za Azure]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


