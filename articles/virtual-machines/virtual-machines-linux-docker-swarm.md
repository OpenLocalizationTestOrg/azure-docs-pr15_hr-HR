<properties
   pageTitle="Uvod u rad docker s swarm na Azure"
   description="U članku se opisuje kako stvoriti grupu VMs s nastavkom VM Docker i koristiti swarm da biste stvorili Docker klaster."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Kako koristiti docker s swarm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ova tema prikazuje vrlo jednostavan način za uporabu [docker](https://www.docker.com/) [swarm](https://github.com/docker/swarm) da biste stvorili klaster swarm upravlja na Azure. Stvara četiri virtualnim strojevima u Azure, jedan će poslužiti kao rukovoditelj swarm i tri kao dio klaster docker domaćini. Kada završite, swarm možete koristiti da biste vidjeli klaster i počnite ga koristiti docker na njemu. Uz to, Azure EŽA pozive u ovoj temi koristiti načinu upravljanja (asm) usluge. 

> [AZURE.NOTE] U ovoj se temi koristi docker s swarm i na Azure EŽA *bez* korištenja **docker računalo** da bi se prikazuju kako funkcioniraju Alati za različite zajedno, no ostaju neovisno. **docker** računala **– swarm** parametre koje omogućuju korištenje **docker strojno** čvorove izravno dodati na swarm. Na primjer, potražite u dokumentaciji za [docker računala](https://github.com/docker/machine) . Ako propuštenih **docker računalo** s instaliranim programom protiv Azure VMs, pročitajte članak [kako koristiti docker računalu s Azure](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Stvaranje docker hosts virtualnim računalima sustava s Azure

U ovoj se temi stvara četiri VMs, ali možete koristiti bilo koji broj koji želite. Poziv s * &lt;lozinka&gt; * zamijenjene lozinku koju ste odabrali.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Kada završite ćete moći koristiti **azure vm popis** da biste vidjeli svoje Azure VMs:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Instaliranje swarm na matrici swarm VM

U ovoj se temi koristi u [spremniku model instalaciju na docker swarm dokumentaciju](https://github.com/docker/swarm#1---docker-image) –, ali vam nije moguće i SSH **swarm matrice**. U ovom modelu **swarm** se preuzima kao spremnik docker radi swarm. Ispod, ne možemo izvesti ovaj korak *daljinski iz naših prijenosno računalo pomoću docker* za povezivanje s **swarm matrica** VM i recite im klaster id stvaranja naredbe, **Stvaranje swarm**. Id klaster je kako **swarm** otkrije članove grupe za swarm. (Mogu Kloniraj spremište i sastavljanje je sami, koji će vam dati Potpuna kontrola i omogućiti ispravljanje pogrešaka.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Je li posljednjeg retka klaster ID-jeve. Kopirajte vezu negdje gdje jer će vam trebati ponovno prilikom uključivanja čvor VMs swarm matrice da biste stvorili "swarm". U ovom se primjeru id klaster je **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Samo da razjasnimo ćemo koristite naš docker lokalne instalacije za povezivanje s **swarm matrica** VM Azure i uputa **swarm matrice** za preuzimanje, instaliranje i pokretanje naredbe **Stvori** koja vraća naš klaster id koji koristimo za otkrivanje svrhe kasnije.
<!-- -->
> Da biste to potvrdili, pokrenite `docker -H tcp://` * &lt;naziv glavnog računala&gt; * ` images` popis procesa spremnik na računalu **swarm matrice** i drugi čvor za usporedbu (jer naišli prethodne naredbe swarm s parametrom **– upravitelja resursa** , spremnik uklonjena kada je dovršena, pomoću **docker ps - a** ne vraća ništa).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Ako ste upoznati s **docker**znali da ostale čvorove imaju nema stavki jer nema slika preuzeti i pokrenuti još.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Uključivanje čvor VMs naše docker klaster

Za svaki čvor popisa informacija o krajnjoj točki pomoću EŽA Azure. U nastavku ćemo učiniti za glavno računalo docker **swarm čvor 1** da biste dobili u čvor docker port (priključak).

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Korištenje **docker** i `-H` mogućnost pokazati docker klijenta na čvor VM pridružiti toj čvor swarm stvarate prosljeđivanjem klaster ID-a i na čvor docker priključak (drugu mogućnost korištenja **– adr**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Koja izgleda dobro. Da biste potvrdili te **swarm** radi **swarm čvor 1** smo upišite:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Ponovite za sve ostale čvorove u klasteru. U našem slučaju moramo **swarm čvor 2** i **swarm čvor 3**.

## <a name="begin-managing-the-swarm-cluster"></a>Početak upravljanja swarm klaster

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

a zatim možete navesti izvan vaše čvorove u svoj klaster:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Otvorite što će se izvoditi na vašem swarm. Da biste potražili vrhunskih, pročitajte članak [https://github.com/docker/swarm/](https://github.com/docker/swarm/)ili možda [videozapisa](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
