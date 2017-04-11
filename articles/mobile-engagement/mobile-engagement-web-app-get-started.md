<properties
    pageTitle="Početak rada s Azure Mobile radnje za web-aplikacije | Microsoft Azure"
    description="Saznajte kako koristiti Azure Mobile radnje analize i automatske obavijesti o web-aplikacijama."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Početak rada s Azure Mobile radnje za web-aplikacije

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi objašnjava korištenje Azure Mobile radnje da biste shvatili Upotreba web-aplikacije.

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ Visual Studio 2015 ili bilo kojem uređivaču po izboru
+ [Web SDK](http://aka.ms/P7b453) 

Taj SDK Web je u pretpregledu samo podržava analize koristilo i još ne podržava slanje pregledniku ili u aplikaciji slanje obavijesti. 

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Postavljanje mobilnih radnje za web-aplikacije

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni integracijom" koji je minimalnim potrebne da biste prikupili podatke.

Pomoću programa Visual Studio demonstrirati integraciju sa servisom Iako možete slijedeći upute s bilo kojeg web-aplikacijom i stvorili izvan Visual Studio ćemo će stvoriti osnovni web-aplikacijama. 

###<a name="create-a-new-web-app"></a>Stvorite novu web-aplikaciju

Sljedeći koraci pretpostavlja koristite Visual Studio 2015 iako koraci su slično u ranijim verzijama programa Visual Studio. 

1. Pokrenite Visual Studio, a na zaslonu **Polazno** odaberite **Novi projekt**.

2. Na skočnom izborniku odaberite **Web** -> **ASP.Net web-aplikacije**. Ispunite aplikaciju **naziv**, **mjesto** i **naziv rješenja**, a zatim kliknite **u redu**.

3. U skočnom **Odaberite predložak** odaberite **prazan** u odjeljku **Predlošci 4,5 ASP.Net** , a zatim kliknite **u redu**. 

Sada ste stvorili novi prazan Web App projekt u koju će smo integrirati Azure Mobile radnje Web SDK-a.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Stvorite novu mapu pod nazivom **javascript** u rješenje i dodajte datoteku Web SDK JS **azure engagement.js** u nju. 

2. Dodajte novu datoteku pod nazivom **main.js** u ovu mapu javascript sljedeći kod. Provjerite je li ažurirati niz za povezivanje. To `azureEngagement` objekt će se koristiti za pristup Web SDK metode. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio sa js datoteke][1]

##<a name="enable-real-time-monitoring"></a>Omogućiti praćenje u stvarnom vremenu

Da biste pokrenuli slanje podataka i provjera jesu li korisnici aktivna, pozadinski Mobile radnje mora poslati barem jedan aktivnosti. Aktivnost u kontekstu web-aplikacijama je web-stranicu. 

1. Stvaranje nove stranice naziva **home.html** u rješenje i postavite ga kao početnu stranicu za web-aplikacije. 
2. Obuhvaćaju dva JavaScript dodali smo neke starije verzije na ovoj stranici dodavanjem sljedeće unutar tijela oznaku. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Ažuriranje body oznake da biste uputili poziv EngagementAgent, `startActivity` način
        
        <body onload="engagement.agent.startActivity('Home')">

4. Evo kako će **home.html** izgledati
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Proširivanje analytics

Evo trenutno dostupno s SDK Web koje možete koristiti za analizu sve načine:

1. Aktivnosti/web-stranice:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Događaji
        
        engagement.agent.sendEvent(name, extras);

3. Pogreške

        engagement.agent.sendError(name, extras);

4. Zadaci

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

