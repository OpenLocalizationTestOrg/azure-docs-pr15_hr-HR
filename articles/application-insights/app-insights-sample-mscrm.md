<properties 
    pageTitle="Vodič: Praćenje Microsoft Dynamics CRM s računala uvida" 
    description="Dohvaćanje telemetrijskih iz Microsoft Dynamics CRM Online pomoću aplikacije uvide. Vodič postavljanja početak podatke, vizualizacija i izvoz." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Vodič: Omogućivanje Telemetrijskih za Microsoft Dynamics CRM Online pomoću aplikacije uvida

U ovom se članku objašnjava dohvaćanje telemetrijskih podataka iz [Microsoft Dynamics CRM Online](https://www.dynamics.com/) pomoću [Uvida aplikacije za Visual Studio](https://azure.microsoft.com/services/application-insights/). Ćemo vas provesti kroz cijeli postupak dodavanja skriptu aplikaciju uvida u aplikaciji, dohvaćanje podataka i vizualizaciju podataka.

>[AZURE.NOTE] [Pregled uzorak rješenja](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Dodavanje aplikacije uvida novi ili postojeći instanca CRM Online 

Da biste pratili vaše aplikacije, dodajte programa SDK aplikacije uvida u aplikaciji. SDK šalje telemetrijskih [portal za aplikacije uvide](https://portal.azure.com), gdje možete koristiti naše Napredna analiza i Alati za dijagnostiku ili izvoz podataka u prostor za pohranu.

### <a name="create-an-application-insights-resource-in-azure"></a>Stvaranje do uvida aplikacije resursa u Azure

1. Pronađite [računa u Microsoft Azure](http://azure.com/pricing). 
2. Prijavite se na [portal za Azure](https://portal.azure.com) i dodajte novi resurs uvida aplikacije. To su gdje obrađuju i prikazati podatke.

    ![Kliknite +, servise za razvojne inženjere, uvida aplikacije.](./media/app-insights-sample-mscrm/01.png)

    Odaberite ASP.NET kao vrsta aplikacije.

3. Otvorite karticu za brzi početak rada i otvorite skripte kod.

    ![](./media/app-insights-sample-mscrm/03.png)

**Kodna stranica ostati otvoren** dok radite na sljedeću koraka u drugom prozoru preglednika. Morat ćete kod uskoro. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Stvaranje web-resursa JavaScript u sustavu Microsoft Dynamics CRM

1. Otvorite CRM Online instancu i prijavite se s administratorskim ovlastima.
2. Sustav prilagoditi prilagodbe, otvaranje Microsoft Dynamics CRM postavke

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Stvaranje JavaScript resursa.

    ![](./media/app-insights-sample-mscrm/07.png)

    Dajte naziv, odaberite **skriptu (JScript)** i otvorite uređivač teksta.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopirajte kod iz aplikacije uvide. Prilikom kopiranja provjerite je li da biste zanemarili skripte oznake. Pogledajte ispod snimka zaslona:

    ![](./media/app-insights-sample-mscrm/09.png)

    Kod obuhvaća ključ instrumentation koja služi za identifikaciju vaše aplikacije uvida resursa.

5. Spremite i objavite.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Obrasci instrument

1. U programu Microsoft CRM Online otvorite obrazac poslovnog subjekta

    ![](./media/app-insights-sample-mscrm/11.png)

2. Otvorite obrazac svojstva

    ![](./media/app-insights-sample-mscrm/12.png)

3. Dodati JavaScript web-resurs koji ste stvorili

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Spremite i objavite ih obrasca.


## <a name="metrics-captured"></a>Metriku zabilježene

Sada ste postavili telemetrijskih snimanja za obrazac. Kad god se podataka poslat će se na vaše aplikacije uvida resurs.

Evo primjera podataka prikazat će se.

#### <a name="application-health"></a>Stanje aplikacije

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Iznimke u pregledniku:

![](./media/app-insights-sample-mscrm/17.png)

Kliknite grafikon da biste dobili dodatne detalje:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Korištenje

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Preglednici

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolokacija-a

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Zahtjev za prikaz unutrašnji stranice

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Ogledni kod

[Pregled uzorak koda](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Čak i dublju analizu možete učiniti ako [Izvoz podataka u Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Uzorak Microsoft Dynamics CRM rješenja

[Evo uzorak rješenja implementirana u sustavu Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>uči više

* [Što je aplikacija uvida?](app-insights-overview.md)
* [Uvid u aplikaciju za web-stranice](app-insights-javascript.md)
* [Dodatne primjere i prikazi](app-insights-code-samples.md)

 
