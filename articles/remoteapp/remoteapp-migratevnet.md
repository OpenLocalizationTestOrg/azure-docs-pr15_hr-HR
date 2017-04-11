<properties
    pageTitle="Kako migrirati iz RemoteApp VNET na programa Azure VNET | Microsoft Azure"
    description="Saznajte kako migrirati iz RemoteApp VNET u VNET programa Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Kako migrirati hibridnog zbirke iz RemoteApp VNET VNET programa Azure

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Dobre vijesti! Ne možemo ste omogućili da implementacija hibridnog RemoteApp zbirke izravno u postojeće Azure virtualne mreža (VNETs) umjesto stvaranja VNETs specifične za RemoteApp. Omogućuje vam da iskoristite prednost najnovije značajke VNET (kao što je ExpressRoute) i dati zbirke hibridnog pristupa izravnu drugih servisa Azure i virtualnim strojevima implementiran na tom VNET.  (To može vidjeti koje bolje performanse i lakše postavljanje od VNET VNET konfiguracije).


Recimo da ste već stvorili hibridnog RemoteApp zbirke naziva *OriginalCollection* s RemoteApp VNET pod nazivom *RemoteAppVNET*. Evo nekoliko koraka migriranja u novi Azure VNET pod nazivom *AzureVNET*.

1.  Na kartici **mreža** za [portal za upravljanje](http://manage.windowsazure.com/)stvorite VNET naziva *AzureVNET*, koristi isto mjesto, DNS konfiguraciju i prostor adrese (za barem jedno od podmreže *AzureVNET* ) kao što je koristiti za *RemoteAppVNET*.
2.  Konfiguriranje *AzureVNET* ili glavno računalo ili nemate mrežne veze radi implementacije servisa Active Directory *OriginalCollection* je domena koje su se pridružite.
3.  Na kartici **RemoteApps** stvorite novu zbirku RemoteApp naziva *Nove zbirke*. (Koristi mogućnost **Stvori s VNET** , nije moguće **Brzo stvoriti**).
3.  Konfiguriranje *NewCollection* uvesti podmreže u *AzureVNET*.
4.  Konfiguriranje *NewCollection* da biste upotrijebili iste slike i domene Uključi informacije koje se koriste za *OriginalCollection*.
5.  Nakon nekoliko sati *NewCollection* prikazat će se na popisu zbirke aktivni stanjem.

Sada, ako ne trebate migrirati sve korisničke informacije iz izvorne zbirke u novu zbirku, učiniti sljedeće korake:

6.  Brisanje *OriginalCollection*.
7.  Brisanje *RemoteAppVNET*.

I gotovo!

Također, ako je potrebno migrirati korisničke informacije iz izvorne zbirke u novu zbirku, učiniti sljedeće korake:

6.  Slanje e-pošte [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) s identifikacijskog Broja za Azure pretplatu, naziv izvornu zbirku i naziv nove zbirke i zamolite ga da biste migrirali korisničke informacije.
7.  2 dana tvrtke tima RemoteApp premjestit će access popis korisnika i sve dokumente korisnika i korisničkih postavki iz izvorne zbirke novu zbirku.
8.  Brisanje *OriginalCollection*.
9.  Brisanje *RemoteAppVNET*.

A sada gotovi ste!

Ako imate pitanja, ili posebne pomoć, pošaljite e-pošte [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
