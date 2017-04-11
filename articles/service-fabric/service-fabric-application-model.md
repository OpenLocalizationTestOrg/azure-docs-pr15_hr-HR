<properties
   pageTitle="Servis tkanina modelu | Microsoft Azure"
   description="Kako se model i opisuju aplikacija i servisa u tkanina servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Model aplikacije u tkanina servisa

Ovaj članak sadrži pregled modela aplikacija tkanina servisa Azure. Također se opisuje kako definirati aplikacija i servisa putem manifesta datoteke te pronađite aplikaciju zapakirani i jeste li spremni za implementaciju.

## <a name="understand-the-application-model"></a>Objašnjenje modela aplikacija

Aplikacija je zbirka sastavnoj servise koje izvesti određene funkcije ili funkcije. Servis izvodi potpune i samostalni funkcija (ga možete pokrenuti i koristiti neovisno o drugim servisima) i sastoji od kod, konfiguriranje i podataka. Za svaku uslugu kod sastoji se od izvršna binarne datoteke, konfiguracije sastoji se od postavke servisa koje je moguće učitati vrijeme izvođenja i podataka sastoji se od proizvoljne statične podataka se utrošiti servis. Svaka komponenta u ovom hijerarhijski modelu može biti neovisno određuju i nadograđena.

![U modelu tkanina servisa][appmodel-diagram]


Vrstu aplikacije koju je kategorizaciju aplikacije i sastoji se od paket vrsta servisa. Vrsta servisa je kategorizaciju servisa. Za kategorizaciju može imati različite postavke i konfiguracija, ali osnovne funkcije ostaje isto. Instance servisa su varijacije konfiguracije različite servisne iste vrste servisa.  

Klase (ili "Vrsta") aplikacija i servisa sustava opisane do XML datoteke (manifesti aplikacija i servisa manifesti) koje se nalaze predlošci prema kojima možete se instancirati aplikacije iz trgovine u klaster slika. Definicija sheme datoteke ServiceManifest.xml i ApplicationManifest.xml instaliran je uz servis tkanina SDK i alate za *C:\Programske datoteke\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Kod za drugu aplikaciju instance pokrenut će se kao zasebna procesa čak i ako se hostira tvrtka čvor isti tkanina servisa. Osim toga, životnim ciklusom svaku instancu aplikacija može upravljati (odnosno Nadogradnja) neovisno. Sljedeći dijagram prikazuje kako vrsta aplikacija sastoje se od servisa vrstama shodno sastoje se od kod, konfiguriranje i paketa. Da biste pojednostavnili dijagram, samo kod/config/podatke paketi za `ServiceType4` prikazuju, iako svaku vrstu servisa želite uvrstiti neke ili sve od tih pakiranje vrste.

![Vrsta tkanina servisa aplikacija i servisa vrste][cluster-imagestore-apptypes]

Dvije različite manifesta datoteke koriste se za opisuju aplikacija i servisa: servis manifest i programski manifest. Te se primjenjuju detaljno započinjati sekcije.

Može biti jedan ili više instanci vrsta servisa active u klasteru. Na primjer, instance s praćenjem stanja servisa i replike, postignete visoke pouzdanosti replikaciju stanje između replike koji se nalaze na različitim čvorovi u klasteru. U ovom replikacije zapravo sadrži zalihosti za uslugu da bi bio dostupan čak i ako se ne uspije jedan čvor klasteru. [Servis za particioniranom](service-fabric-concepts-partitioning.md) dodatno dijeli njegov stanje (i pristup uzorcima da to stanje) preko čvorovi u klasteru.

Sljedeći dijagram prikazuje odnos između aplikacija i servisa instance, particije i replike.

![Particije i replike unutar servisa][cluster-application-instances]


>[AZURE.TIP] Možete pogledati izgleda aplikacije klasteru pomoću alata za servis tkanina Explorer dostupna na http://&lt;yourclusteraddress&gt;: 19080/Explorer. Dodatne informacije potražite u članku [vizualizacija svoj klaster pomoću programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Opišite uslugu

