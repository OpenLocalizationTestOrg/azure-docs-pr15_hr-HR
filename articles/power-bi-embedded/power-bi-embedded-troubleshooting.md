<properties
   pageTitle="Microsoft Power BI ugrađene Preview za otklanjanje poteškoća"
   description="Microsoft Power BI uloženi Preview za otklanjanje poteškoća"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Microsoft Power BI ugrađene Preview za otklanjanje poteškoća
U ovom se članku navedeni Odgovori za otklanjanje poteškoća s **Power BI ugrađeni**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Postavljanje niza veze za SQL Server
Da biste postavili SQL Server niz za povezivanje, morate članaka. U nastavku je primjeru niz veze za SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Da biste saznali više o niza veze za SQL Server, potražite u sljedećim člancima:

-   [SQL Server veze nizova](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Postavljanje vjerodajnica
Velikim slovom koje se vjerodajnice za razvoj ili pripremna okruženju, kao što je korisničko ime i lozinku, možda ćete morati ažuriranje vjerodajnica koje odgovaraju radnog rješenja.

## <a name="see-also"></a>Vidi također
- [Početak rada s uzorak](power-bi-embedded-get-started-sample.md)
- [Što je Power BI ugrađene](power-bi-embedded-what-is-power-bi-embedded.md)
