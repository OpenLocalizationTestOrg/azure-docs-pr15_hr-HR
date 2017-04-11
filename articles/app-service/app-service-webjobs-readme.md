<properties
    pageTitle="WebJobs u aplikacije servisa za Azure"
    description="Saznajte kako izraditi WebJobs pokrenuti testove pozadine, interakciju sa servisa kao što je prostor za pohranu i Bus servisa i stvaranje zakazane zadatke."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Korištenje WebJobs u aplikacije servisa za Azure

U ovom članku navode veze na resurse dokumentaciju o načinu korištenja Azure WebJobs i Azure WebJobs SDK. Azure WebJobs pružaju jednostavan način da biste pokrenuli skripte ili programa kao pozadinski procesi [Web](http://go.microsoft.com/fwlink/?LinkId=529714)aplikacija servisa. Možete prenijeti i pokrenuti izvršne datoteke kao što su kao cmd, aplikacije za Outlook, exe (.NET), ps1, p, i Kopiraj, js i posudu. Ti programi pokrenuti kao WebJobs rasporedu (cron) ili kontinuirano.

WebJobs SDK lakše koristiti Azure prostora za pohranu. WebJobs SDK ima povezivanja i okidača sustav koji funkcionira s blob polja za pohranu za Microsoft Azure, redovima i tablice kao i redova Bus servisa.

Stvaranje, uvođenje i upravljanje WebJobs je objedinjenog s Integriranom tooling u Visual Studio. Stvaranje WebJobs iz predložaka, objavljivanje i upravljanje (Pokreni/zaustavi/monitor/Provjera pogrešaka) ih.

Na nadzornoj ploči WebJobs na portalu za Azure pruža mogućnosti Napredna upravljanja koji će vam potpunu kontrolu nad izvođenja WebJobs, uključujući mogućnost za pozivanje pojedinačne funkcija unutar WebJobs. Na nadzornoj ploči prikazuje i funkcija runtimes i zapisivanje izlaz.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