Servis manifest deklarativno definira vrsta servisa i verziju. Određuje metapodataka servisa kao što su vrsta servisa, svojstva stanja, metriku za ujednačavanje opterećenja, binarne datoteke servisa i konfiguracijske datoteke.  Drugim riječima, on se opisuje kod, konfiguriranje i podataka iz paketa sastavite paket servisa za podršku jednu ili više vrsta servisa. Evo jednostavnog primjera manifest servisa:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

Atributi **verzija** su nizovi nestrukturirane, a ne raščlaniti sustav. To su koriste verziju svaki komponenta za nadogradnje.

**ServiceTypes** deklarira s kojim je vrstama servisa podržanog **CodePackages** u ovu objavu. Kada je servis instancirati protiv jednu od ovih vrsta servisa, sve kod paketa deklarirani u ovu objavu aktiviraju se tako da pokrenete njihove točke unosa. Da biste registrirali vrste podržanom servisu za vrijeme izvođenja se očekuje dobivene procesa. Imajte na umu da se vrste servisnih deklarirane pri manifesta razinu i ne kod razinu paketa. Pa kad postoji više paketa kod, oni sve aktiviraju kad god sustav traži bilo koju od vrste deklarirane servisa.

**SetupEntryPoint** je točka unosa povlaštene koji se izvodi s istim vjerodajnicama kao tkanina servisa (obično račun *LocalSystem* ) prije Ulazna točka. Izvršna datoteka određen **Ulazna** je obično dugoročnih servis glavnog računala. Prisutnosti zasebnu instalaciju Ulazna točka izbjegava morate pokrenuti servis glavnog računala s ovlastima visoke za prošireni vremenskog razdoblja. Izvršna datoteka određen **Ulazna** pokreće se nakon **SetupEntryPoint** zatvara uspješno. Postupak dobivene je nadzirati i ponovno pokrenuti (počevši od ponovno **SetupEntryPoint**) ako ikad prekida ili ruši.

**DataPackage** deklarira u mapu pod nazivom po atribut **naziva** koji sadrži proizvoljne statične podatke za potrošena proces vrijeme izvođenja.

**ConfigPackage** deklarira u mapu pod nazivom po atribut **naziva** koji sadrži *Settings.xml* datoteku. Datoteka sadrži sekcije korisnički definirane, ključ vrijednost par postavke koje postupak mogu čitati natrag u vrijeme izvođenja. Tijekom nadogradnje, ako samo **ConfigPackage** **verzija** promijenjena, zatim izvodi postupak ne ponovnog pokretanja. Umjesto toga u povratnog obavještava postupka promijenjenih konfiguracijske postavke tako da se mogu ponovno učitati dinamički. Evo primjera *Settings.xml* datoteke:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Servis manifest mogu sadržavati više kod, konfiguriranje i paketa podataka. Svaki od tih može biti određuju zasebno.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Opišite aplikacije


Programski manifest deklarativno opisuju vrsta aplikacije i verziju. Određuje metapodataka za sastavljanje servisa kao što su stabilan imena particija shemu, instance broj/replikacije faktor, sigurnost/odvajanja pravila, položaj ograničenja, nadjačavanja konfiguraciju i vrste sastavnoj servisa. Opisane su i domene za ujednačavanje opterećenja u koju se smješta aplikacije.

Dakle, programski manifest opisuje elemenata na razini aplikacije i reference manifesti servisa da biste sastavili vrstu aplikacije. Evo jednostavnog primjera programski manifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Kao što su manifesti servisa **verziju** atributi su nizovi nestrukturirane i ne raščlaniti sustav. Ovo su također koristi verziju svaki komponenta za nadogradnji.

**ServiceManifestImport** sadrži reference na servis manifesti koje sastavite ovu vrstu aplikacije. Manifesti uvezene servisa određuju kojim je vrstama servisa vrijede unutar ovu vrstu aplikacije.

