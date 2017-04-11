<properties
   pageTitle="Životni ciklus aplikacije u servis tkanina | Microsoft Azure"
   description="Opisuje razvoj, implementaciji, testiranje, nadogradnje, održavanje i uklanjanje aplikacija tkanina servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Životni ciklus tkanina servisa aplikacija
Kao što je s drugim platformama u aplikaciju na tkanina servisa Azure obično prolazi kroz sljedeće faze: dizajn, razvoj, testiranje, implementaciju, nadogradnje, održavanje i uklanjanje. Servis tkanina omogućuje jednostavno prva liga podršku za životni ciklus punu aplikaciju oblaka aplikacijama, od razvoja do implementaciju, dnevno upravljanje i održavanje usmjerenog deaktivirati. Model servisa omogućuje nekoliko različite uloge neovisno sudjelovanja u životnom ciklusu aplikacije. Ovaj članak sadrži pregled API-ji i kako se koriste različite uloge tijekom faze životnom ciklusu tkanina servisa aplikacija.

## <a name="service-model-roles"></a>Uloge servisa modela
Model uloge servisa su:

- **Servis za razvojne inženjere**: razvija modularan i generički servise koji se može ponovno purposed i koristiti u više aplikacija iste vrste ili različite vrste. Na primjer, usluga reda čekanja može koristiti za stvaranje ticketing aplikacije (korisnička podrška) ili aplikaciju za e-trgovine (košarici).

- **Razvojni inženjer**: stvara aplikacije po integriranje zbirka servise za ispunjavanje određenih preduvjete ili scenarija. Ako, na primjer, programa e-trgovine web-mjesto možda integrirati "JSON bez praćenja stanja Front-End servis," "Dražba s praćenjem stanja servisa" i "Usluga s praćenjem stanja reda čekanja" da biste sastavili auctioning rješenja.

- **Administrator aplikacije**: odlučuje konfiguracije aplikacije (ispunjavanje parametri predložaka konfiguracija), implementacije (mapiranje dostupnih resursa) i kvaliteta servisa. Ako, na primjer, administrator aplikacije odlučuje jezik regionalnu shemu (na engleskom za Sjedinjenih Država) ili japanski Japanu, na primjer aplikacije. Neku drugu aplikaciju distribuiranih može sadržavati različite postavke.

- **Operator**: uvodi aplikacijama ovisno o konfiguraciji aplikacije i zahtjeve navedeni administratora aplikacije. Ako, na primjer, operator dodjeljuje uvodi aplikacije i osigurava da se izvodi u Azure. Operatori nadzora informacije o stanju i performansama aplikacije i održavanja fizičke infrastrukture prema potrebi.


## <a name="develop"></a>Razvoj
1. *Servis za razvojne inženjere* razvija različite vrste usluga pomoću model programiranja [Pouzdanog Glumci](service-fabric-reliable-actors-introduction.md) ili [Pouzdanog Services](service-fabric-reliable-services-introduction.md) .
2. *Servis za razvojne inženjere* deklarativno opisuju vrste razvijen servisa u datoteci manifesta servis koji se sastoji od jednog ili više paketa kod, konfiguriranje i podataka.
3. *Razvojni inženjer* zatim sastavlja aplikacije pomoću servisa za različite vrste.
4. *Razvojni inženjer* deklarativno opisuju vrsta aplikacije u programski manifest Upućivanjem manifesti servisa sastavnoj services i prikladno nadjačavanje i parameterizing različite postavke konfiguracije i implementacija sastavnoj servisa.

Potražite u članku [Početak rada s pouzdanog Glumci](service-fabric-reliable-actors-get-started.md) i [Početak rada s pouzdanog servisima](service-fabric-reliable-services-quick-start.md) primjere.

## <a name="deploy"></a>Implementacija
1. *Administrator aplikacije* tailors vrsta aplikacije za određenu aplikaciju uvesti servisa tkanina klaster navođenjem odgovarajuće parametre **ApplicationType** element u manifestu aplikacije.

