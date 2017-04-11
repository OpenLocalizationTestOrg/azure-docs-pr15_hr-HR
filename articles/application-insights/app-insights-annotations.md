<properties
    pageTitle="Pustite primjedbe za aplikaciju uvida | Microsoft Azure"
    description="Dodajte implementacije ili stvaranje oznaka za grafikone explorer mjernih podataka u aplikaciji uvida."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Pustite primjedbe u aplikaciji uvida

Pustite primjedbe na [Metriku Explorer](app-insights-metrics-explorer.md) grafikonima prikazuje gdje implementiran novi Sastavi. Jednostavno da biste vidjeli je li promjene imala učinak na performanse vaše aplikacije. Ih je moguće automatski stvoriti tako da [Team Services za Visual Studio stvaranje sustava](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs), a možete i [stvoriti ih PowerShell](#create-annotations-from-powershell).

![Primjer primjedbe s vidljivi korelacije s vremenom odgovor poslužitelja](./media/app-insights-annotations/00.png)

Izdanje primjedbe su značajka sastavljanje oblaku i otpustite servisa programa Visual Studio Team Services. 

## <a name="install-the-annotations-extension-one-time"></a>Instalirajte proširenje primjedbe (jedna jedinica vremena)

Da biste mogli stvoriti primjedbe izdanje, morat ćete instalirati jedan od više dostupna proširenja timskog servisa Visual Studio tržištu.

1. Prijavite se u [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projekt.
2. U Visual Studio trgovine, [dobiti proširenje izdanje opaske](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), i njezino dodavanje računa za servise u tim.

![At gornjeg desnog Services timskog web-stranice, otvaranje trgovine. Odaberite vizualni Team Services, a zatim u odjeljku Sastavljanje i izdanje, odaberite više potražite u članku.](./media/app-insights-annotations/10.png)

Što je potrebno za to jednom za vaš račun za Visual Studio Team Services. Izdanje primjedbe sada moguće je konfigurirati za neki projekt na vašem računu. 

## <a name="get-an-api-key-from-application-insights"></a>Ključ za API iz aplikacije uvida

Morate učiniti za svaki izdanje predložak koji želite stvoriti izdanje primjedbe.


1. Prijava na [Microsoft Azure Portal](https://portal.azure.com) i otvorite aplikaciju uvida resurs koji nadzire aplikacije. (Ili [stvorili sada](app-insights-overview.md), ako to još niste učinili).
2. Otvorite **Access API -JA**, a ponijeti **Id uvida aplikacije**.

    ![U portal.azure.com, otvorite svoje aplikacije uvida resursa i odaberite postavke. Otvorite Access API-JA. Kopiranje ID aplikacije](./media/app-insights-annotations/20.png)

2. U zasebnom prozoru preglednika, otvorite (ili stvorite) izdanje predložak koji upravlja vaš implementaciji servisa za Visual Studio tima. 

    Dodavanje zadatka i s izbornika odaberite zadatak aplikacije uvida izdanje opaske.

    Zalijepite **Id aplikacije** koju ste kopirali iz plohu pristup API-JA.

    ![U Visual Studio Team Services, otvorite izdanje, odaberite definiciju izdanje i odaberite Uređivanje. Kliknite Dodavanje zadataka i odaberite aplikaciju uvida izdanje opaske. Lijepljenje uvida ID aplikacije](./media/app-insights-annotations/30.png)

3. Polje postavljeno na **APIKey** varijabla `$(ApiKey)`.

4. U plohu pristup API stvoriti novi ključ za API-JA i preuzeti kopiju.

    ![U plohu API pristup u prozoru Azure, kliknite Stvori ključ za API-JA. Navedite komentar, provjerite pisanje bilježaka i kliknite Stvori ključ. Kopirajte novog ključa.](./media/app-insights-annotations/40.png)

4. Otvorite karticu konfiguracija izdanje predloška.

    Stvara definiciju varijable za `ApiKey`.

    Zalijepite ključ za API na definiciju varijable ApiKey.

    ![U prozoru Team Services odaberite karticu konfiguracija, a zatim kliknite Dodaj varijabli. Postavite naziv ApiKey i u vrijednost, zalijepite ključ samo generira.](./media/app-insights-annotations/50.png)


5. Na kraju, **spremite** definiciju izdanje.

## <a name="create-annotations-from-powershell"></a>Stvaranje bilježaka iz komponente PowerShell

Možete stvoriti i primjedbe iz izvođenja procesa koji vam se sviđa (bez korištenja Dodavanje veze za VANJSKIH Team System). 

Pronađite [skriptu Powershell iz GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

Koristite ovako:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Početak u `applicationId` i `apiKey` iz vaše aplikacije uvida resursa: otvaranje postavke, Access API-JA i Kopiraj ID aplikacije. Zatim kliknite Stvori ključ za API-JA i kopirajte tipku. 

## <a name="release-annotations"></a>Pustite primjedbe

Sada kad god koristite izdanje predložak za implementaciju novo izdanje, primjedbe poslat će se aplikacija uvid u. Primjedbe pojavit će se na grafikonima u programu Explorer metriku.

Kliknite na bilo koju oznaku opaske da biste otvorili pojedinosti o izdanju, uključujući podnositelja zahtjeva, grana izvor kontrole, otpustite definicija, okruženje i drugo.


![Kliknite bilo koju oznaku opaske izdanje.](./media/app-insights-annotations/60.png)
