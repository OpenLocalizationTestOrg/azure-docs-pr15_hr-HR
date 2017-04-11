<properties 
    pageTitle="Implementacija QuickBooks Azure RemoteApp | Microsoft Azure" 
    description="Saznajte kako omogućiti zajedničko korištenje QuickBooks Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Kako implementirati QuickBooks RemoteApp Azure

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Da biste zajednički koristili QuickBooks kao aplikaciju Azure RemoteApp poslužite se sljedećim informacijama.


QuickBooks 2015 Enterprise možete zajednički koristiti s Azure RemoteApp u zbirci hibridnog ili oblačić. Datoteku tvrtke mora se nalaziti na VM sa sustavom QuickBooks poslužitelj baze podataka koji razlikuje se od poslužitelje Azure RemoteApp. Nikad ne spremiti datoteku tvrtke na sliku Azure RemoteApp – gubitka podataka ubuduće se očekuje ako to učinite. Samo QuickBooks Enterprise podržava hosting QuickBooks datoteke na vanjski zajedničko korištenje s poslužiteljem baze podataka QuickBooks dostupnim putem mreže standardne Windows.   

> [AZURE.IMPORTANT] Poslužitelj baze podataka za QuickBooks koji se nalazi datoteku tvrtke mora se nalaziti na zasebnom VM unutar iste VNET kao zbirku Azure RemoteApp.  

## <a name="steps-to-deploy-quickbooks"></a>Koraci za implementaciju QuickBooks

1. Stvaranje programa Azure VM i instalirajte QuickBooks, QuickBooks poslužitelj baze podataka, i postavite datoteku tvrtke na Azure VM.  Provjerite jeste li ispravno konfigurirati pravila vatrozida.
2. Instalirajte QuickBooks na [prilagođenu sliku](remoteapp-imageoptions.md) i stvorite [zbirku Azure RemoteApp](remoteapp-collections.md), oblaka ili hibridnog u točno istom VNET gdje se nalazi VM hosting QuickBooks poslužitelj baze podataka s datotekama tvrtke. 
3.  [Objavljivanje](remoteapp-publish.md) QuickBooks aplikacija korisnicima
4.  Azure RemoteApp hostira QuickBooks klijenta, kretanje pomoću standardne Windows mreže mogu pridonijeti VM hosting QuickBooks poslužitelj baze podataka i otvorite datoteku tvrtke. 

## <a name="documentation-references"></a>Dokumentacija reference

- QuickBooks [podržani konfiguracija](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [Mogućnosti implementacije](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Možete odjaviti i Ignite prezentacija, [Osnove sustava Microsoft Azure RemoteApp Management i administracije](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) – brzi prijelaz naprijed na 1:02:45 da biste pristupili dio QuickBooks.

## <a name="deployment-architecture"></a>Arhitektura implementacije

![QuickBooks + Azure RemoteApp implementacije](./media/remoteapp-quickbooks/ra-quickbooks.png)