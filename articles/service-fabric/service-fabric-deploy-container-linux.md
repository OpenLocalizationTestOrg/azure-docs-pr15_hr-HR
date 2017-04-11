<properties
   pageTitle="Servis tkanina i implementacija spremnika u Linux | Microsoft Azure"
   description="Servisa tkanina i korištenje Docker spremnika za implementaciju aplikacije microservice. U ovom se članku opisuju mogućnosti koje tkanina servis nudi za spremnika i kako implementirati spremnik sliku Docker u klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Implementacija spremniku Docker tkanina servisa

> [AZURE.SELECTOR]
- [Implementacija spremnik za Windows](service-fabric-deploy-container.md)
- [Implementacija Docker spremnik](service-fabric-deploy-container-linux.md)

U ovom se članku vodit će vas kroz stvaranje containerized servisa u Docker spremnika na Linux.

Servis tkanina ima nekoliko mogućnosti spremnik koji vam pomoći s stvaranje aplikacija koja se sastoji od microservices koji su containerized. One se nazivaju containerized services.

Mogućnosti uključuju;

- Spremnik slika implementacije i aktivacijom
- Upravljanje resursa
- Provjera autentičnosti spremište
- Spremnik priključak za mapiranje priključak glavnog računala
- Spremnik spremnik otkrivanje i komunikacija
- Mogućnost konfiguracija i postavljanje varijable okruženja


## <a name="packaging-a-docker-container-with-yeoman"></a>Pakiranje docker kontejner s yeoman
Kada pakiranje spremnik na Linux, možete odabrati bilo da koristite yeoman predložak ili [ručno stvaranje paketa aplikacije](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Servis tkanina aplikacije mogu sadržavati jedan ili više spremnika, svaka s određenim uloga u izlaganja funkcionalnosti aplikacije. SDK tkanina servisa za Linux obuhvaća [Yeoman](http://yeoman.io/) generator koja olakšava stvaranje aplikacija i dodajte sliku kontejner. Da biste stvorili novu aplikaciju s jednom Docker spremnika koji se naziva *SimpleContainerApp*ćemo pomoću Yeoman. Možete dodati nove servise kasnije tako da uredite u generirani manifest datoteke.

## <a name="create-the-application"></a>Stvaranje aplikacije

1. U terminal, upišite **yo azuresfguest**.

2. Za framework odaberite **kontejner** .

3. Naziv aplikacije SimpleContainerApp npr.

4. Unesite URL za sliku kontejner s DockerHub repo. Otvorit će se obrazac [repo] / [Slika naziv]

![Generator Yeoman tkanina servisa za spremnika][sf-yeoman]

## <a name="deploy-the-application"></a>Implementacija aplikacije

Kada je ugrađena aplikacija, možete je implementirati u lokalnom klaster pomoću EŽA Azure.

1. Povezivanje s lokalnom klaster tkanina servisa.

    ```bash
    azure servicefabric cluster connect
    ```

2. Pomoću skripte Instaliraj navedenih u predlošku kopirajte Aplikacijski paket na klaster slika Store, registrirati vrsta aplikacije i stvoriti instancu aplikacije.

    ```bash
    ./install.sh
    ```

3. Otvorite preglednik i idite na servis tkanina Explorer pri http://localhost:19080/Explorer (Zamijeni localhost s privatnim IP od VM ako koristite Vagrant operacijskom sustavu Mac OS X).

4. Proširite čvor aplikacije i Primijetit ćete da je sada unos za vrstu vašeg računala, a drugi za prvu instancu vrste.

5. Koristite skriptu Deinstaliraj navedenih u predlošku da biste izbrisali instanci aplikacije i unregister vrsta aplikacije.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Daljnji koraci

- [Pregled servisa tkanina i spremnika](service-fabric-containers-overview.md)
- [Interakcija s klastere tkanina servis pomoću EŽA Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

