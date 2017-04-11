<properties
    pageTitle="Korištenje Azure prostora za pohranu u aplikacijama iz Windows trgovine | Microsoft Azure"
    description="Upute za stvaranje aplikacija iz Windows trgovine koji koristi spremište blobova platforme Azure, reda čekanja, tablice ili datoteke."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Kako koristiti Azure prostora za pohranu u aplikacija iz Windows trgovine

## <a name="overview"></a>Pregled

Ovaj vodič pokazuje kako započeti s razvoj aplikacija iz Windows trgovine koji koristi Azure prostora za pohranu.

## <a name="download-required-tools"></a>Preuzimanje potrebni alati

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) olakšava stvaranje, ispravljanje pogrešaka, localize paket, i implementaciju aplikacija iz Windows trgovine. Potreban je Visual Studio 2012 ili noviji.
- [Azure prostora za pohranu klijenta biblioteke](https://www.nuget.org/packages/WindowsAzure.Storage) omogućuje Biblioteka klasa za Windows Runtime za rad s Azure prostora za pohranu.
- [WCF podataka servisa Alati za Windows trgovine aplikacija](http://www.microsoft.com/download/details.aspx?id=30714) proširuje na Dodavanje servisa Reference s podrškom za OData klijentske aplikacije iz Windows trgovine u Visual Studio.

## <a name="develop-apps"></a>Razvijajte aplikacije

### <a name="getting-ready"></a>Priprema

Stvaranje novog iz Windows trgovine app projekta u Visual Studio 2012 ili novijem:

![iz trgovine aplikacija – prostora za pohranu – Dodavanje veze za vanjskih-projekt][store-apps-storage-vs-project]

Zatim dodajte referenca klijentska biblioteka za pohranu Azure **reference**desnom tipkom miša kliknete **Dodaj referenca**i pregledavanje prostora za pohranu klijenta biblioteke za Windows Runtime koji ste preuzeli:

![iz trgovine aplikacijama-prostora za pohranu – odaberite-biblioteka][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Biblioteku pomoću servisa Blob i reda čekanja

Sada je aplikacija spremni blobova platforme Azure i red čekanja usluge. Dodajte sljedeće naredbe **pomoću** tako da se vrste Azure prostora za pohranu možete se pozivati izravno:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Zatim dodajte gumb na stranicu. Dodati sljedeći kod njegov **kliknite** događaj i izmjena načina rukovatelja događajima pomoću [ključnih riječi asinkrone](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Kod podrazumijeva da imate dva niza varijable, *accountName* i *accountKey*. Predstavljaju naziv računa za pohranu i ključ za račun koji je povezan s tog računa.

Stvaranje i pokretanje aplikacija. Klikom na gumb Provjerite postoji li kontejner *container1* na vašem računu i stvorite je ako nije.

### <a name="using-the-library-with-the-table-service"></a>Pomoću biblioteke sa servisom tablice

Vrste koje se koriste za komunikaciju sa servisom Azure tablice ovise o WCF Data Services za biblioteku aplikacija iz Windows trgovine. Nakon toga dodajte referenca potrebna WCF biblioteke pomoću konzole za Upravitelj paketa:

![iz trgovine aplikacijama-prostora za pohranu-paket-Upravitelj][store-apps-storage-package-manager]

Koristite sljedeću naredbu upravitelju paketa točke na mjesto na računalu:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Ta se naredba dodat će automatski sve potrebne reference u projekt. Ako ne želite koristiti konzolu za Upravitelj paketa, možete dodati WCF podataka servisa NuGet mapu na lokalnom računalu popis izvora paketa i zatim dodajte vodič kroz korisničko Sučelje, kao što je opisano u [Upravljanju NuGet paketa pomoću dijaloškog okvira](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Kada se pozivati WCF podataka servisa NuGet paketa, promijenite kod u vaš tipka **kliknite** događaj:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Kod provjerava je li tablicu nazvanu *tablica1* postoji na vašem računu i stvara je ako nije.

Možete dodati reference na Microsoft.WindowsAzure.Storage.Table.dll, koji je dostupan u istoj paketa koji ste preuzeli. Biblioteka sadrži dodatnim funkcijama, kao što su utemeljene na odraza serijalizacije i generički upita. Imajte na umu da ova biblioteka ne podržava JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
