<properties
   pageTitle="Postavljanje okruženja za razvoj | Microsoft Azure"
   description="Instalirajte izvođenja, SDK i alate i stvaranje lokalne razvoj klaster. Nakon dovršetka ovaj će instalacijski program, bit će spremna za izgradnju aplikacije."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Priprema razvojno okruženje

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Stvaranje i pokretanje [aplikacija tkanina servisa Azure] [ 1] na vašem računalu razvoj instalirati izvođenja, SDK i alate. Morate omogućiti izvršavanje skripte komponente Windows PowerShell obuhvaćeno SDK-a.

## <a name="prerequisites"></a>Preduvjeti
### <a name="supported-operating-system-versions"></a>Podržani operacijski sustav verzije
Za razvoj podržane su sljedeće verzije operacijski sustav:

- Windows 7
- Windows 8 i Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 sadrži Windows PowerShell 2.0 samo prema zadanim postavkama. Servis tkanina PowerShell cmdleti zahtijeva PowerShell 3.0 ili noviji. Možete [preuzeti Windows PowerShell 5.0] [ powershell5-download] iz Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>Instalirajte izvođenja, SDK i Alati

Instalacijski program platformu Web nudi dva konfiguracije za razvoj tkanina servisa:

- [Instalacija runtime tkanina servisa, SDK i alate za Visual Studio 2015 (zahtijeva Visual Studio 2015 ažuriranje 2 ili novija verzija)][full-bundle-vs2015]
- [Instalirajte servis tkanina izvođenja i SDK samo (bez Visual Studio tools)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Omogućivanje izvođenja skriptu PowerShell

Servis tkanina koristi skripte komponente Windows PowerShell za stvaranje lokalne razvoj klaster i za implementaciju aplikacije Visual Studio. Prema zadanim postavkama sustava Windows blokira te skripte pokretanje. Da biste im omogućili, morate izmjenu pravila izvođenja PowerShell. Otvorite PowerShell kao administrator, a zatim unesite sljedeću naredbu:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste završili gore razvojno okruženje, pokrenite stvaranje i pokretanje aplikacija.

- [Stvaranje prve aplikacije servisa tkanina u Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Saznajte kako uvesti i upravljanje aplikacijama na vašem lokalnom klaster](service-fabric-get-started-with-a-local-cluster.md)
- [Dodatne informacije o programskom modela: pouzdanog servise i pouzdan Glumci](service-fabric-choose-framework.md)
- [Pogledajte primjere koda tkanina servisa na GitHub](https://aka.ms/servicefabricsamples)
- [Vizualizacija svoj klaster pomoću programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md)
- [Slijedite servisa tkanina tečaj da biste dobili općenite Uvod u platforme](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Servis tkanina kampanje stranice"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "DODAVANJE VEZE ZA VANJSKIH RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Dodavanje veze za VANJSKIH 2015 WebPI veze"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI veza"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Temeljni SDK WebPI veze"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
