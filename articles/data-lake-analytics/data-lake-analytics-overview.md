<properties 
   pageTitle="Pregled sustava Microsoft Azure podataka Lake analize | Azure" 
   description="Analize podataka Lake je usluga izračuni Azure velikih skupova podataka koje možete koristiti podatke na pogon poslovanja pomoću uvida koje ste dobili iz podataka u oblaku, bez obzira na to gdje se nalazi i bez obzira na njegovu veličinu. Analize podataka Lake omogućuje to u najjednostavniji, većina skalabilni i najčešće najekonomičniji mogući način. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Pregled analize podataka Lake za Microsoft Azure

## <a name="what-is-azure-data-lake-analytics"></a>Što je Lake analize podataka za Azure?

Azure analize podataka Lake novi je servis, ugrađeni da biste se lakše analize velikih skupova podataka. Servis omogućuje fokusiranje na pisanje, pokrenuti i upravljanje zadacima, umjesto operacijski distributed infrastrukture. Umjesto implementacija, konfiguriranje i ugađanje hardver pisati upite pretvaranje podataka i izdvojiti koristan uvide. Servis za analytics možete obrađivati poslove sve skaliranje trenutačno postavljanjem na poziva za koliko power vam je potrebna. Samo plaćate za svoj posao kada je pokrenut upućivanje učinkovit. Servis za analytics podržava Azure Active Directory vam jednostavno upravljanje pristup i uloge, integriran s lokalnim sustavom za identitet. Obuhvaća i U-SQL, na jeziku koji se ujednačuje prednosti SQL s izražajna moć kod korisnika. U-SQL-skalabilni raspodijeljeno runtime omogućuje učinkovito analize podataka u spremištu i na poslužiteljima SQL Azure, baze podataka SQL Azure i Azure SQL Data Warehouse.


## <a name="key-capabilities"></a>Ključne mogućnosti

- **Dinamični skaliranja** 

    Analize podataka Lake architected je od početka projektirane za skaliranje oblaka i performanse.  Dinamički dodjeljuje resurse i omogućuje vam da učinite analize terabajta ili čak i exabytes podataka. Nakon dovršetka posla, ga vjetrovi dolje resursi automatski, a koji morate platiti samo za power obrada koristi. Dok ne povećate ili smanjite veličinu podataka pohranjenih ili količinu računalnim koristi, ne morate da biste prebrisali parametre. Omogućuje fokusiranje na poslovne logike samo, a ne na kako obraditi i spremiti velikih skupova podataka. 

- **Razvoj brže, ispravljanje pogrešaka i optimizirati pametnije pomoću poznatih alata**

    Analize podataka Lake ima precizno Integracija s programom Visual Studio, da bi mogli koristiti poznate alate za pokretanje, za ispravljanje pogrešaka i ugađanje kod. Vizualizacija U SQL poslova dopuštaju prikaz načinom kod na razini, tako da možete jednostavno prepoznati o performansama grla i optimiziranje troškove. 

- **U SQL: jednostavno i poznate Napredna i extensible**

    Podataka Lake Analytics uključuje U-SQL, jezika za upite koji se proteže poznatih, jednostavno, deklarativno prirode SQL s izražajna moć C#. Jezik U SQL se temelji na isti raspodijeljeno runtime powers sustavi velikih skupova podataka unutar Microsoft. Milijune SQL i .NET razvojni inženjeri sada možete obradu i sve svoje podatke s vještina koji već imaju analizirati.

- **Integracija integrira vaš IT ulaganja**

    Analize Lake podataka možete koristiti svoje postojeće IT ulaganja za identitet, upravljanje, sigurnost i skladištenju. Pojednostavnjuje upravljanja podacima i olakšava Proširivanje trenutne aplikacije za podatke. Analize podataka Lake integriran s servisa Active Directory za upravljanje korisnicima i dozvolama i u sklopu ugrađeni praćenje i nadzor.

- **Stvarni jeftin i troškova**

    Analize podataka Lake je učinkovit rješenje za izvođenje radnih opterećenja velikih skupova podataka. Plaćate na temelju po zadatka kada se obrađuju podataka. Potrebni su bez hardver, licence ili ugovore specifične za usluge podrške. Sustav automatski mijenja veličinu prema gore ili dolje kao što je posao pokreće i dovrši, što znači da nikada ne plaćate dulje nego što vam je potrebno. 

- **Radi sa svim podacima za Azure**

    Analize Lake podataka možete raditi s brojem izvora podataka za Azure: spremište blobova platforme Azure, baze podataka Azure SQL i naravno analize podataka Lake posebno optimiziran je za rad s Lake spremišta podataka za Azure - najvišu razinu performanse, propusnost i parallelization za pružanje radnih opterećenja velikih skupova podataka.

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
    - [Pristup dijagnostički zapisnici Lake analitičkih podataka za Azure](data-lake-analytics-diagnostic-logs.md)

- Praktični vodič završetka do kraja
    - [Interaktivni vodiči za analize korištenja Azure podataka Lake](data-lake-analytics-use-interactive-tutorials.md)
    - [Analiza zapisnika web-mjesta pomoću Lake analize podataka za Azure](data-lake-analytics-analyze-weblogs.md)

- Recite nam što mislite
    - [Komentiranje naš zaostale dokumentacija](data-lake-analytics-documentation-backlog.md)
    - [Pošaljite zahtjev za značajku](http://aka.ms/adlafeedback)
    - [Zatražite pomoć na forumima](http://aka.ms/adlaforums)


