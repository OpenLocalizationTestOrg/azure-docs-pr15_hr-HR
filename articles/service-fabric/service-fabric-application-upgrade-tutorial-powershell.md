<properties
   pageTitle="Nadogradnja aplikacije servisa tkanina pomoću komponente PowerShell | Microsoft Azure"
   description="U ovom se članku vodi kroz iskustva implementaciji servisa tkanina aplikacije, promjena kod i vodoravnim out nadogradnju pomoću komponente PowerShell."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="service-fabric-application-upgrade-using-powershell"></a>Nadogradnja aplikacije za tkanina servis pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Najčešće korištene i preporučena nadogradnje pristup je nadziranim rolling nadogradnje.  Azure tkanina servisa nadzire funkcioniranje aplikacije nadograđen na temelju skup pravila stanja. Nakon nadogradnje ažuriranje domene (UD) tkanina servisa procjenjuje stanja računala i nastavlja se na sljedeće ažuriranje domene ili ne uspije nadogradnje ovisno o pravilnicima o stanju sustava.

Nadzirane aplikacije nadogradnje mogu izvršiti pomoću na upravljanih nativni API-ji, PowerShell ili OSTALE. Upute o nadogradnje pomoću Visual Studio, potražite u članku [Nadogradnja aplikacije pomoću Visual Studio](service-fabric-application-upgrade-tutorial.md).

Pomoću servisa tkanina nadzirati vodoravnim nadogradnje administratora aplikacije možete konfigurirati pravila za procjenu stanja koji tkanina servis koristi da bi se utvrdilo dobar aplikacije. Osim toga, administrator možete konfigurirati akcije da bi se otvorila kada analizu stanja ne uspije (na primjer, način automatsko vraćanje.) U ovom se odjeljku objašnjavaju nadziranim nadogradnje zbog nekog od primjera SDK koji koristi PowerShell.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Korak 1: Stvaranje i implementacija uzorka vizualne objekte


Stvaranje i objavljivanje aplikacija tako da desnom tipkom miša na aplikaciju projekta, **VisualObjectsApplication,** i odaberete naredbu **Objavi** .  Dodatne informacije potražite u članku [aplikacije servisa tkanina nadograditi vodič](service-fabric-application-upgrade-tutorial.md).  Osim toga, možete koristiti ljuske PowerShell za implementaciju aplikacije.

> [AZURE.NOTE] Prije bilo koju naredbu tkanina servisa može se koristiti u PowerShell, najprije morate povezati s klaster pomoću na `Connect-ServiceFabricCluster` cmdlet. Isto tako, pretpostavlja se da klaster već je postavljen na lokalnom računalu. Potražite u članku o [postavljanju okruženje za razvoj tkanina servisa](service-fabric-get-started.md).

Nakon Stvaranje projekta u Visual Studio, pomoću naredbe ljuske PowerShell **Kopiraj ServiceFabricApplicationPackage** da biste kopirali Aplikacijski paket na ImageStore. Sljedeći je korak da biste registrirali aplikaciju servisa tkanina runtime pomoću cmdleta **Register ServiceFabricApplicationPackage** . U posljednjem koraku je da biste pokrenuli instance aplikacije pomoću cmdleta **New-ServiceFabricApplication** .  Ta tri koraka odgovaraju pomoću stavku izbornika **uvođenja** u Visual Studio.

Sada možete koristiti [Servis tkanina Explorer da biste pogledali klaster i računala](service-fabric-visualizing-your-cluster.md). Aplikacija ima web-servisa koji mogu biti preusmjereni u pregledniku Internet Explorer tako da upišete [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) u adresnu traku.  Trebali biste vidjeti neke plutajućih objekata vizualne kretanje po zaslonu.  Osim toga, možete koristiti **Get-ServiceFabricApplication** da biste provjerili status aplikacija.

## <a name="step-2-update-the-visual-objects-sample"></a>Korak 2: Ažuriranje uzorka vizualne objekte

Mogli biste primijetiti da s verzijom koji je uveden u koraku 1 vizualne objekte Rotiraj. Pogledajmo Nadogradite ovu aplikaciju u jednu gdje se vizualne objekte i rotiranje.

Odaberite mogućnost projekt VisualObjects.ActorService unutar rješenja VisualObjects pa otvorite datoteku StatefulVisualObjectActor.cs. U toj datoteci, idite na metodu `MoveObject`, komentar izvan `this.State.Move()`, i uklonite `this.State.Move(true)`. Ta promjena Rotira objekte nakon nadogradnje usluge.

Moramo i ažuriranje datoteke *ServiceManifest.xml* (u odjeljku PackageRoot) projekt **VisualObjects.ActorService**. Ažurirajte *CodePackage* i servis verzija 2.0 i odgovarajuće retke u datoteci *ServiceManifest.xml* .
Mogućnost za Visual Studio *Uredi Manifest datoteke* možete koristiti kada je kliknete desnom tipkom miša na rješenja da biste unijeli promjene datoteka manifesta.


Nakon što su promjene, manifest trebala bi izgledati ovako (Istaknuti dijelovi pokazuju promjene):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Sada se ažurira datoteka *ApplicationManifest.xml* (pronaći u odjeljku **VisualObjects** projekta u odjeljku rješenje **VisualObjects** ) verzije 2.0 **VisualObjects.ActorService** projekta. Osim toga, verzija aplikacije ažurira se 2.0.0.0 iz 1.0.0.0. *ApplicationManifest.xml* trebao bi izgledati kao što su sljedeće isječak:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Sada sastavljanje projekt tako da odaberete samo **ActorService** projekta, pa desnom tipkom miša i odaberite mogućnost **stvaranja** u Visual Studio. Ako odaberete da **ponovno stvaranje sve**, trebali biste ažurirati verzije za sve projekte jer kod promjena. Sljedeći, recimo pakiranje ažurirane aplikacije tako da desnom tipkom miša na ***VisualObjectsApplication***, odabirom na izborniku tkanina servisa, a zatim odaberite **paketa**. Ova akcija stvara Aplikacijski paket koji možete uvesti.  Ažurirani aplikacija je spremna za uvođenje.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Korak 3: Odlučivanje o stanju pravila i nadogradnja parametara

