<properties 
    pageTitle="Upute za stvaranje web-aplikacijama s uslugom aplikacije na Linux | Microsoft Azure" 
    description="Web app stvaranje tijeka rada za aplikacije servisa za na Linux." 
    keywords="Azure aplikacije servisa za web-aplikacije, linux, oss"
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

# <a name="create-a-web-app-with-app-service-on-linux"></a>Stvaranje web-aplikacijama pomoću aplikacije servisa za na Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Pomoću portala za upravljanje da biste stvorili web-aplikaciju programa
Možete početi stvarati web-aplikaciju programa na Linux s [portala za upravljanje](https://portal.azure.com) kao što je prikazano na slici u nastavku.

![][1]

Kada odaberete mogućnost u nastavku, koji će se prikazati plohu stvaranje kao što je prikazano na slici u nastavku. 

![][2]

-   Naziv web-aplikaciju programa.
-   Odaberite postojeću grupu resursa ili stvorite novi. (Pogledajte članak regije koje su dostupne u [odjeljku ograničenja](./app-service-linux-intro.md)).
-   Odaberite postojeću aplikaciju tarifa za servis ili stvorite novi nešto (pogledajte aplikacije servisa plan bilješke u [odjeljku ograničenja](./app-service-linux-intro.md)). 
-   Odaberite aplikaciju stog namjeravate koristiti. Dobit ćete odabrati između nekoliko verzija Node.js i PHP. 

Nakon što dodate aplikaciju stvorili, možete promijeniti stog aplikacije iz postavki aplikacije kao što je prikazano na slici u nastavku.

![][3]

## <a name="deploying-your-web-app"></a>Implementacija web-aplikaciju programa

Odabir "Mogućnosti implementacije" s portala za upravljanje vam nudi mogućnost da biste koristili lokalno spremište za brojka ili GitHub za implementaciju aplikacije. Nakon toga su upute na sličan način-Linux web App, a možete prema uputama u našem [lokalnu implementaciju brojka](./app-service-deploy-local-git.md) ili našem članku [Neprekinuti implementacije](./app-service-continuous-deployment.md) za GitHub.

Prijenos aplikacije na web-mjesto možete koristiti i FTP. Krajnja točka FTP za web-aplikacije možete pristupiti iz zapisnika odjeljak dijagnostički kao što je prikazano na slici u nastavku.

![][4]


## <a name="next-steps"></a>Daljnji koraci ##

* [Što je aplikacije servisa za na Linux?](./app-service-linux-intro.md)
* [Pomoću konfiguracije PM2 za Node.js u web-aplikacijama na Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
