<properties
   pageTitle="Početak rada s implementacija i nadogradnje aplikacije na vašem lokalnom klaster | Microsoft Azure"
   description="Postavljanje lokalnog servisa tkanina klaster, uvođenje postojeće aplikacije da biste ga i nadogradnja te aplikacije."
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
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Početak rada s implementacija i nadogradnje aplikacije na vašem lokalnom klaster
Tkanina SDK-a servisa Azure sadrži potpuno lokalne razvojno okruženje koje možete koristiti za brzi početak rada s uvođenje i upravljanje aplikacijama na lokalni klaster. U ovom se članku Stvaranje lokalne klaster, uvođenje postojeće aplikacije da biste ga i nadogradnja tu aplikaciju za novu verziju, sve iz komponente Windows PowerShell.

> [AZURE.NOTE] U ovom se članku pretpostavlja da ste već [postavili okruženje za razvoj](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Stvaranje lokalne klaster
Servis tkanina klaster predstavlja skup hardver resursa koje možete implementirati aplikacijama. Obično klaster sastoji se od bilo kojeg mjesta od pet mnogo tisućama slika s računala. Međutim, SDK tkanina servisa sadrži klaster konfiguracije koji se može izvoditi na jednom računalu.

Važno da biste shvatili da klaster lokalne tkanina servis nije emulator ili simulator je. Pokreće se isti platformu kod koji se nalazi na više strojne grupe klastere. Jedina razlika je da se izvodi procesa platformi koje obično dodijeliti većem pet strojeva na jednom računalu.

SDK-a nudi dva načina da biste postavili lokalne klaster: skripte komponente Windows PowerShell i aplikaciju za palete sustava Upravitelja lokalnih klaster. U ovom ćete praktičnom vodiču koristimo skriptu PowerShell.

> [AZURE.NOTE] Ako ste već stvorili lokalnu klaster Uvođenjem aplikacije Visual Studio, možete preskočiti ovaj odjeljak.


1. Novi prozor PowerShell pokrenite kao administrator.

2. Da biste pokrenuli instalacijski program skriptu klaster iz mape SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Postavljanje klaster vodi trenutak. Kada se instalacija završi, trebali biste vidjeti izlaz slično:

    ![Izlaz postavljanje klaster][cluster-setup-success]

    Sada ste spremni pokušati implementacija aplikaciju za svoj klaster.

## <a name="deploy-an-application"></a>Implementacija aplikacije
Tkanina SDK servisa sadrži bogatog skupa okviri i tooling za stvaranje aplikacija za razvojne inženjere. Ako vas zanima učenjem kako se stvaraju aplikacije u Visual Studio, potražite u članku [Stvaranje prve tkanina servisa aplikacija u Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

U ovom ćete praktičnom vodiču koristit ćemo postojeće aplikacije uzorka (pod nazivom WordCount) tako da se ne možemo možete se fokusirati na upravljanje aspekte platformu – uključujući implementaciju, praćenje i nadogradnja.


1. Novi prozor PowerShell pokrenite kao administrator.

2. Uvoz modul ljuske PowerShell za SDK tkanina za servis.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Stvaranje direktorija za pohranu aplikacije koji preuzeti i implementaciju, kao što su C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [Preuzimanje aplikacije WordCount](http://aka.ms/servicefabric-wordcountapp) mjesto koji ste stvorili.  Napomena: preglednika Microsoft Edge sprema datoteku s nastavkom *.zip* .  Promijenite naziv datotečnog nastavka *.sfpkg*.

5. Povezivanje s lokalnom klaster:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Stvaranje nove aplikacije pomoću naredbe za implementaciju u SDK s imenom i put do Aplikacijski paket.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Ako je sve dobro prošlo, trebali biste vidjeti izlaz kao što je sljedeća:

    ![Implementacija aplikacije lokalne klaster][deploy-app-to-local-cluster]

7. Da biste vidjeli aplikaciju u praksi, pokrenite preglednik i idite na [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Trebali biste vidjeti:

    ![Distribuiranih aplikaciju korisničkog Sučelja][deployed-app-ui]

    Aplikacija WordCount je vrlo jednostavne. Obuhvaća klijentsko JavaScript kod da biste generirali slučajni pet znakova "riječi", koji se zatim povezivati s aplikacijom putem API servisa platforme ASP.NET Web. S praćenjem stanja servisa prati broj riječi koje se broje. Oni imaju na temelju prvog znaka u programu word. Izvorni kod možete pronaći aplikacije WordCount u [Uvod uzorka](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    Aplikacije koje ćemo uvesti sadrži četiri particije. Da bi se riječi koje počinju od A do G spremaju se u prvom particije, riječi koje počinju H do N spremaju se u drugom particija i tako dalje.

## <a name="view-application-details-and-status"></a>Prikaz detalja iz aplikacije i status
Nakon što smo uveli aplikaciju, pogledajmo neke detalje aplikacije u ljusci PowerShell.

1. Upit sve distribuiranih aplikacije na skupine:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Uz pretpostavku da ste uvesti samo aplikaciju WordCount, vidjet ćete nešto nalik na:

    ![Upit sve distribuiranih aplikacije komponente PowerShell][ps-getsfapp]

2. Idite na sljedeću razinu postavljanjem upita skup servise uvrštene u aplikaciji WordCount.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Popis servisa za aplikaciju u ljusci PowerShell][ps-getsfsvc]

    Aplikacija sastoji se od dva servisa – web-sučelja i s praćenjem stanja servisa koja upravlja riječi.

3. Na kraju, pogledajte popis razdjeljivanja za WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Prikaz particija servisa u ljusci PowerShell][ps-getsfpartitions]

    Skup naredbi koje ste koristili, kao što su sve naredbe ljuske PowerShell tkanina servisa, dostupni su za sve klaster koje možete se povezati s lokalnom ili udaljenom.

    Više vizualne način za interakciju s klaster možete koristiti alat za koji se temelji na web servisa tkanina Explorer tako da odete [http://localhost:19080/Explorer](http://localhost:19080/Explorer) u pregledniku.

    ![Prikaz detalja o aplikacije u programu Explorer tkanina servisa][sfx-service-overview]

    > [AZURE.NOTE] Dodatne informacije o servisa tkanina Explorer potražite u članku [vizualizacija svoj klaster pomoću programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Nadogradnja aplikacije
Servis tkanina nudi nadogradnje bez nedostupnost prateći stanja aplikacije kao što je kumulira preko klaster. Pogledajmo izvrši jednostavne nadogradnju aplikacije WordCount.

Novu verziju aplikacije sada broji samo riječi koje počinju s znak za samoglasnik. Tijekom nadogradnje kumulira, dobivamo dva promjene u odjeljku postavke aplikacije. Najprije stopa rastom Brojanje treba usporiti, budući da se broje se manje riječi. Drugo, budući da prvi particija ima dvije samoglasnike (A "i" E "), a sve particije sadrže samo jedan svaki, njegov count naposljetku trebala da biste outpace na druge.

1. [Preuzmite paket v2 WordCount](http://aka.ms/servicefabric-wordcountappv2) na isto mjesto na kojem ste preuzeli paket v1.

2. Vratite prozor programa PowerShell i pomoću naredbe za nadogradnju u SDK da biste registrirali novu verziju u klasteru. Početak nadogradnje na tkanina: / WordCount aplikacije.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Trebali biste vidjeti Izlaz u ljusci PowerShell koji počinje izgleda otprilike ovako kao nadogradnje.

    ![Tijek nadogradnje u ljusci PowerShell][ps-appupgradeprogress]

3. Dok je nadogradnja nastavka, možda ga pronaći olakšava praćenje njezin status iz programa Explorer tkanina servisa. Pokretanje prozoru preglednika, a zatim otvorite [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Proširite **aplikacije** u stabla na lijevoj strani, a zatim odaberite **WordCount**i na kraju **tkanina: / WordCount**. Na kartici osnove vidjet ćete stanja nadogradnje kao nastavlja se kroz na klaster nadogradnje domene.

    ![Tijek nadogradnje u programu Explorer tkanina servisa][sfx-upgradeprogress]

    Tijekom nadogradnje nastavlja se kroz svaku domenu, provjere stanja se izvode da biste bili sigurni da su aplikacije ispravno ponaša.

4. Ako ponovno pokrenite starije upit za postavljanje servisa u na tkanina: / WordCount aplikacije, Uočite da verzija WordCountService promijenjena, ali verziju WordCountWebService nije:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Upit aplikacijske usluge nakon nadogradnje][ps-getsfsvc-postupgrade]

    Ističe način na koji servis tkanina upravlja nadogradnje aplikacije. Ga koji dodiruju samo skup servisa (ili kod/konfiguracije paketa unutar tih servisa) koje su se promijenile, što omogućuje postupka nadogradnje brže i više pouzdane.

5. Na kraju, vratite se u pregledniku da biste pridržavajte se ponašanje novu verziju aplikacije. Kako treba, nastavlja na broj sporije i prvi particija završava s malo više glasnoću.

    ![Prikaz nove verzije aplikacije u pregledniku][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Čišćenje

Prije nego kraj, dobro je važno Imajte na umu da je lokalni klaster real. Aplikacije i dalje se izvoditi u pozadini dok ih uklonite.  Ovisno o prirode aplikacije, pokrenute aplikacije mogu zauzeti značajan resursa na vašem računalu. Imate nekoliko mogućnosti za upravljanje aplikacijama i skupine:

1. Da biste uklonili pojedinačnih aplikacija i sve njegove podatke, pokrenite sljedeću naredbu:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Ili brisanje aplikacije iz izbornika **AKCIJE** za servis tkanina Explorer ili na kontekstnom izborniku u prikazu popisa aplikacija u lijevom oknu.

    ![Brisanje aplikacije je servis tkanina Explorer][sfe-delete-application]

2. Nakon brisanja aplikacije iz skupine, možete odjaviti verzije 1.0.0 i 2.0.0 vrste WordCount aplikacije. Brisanje uklanja paketa aplikacije, uključujući kod i konfiguracija iz trgovine u klaster slika.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Ili u programu Explorer tkanina servisa, odaberite **Vrstu Oslobađanje resursa** za aplikaciju.

3. Da biste isključiti klaster, no zadržali podatke aplikacije i kašnjenja, kliknite **Zaustavi lokalne klaster** u aplikaciji palete sustava.

4. Da biste izbrisali klaster potpuno, kliknite **Uklanjanje lokalnog klaster** u aplikaciji palete sustava. Neka druga spora implementacija sljedeći put pritisnite F5 u Visual Studio rezultirat će tu mogućnost. Uklanjanje lokalnog klaster samo ako ne namjeravate koristiti za neko vrijeme ili ako je potrebno da biste oslobodili resurse.

## <a name="1-node-and-5-node-cluster-mode"></a>1 čvor i 5 način čvor klaster

Kada radite s lokalnom klaster za razvoj aplikacija, koje često obavljat brzi iteracija pisanja koda za ispravljanje pogrešaka, promjena kod, ispravljanje pogrešaka itd. Da biste lakše optimizirati taj postupak, lokalni klaster možete pokrenuti na dva načina: 1 čvor ili 5 čvor. Oba načina klaster imati njihove prednosti.
5 čvor klaster način omogućuje rad s realni klaster. Možete testirati scenariji za prebacivanje, rad s više instanci i replike servisa.
1 način klaster čvor optimiziran je za brze implementacije i registracija servisa da biste lakše brzu provjeru valjanosti kod korištenjem runtime tkanina servisa.

1 čvor skupine način i 5 način klaster čvor nije emulator ili simulator. Pokreće se isti platformu kod koji se nalazi na više strojne grupe klastere.

> [AZURE.NOTE] Ta je značajka dostupna u SDK verziju 5,2 i iznad.

Da biste promijenili način klaster 1 čvor klaster, koristite Upravitelj klaster lokalne tkanina servisa ili pomoću komponente PowerShell na sljedeći način:

1. Novi prozor PowerShell pokrenite kao administrator.

2. Da biste pokrenuli instalacijski program skriptu klaster iz mape SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Postavljanje klaster vodi trenutak. Kada se instalacija završi, trebali biste vidjeti izlaz slično:
    
    ![Izlaz postavljanje klaster][cluster-setup-success-1-node]

Ako koristite Upravitelj klaster lokalne tkanina servisa:

![Način rada za promjenu klaster][switch-cluster-mode]

> [AZURE.WARNING] Kada promijenite način klaster, trenutni Klaster se uklanja iz sustava i stvara novi klaster. Podaci koji se moraju koje ste spremili na klaster će se izbrisati kada promijenite način klaster.

## <a name="next-steps"></a>Daljnji koraci
- Sad kad ste implementiran i nadograditi neke ugrađene aplikacije, možete ga [pokušajte Izrada vlastite u Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
- Na programa [Azure klaster](service-fabric-cluster-creation-via-portal.md) te mogu izvršavati sve akcije izvršiti na lokalni klaster u ovom članku.
- Nadogradnja koji smo izvesti u ovom članku je osnovni. Pogledajte na [nadogradnju dokumentaciju](service-fabric-application-upgrade.md) da biste saznali više o Fleksibilno nadogradnje servisa tkanina.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