Upoznajte se s [parametara za nadogradnju aplikacije](service-fabric-application-upgrade-parameters.md) i u [postupku nadogradnje](service-fabric-application-upgrade.md) da biste dobili dobro upoznavanje sa različite nadogradnje parametre, istek i primijeniti kriterij za stanje. Za ovaj vodič kriterija za procjenu stanje servisa je postavljeno na zadani (i preporučuje se) vrijednosti, što znači da sve servise i instance treba _dobar_ nakon nadogradnje.  

Međutim, recimo povećati *HealthCheckStableDuration* do 60 sekundi (tako da je usluge dobar barem 20 sekundi prije nadogradnje nastavlja se na sljedeće ažuriranje domene).  Pogledajmo postaviti *UpgradeDomainTimeout* biti 1200 sekundi i *UpgradeTimeout* biti 3000 sekundi.

Na kraju, recimo postaviti *UpgradeFailureAction* za vraćanje. Ova mogućnost zahtijeva tkanina servisa da biste vratili prethodnu verziju aplikacije Ako naiđe na probleme tijekom nadogradnje. Dakle, prilikom nadogradnje (u koraku 4), sljedećih parametara određeni su klauzulom:

FailureAction = vraćanje

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Korak 4: Priprema računala za nadogradnju

Sada je li aplikacija ugrađeni i spremna za nadogradnju. Ako otvorite prema gore u prozoru ljuske PowerShell kao administrator i vrsta **Get-ServiceFabricApplication**ga treba obavijesti da je vrsta 1.0.0.0 programa **VisualObjects** koji je uveden.  

U odjeljku sljedeći put relativni gdje ste dekomprimirati tkanina SDK servis - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*pohranjuju se Aplikacijski paket. Trebali biste pronašli mapu "Package" u taj imenik na kojem je pohranjena Aplikacijski paket. Provjerite vremenske oznake da biste bili sigurni da je ažuran (ćete možda morati izmijeniti putove komponenta kao i).

Sada ćemo kopirajte ažurirane Aplikacijski paket ImageStore tkanina servisa (paketa aplikacije se pohranjuju po tkanina servisa). Parametar *ApplicationPackagePathInImageStore* obavještava servisa tkanina gdje možete pronaći Aplikacijski paket. Ne možemo staviti ažurirane aplikacije u "VisualObjects\_V2" pomoću sljedeće naredbe (možda morati ponovno komponenta izmijeniti putovi).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Sljedeći je korak da biste registrirali ove aplikacije pomoću servisa tkanina, što možete izvršiti pomoću sljedeće naredbe:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Ako prethodna naredba ne uspije, vjerojatno je potrebno izvođenje svih servisa. Kao što je rečeno korak 2, možda ćete morati ažurirati verziju WebService kao i.

## <a name="step-5-start-the-application-upgrade"></a>Korak 5: Pokretanje nadogradnje aplikacije

Sada ćemo sve je spremno za pokretanje nadogradnje aplikacije pomoću sljedeće naredbe:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Naziv aplikacije ostaje isto kakvo je opisan u datoteci *ApplicationManifest.xml* . Servis tkanina koristi taj naziv da biste odredili koja će se aplikacija početak nadogradnje. Ako ste postavili istek biti premalen, možda ćete naići poruke pogreške koja navodi problem. Uputite na odjeljak otklanjanje poteškoća ili povećati na istek.

Sada kao prihod nadogradnju aplikacije možete nadzirati pomoću programa Explorer tkanina servisa ili pomoću ljuske PowerShell za sljedeće naredbe: **tkanina Get-ServiceFabricApplicationUpgrade: / VisualObjects**.

Za nekoliko minuta, trebali biste status koju ste dobili pomoću prethodne naredbe ljuske PowerShell stanja nisu li nadograditi sve ažuriranje domene (gotov). I trebali biste pronašli da je vizualne objekte u prozoru preglednika počele rotiranje!

Možete pokušati nadogradnje s verzije 2 verziju 3 ili iz verzije 2 verziju 1 za vježbu. Premještanje iz verzije 2 verzija 1 i smatra nadogradnju. Isprobavati istek i pravila stanja da biste se upoznali s njima. Kada se implementacije Azure klaster, parametre morati postaviti pravilno. Da biste postavili na istek conservatively dobro je.


## <a name="next-steps"></a>Daljnji koraci

[Nadogradnja aplikacije pomoću Visual Studio](service-fabric-application-upgrade-tutorial.md) vodit će vas kroz nadogradnju aplikacije pomoću Visual Studio.

Upravljanje kako nadograđuje aplikacije pomoću [nadograditi parametara](service-fabric-application-upgrade-parameters.md).

Provjerite svoje nadogradnje aplikacije kompatibilne Naučite koristiti [serijalizacije podataka](service-fabric-application-upgrade-data-serialization.md).

Saznajte kako koristiti napredne funkcije tijekom nadogradnje aplikacije za [Dodatne teme](service-fabric-application-upgrade-advanced.md).

Riješite uobičajene probleme u aplikaciji nadogradnje tako da upućuju na korake u [nadogradnje aplikacije za otklanjanje poteškoća](service-fabric-application-upgrade-troubleshooting.md).
