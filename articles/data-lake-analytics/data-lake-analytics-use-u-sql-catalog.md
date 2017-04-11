<properties
   pageTitle="Predstavljanje Azure podataka Lake analize U-SQL kataloga | Azure"
   description="Predstavljanje Azure podataka Lake analize U-SQL kataloga"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Korištenje kataloga U SQL

Katalog U SQL koristi se za strukturiranje podataka i kod tako da ih mogu zajednički koristiti tako da U SQL skripte. Katalog omogućuje najveće performanse moguće s podacima u Lake Azure podataka.

Svaki račun za Azure podataka Lake analize ima točno jedan U SQL kataloga pridružen. Nije moguće izbrisati kataloga U SQL. Trenutno U SQL katalozi nije moguće zajednički koristiti između računa spremišta Lake podataka.

Svaki kataloga U SQL sadrži bazu pod nazivom **matrice**. Matrica baza podataka nije moguće izbrisati.  Svaki kataloga U SQL mogu sadržavati više dodatnih baza podataka.

U SQL baze podataka sadrži:

- Skupovi – zajedničko korištenje .NET kod među U SQL skripte.
- Funkcije tablice vrijednosti – zajedničko korištenje U SQL koda među U SQL skripte.
- Tablica – zajedničko korištenje podataka među U SQL skripte.
- Sheme – zajedničko korištenje tablica sheme među U SQL skripte.

## <a name="manage-catalogs"></a>Upravljanje kataloga
Svaki račun za Azure podataka Lake analize ima zadani Lake spremišta podataka za Azure račun pridružen. Taj račun spremišta podataka Lake naziva kao zadanog računa za spremište Lake podataka. U SQL kataloga se pohranjuju u zadani račun spremišta Lake podataka u mapi /catalog. Izbrišite sve datoteke u mapi /catalog.

### <a name="use-azure-portal"></a>Pomoću portala za Azure

Potražite u članku [Upravljanje analize Lake podataka pomoću portala](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Pomoću alata za Lake podataka za Visual Studio.

Data Lake Tools za Visual Studio možete koristiti da biste upravljali kataloga.  Dodatne informacije o alatima potražite u članku [Korištenje Alati podataka Lake za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Da biste upravljali kataloga**

1. Otvorite Visual Studio i povezati azure. Upute potražite u članku [Povezivanje s Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Otvorite **Eksplorer za poslužitelj** , pritisnite **CTRL + ALT + S**.
2. Iz programa **Explorer poslužitelja**proširite **Azure**, proširite **Analize podataka Lake**, proširite računa analize podataka Lake, proširite **baze podataka**, a proširite **matrice**.



    - Da biste dodali novu bazu podataka, desnom tipkom miša kliknite **bazu podataka**, a zatim **Stvoriti bazu podataka**.
    - Da biste dodali novi skup, desnom tipkom miša kliknite **skupovi**, a zatim **Registrirati skupa**.
    - Da biste dodali novu shemu, desnom tipkom miša kliknite **sheme**, a zatim kliknite "Stvori shemu **.
    - Da biste dodali novu tablicu, desnom tipkom miša kliknite **tablica**, a zatim kliknite "" Stvaranje tablice **.
    - Da biste dodali nove funkcije tablice s vrijednostima, potražite u članku [U razvoju-SQL korisnički definirane operatori za analize podataka Lake zadatke](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Pregled kataloga U SQL Visual Studio](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Vidi također

- Početak rada
    - [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
    - [Početak rada s podacima Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Početak rada s podacima Lake analize pomoću Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md)

- U SQL i razvoj
    - [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md)
    - [Koristite funkcije prozora U SQL Azure podataka Lake analize posla](data-lake-analytics-use-window-functions.md)
    - [Razvoj U SQL korisnički definirane operatori za analize podataka Lake poslove](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Upravljanje
    - [Upravljanje analizom Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-manage-use-portal.md)
    - [Upravljanje Azure podataka Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Praćenje i rješavanje problema s poslove Lake analize podataka za Azure pomoću portala za Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Praktični vodič završetka do kraja
    - [Interaktivni vodiči za analize korištenja Azure podataka Lake](data-lake-analytics-use-interactive-tutorials.md)
    - [Analiza zapisnika web-mjesta pomoću Lake analize podataka za Azure](data-lake-analytics-analyze-weblogs.md)
