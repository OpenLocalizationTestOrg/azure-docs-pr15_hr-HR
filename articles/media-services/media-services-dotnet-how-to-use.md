<properties 
    pageTitle="Kako postaviti računalo za Media Services razvoja s .NET" 
    description="Saznajte više o preduvjetima za Media Services pomoću Media Services SDK za .NET. I Saznajte kako stvoriti aplikaciju za Visual Studio." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Razvoj Media Services s .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

U ovoj se temi opisuje postupak da biste pokrenuli razvoja Media Services aplikacije koje se koriste .NET.

Biblioteka **Azure Media Services.NET SDK** omogućuje programu protiv Media Services pomoću .NET. Da biste lakše čak i za razvoj s .NET, u biblioteku **Azure Media Services .NET SDK proširenja** navedeni su. Biblioteka sadrži skup načina nastavka i funkcije preglednika koje će pojednostaviti .NET kod. Obje biblioteke su dostupne putem **NuGet** i **GitHub**.


##<a name="prerequisites"></a>Preduvjeti

-   S računom Media Services u novu ili postojeću pretplatu Azure. Potražite u temi [Stvaranje računa za servise medijske sadržaje](media-services-portal-create-account.md).
-   Operacijske sustave: Windows 10, Windows 7, Windows 2008 R2 ili Windows 8.
-   .NET framework 4,5.
-    Visual Studio 2015, Visual Studio 2013, Visual Studio 2012 ili Visual Studio 2010 SP1 (Professional, Premium, Ultimate ili Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Stvaranje i konfiguriranje projekta za Visual Studio

U ovom se odjeljku objašnjava stvaranje projekta u Visual Studio i postaviti za razvoj Media Services.  U ovom slučaju projekta je aplikacije konzole za Windows C#, no iste korake postavljanja ovi odnose na druge vrste projekata možete stvoriti za aplikacije Media Services (na primjer, aplikaciju za Windows obrasce ili ASP.NET Web-aplikacije).

U ovom se odjeljku pokazuje kako koristiti **NuGet** da biste dodali Media Services .NET SDK i drugih biblioteka zavisne.

Umjesto toga, možete dobiti najnovije bitova Media Services .NET SDK od GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) i [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), sastavljanje rješenja i dodati reference projekt klijenta. Imajte na umu potrebne ovisnosti se preuzimaju i izdvojene automatski.

1. Stvaranje nove C# konzole za aplikacije u Visual Studio 2010 SP1 ili novije verzije za dodavanje veze za VANJSKIH. Unesite **naziv**, **mjesto**i **naziv rješenja**, a zatim kliknite u redu.

2. Stvaranje rješenja.

2. Da biste instalirali i dodavanje **Azure Media Services .NET SDK proširenja**pomoću **NuGet** . Instalirate paket, instalira **Media Services.NET SDK** i dodaje ostale potrebne ovisnosti.

    Provjerite imate li najnovija verzija sustava NuGet instaliran. Dodatne informacije i instalaciju upute potražite u članku [NuGet](http://nuget.codeplex.com/).

2. U pregledniku rješenja, desnom tipkom miša kliknite naziv projekta, a zatim odaberite upravljanje NuGet paketa...

    Pojavit će se dijaloški okvir upravljanje NuGet paketa.

3. U galeriji Online tražiti Azure MediaServices proširenja, odaberite Azure Media Services .NET SDK proširenja i kliknite gumb Instaliraj.

    Mijenja se projekt i dodat će se reference na proširenja za SDK .NET Media Services, Media Services .NET SDK i ostalih zavisne sklopova.

4. Da biste povećali razinu čišćenje razvojno okruženje, razmislite o omogućivanju NuGet paket vratiti. Dodatne informacije potražite u članku [NuGet paketa vraćanje "](http://docs.nuget.org/consume/package-restore).

3. Dodavanje reference u sklopu **System.Configuration** . Ovaj skup sadrži na System.Configuration. **ConfigurationManager** klasa koja se koristi za dohvaćanje datoteka konfiguracije (na primjer, App.config).

    Da biste dodali reference pomoću dijaloškog okvira upravljajte reference, desnom tipkom miša kliknite naziv projekta u pregledniku rješenja. Zatim odaberite Dodaj i reference.

    Pojavit će se dijaloški okvir upravljanje reference.

4. U odjeljku .NET framework skupine, pronaći i odaberite u sklopu System.Configuration, a zatim pritisnite u redu.
5. Otvorite datoteku App.config (Dodavanje datoteke u projekt ako je dodana po zadanom) i dodajte odjeljak *appSettings* u datoteku.     
Postavljanje vrijednosti za servisa Azure Media Services imenom i računom ključ računa, kao što je prikazano u sljedećem primjeru.

    Da biste pronašli naziv i ključ vrijednosti, idite na portal za Azure i odaberite svoj račun. Pojavit će se prozor postavke na desnoj strani. U prozoru postavke odaberite tipke. Klikom na ikonu pokraj svake tekstni okvir kopira vrijednost u međuspremnik sustava.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Prebriši postojeće **pomoću** naredbe na početku datoteke Program.cs sljedeći kod.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

Sada ste spremni za početak razvoj aplikacija za Media Services.    


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
