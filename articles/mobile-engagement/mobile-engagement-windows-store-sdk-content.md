<properties 
    pageTitle="Windows SDK univerzalni aplikacije sadržaja" 
    description="Saznajte više o sadržaj Windows univerzalni aplikacije SDK za Azure Mobile radnje"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-content"></a>Windows SDK univerzalni aplikacije sadržaja

Ovaj dokument navodi i opisuje sadržaj uvesti SDK u aplikaciji.

##<a name="the-resources-folder"></a>Na `/Resources` mape

Ova mapa sadrži resurse koji je potrebno Mobile radnje. Možete i prilagoditi ih tako da stane aplikacije.

- `EngagementConfiguration.xml`Datoteka konfiguracije: Mobile radnje, to je gdje možete prilagoditi postavke Mobile radnje (niz za povezivanje radnje Mobile, izvješće rušenje...).

### <a name="html-folder"></a>/HTML mape

- `EngagementNotification.html`: Na `Notification` web-dizajna html prikaz u aplikaciji reklamni natpisi.

- `EngagementAnnouncement.html`: Na `Announcement` web-dizajna html prikaz u aplikaciji interstitial prikaza.

### <a name="images-folder"></a>/Images mape

- `EngagementIconNotification.png`Ikonom: marke s lijeve strane obavijesti, zamijeniti ovaj ikonom marku.

- `EngagementIconOk.png`: Na `Ok` ikonu razgovor stranice sadržaja za gumb akcije ili provjere valjanosti.

- `EngagementIconNOK.png`: Na `NOK` ikona koristiti prilikom provjere valjanosti gumb stranice sadržaja razgovor nije omogućen.
 
- `EngagementIconClose.png`: Na `Close` ikonu razgovor obavijesti i sadržaja za gumb Odbaci.

### <a name="overlay-folder"></a>/overlay mape

- `EngagementPageOverlay.cs`: Prekriveno stranica odgovoran za dodavanje na radnje dosegne korisničkog Sučelja u aplikaciju njegov djetetu.
  
