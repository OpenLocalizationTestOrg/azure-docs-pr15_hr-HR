<properties
   pageTitle="Implementacija u ASP.NET osnovne Linux Docker spremnik udaljene glavno računalo za Docker | Microsoft Azure"
   description="Saznajte kako koristiti Visual Studio Tools for Docker za implementaciju web-aplikaciju programa ASP.NET Core Docker spremniku sustavom Linux VM Azure Docker glavnog računala"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Implementacija u spremniku ASP.NET udaljenog Docker glavnog računala

## <a name="overview"></a>Pregled
Docker je modul laganih spremnika, slično kao u nekim načinima virtualnog računala koje možete koristiti za glavno računalo aplikacija i servisa.
Pomoću ovog praktičnog vodiča vodit će vas kroz pomoću proširenje za [Visual Studio Tools 2015 za Docker](http://aka.ms/DockerToolsForVS) za implementaciju aplikacije ASP.NET osnovne glavno računalo za Docker na Azure pomoću komponente PowerShell.

## <a name="prerequisites"></a>Preduvjeti
Sljedeći je potrebno za dovršetak ovog praktičnog vodiča:

- Stvaranje programa VM glavno računalo za Azure Docker kao što je opisano [kako koristiti docker računalu s Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Instalirajte [Visual Studio 2015 ažuriranje 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Instalirajte [Visual Studio Tools 2015 za Docker – pregled](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. stvaranje web-aplikacije programa Core platforme ASP.NET
Sljedeći koraci vodit će vas kroz stvaranje osnovne ASP.NET osnovne aplikacije koja će se koristiti u ovom ćete praktičnom vodiču.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. dodali Docker podrške

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Koristite skriptu PowerShell DockerTask.ps1 

1.  Otvorite upit ljuske PowerShell za korijenski direktorij projekta. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Provjerite valjanost u alat za analizu daljinske pokrenuto glavno računalo. Trebali biste vidjeti stanje = pokretanje 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Ako koristite Docker Beta na glavno računalo neće se nalaziti.

1.  Sastavljanje pomoću aplikacije parametar - međuverzije

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Ako koristite Docker Beta, izostavite argument - računala
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Pokrenite aplikaciju, pomoću parametar - cilja

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Ako koristite Docker Beta, izostavite argument - računala
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Kada se dovrši docker, trebali biste vidjeti rezultate sličnu ovoj:

    ![Prikaz aplikacije][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
