<properties
   pageTitle="Ispravljanje pogrešaka aplikacije u Visual Studio | Microsoft Azure"
   description="Poboljšati pouzdanost i performanse servisa razvoja i pogrešaka ih u Visual Studio na lokalni razvoj klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Ispravljanje pogrešaka aplikacije usluge tkanina pomoću Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Lokalni program tkanina servisa za ispravljanje pogrešaka

Uštedjet ćete vrijeme i novac implementacija i ispravljanje pogrešaka aplikacije tkanina servisa Azure na lokalnom računalu razvoj. Visual Studio možete implementirati aplikaciju lokalne klaster i automatski crtom povezati program za ispravljanje pogrešaka da biste sve instance aplikacije.

1. Pokrenite lokalne razvoj klaster tako da slijedite korake u [odjeljku Postavljanje okruženje za razvoj tkanina servisa](service-fabric-get-started.md).

2. Pritisnite **F5** ili kliknite **ispravljanje pogrešaka** > **Pokrenuti ispravljanje pogrešaka**.

    ![Pokretanje aplikacije za ispravljanje pogrešaka][startdebugging]

3. Prekidne točke u kod i koraka pomoću aplikacije možete postaviti tako da kliknete naredbi na izborniku **za ispravljanje pogrešaka** .

    > [AZURE.NOTE] Visual Studio pridružuje sve instance aplikacije. Dok ste vodi kod, prekidne točke možda se ne pronalazi više procesa rezultira Istodobni sesije. Onemogućite na prekidne točke kada ih se pronalaženja objekata, tako da svaki točku prekida uvjetno ID niti ili pomoću dijagnostičkih događaja.

4. Prozor **Dijagnostičkih događaja** će automatski otvoriti da bi mogli vidjeti dijagnostičkih događaja u stvarnom vremenu.

    ![Prikaz dijagnostičkih događaja u stvarnom vremenu][diagnosticevents]

5. Prozor **Dijagnostičkih događaja** možete otvoriti i u programu Explorer oblaka.  U odjeljku **Servis tkanina**, desnom tipkom miša kliknite bilo koju čvor, a zatim odaberite **Prikaz strujanje kašnjenja**.

    ![Otvorite prozor dijagnostičkih događaja][viewdiagnosticevents]

    Ako želite filtrirati na kašnjenja određenog servisa ili aplikacije jednostavno omogućiti strujanje kašnjenja na tom određenog servisa ili aplikacije.

6. Dijagnostičkih događaja mogu vidjeti u datoteci automatski generirani **ServiceEventSource.cs** i nazivaju se iz aplikacije koda.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Prozor **Dijagnostičkih događaja** podržava filtriranje, zadržite pokazivač miša i pregled događaja u stvarnom vremenu.  Filtar je pretraživanje jednostavne niz događaja poruke, uključujući njegov sadržaj.

    ![Filtriranje, zadržite pokazivač i nastavite ili provjera događaja u stvarnom vremenu][diagnosticeventsactions]