**DefaultServices** deklarira instanci servisa koji se automatski stvaraju kad god je aplikacija instancirati protiv ovu vrstu aplikacije. Zadane servise su samo praktičnosti i ponašati kao normalni servisi u svakoj slijedi nakon što ste je stvorili. Su nadograditi zajedno s ostalim servisima u instanci aplikacije i kao i moguće ukloniti.

> [AZURE.NOTE] Programski manifest može sadržavati više servisa manifesta uvozi i zadane servise. Svaki servis za uvoz manifesta samostalno može biti određuju.

Da biste saznali kako da biste zadržali različitih aplikacija i servisa parametara za pojedinačne okruženja, potražite u članku [Upravljanje aplikacije parametara za više okruženja](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Paket aplikacije

### <a name="package-layout"></a>Izgled paketa

Programski manifest, manifest(s) servisa i druge datoteke potrebne paket mora biti organiziran određeni raspored preko implementacije u servis tkanina klaster. Primjer manifesti u ovom članku morat ćete organizirati u sljedeću strukturu direktorija:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Tako da odgovara **naziv** atributa svaki element odgovarajuće imenovane su mape. Na primjer, ako manifest servisa sadrži dvije paketa kod s nazivima **MyCodeA** i **MyCodeB**, zatim dvije mape s istim nazivima će sadržavati potrebne binarne datoteke za svaki paket kod.

### <a name="use-setupentrypoint"></a>Korištenje SetupEntryPoint

Uobičajeni scenariji za korištenje **SetupEntryPoint** se kada morate pokrenuti izvršne datoteke prije pokretanja servisa ili vam potrebne za izvođenje operacije s dodatnim ovlastima. Ako, na primjer:

- Postavljanje i pokretanje varijable okruženja koje treba izvršne datoteke servisa. Nije ograničen na samo izvršne datoteke napisali putem modelima programiranje tkanina servisa. Na primjer, npm.exe mora neke varijable okruženja konfiguriran za implementaciju node.js aplikacije.

- Postavljanje kontrola pristupa tako da instalirate sigurnosni certifikati.

### <a name="build-a-package-by-using-visual-studio"></a>Izrada paket korištenjem Visual Studio

Ako koristite Visual Studio 2015 da biste stvorili aplikacije, koristite naredbu paketa da biste automatski stvorili paket koji odgovara rasporedu prethodno opisan.

Da biste stvorili paket, desnom tipkom miša kliknite projekt aplikacije u pregledniku rješenja i odaberite naredbu paketa, kao što je prikazano u nastavku:

![Pakiranje aplikacije s Visual Studio][vs-package-command]

Po dovršetku pakiranje pronaći ćete na mjesto paketa u **izlaznom** prozoru. Imajte na umu korak pakiranje pojavljuje automatski kada implementacije ili aplikacije u Visual Studio za ispravljanje pogrešaka.

### <a name="test-the-package"></a>Testiranje paketa

Struktura paketa lokalno putem komponente PowerShell možete provjeriti pomoću naredbe **Test ServiceFabricApplicationPackage** . Ta naredba će potražiti manifest analize problema i provjerite je li sve reference. Imajte na umu da ta naredba provjerava samo strukturni ispravnosti mape i datoteke u paketu. Ga će ne provjerite je li paket sadržaja kod ili podataka izvan Provjera jesu li sve potrebne datoteke prezentacija.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Ta se pogreška prikazuje da je datoteka *MySetup.bat* poziva manifest servisa **SetupEntryPoint** nedostaje package kod. Kada dodate datoteke nedostaje prosljeđuje potvrdu aplikacije:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Kada aplikacija pravilno pakiranje i prosljeđuje potvrdu, tada je jeste li spremni za implementaciju.

## <a name="next-steps"></a>Daljnji koraci

[Uvođenje i uklanjanje aplikacija][10]

[Upravljanje parametara aplikacije za više okruženja][11]

[RunAs: Pokrenut servis tkanina aplikacije s različitim sigurnosnim dozvolama][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
