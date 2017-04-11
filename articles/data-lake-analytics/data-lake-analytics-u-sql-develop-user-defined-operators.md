<properties 
   pageTitle="U SQL korisnički definirane operatori za Azure podataka Lake analize zadataka za razvoj | Azure" 
   description="Saznajte kako razvoj korisnički definirane operatora će koristiti te ponovno koristiti u analize podataka Lake zadatke. " 
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


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Razvoj U SQL korisnički definirane operatori za Azure podataka Lake analize poslove

Saznajte kako razvoj korisnički definirane operatora će koristiti te ponovno koristiti u analize podataka Lake zadatke. Morate razviti prilagođene operator za pretvaranje države imena.

##<a name="prerequisites"></a>Preduvjeti

- Visual Studio 2015, Visual Studio 2013 ažurirati 4 ili Visual Studio 2012 Visual C++ instaliran 
- Microsoft Azure SDK za .NET verzije 2.5 ili noviji.  Instalirajte ga pomoću platforme installer Web.
- Analize podataka Lake račun.  Potražite u članku [Početak rada s analize Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-get-started-portal.md).
- Idite do Praktični vodič za [Početak rada s Azure podataka Lake analize U SQL Studio](data-lake-analytics-u-sql-get-started.md) .
- Povezivanje s Azure, pročitajte članak [Početak rada s Azure podataka Lake analize U SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Prijenos izvorišnih podataka potražite u članku [Početak rada s Azure podataka Lake analize U SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definiranje i korištenje operatora korisnički definirano u U SQL

**Stvaranje i slanje U SQL posla** 

1. Na izborniku **datoteka** kliknite **Novo**, a zatim **projekta**.
2. Odaberite vrstu **Projekta U SQL** .

    ![novi projekt U SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Kliknite **u redu**. Visual studio stvara rješenje pomoću Script.usql datoteke.
4. U **Pregledniku rješenja**, proširite Script.usql, a zatim dvokliknite **Script.usql.cs**.
5. Zalijepite sljedeći kod u datoteku:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Otvorite Script.usql i zalijepite sljedeću skriptu U SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. U **Pregledniku rješenja**, desnom tipkom miša kliknite **Script.usql**, a zatim **Stvoriti skriptu**.
6. U **Pregledniku rješenja**, desnom tipkom miša kliknite **Script.usql**, a zatim **Pošalji skripte**.
7. Ako još niste povezati Azure pretplatu, bit će zatraži da unesete vjerodajnice račun za Azure.
7. Kliknite **Pošalji**. Slanje rezultata i veze za posao dostupni su u prozoru Rezultati po dovršetku slanja.
8. Mora kliknuti gumb Osvježi da biste vidjeli najnovije stanja zadatka i osvježavanje zaslona.

**Da biste vidjeli izlaz posla**

1. Iz programa **Explorer poslužitelja**proširite **Azure**, proširite **Analize podataka Lake**, proširite računa analize podataka Lake, proširite **Račune za pohranu**, desnom tipkom miša kliknite pohranu zadani, a zatim **Explorer**. 
2. Proširite uzorka, proširite izlaze, a zatim dvokliknite **Drivers.csv**.


##<a name="see-also"></a>Vidi također

- [Početak rada s podacima Lake analize pomoću komponente PowerShell](data-lake-analytics-get-started-powershell.md)
- [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
- [Korištenje alata za Lake podataka za Visual Studio za razvoj aplikacija U SQL](data-lake-analytics-data-lake-tools-get-started.md)