2. *Operator* prenosi Aplikacijski paket u trgovini klaster slika pomoću [metode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) ili [ **Kopiraj ServiceFabricApplicationPackage** cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Aplikacijski paket sadrži programski manifest i skup pakete usluga. Servis tkanina uvodi aplikacije iz paketa aplikacije pohranjene u spremištu slike koje mogu biti u spremište blobova platforme Azure ili sistemski servis tkanina servisa.

3. *Operator* zatim dodjeljuje vrsta aplikacije u ciljne skupine iz paketa prenesene aplikacije pomoću [metode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register ServiceFabricApplicationType** cmdlet](https://msdn.microsoft.com/library/azure/mt125958.aspx)ili [operacija OSTATAK **Nakon dodjele resursa aplikacije sustava** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Nakon dodjele resursa aplikacije, *operator* počinje aplikacije danih *administratora aplikacije* pomoću [metode **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), [cmdleta **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)ili [OSTALE **Aplikacije za stvaranje** operacije](https://msdn.microsoft.com/library/azure/dn707676.aspx)parametara.

5. Nakon postavila aplikacije *operator* koristi [ **CreateServiceAsync** način](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), [cmdleta **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)ili [ **Stvaranje servisa** OSTATAK postupka](https://msdn.microsoft.com/library/azure/dn707657.aspx) za stvaranje novih instanci servisa za aplikaciju na temelju vrste raspoloživ servis.

6. Aplikacija sada izvodi u skupini tkanina servisa.

Primjeri potražite u članku [uvođenja aplikacije](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Test
1. Nakon što implementirate klaster lokalne razvoj ili test klaster, *servis za razvojne inženjere* pokreće test scenarij ugrađene prebacivanje putem klase [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) i [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) ili [ **Pozovite ServiceFabricFailoverTestScenario** cmdlet](https://msdn.microsoft.com/library/azure/mt644783.aspx). Scenarij test prebacivanje pokreće navedeni servis putem važne Prijelazi i failovers da biste bili sigurni da je i dalje dostupne i radi.

2. *Servis za razvojne inženjere* zatim pokreće scenarij test ugrađene chaos pomoću klase [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) i [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) ili [cmdlet **Pozovite ServiceFabricChaosTestScenario** ](https://msdn.microsoft.com/library/azure/mt644774.aspx). Scenarij test chaos slučajno induces više čvor, kod paketa i kvarove replike u klaster.

3. *Servis za razvojne inženjere* [testira servis za servis komunikacije](service-fabric-testability-scenarios-service-communication.md) po test scenariji kojima se primarni replike oko klaster za izradu.

Dodatne informacije potražite u članku [Uvod u servisu Analysis kvara](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Nadogradnja
1. *Servis za razvojne inženjere* ažurira sastavnoj services instancirani aplikacije i/ili rješava programskih pogrešaka i njihovi novu verziju manifest servisa.

2. *Razvojni inženjer* nadjačava i parameterizes postavke konfiguracije i implementacija dosljedan servisa i nudi novu verziju programski manifest. Razvojni inženjer zatim ugrađuje nove verzije manifesti servisa u aplikaciju i njihovi novu verziju vrsta aplikacije u paketa za aplikaciju.

3. *Administrator aplikacije* ugrađuje novu verziju vrsta aplikacije u ciljnu aplikaciju ažuriranjem odgovarajuće parametre.

5. *Operator* prenosi ažurirane Aplikacijski paket u spremište klaster slika pomoću [metode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) ili [ **Kopiraj ServiceFabricApplicationPackage** cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Aplikacijski paket sadrži programski manifest i skup pakete usluga.

6. *Operator* dodjeljuje novu verziju aplikacije ciljne skupine pomoću [ **ProvisionApplicationAsync** način](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register ServiceFabricApplicationType** cmdlet](https://msdn.microsoft.com/library/azure/mt125958.aspx)ili [operacija OSTATAK **Nakon dodjele resursa aplikacije** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. *Operator* nadograđuje ciljne aplikacije u novu verziju pomoću [ **UpgradeApplicationAsync** način](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), [ **Start ServiceFabricApplicationUpgrade** cmdlet](https://msdn.microsoft.com/library/azure/mt125975.aspx)ili [ **nadogradnju aplikacije** ostalih operacija](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. *Operator* provjerava tijeku Nadogradnja pomoću [metode **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), [cmdlet **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)ili [operacija OSTALE **Dobiti tijeku nadogradnju aplikacije** ](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Ako je potrebno, *operator* mijenja i ponovno primjenjuje parametre trenutnu nadogradnju aplikacije pomoću [metode **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), [ **Ažuriranje ServiceFabricApplicationUpgrade** cmdlet](https://msdn.microsoft.com/library/azure/mt126030.aspx)ili [postupak **Nadogradnje za ažuriranje aplikacije** OSTALE](https://msdn.microsoft.com/library/azure/mt628489.aspx).

10. Ako je potrebno, *operator* će vratiti trenutnu nadogradnju aplikacije pomoću [metode **RollbackApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), [ **Start ServiceFabricApplicationRollback** cmdlet](https://msdn.microsoft.com/library/azure/mt125833.aspx)ili [postupak **Nadogradnje za vraćanje aplikacije** OSTALE](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Servis tkanina nadograđuje ciljne aplikacije koji se izvodi u klasteru bez gubitka raspoloživost njegove servise za sastavnoj.

U odjeljku [aplikacije nadograditi vodič](service-fabric-application-upgrade-tutorial.md) za primjere.

## <a name="maintain"></a>Održavanje
1. Nadograđuje operacijski sustav i zakrpa, servis tkanina sučelja s Azure infrastrukturu za jamči raspoloživost sve programe koji se izvodi u klasteru.

2. Nadograđuje i zakrpa platformi tkanina servisa, tkanina servisa nadograđuje sam bez gubitka dostupnost bilo kojeg od aplikacije koje rade na klaster.

3. *Administrator aplikacije* odobri Dodavanje ili uklanjanje čvorove iz klaster nakon analiza povijesne kapaciteta Upotreba podataka i Predviđeni buduće zahtjev.

4. *Operator* dodaje i uklanja čvorove određen *administratora aplikacije*.

5. Kada se dodaju novi čvorovi ili postojeći čvorove uklanjaju se iz skupine, servis tkanina automatski učitavanja-salda pokrenute aplikacije preko sve čvorove u skupini da biste postigli optimalne performanse.

## <a name="remove"></a>Uklanjanje
1. *Operator* možete izbrisati instancu pokrenuti servis u klasteru bez uklanjanja cijelu aplikaciju pomoću [metode **DeleteServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), [ **Ukloni ServiceFabricService** cmdlet](https://msdn.microsoft.com/library/azure/mt126033.aspx)ili [ **Brisanje servisa** OSTATAK postupka](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. *Operator* možete i izbrisati instancu aplikacije i sve njegove servise pomoću [metode **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), [ **Ukloni ServiceFabricApplication** cmdlet](https://msdn.microsoft.com/library/azure/mt125914.aspx)ili [ **Brisanje aplikacije** OSTATAK postupka](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Kada aplikacija i servisa da su zaustavljena, *operator* možete Oslobađanje resursa vrsta aplikacije pomoću [metode **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), [ **Neregistriranja ServiceFabricApplicationType** cmdlet](https://msdn.microsoft.com/library/azure/mt125885.aspx)ili [ **Oslobađanje resursa aplikacije** OSTATAK postupka](https://msdn.microsoft.com/library/azure/dn707671.aspx). Oslobađanje resursa vrsta aplikacije uklonite Aplikacijski paket iz na ImageStore. Aplikacijski paket morate ručno ukloniti.

4. *Operator* uklanja Aplikacijski paket iz ImageStore pomoću [metode **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) ili [ **Uklanjanje ServiceFabricApplicationPackage** cmdlet](https://msdn.microsoft.com/library/azure/mt163532.aspx).

Primjeri potražite u članku [uvođenja aplikacije](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o razvoju, testiranje i upravljanje aplikacijama servisa tkanina i servise, pogledajte:

- [Pouzdan Glumci](service-fabric-reliable-actors-introduction.md)
- [Pouzdani servisi](service-fabric-reliable-services-introduction.md)
- [Implementacija aplikacije](service-fabric-deploy-remove-applications.md)
- [Nadogradnja aplikacije](service-fabric-application-upgrade.md)
- [Pregled testability](service-fabric-testability-overview.md)
- [Ogledna životni ciklus utemeljen na OSTALE aplikacije](service-fabric-rest-based-application-lifecycle-sample.md)
