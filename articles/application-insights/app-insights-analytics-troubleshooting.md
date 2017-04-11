<properties 
    pageTitle="Otklanjanje poteškoća s Analytics – alat za napredno pretraživanje aplikacije uvida | Microsoft Azure" 
    description="Problemi s analize uvida aplikacije? Počnite ovdje. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Otklanjanje poteškoća prilikom analize u aplikaciji uvida


Problemi s [analize uvida aplikacije](app-insights-analytics.md)? Počnite ovdje. Analitički je alat za napredno pretraživanje uvida aplikacije za Visual Studio.



## <a name="limits"></a>Ograničenja

* Trenutno, rezultati upita ograničeni su na samo putem tjedan proteklih podataka.
* Preglednici ispitivanja na: najnovija izdanja Chrome, rub i Internet Explorer.


## <a name="known-incompatible-browser-extensions"></a>Datotečne nastavke za poznate nekompatibilnog preglednika

* Ghostery

Onemogućite proširenja ili koristite neki drugi preglednik.


##<a name="e-a"></a>"Neočekivana pogreška"

![Zaslon neočekivana pogreška](./media/app-insights-analytics-troubleshooting/010.png)

Tijekom izvođenja portala – neobrađenu iznimku pojavila se interna pogreška.

* Čišćenje predmemorije u pregledniku. 

## <a name="e-b"></a>403... Pokušajte ponovno učitajte

![403... Pokušajte ponovno učitajte](./media/app-insights-analytics-troubleshooting/020.png)

Za provjeru autentičnosti povezane (tijekom provjere autentičnosti ili tijekom pristup tokena generacije) pojavila se pogreška. Na portalu možda nije moguće oporaviti bez promjene postavki preglednika.

* Provjerite je li u web-pregledniku [su omogućeni kolačići treće strane](#cookies) . 


## <a name="authentication"></a>403... Provjerite sigurnosne zone

![403.. .verify sigurnosne zone](./media/app-insights-analytics-troubleshooting/030.png)

Za provjeru autentičnosti povezane (tijekom provjere autentičnosti ili tijekom pristup tokena generacije) pojavila se pogreška. Na portalu možda nije moguće oporaviti bez promjene postavki preglednika.

1. Provjerite je li u web-pregledniku [su omogućeni kolačići treće strane](#cookies) . 

2. Jeste li koristili favorite, knjižnu oznaku ili spremljenu vezu da biste otvorili portal analize? Su prijavljeni pomoću vjerodajnica za različite od koristi prilikom spremanja vezu?

2. Pokušajte koristiti u prozoru preglednika u-privatno/inkognito (nakon zatvaranja takve windows). Morat ćete unijeti vjerodajnice. 

2. Otvorite novi prozor (obična) preglednik i idite na [Azure](https://portal.azure.com). Odjavite se. Zatim otvorite vezu i prijavite se pomoću odgovarajuće vjerodajnice.

2. Korisnicima rub i Internet Explorer možete također se ova pogreška kad zonu pouzdanih postavke nisu podržane.

    Provjerite je li [portal analize](https://analytics.applicationinsights.io) i [Azure Active Directory portal](https://portal.azure.com) nalaze u istom sigurnosne zone:

 * U pregledniku Internet Explorer otvorite **Internetske mogućnosti**, **Sigurnost**, **Pouzdana mjesta**, **web-mjesta**:

    ![Dijaloški okvir Internetske mogućnosti, dodavanje web-mjesta u pouzdana web-mjesta](./media/app-insights-analytics-troubleshooting/033.png)

    Na popisu web-mjesta ako su neki od sljedećih URL-ovi uključene, provjerite na druge jesu li uključene i:

    https://Analytics.applicationinsights.IO<br/>
   https://login.microsoftonline.com<br/>
   https://login.Windows.NET


## <a name="e-d"></a>404... Resurs nije pronađen

![404... resurs nije pronađen](./media/app-insights-analytics-troubleshooting/040.png)

Aplikacija resursa izbrisan iz aplikacije uvida i više nije dostupna. To se može dogoditi ako ste spremili URL stranice analize.


## <a name="e-e"></a>403... Nema autorizacije

![403... Nemate ovlasti](./media/app-insights-analytics-troubleshooting/050.png)

Nemate dozvolu za otvaranje ovu aplikaciju u analize.

* Je li se veza s nekom drugom? Zamolite ih da biste bili sigurni da se [čitatelji ili suradnika za ovu grupu resursa](app-insights-resources-roles-access-control.md).
* Jeste li spremiti veze pomoću vjerodajnica za različite? Otvorite [portal za Azure](https://portal.azure.com), odjavite, a zatim pokušajte ovu vezu ponovno pruža ispravne vjerodajnice.

## <a name="html-storage"></a>403... Spremanje u obliku HTML5

Portal za naše koristi HTML5 localStorage i sessionStorage.

* Chrome: Postavke zaštite privatnosti, postavke sadržaja.
* Internet Explorer: Internetske mogućnosti, kartica Napredno, sigurnost, Omogući DOM pohranu


![403... Pokušajte da biste omogućili prostora za pohranu u obliku HTML5](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404... Pretplate nije pronađen


![404... Pretplate nije pronađen](./media/app-insights-analytics-troubleshooting/070.png)

URL nije valjan. 

* Otvorite aplikaciju resurs portalu [Uvida aplikacije](https://portal.azure.com). Zatim pomoću gumba analize.

## <a name="e-h"></a>404... stranica ne postoji

![404... Stranica ne postoji](./media/app-insights-analytics-troubleshooting/080.png)

URL nije valjan.

* Otvorite aplikaciju resurs portalu [Uvida aplikacije](https://portal.azure.com). Zatim pomoću gumba analize.

## <a name="cookies"></a>Omogućivanje kolačiće trećih strana

  Saznajte [kako onemogućiti kolačiće trećih strana](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ali obratite pozornost na to ćemo morati **omogućiti** ih.

## <a name="e-x"></a>Ako sve ostalo ne uspije    

[Obratite nam se](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

