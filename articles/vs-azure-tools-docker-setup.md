<properties
   pageTitle="Konfiguriranje glavnog računala Docker s VirtualBox | Microsoft Azure"
   description="Detaljne upute za konfiguriranje zadane instance Docker pomoću računala Docker i VirtualBox"
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
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Konfiguriranje glavnog računala Docker s VirtualBox

## <a name="overview"></a>Pregled
U ovom se članku će vas voditi kroz konfiguriranje zadane instance Docker pomoću računala Docker i VirtualBox. Ako koristite [beta Docker za Windows](http://beta.docker.com/), tu konfiguraciju nije potreban.

## <a name="prerequisites"></a>Preduvjeti
Alati za sljedeće moraju biti instalirani.

- [Docker alatnog okvira](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfiguriranje klijenta Docker s komponentom Windows PowerShell

Da biste konfigurirali Docker klijent, jednostavno otvorite Windows PowerShell i poduzeti sljedeće korake:

1. Stvaranje zadane instance docker glavnog računala.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Provjerite je li zadane instance konfiguriran i pokrenuti. (Trebali biste vidjeti instance pod nazivom "Zadano" izvodi.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![Izlaz ls docker računalu][0]
 
1. Postavljanje zadanih kao glavno računalo za trenutni i konfiguriranje vaše ljuske.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Prikaz aktivne spremnika Docker. Na popisu mora biti prazna.

    ```PowerShell
    docker ps
    ```

    ![docker ps Izlaz][1]
 
> [AZURE.NOTE]Svaki put kada ponovno pokrenete računalo razvoj morat ćete ponovno pokrenuti vaše lokalne docker glavnog računala.
> Da biste to učinili, problema sljedeću naredbu u naredbenom retku: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
