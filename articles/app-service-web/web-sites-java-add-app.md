<properties 
    pageTitle="Dodavanje jezika Java aplikacije na web-aplikacije za Azure aplikacije servisa" 
    description="Pomoću ovog praktičnog vodiča prikazuje kako dodati na stranicu ili aplikaciju za instanci programa Azure servisa Web aplikacija koja je već konfigurirana za korištenje Java." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Dodavanje jezika Java aplikacije na web-aplikacije za Azure aplikacije servisa

Nakon što ste pokrenuti web-aplikaciju programa Java u [Aplikacije servisa za Azure][] kao navedenih na [Stvaranje web-aplikacijama Java u aplikacije servisa za Azure](web-sites-java-get-started.md), možete prenijeti aplikacije potvrđivanjem vaše WAR u mapi **webapps** .

Navigacija put do mape **webapps** razlikuje se ovisno o kako postaviti svoje instance web-aplikacije.

- Ako postavljate web-aplikaciju programa pomoću servisa Azure Marketplace, put do mape u **webapps** je u obliku **d:\home\site\wwwroot\bin\application\_server\webapps**, pri čemu **aplikacije\_poslužitelja** je naziv poslužitelja aplikacija na snazi za svoje web-aplikacije instancu. 
- Ako postavljate web-aplikaciju programa Azure konfiguraciju korisničkog Sučelja, put do mape u **webapps** je obrazac **d:\home\site\wwwroot\webapps**. 

Imajte na umu pomoću kontrola izvora da biste prenijeli aplikacije ili web-stranice, uključujući [Neprekidan Integracija scenarija](app-service-continuous-deployment.md). FTP je i mogućnost prijenos aplikacije ili web-stranice.

Napomena Tomcat web-aplikacijama: kada prenesete WAR datoteku u mapu **webapps** , poslužitelj aplikacije Tomcat otkriva koji ste dodali i će automatski učitati. Imajte na umu da ako kopirate datoteke (osim WAR datoteke) u KORIJENSKOM direktoriju, poslužitelj aplikacije morat ćete ponovno pokrenuti prije no što ih koriste. Funkcija automatskoučitavanje web-aplikacijama Tomcat Java sustavom Azure temelji se na novu datoteku WAR dodani, ili nove datoteke ili mape koje su dodane u mapu **webapps** . 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Java](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Aplikacije servisa za Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
