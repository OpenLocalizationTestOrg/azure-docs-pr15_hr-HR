<properties
    pageTitle="Upravljanje aplikacijama servisa tkanina automatizirati pomoću komponente PowerShell | Microsoft Azure"
    description="Implementacija nadogradnje, testirajte i uklanjanje servisa tkanina aplikacije pomoću komponente PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatiziranje životni ciklus aplikacije pomoću komponente PowerShell

Moguće je automatizirati brojne aspekte [životni ciklus tkanina servisa aplikacija](service-fabric-application-lifecycle.md) .  U ovom se članku objašnjava Automatizacija uobičajenih zadataka za implementaciju, nadogradnje, uklanjanje i testiranje aplikacije tkanina servisa Azure pomoću komponente PowerShell.  Upravljani i HTTP API-ji za upravljanje aplikacije dostupne su i. [Životni ciklus aplikacije](service-fabric-application-lifecycle.md) za dodatne informacije potražite u članku.  

## <a name="prerequisites"></a>Preduvjeti
Prije nego što se premjestili zadacima u članku, ne zaboravite:

+ Upoznavanje s konceptima servisa tkanina opisane u [Tehnički pregled tkanina servisa](service-fabric-technical-overview.md).
+ [Instalacija izvođenja, SDK i alate](service-fabric-get-started.md), koji se instalira i modul ljuske PowerShell **ServiceFabric** .
+ [Izvršavanje skriptu PowerShell omogućiti](service-fabric-get-started.md#enable-powershell-script-execution).
+ Pokrenite lokalne klaster.  Pokrenite novi prozor PowerShell kao administrator i pokrenuti skriptu postavljanje klaster iz mape SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Prije pokretanja naredbe ljuske PowerShell u ovom članku, najprije povezati s lokalnog servisa tkanina klaster pomoću [**Povezivanje ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ Sljedeće zadatke obavezan v1 Aplikacijski paket za implementaciju i v2 Aplikacijski paket za nadogradnju. Preuzmite [ **WordCount** primjer aplikacije](http://aka.ms/servicefabricsamples) (koja se nalazi u uzorcima početak rada). Stvaranje i paket aplikacije u Visual Studio (desnom tipkom miša kliknite **WordCount** u pregledniku rješenja i odaberite **paket**). Kopirajte paket v1 u `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` da biste `C:\Temp\WordCount`. Kopiraj `C:\Temp\WordCount` da biste `C:\Temp\WordCountV2`, stvaranje v2 Aplikacijski paket za nadogradnju. Otvaranje `C:\Temp\WordCountV2\ApplicationManifest.xml` u uređivaču teksta. Element **ApplicationManifest** promijeniti atribut **ApplicationTypeVersion** s "1.0.0" u "2.0.0" da biste ažurirali broj verzije aplikacije. Spremite datoteku promijenjene ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Zadatak: Implementacija tkanina servisa aplikacija

Nakon što ste ugrađen i pakirat aplikacije (ili preuzeli Aplikacijski paket), možete implementirati aplikaciju u lokalnom klaster tkanina servisa. Uvođenje uključuje prijenos Aplikacijski paket, Registracija vrsta aplikacije i stvaranja instance aplikacije. Poslužite se uputama u ovom odjeljku za implementaciju novu aplikaciju za klaster.

### <a name="step-1-upload-the-application-package"></a>Korak 1: Prijenos paketa aplikacije
Prijenos paketa aplikacije u trgovini slika stavlja na mjestu pristupačnog interni tkanina servisa komponente.  Aplikacijski paket sadrži potrebne programski manifest, manifesti servisa i kod, konfiguracije i pakete podataka da biste stvorili aplikacija i servisa instance.  Naredba [**Kopiraj ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) prenosi paketa. Ako, na primjer:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Korak 2: Registrirati vrsta aplikacije
Registracija Aplikacijski paket olakšava vrsta aplikacije i verziju deklarirani u manifestu aplikacije dostupne za korištenje. Sustav čita paketa prenijeli u okviru korak 1, provjerite je li paket (jednaku vrijednost za [**Testiranje ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) izvodi lokalno), obradu sadržaj paketa i kopirajte obrađeni paket na mjesto Interna sustava.  Pokrenite cmdlet [**Register ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCount
```
Da biste vidjeli sve vrste aplikacija registrira u klaster, pokrenite cmdlet [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Korak 3: Stvaranje instanci aplikacije
Aplikaciju možete se instancirati pomoću bilo koja verzija vrsta aplikacije registrirani uspješno pomoću naredbe [**Nova ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) . Naziv svaku aplikaciju deklarirane pri implementacija vrijeme i mora započeti s na **tkanina:** shema i biti jedinstvena za svaku instancu aplikacije. Naziv vrste aplikacije i verzija aplikacije vrsta prijavljen je na **ApplicationManifest.xml** datoteka paketa aplikacije. Ako su zadane servisi definiran u programski manifest vrsta ciljne aplikacije, oni se stvaraju trenutno.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

[**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) -command dohvaća sve instance aplikacije stvorene uspješno, zajedno sa svojim ukupnog statusa. [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) command dohvaća sve instance servisa uspješno stvorenih unutar danog aplikacije instance. Navedene su zadane servise (ako ih ima).

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Zadatak: Nadogradnja tkanina servisa aplikacija
Možete nadograditi prethodno distribuiranih servisa tkanina aplikacijom paketa za aplikaciju. Ovaj zadatak nadograđuje WordCount aplikacije koji je uveden u "zadatak: implementaciju aplikacija za servis tkanina." Da biste saznali više, pročitajte putem [aplikacije servisa tkanina nadogradnju](service-fabric-application-upgrade.md) .

Da biste zadržali stvari jednostavne u ovom primjeru, samo broj verzije aplikacije je ažuriran u Aplikacijski paket WordCountV2 stvorene u preduvjete. Realističniji scenarij bi obuhvaćati ažuriranja servisa kod, konfiguriranje ili podatkovnih datoteka i ponovno stvaranje i pakiranje aplikacije s brojevima ažuriranu verziju.  

### <a name="step-1-upload-the-updated-application-package"></a>Korak 1: Prijenos ažurirane aplikacije paketa
Aplikacija v1 WordCount je spremna za nadogradnju. Ako otvorite prema gore u prozoru ljuske PowerShell kao administrator i vrsta [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx), vidjet ćete je implementiran tu verziju 1.0.0 vrsta aplikacije WordCount.  

Sada kopirajte paket ažurirane aplikacije u trgovini slika tkanina servisa (paketa aplikacije se pohranjuju po tkanina servisa). Parametar **ApplicationPackagePathInImageStore** obavještava servisa tkanina gdje možete pronaći Aplikacijski paket. Sljedeća naredba kopira Aplikacijski paket **WordCountV2** u spremištu slike:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Korak 2: Registrirati vrsta ažurirane aplikacije
Sljedeći je korak da biste registrirali novu verziju aplikacije pomoću servisa tkanina, što se može izvršiti pomoću cmdleta [**Register ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Korak 3: Pokretanje nadogradnje
Različite nadogradnje parametara, vremenska ograničenja i stanje kriterijima se mogu primijeniti na aplikaciju nadogradnje. Pročitajte [parametara za nadogradnju aplikacije](service-fabric-application-upgrade-parameters.md) i [postupak nadogradnje](service-fabric-application-upgrade.md) dokumenata da biste saznali više. Sve servise i instance mora biti _dobar_ nakon nadogradnje.  Postavljanje **HealthCheckStableDuration** do 60 sekundi (tako da se usluge su dobar barem 20 sekundi prije nadogradnje nastavlja se na sljedeći nadogradnje domenu).  Postaviti **UpgradeDomainTimeout** 1200 sekundi i **UpgradeTimeout** 3000 sekundi. Na kraju, postavite **UpgradeFailureAction** **Vraćanje**, koji se zatraži da tkanina servisa će vratiti aplikacije na prethodnu verziju u slučaju pogreške su tijekom nadogradnje.

Sada možete započeti nadogradnju aplikacije pomoću cmdleta [**Start ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) :

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Imajte na umu da naziv aplikacije isto kao prethodno distribuiranih v1.0.0 naziv aplikacije (tkanina: / WordCount). Servis tkanina koristi taj naziv da biste odredili koja će se aplikacija početak nadogradnje. Ako ste postavili istek biti premalen, mogli biste naići poruku pogreške isteka da problem. Pogledajte [nadogradnje aplikacije za otklanjanje poteškoća s](service-fabric-application-upgrade-troubleshooting.md)ili povećati na istek.

### <a name="step-4-check-upgrade-progress"></a>Korak 4: Potvrdite nadogradnje tijeku
Možete nadzirati tijek nadogradnje aplikacije pomoću [Programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md)ili pomoću cmdleta [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) :

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

U nekoliko minuta cmdlet [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) prikazuje da su sve nadogradnje domene nadograditi (gotov).

## <a name="task-test-a-service-fabric-application"></a>Zadatak: Testiranje tkanina servisa aplikacija

Da biste napisali services visoke kvalitete, razvojni inženjeri morat ćete moći induce kvarove nepouzdanih infrastrukture za testiranje stabilnosti svoje usluge. Servis tkanina omogućuje razvojnim inženjerima induce kvara akcije i testiranje servisa x. neuspjeha pomoću scenariji test za chaos i prebacivanje.  Dodatne informacije pročitajte [Pregled Testability](service-fabric-testability-overview.md) .

### <a name="step-1-run-the-chaos-test-scenario"></a>Korak 1: Pokrenite test scenarij chaos
Scenarij test chaos generira kvarove preko cijele klaster tkanina servisa. Scenarij komprimira kvarove obično vidjeti putem mjeseci ili godina do nekoliko sati. Kombinacija interleaved kvarove pomoću tečaja visoke kvara pronalazi kut slučajeva koje bi inače Propušteni. U sljedećem primjeru scenarija test chaos traje 60 minuta.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Korak 2: Pokrenite test scenarij prebacivanje
Za prebacivanje testirajte scenarij ciljnih web-mjesta servisa za pojedine particije umjesto čitava grupa i ostale servise ostavlja ostaju isti. Scenarij zadanu ponovno izračunava kroz slijed Simulirani kvarove u provjeri valjanosti servisa tijekom poslovne logike. Pogreške u provjeri valjanosti servisa pokazuje problem koji je potrebno dodatno istrage. Prebacivanje test induces samo jedan kvara odjednom, umjesto test scenarij chaos koje možete induce više kvarove. U sljedećem primjeru pokreće prebacivanje test za 60 minuta odnosu na tkanina: / WordCount/WordCountService servisa.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Zadatak: Uklanjanje tkanina servisa aplikacija
Možete izbrisati instance komponente distribuiranih aplikacije, ukloniti vrsta dodijeljenu aplikacije iz skupine i uklanjanje Aplikacijski paket na ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Korak 1: Uklanjanje instance aplikacije
Kada aplikacija instance više nije potrebna, pomoću cmdleta [**Ukloni ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) trajno možete ukloniti ga. Time se automatski sve servise koji pripadaju aplikacije, trajno uklanjanje svih stanje servisa. Ovaj postupak ne može poništiti, a zatim stanja računala nije moguće oporaviti.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Korak 2: Unregister vrsta aplikacije
Kada više ne trebate određenu verziju vrstu aplikacije, odjaviti ga pomoću cmdleta [**Neregistriranja ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) . Poništavanje registriranja Neiskorišteni vrste izdaje prostora za pohranu koriste paketa aplikacije u trgovini slike. Vrstu aplikacije može biti registriran pod uvjetom da nema aplikacija instancirati slučajeve vezane uz njega ili na čekanju nadogradnje aplikacije ga referencira.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Korak 3: Uklanjanje paketa aplikacije
Kada je vrsta aplikacije nije registriran, Aplikacijski paket moguće je ukloniti iz trgovine slika pomoću cmdleta [**Ukloni ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) .

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Daljnji koraci
[Životni ciklus tkanina servisa aplikacija](service-fabric-application-lifecycle.md)

[Implementacija aplikacije](service-fabric-deploy-remove-applications.md)

[Nadogradnja aplikacije](service-fabric-application-upgrade.md)

[Azure servisa tkanina cmdleta](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure servisa tkanina testability cmdleta](https://msdn.microsoft.com/library/azure/mt125844.aspx)
