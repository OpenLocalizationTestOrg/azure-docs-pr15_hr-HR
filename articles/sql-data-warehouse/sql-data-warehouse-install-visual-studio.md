<properties
   pageTitle="Instalirajte Visual Studio i SSDT za SQL Data Warehouse | Microsoft Azure"
   description="Instalirajte Visual Studio i SQL Server razvoj Tools (SSDT) za Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Instalirajte Visual Studio 2015 i SSDT za SQL Data Warehouse

Za razvoj aplikacija za SQL Data Warehouse, preporučujemo korištenje Visual Studio 2015 s najnovija verzija sustava SQL Server podataka Alati (SSDT).  Visual Studio 2013 Update 5 sa SSDT podržano je i kompatibilnost sa starijim verzijama.  

Pomoću Visual Studio SSDT će vam omogućiti da biste koristili Explorer objekta SQL Server da biste vizualno Istraživanje tablice, prikaza, pohranjene procedure i mnogo više objekata u SQL Data Warehouse kao i pokretanje upita.

> [AZURE.NOTE] SQL Data Warehouse ne podržava još Visual Studio baze podataka projekti.  Ta značajka dodat će se u budućim verzijama.

## <a name="step-1-install-visual-studio-2015"></a>Korak 1: Instalacija Visual Studio 2015.

Slijedite ove veze da biste preuzeli i instalirali Visual Studio 2015. Ako već imate Visual Studio 2013 ili 2015 instaliran, možete prijeđite na drugi korak 2, instalirajte SSDT.

1. [Preuzimanje Visual Studio 2015][].
2. Slijedite vodič za [Instaliranje Visual Studio][] na MSDN-u, a zatim odaberite zadane konfiguracije.

## <a name="step-2-install-ssdt"></a>Korak 2: Instalacija SSDT

Da biste instalirali SSDT za Visual Studio jednostavno potražiti SSDT ažuriranje iz programa Visual Studio slijedeći ove korake.

1. Kliknite **Alati**u Visual Studio / **proširenja i ažuriranja...**  /  **Ažuriranja**
2. Odaberite **Ažuriranja proizvoda** , a zatim potražite **tooling Microsoft SQL Server Update za bazu podataka**

Ako se ne pronađe ažuriranje, trebali biste imati instalirali najnoviju verziju.  Da biste potvrdili SSDT je instaliran, kliknite **Pomoć** / **O Microsoft Visual Studio** i izgleda za SQL Server Data Tools na popisu.  Najnoviju verziju SSDT je 14.0.60525.0.  Ako mogućnost instalacije nije dostupna u Visual Studio, umjesto toga možete posjetiti u SSDT stranice za [Preuzimanje][] da biste preuzeli i instalirali SSDT ručno.

## <a name="next-steps"></a>Daljnji koraci

Sad kad imate najnoviju verziju SSDT, spremni ste za [Povezivanje][] za SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[povezivanje]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Preuzimanje Visual Studio 2015.]: https://www.visualstudio.com/downloads/
[Instaliranje Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[Preuzimanje SSDT]: https://msdn.microsoft.com/library/mt204009.aspx
