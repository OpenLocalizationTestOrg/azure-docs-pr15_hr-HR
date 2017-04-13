<properties
   pageTitle="Ispravljanje pogrešaka aplikacije u spremniku za lokalni Docker | Microsoft Azure"
   description="Kako preinačiti aplikaciju koja se izvodi u spremniku za lokalni Docker, osvježavanje spremnik putem uređivanje i osvježavanje i postavljanje prekidne točke za ispravljanje pogrešaka"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Ispravljanje pogrešaka aplikacije u spremniku za lokalni Docker

## <a name="overview"></a>Pregled
Visual Studio Tools for Docker omogućuje dosljedno u razvoju web i provjeriti aplikacije lokalno u spremniku Linux Docker.
Ne morate ponovno pokrenuti spremnik svaki put kada budete izrađivali kod promjena.
U ovom se članku će prikazuju kako koristiti značajku "Uređivanje i osvježavanje" za pokretanje aplikacije ASP.NET Core Web u spremniku za lokalni Docker, unesite željene promjene, a zatim osvježite preglednik da biste vidjeli te promjene.
Ga i vidjet ćete kako postaviti prekidne točke za ispravljanje pogrešaka.

> [AZURE.NOTE] Podrška za Windows spremnik će biti uskoro u buduće izdanje

## <a name="prerequisites"></a>Preduvjeti
Alati za sljedeće moraju biti instalirani.

- [Visual Studio 2015 ažuriranje 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Instalirajte [Visual Studio 2015 ažuriranje 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Da biste pokrenuli Docker spremnika lokalno, morat ćete lokalni docker klijenta.
Možete koristiti objavljenu [Docker alatnog okvira](https://www.docker.com/products/overview#/docker_toolbox) koji su potrebni Hyper-V tako da se onemogućiti ili koristite [Docker za Windows Beta](https://beta.docker.com) koji koristi Hyper-V i potreban je Windows 10.

Ako koristite alatnog okvira Docker, morat ćete [konfigurirati klijenta Docker](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. stvorite web-aplikaciju

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. dodali Docker podrške

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. uređivanje koda i osvježavanje

Brzo iteracija promjene, možete pokrenuti program u spremniku i nastavili mijenjati, koji ih prikazuju kao što činite s IIS Express.

1. Postavljanje konfiguracije rješenja `Debug` i pritisnite ** &lt;CTRL + F5 >** omogućuje stvaranje docker sliku i izvodi lokalno.

    Kada spremnik slika sadrži je ugrađena pa se izvodi u spremniku Docker Visual Studio će se pokrenuti web-aplikaciju u zadanom pregledniku.
    Ako su putem preglednika Microsoft Edge ili u suprotnom sadrže pogreške, pogledajte odjeljak za [Otklanjanje poteškoća](vs-azure-tools-docker-troubleshooting-docker-errors.md) .

1. Otvorite stranicu o koji je koju smo ćete naš promjene.

1. Vratite se u Visual Studio i otvorite `Views\Home\About.cshtml`.

1. Dodajte sljedeće HTML sadržaj na kraj datoteku i spremite promjene.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Kada se dovrši sastavljanje .NET i vidjeti te retke, vratite se u pregledniku i osvježite stranicu o, prikazujete u izlaznom prozoru.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Promjene primijenjeni!

## <a name="4-debug-with-breakpoints"></a>4. za ispravljanje pogrešaka s prekidne točke

Često, promjene ćete dodatno provjere, korištenje pogrešaka značajke programa Visual Studio.

1.  Vratite se u Visual Studio i otvaranje`Controllers\HomeController.cs`

1.  Zamijenite sadržaj metodu About() sljedeće:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Postavljanje programa točku prekida s lijeve strane na `string message`... redak.

1.  Kliknite ** &lt;F5 >** da biste pokrenuli ispravljanje pogrešaka.

1.  Idite na stranicu o pogoditi iz prvog vaše točku prekida.

1.  Prijeđite u Visual Studio da biste pogledali na točku prekida pa provjerite vrijednost poruke.

    ![][2]

##<a name="summary"></a>Sažetak

[Visual Studio Tools 2015 za Docker](https://aka.ms/DockerToolsForVS), možete ih dohvaćati produktivnost lokalno, rad s realism radni od razvoj unutar spremnika Docker.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

[Otklanjanje poteškoća s razvoj Docker Visual Studio](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Dodatne informacije o Docker s Visual Studio i Windows Azure

- [Docker alate za Visual Studio](http://aka.ms/dockertoolsforvs) - razvoj .NET Core kod u spremniku
- [Docker alate za Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - stvorite i implementirajte spremnika docker
- [Alati za docker za Visual Studio kod](http://aka.ms/dockertoolsforvscode) - jezične servise za uređivanje datoteke docker s više e2e scenariji uskoro
- [Informacije o kontejneru sustava Windows](http://aka.ms/containers)– Windows Server i podaci o poslužitelju Nano
- [Servis za Azure spremnik](https://azure.microsoft.com/services/container-service/) - [sadržaj servisa Azure spremnik](http://aka.ms/AzureContainerService)
-    Više primjera rad s Docker potražite u članku [Rad s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) iz povezivanje [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [pokazni videozapis](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Dodatne početak rada s HealthClinic.biz videozapis, potražite u članku [Početak rada za alate za razvojne inženjere za Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Alati za različite Docker

[Neki alati sjajno docker (Steve Lasker blog)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Dobar članci

[Uvod u Microservices iz NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Prezentacija

- [Steve Lasker: Dodavanje veze za VANJSKIH Live Las Vegas 2016 – Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Uvod u ASP.NET osnovne @ sastavljanje 2016 – gdje ste pri pokazni videozapis](https://channel9.msdn.com/Events/Build/2016/B810)
- [Razvoj aplikacija za .NET u spremnika, 9 kanala](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
