<properties
   pageTitle="Stvaranje Docker hosts u Azure s računala Docker | Microsoft Azure"
   description="U članku se opisuje korištenje Docker računala da biste stvorili docker domaćini u Azure."
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

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Stvaranje Docker domaćini u Azure s Docker računala

Pokretanje [Docker](https://www.docker.com/) spremnika potreban je glavno računalo VM izvodi docker daemon.
U ovoj se temi opisuje kako pomoću naredbe [docker računala](https://docs.docker.com/machine/) da biste stvorili novi Linux VMs konfiguriran pomoću daemon Docker koji se izvodi u Azure. 

**Napomena:** 
- *U ovom se članku ovisi o docker strojno verziju 0.7.0 ili noviji*
- *Windows spremnika će podržavati kroz docker računala u skorijoj budućnosti*

## <a name="create-vms-with-docker-machine"></a>Stvaranje VMs s Docker računala

Stvaranje docker VMs glavno računalo u Azure s na `docker-machine create` naredba pomoću na `azure` upravljački program. 

Upravljački program za Azure ćete svoj ID pretplate. [Azure EŽA](xplat-cli-install.md) ili [Azure Portal](https://portal.azure.com) možete koristiti za dohvaćanje pretplate Azure. 

**Pomoću portala za Azure**
- Odaberite pretplate na stranici lijevom navigacijskom oknu, a kopirajte id pretplate.

**Korištenje Azure EŽA**
- Vrsta ```azure account list``` i kopirajte id pretplate.

Vrsta `docker-machine create --driver azure` da biste vidjeli mogućnosti i njihove zadane vrijednosti.
Možete pogledati i [upravljački program za Azure Docker dokumentaciju](https://docs.docker.com/machine/drivers/azure/) za dodatne informacije. 

U sljedećem primjeru oslanja nakon zadane vrijednosti, ali ga otvoriti priključak 80 VM za pristup Internetu. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Odaberite docker glavno računalo s docker računala
Nakon što dodate stavku u docker računala za vaše glavnog računala, možete postaviti zadano glavno računalo kada se pokrene docker naredbe.
##<a name="using-powershell"></a>Pomoću komponente PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Korištenje tulumu

```bash
eval $(docker-machine env MyDockerHost)
```

Sada možete pokrenuti naredbe docker protiv navedeni glavnog računala

```
docker ps
docker info
```

## <a name="run-a-container"></a>Pokretanje spremnik

Pomoću glavnog računala je konfiguriran, sada možete pokrenuti jednostavne web-poslužitelju da biste testirali li ispravno konfigurirano vaše glavnog računala.
U nastavku ćemo koristi standardnu nginx sliku, odrediti treba li preslušali na priključak 80, a da je ponovnog pokretanja voditelju VM spremnik će se ponovno pokrenite kao i (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Rezultat izgleda otprilike ovako:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testiranje spremnik

Pregledajte izvodi spremnika pomoću `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

I provjerite izvodi spremnik upišite `docker-machine ip <VM name>` da biste pronašli IP adresa za unos u pregledniku:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Tekući ngnix spremnik](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Sažetak
S docker računala možete jednostavno Dodjela docker domaćini u Azure za vaše provjere valjanosti za pojedinačne docker glavnog računala.
Radnom hostiranja od spremnika, potražite u članku [Servis spremnik Azure](http://aka.ms/AzureContainerService)

Za razvoj aplikacija Core .NET pomoću Visual Studio, potražite u članku [Docker alate za Visual Studio](http://aka.ms/DockerToolsForVS)
