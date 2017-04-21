
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Usporedba niz za povezivanje


U sljedećoj su tablici uspoređuje niza veze koju je potrebno C# programa za povezivanje s lokalnog sustava SQL Server nasuprot baze podataka SQL Azure u oblaku. Razlike su podebljano.


| Niz za povezivanje<br/>Baze podataka Azure SQL | Niz za povezivanje<br/>Microsoft SQL Server |
| :-- | :-- |
| Poslužitelj =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Korisnički ID = {your_loginName_here}**@{your_serverName_here}**;<br/>Lozinka = {your_password_here};<br/>**Baza podataka = {your_databaseName_here};**<br/>**Vremensko ograničenje veze = 30**;<br/>**Šifriranje = True**;<br/>**Certifikatpouzdanogposlužitelja = False**; | Poslužitelj = {your_serverName_here};<br/>Korisnički ID = {your_loginName_here};<br/>Lozinka = {your_password_here}; |


Na **baze podataka =** nije obavezan za SQL Server, ali je potreban za SQL baze podataka.


[.NET ADO SqlConnectionStringBuilder svojstva](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) - govori o sve parametre detalja.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
