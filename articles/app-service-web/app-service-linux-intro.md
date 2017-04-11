<properties 
    pageTitle="Uvod u aplikaciju servisa na Linux | Microsoft Azure" 
    description="Saznajte više o aplikacije servisa za na Linux." 
    keywords="Azure aplikacije servisa, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Uvod u aplikaciju servisa na Linux
Aplikacije servisa za na Linux trenutno javno pretpregled i ima nativnu podršku izvodi web-aplikacije na Linux. 

## <a name="overview"></a>Pregled ##
Klijentima pomoću aplikacije servisa za na Linux na glavno računalo web-aplikacije nativno na Linux za podržane aplikacije snop. U sljedećoj sekciji značajke popis trenutno podržanih aplikacija snop.

## <a name="features"></a>Značajke ##
Aplikacije servisa za na Linux trenutno podržava sljedećih snop aplikacije

- Node.js
- PHP

Korisnici mogu implementirati svoje aplikacije pomoću

- FTP.
- Lokalni brojka.
- GitHub ili BitBucket.

Aplikacija skaliranja


- Korisnici mogu mijenjati veličinu svoje web-aplikacije gore i dolje tako da promijenite sloju u planirajte svoje aplikacije servisa. 
- Korisnici možete izvan svoje aplikacije Vremensko mjerilo i pokrenuti svoje aplikacije preko više instanci unutar ograničenja koja nalaže njihove SKU-om.

Za Kudu neke osnovne funkcije će funkcionirati

- Okruženje.
- Implementacija.
- Osnovni konzole.

## <a name="limitations"></a>Ograničenja ##

Portal za Azure upravljanje samo vi trenutno podržane značajke za aplikacije servisa za na Linux i ostale sakrili. Kao naš tim za omogućivanje dodatnih značajki smo će zadržati to promišljanje o onome portal za upravljanje. Neke značajke kao što su VNET integracije i AAD / provjere autentičnosti proizvođača ili proširenja web-mjesta Kudu trenutno ne radi. No, kao što smo dobili te rad ćemo ažurirati naš dokumentaciju i blog o promjenama.

Javni pretpregledu trenutno dostupna je samo u sljedećim regijama

-   Zapad SAD-a.
-   Zapad Europa.
-   Jugoistočne Azije.

Web app na Linux podržano je samo u namjenski aplikacije servisa za tarife te su besplatno ili zajednički se koristi sloju. Osim toga, aplikacije servisa za obične i Linux web-aplikacije su tarife isključivih, tako da ne možete stvoriti web-aplikacijama Linux u tarifu za servis aplikacije koje nisu Linux.

Web-aplikacijom na Linux moraju se stvoriti u grupu resursa koje sadrže web-aplikacije koje nisu Linux u istom području.

Zbog nedostatka Preklapajući recikliranje web-aplikacije kupci očekivati imate mali nedostupnost slučaju web-aplikacijama pokrene. 

## <a name="next-steps"></a>Daljnji koraci ##

Slijedite sljedeće veze za početak rada sa servisom aplikacije na Linux. Ponovno postavite pitanja i problemi na [našem forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Stvaranje web-aplikacije u aplikaciju servisa na Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Pomoću konfiguracije PM2 za Node.js u web-aplikacijama na Linux](./app-service-linux-using-nodejs-pm2.md)