8. Ispravljanje pogrešaka usluge je kao što su druge aplikacije za ispravljanje pogrešaka. Postavit obično prekidne točke do Visual Studio za jednostavno ispravljanje pogrešaka. Iako pouzdanog zbirke replicirati preko više čvorove, oni i dalje implementirati IEnumerable. To znači da koristite prikaz rezultata u Visual Studio tijekom ispravljanje pogrešaka da biste vidjeli što ste pohranili unutar. Jednostavno postavite na točku prekida bilo gdje u kodu.

    ![Pokretanje aplikacije za ispravljanje pogrešaka][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Udaljenog računala tkanina servisa za ispravljanje pogrešaka

Ako aplikacija servisa tkanina su pokrenuti na servis tkanina klaster u Azure, vam se za daljinsko te, izravno iz Visual Studio ispravljanje pogrešaka.

> [AZURE.NOTE] Značajka zahtijeva [servisa tkanina SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) i [Azure SDK za .NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Daljinsko uklanjanje programskih pogrešaka je namijenjena razvojni i testiranje scenarijima za, a ne može koristiti u radnog okruženja, zbog utjecaj na pokrenute aplikacije.

1. Pomaknite se na svoj klaster u **Programu Explorer oblaka**, desnom tipkom miša kliknite pa odaberite **Omogući ispravljanje pogrešaka**

    ![Omogućivanje udaljene ispravljanje pogrešaka][enableremotedebugging]

    To će započnite-proces omogućivanja udaljene proširenja za ispravljanje pogrešaka na vaše čvorove klasteru potrebna mrežnim konfiguracijama.

2. Desnom tipkom miša kliknite čvor klaster u **Programu Explorer oblaka**, a zatim odaberite **Priloži alat za ispravljanje pogrešaka**

    ![Prilaganje ispravljanje pogrešaka][attachdebugger]

3. U dijaloškom okviru **prilaganje za obradu** odaberite postupak koji želite ispraviti pogreške, a zatim kliknite **Dodaj privitak**

    ![Odaberite postupak][chooseprocess]

    Naziv postupka koju želite priložiti, jednako je naziv naziv skupa projekta za servis.

    Program za ispravljanje pogrešaka priložiti će sve čvorove izvođenje postupka.
    - U slučaju gdje su pogrešaka bez praćenja stanja servisa, sve instance servisa na sve čvorove su dio sesije ispravljanja pogrešaka.
    - Ako su pogrešaka s praćenjem stanja servisa, samo primarni replike sve particije bit će aktivne i stoga prepoznate program za ispravljanje pogrešaka. Primarni replike premješta sesiji ispravljanje pogrešaka, obrada te replike i dalje biti dio sesije ispravljanja pogrešaka.
    - Da bi se samo Uhvatite odgovarajući particije ili pojavljivanja na servisu, možete koristiti uvjetno prekidne točke samo prekinuti određene particija ili instance.

    ![Uvjetno prekidnu točku.][conditionalbreakpoint]

    > [AZURE.NOTE] Trenutno ne podržavamo klaster tkanina servisa s više instanci s istim nazivom izvršna servisa za ispravljanje pogrešaka.

4. Kada završite s ispravljanje pogrešaka aplikacije, onemogućivanje udaljene proširenja za ispravljanje pogrešaka klaster u **Oblak Explorer** desnom tipkom miša i odaberite **Onemogući ispravljanje pogrešaka**

    ![Onemogućite udaljene ispravljanje pogrešaka][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Strujanje kašnjenja iz čvor udaljene klaster

Ćete također mogu strujanje kašnjenja izravno iz udaljene klaster čvor za Visual Studio. Ova značajka omogućuje praćenje događaja ETW strujanje, Proizvodi na servis tkanina čvor klaster, izravno u Visual Studio.

> [AZURE.NOTE] Značajka zahtijeva [servisa tkanina SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) i [Azure SDK za .NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Strujanje kašnjenja je namijenjena razvojni i testiranje scenarijima za, a ne može koristiti u radnog okruženja, zbog utjecaj na pokrenute aplikacije.
> U scenariju s radnog potrebno je za prosljeđivanje događaja korištenjem dijagnostike Azure.

1. Pomaknite se na svoj klaster u **Programu Explorer oblaka**, desnom tipkom miša pa odaberite **Omogući strujanje kašnjenja**

    ![Omogućivanje udaljene strujanje kašnjenja][enablestreamingtraces]

    To će započnite-proces omogućivanja strujanje proširenje kašnjenja na vaše čvorove klasteru potrebna mrežnim konfiguracijama.

2. Proširite **čvorove** elementa u **Programu Explorer oblaka**, desnom tipkom miša kliknite čvor želite strujanje kašnjenja iz, a zatim odaberite **Prikaz strujanje kašnjenja**

    ![Prikaz udaljene strujanje kašnjenja][viewremotestreamingtraces]

    Ponovite korak 2 za proizvoljan broj čvorove obliku u kojem želite da biste vidjeli kašnjenja iz. Svaki strujanje čvorove prikazat će se u prozoru namjenski.

    Sada se možete vidjeti kašnjenja čuje tkanina usluge i usluge. Ako želite filtrirati događaja koji se prikazuje samo određenoj aplikaciji, ondje upišite naziv aplikacije u filtru.

    ![Prikaz strujanje kašnjenja][viewingstreamingtraces]

4. Kada ste gotovi strujanje kašnjenja iz svoj klaster, možete onemogućiti udaljene strujanje kašnjenja, tako da desnom tipkom miša kliknete klaster u **Programu Explorer oblaka** i odaberite **Onemogući strujanje kašnjenja**

    ![Onemogućivanje udaljene strujanje kašnjenja][disablestreamingtraces]

## <a name="next-steps"></a>Daljnji koraci

- [Testiranje servisa tkanina servisa](service-fabric-testability-overview.md).
- [Upravljanje tkanina servisa aplikacija u Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
