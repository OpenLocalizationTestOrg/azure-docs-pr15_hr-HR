<properties
   pageTitle="Stvaranje i upravljanje klaster tkanina servisa Azure samostalne | Microsoft Azure"
   description="Stvaranje i upravljanje njima u klaster tkanina servisa Azure na bilo kojem računalu (fizičke ili virtualne) sa sustavom Windows Server, hoće li je lokalnog ili u bilo kojem oblaka."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Stvaranje i upravljanje klaster sustavom Windows Server

Tkanina servisa Azure možete koristiti da biste stvorili klastere tkanina servisa na virtualnim strojevima ni računala sa sustavom Windows Server. Znači implementacije, i pokrenuti servis tkanina aplikacije u bilo kojem okruženje koje sadrži skup međusobno povezanih računala za Windows Server, na se lokalno i s bilo kojeg davatelja usluga oblaka. Servis tkanina nudi instalacijski paket da biste stvorili servisa tkanina klastere naziva samostalne paketa Windows Server.

U ovom se članku vodit će vas kroz korake za stvaranje klaster pomoću značajke pakiranja samostalne za servis tkanina lokalno, premda ga može biti jednostavno prilagođen drugom okruženju kao što su drugih davatelja oblaka.

>[AZURE.NOTE] Samostalne paketa Windows Server možda sadrži značajke koje su trenutno u pretpregledu, a nije podržana za komercijalno korištenje. Da biste vidjeli popis značajki koje su u pretpregledu, potražite u članku "Pretpregled značajke ugrađene paket." Možete [preuzeti kopiju EULA](http://go.microsoft.com/fwlink/?LinkID=733084) sada.


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Pronaći podršku za samostalne paketa tkanina servisa

- Postavite pitanje zajednici o samostalni paket tkanina servisa za Windows Server u na [forumu tkanina servisa Azure](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).

- Otvorite s karata [Profesionalna podrška za servis tkanina](http://support.microsoft.com/oas/default.aspx?prid=16146 ).  Dodatne informacije o [Podršci za Professional od Microsofta](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>Preuzmite paket servisa tkanina samostalni


[Preuzmite paket samostalne za servis tkanina za Windows Server 2012 R2 ili novijim](http://go.microsoft.com/fwlink/?LinkId=730690), koja je Microsoft.Azure.ServiceFabric.WindowsServer. &lt;verziju&gt;.zip.


U paket preuzimanja, vidjet ćete sljedeće datoteke:

|**Naziv datoteke**|**Kratak opis**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|.Cab datoteku koja sadrži binarne datoteke koje su uvedene na svakom računalu u klasteru.|
|ClusterConfig.Unsecure.DevCluster.json|Klaster konfiguracije ogledne datoteke koja sadrži postavke programa nezaštićenu, tri čvor jednom računalu (ili virtualnog računala) razvoj klaster, uključujući podatke za svaku čvor u klasteru. |
|ClusterConfig.Unsecure.MultiMachine.json|Klaster konfiguracije ogledne datoteke koja sadrži postavke programa nezaštićenu, više računala (ili virtualnog računala) klaster, uključujući podatke za svaku računalo u klasteru.|
|ClusterConfig.Windows.DevCluster.json|Klaster konfiguracije ogledne datoteke koja sadrži sve postavke za sigurnu, tri čvor jednom računalu (ili virtualnog računala) razvoj klaster, uključujući podatke za svaku čvor koji se nalazi u klasteru. Klaster je osigurani pomoću [identiteta za Windows](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.Windows.MultiMachine.json|Klaster konfiguracije ogledne datoteke koja sadrži sve postavke za sigurnu, više računala (ili virtualnog računala) klaster pomoću sigurnost sustava Windows, uključujući podatke za svaku računala koja se nalazi u sigurne klaster. Klaster je osigurani pomoću [identiteta za Windows](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.x509.DevCluster.json|Klaster konfiguracije ogledne datoteke koja sadrži sve postavke za sigurnu, tri čvor jednom računalu (ili virtualnog računala) razvoj klaster, uključujući podatke za svaku čvor u klasteru. Klaster zaštićen je x509 certifikata.|
|ClusterConfig.x509.MultiMachine.json|Klaster konfiguracije ogledne datoteke koja sadrži sve postavke za klaster sigurne, više računala (ili virtualnog računala), uključujući informacije za svaki čvor sigurne klaster. Klaster zaštićen je x509 certifikata.|
|EULA_ENU.txt|Licencne odredbe za korištenje paketa Windows Server samostalne tkanina usluge za Microsoft Azure. Sada možete [preuzeti kopiju EULA](http://go.microsoft.com/fwlink/?LinkID=733084) .|
|Readme.txt|Veza na napomene i upute za instalaciju osnovni. Preporučuje se podskup upute u ovom dokumentu.|
|CreateServiceFabricCluster.ps1|Skriptu PowerShell koji stvara klaster pomoću postavki u ClusterConfig.json.|
|RemoveServiceFabricCluster.ps1|Skriptu PowerShell koji uklanja klaster pomoću postavki u ClusterConfig.json.|
|ThirdPartyNotice.rtf |Obavijest o softver drugih proizvođača koji nije u paketu.|
|AddNode.ps1|Skriptu PowerShell za dodavanje čvor postojeće implementiran klaster.|
|RemoveNode.ps1|Skriptu PowerShell za uklanjanje čvor iz postojećeg implementiran klaster.|
|CleanFabric.ps1|Skriptu PowerShell za čišćenje samostalne instalacije servisa tkanina isključivanje računala. Prethodne instalacije MSI mora se ukloniti pomoću vlastite povezane uninstallers.|
|TestConfiguration.ps1|Skriptu PowerShell za analizu infrastrukture kao što je navedeno u na Cluster.json.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Planiranje i pripremu implementaciju sustava klaster
Prije stvaranja svoj klaster, slijedite sljedeće korake.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Korak 1: Planiranje infrastruktura za klaster
Namjeravate stvoriti servisa tkanina klaster na računalima ste vlasnik, tako da možete odlučiti koju vrstu pogreške želite klaster za preživi. Ako, na primjer, učinite potrebne razdvojite power retke ili internetske veze dodjeljuje te strojeva? Osim toga, razmislite o fizičke sigurnost tih računala. Gdje strojeva nalaze i tko mora pristup do njih? Nakon unosa tih odluka logično možete mapirati strojeva različitih domena kvara (pogledajte koraku 4). Infrastruktura za planiranje za klastere radni je složenije od za klastere test.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Korak 2: Priprema računala za zadovoljava preduvjete
Preduvjeti za svaki stroju koji želite dodati skupine:

- Preporučuje se najmanje 16 GB RAM-a.
- Preporučuje se najmanje 40 GB slobodnog prostora na disku.
- Preporučuje se 4 core ili noviji procesora.
- Povezivanje sigurne mrežu ili mreže za svim računalima.
- Windows Server 2012 R2 ili Windows Server 2012 (morate imati instaliran [KB2858668](https://support.microsoft.com/kb/2858668) ).
- [.NET framework 4.5.1 ili noviji](https://www.microsoft.com/download/details.aspx?id=40773), puna instalacija.
- [Komponente Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- [Servis za RemoteRegistry](https://technet.microsoft.com/library/cc754820) mora biti pokrenut na svim računalima.

Administrator klaster implementaciji i konfiguraciji klaster mora imati [administratorske ovlasti](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) na svim strojeva. Tkanina servis nije moguće instalirati na kontroleru domene.

### <a name="step-3-determine-the-initial-cluster-size"></a>Korak 3: Određivanje početnog klaster
Svaki čvor klasteru samostalne tkanina servis je tkanina servisa runtime implementiran i član klaster. U uvođenja uobičajeni radni postoji jedan čvor po instancu OS (fizičke ili virtualne). Klaster ovisi o poslovnim potrebama. Međutim, morate imati minimalne Klaster od tri čvorove (strojeva ili virtualnim strojevima).
Razvoj svrhe imate više od jedne čvor na određenom računalu. U okruženju proizvodnje servisa tkanina podržava samo jedan čvor po fizičke ili virtualnog računala.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Korak 4: Određivanje broja domena kvara i nadogradnja domene
*Kvara domene* (FD) je fizičke jedinice pogreške i izravno povezani s fizičke infrastrukture u centre za podatke. Domene kvara sastoji se od hardverske komponente (računala, parametri, mreža i više) zajednički koristite jednu točku nije uspjelo. Iako nema 1:1 mapiranja između kvara domene i nosači za, slobodnije govoriti, svaki bicikle možete smatrati kvara domene. Kada se preporučuje čvorove u svoj klaster, preporučujemo da distribuirati čvorove među najmanje tri kvara domene.

Kada odredite FDs u ClusterConfig.json, možete odabrati naziv za svaki FD. Tkanina servisa ne podržava hijerarhijski FDs, pa možete odražavaju topologiji infrastrukture u njima.  Na primjer, sljedeće FDs vrijede:

- "faultDomain": "fd: / Rack1/Room1/Machine1"
- "faultDomain": "fd: / FD1"
- "faultDomain": "fd: / Room1/Rack1/PDU1/M1"


Logička jedinica čvorove je *Nadogradnja domene* (UD) je. Tijekom nadogradnje servisa tkanina orchestrated (nadogradnju aplikacije ili klaster nadogradnje), sve čvorove u na UD uzimaju se prema dolje da biste izvršili nadogradnju dok čvorove u drugim UDs ostaju dostupna vam poslužiti zahtjeva. Nadograđuje opremu koju izvedete pomoću vašeg računala poštovati UDs, pa ga morate ih nešto Strojna grupa istovremeno.

Najjednostavniji način da razmislite o koncepte je treba uzeti u obzir FDs kao jedinice neplanirano pogreške i UDs kao jedinice planiranog održavanja.

Kada odredite UDs u ClusterConfig.json, možete odabrati naziv za svaki UD. Na primjer, sljedeća su imena valjana:

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Plavo"

Detaljnije informacije o nadogradnje domene i kvara domena potražite u članku [s opisom klaster tkanina servisa](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Korak 5: Preuzmite paket samostalne tkanina servisa za Windows Server
[Preuzmite paket samostalne tkanina servisa za Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) i raspakirajte paketa za implementaciju računala koja nije dio klaster ili na neki od računala koja će biti dio svoj klaster. Možda preimenovati mapu raspakiranu datoteku paketa `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>Stvaranje svoj klaster

Nakon razmijenjeno kroz korake planirati i pripremiti, spremni ste za stvaranje svoj klaster.

### <a name="step-1-modify-cluster-configuration"></a>Korak 1: Izmjena konfiguracije klaster
Klaster je opisano u ClusterConfig.json. Detalje o sekcija u ovoj datoteci potražite u članku [Konfiguracija postavki za samostalne Windows klaster](service-fabric-cluster-manifest.md).
Otvorite neku od datoteka ClusterConfig.json iz paketa koji ste preuzeli i promijenite sljedeće postavke:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Postavke konfiguracije**|**Opis**|
|-----------------------|--------------------------|
|**NodeTypes**|Vrste čvor omogućuju vam da podijelite svoje klaster čvorove u različite grupe. Klaster, morate imati barem jedan NodeType. Sve čvorove u grupi sa sljedećim karakteristikama uobičajenih: <br> **Naziv** – to je naziv vrste čvor. <br>**Priključci krajnjoj točki** – to su različite imenovani krajnje točke (Priključci) koje su povezane s tom vrstom čvor. Možete koristiti bilo koji broj priključka koji želite, pod uvjetom da nisu u sukobu s bilo što drugo u ovu objavu, a još nisu zauzet nekim drugim aplikacije koji se izvode na računalu/VM. <br> **Položaj svojstva** - ove opisuju svojstva za tu vrstu čvor koje koristite kao položaj ograničenja servisa sustava ili servisa. Svojstva su parove korisnički definirane ključ/vrijednosti koje će vam pružiti dodatni podaci metapodataka za dani čvor. Primjeri čvor svojstva bi ima li čvor tvrdi disk ili grafička kartica, a zatim broj spindles u njegov tvrdom disku, jezgri i druga svojstva fizičke. <br> **Kapaciteta** - čvor kapaciteta definiranje naziva i količinu određeni resurs koji ima određene čvor dostupne za potrošnju. Na primjer, čvor možda definirati ima kapacitet metrike pod nazivom "MemoryInMb" i je li dostupna 2048 MB po zadanom. Ove kapaciteta se koriste prilikom izvođenja da biste bili sigurni servise koje zahtijevaju određene količine resursi smještaju na čvorove koje sadrže te resurse koji su dostupni u potrebna količina.<br>**IsPrimary** – ako imate više od jedne NodeType definirani bili sigurni da samo jedna postavljen na primarni s na vrijednost *true*, koji se gdje pokretanje servisa sustava. Sve ostale vrste čvor trebalo biti postavljeno na vrijednost *false*|
|**Čvorovi**|To su pojedinosti za svaku čvorove koje su dio klaster (Vrsta čvora, čvor naziva, IP adresa, kvara domenu i nadogradnje domenu čvora). Strojeva želite klaster će biti stvoren na treba se nalaziti s njihove IP adrese. <br> Ako koristite istu IP adresu za sve čvorove, zatim jedan okvir klaster stvoriti, koji možete koristiti za testiranje. Nemojte koristiti jedan okvir klastere za implementaciju radnih opterećenja radnog.|


### <a name="step-2-run-the-testconfiguration-script"></a>Korak 2: Pokrenuti skriptu TestConfiguration

Skripta TestConfiguration testira preduvjete infrastrukture kako je definirano u cluster.json da biste bili sigurni da se dodijeliti potrebne dozvole, međusobno povezani strojeva i druge atribute definiraju tako da možete uspjeti implementacijskih. Zapravo je mini najbolje prakse za analizu. Nastavit ćemo da biste dodali dodatne provjere valjanosti u ovaj alat s vremenom da biste Robusniji.

Ova skripta mogu se izvoditi na bilo kojem računalu koja ima pristup svim strojevima koji su navedeni kao čvorove u konfiguracijskoj datoteci klaster administratora. Stroju koji se izvršava Ova skripta mora biti dio klaster.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Korak 3: Pokrenuti skriptu klaster Stvori
Nakon promjene konfiguracije klaster u dokumentu JSON i dodati sve podatke čvor, pokrenite stvaranje klaster skriptu PowerShell *CreateServiceFabricCluster.ps1* iz mape paketa i prenesite na putu JSON konfiguracijskoj datoteci. Kada se dovrši, prihvatite EULA.

Ova skripta mogu se izvoditi na bilo kojem računalu koja ima pristup svim strojevima koji su navedeni kao čvorove u konfiguracijskoj datoteci klaster administratora. Na računalu koje se izvršava Ova skripta mora biti dio klaster.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] Zapisnici implementacije dostupne su lokalno na VM/stroju koji ste pokrenuli CreateServiceFabricCluster PowerShell. Se ona nalazi u podmapu pod nazivom DeploymentTraces u mapi koju ste pokrenuli naredbu komponente PowerShell. Da biste vidjeli ako tkanina servis je uveden pravilno na računalo, možete pronaći instalirane datoteke u direktoriju C:\ProgramData i FabricHost.exe i Fabric.exe procesi mogu se vidjeti pokrenutom u upravitelju zadataka.

### <a name="step-4-connect-to-the-cluster"></a>Korak 4: Povezivanje s klaster

Da biste povezali sigurne klaster, potražite u članku [tkanina servis za povezivanje s sigurne klaster](service-fabric-connect-to-secure-cluster.md).

Da biste povezali nesigurnom klaster, pokrenite sljedeću naredbu komponente PowerShell:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Korak 5: Pozovite Explorer tkanina servisa

Sada se možete povezati s klaster pomoću servisa ili izravno iz jednog od računala s http://localhost:19080/Explorer/index.html ili daljinski http://< programa Explorer tkanina*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Dodavanje i uklanjanje čvorove

Možete dodati ili ukloniti čvorove za svoj klaster samostalne tkanina servisa kao što je promijene svoje poslovne potrebe. Potražite u članku [Dodavanje i uklanjanje čvorove na servis tkanina samostalne klaster](service-fabric-cluster-windows-server-add-remove-nodes.md) detaljne upute.


## <a name="remove-a-cluster"></a>Uklanjanje klaster

Da biste uklonili klaster, pokrenite skriptu PowerShell *RemoveServiceFabricCluster.ps1* iz mape paketa i prenesite na putu JSON konfiguracijskoj datoteci. Po želji možete navesti mjesto za zapisnik brisanja.

Ova skripta mogu se izvoditi na bilo kojem računalu koja ima pristup svim strojevima koji su navedeni kao čvorove u konfiguracijskoj datoteci klaster administratora. Na računalu koje se izvršava Ova skripta mora biti dio klaster.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Telemetrijskih podataka koji se prikupljaju i upute za uključivanje iz njega

Po zadanom proizvoda prikuplja telemetrijskih o korištenju servisa tkanina radi poboljšanja proizvoda. Najbolja praksa analizatora koji se izvodi u sklopu provjere instalacijski program za povezivanje za [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Ako nije dostupna, osim ako budućih telemetrijskih neće uspjeti postavljanje.

  1. Kanal za telemetriju će se pokušati sljedeće podatke želite poslati [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) jednom svaki dan. To je najbolje trud Prenesi, a ne utječe na funkcionalnost klaster. Za telemetriju samo pošalje čvor koji se izvodi na prebacivanje Upravitelj primarni. Nema čvorove pošaljite telemetrijskih.

  2. Za telemetriju sastoji se od sljedećeg:

-            Reporting services
-            Broj ServiceTypes
-            Broj aplikacija
-            Broj ApplicationUpgrades
-            Broj FailoverUnits
-            Broj InBuildFailoverUnits
-            Broj UnhealthyFailoverUnits
-            Broj replike
-            Broj InBuildReplicas
-            Broj StandByReplicas
-            Broj OfflineReplicas
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Broj čvorove
-            IsContextComplete: True i False
-            ClusterId: To je GUID slučajno generirane za svaki klaster
-            ServiceFabricVersion
-             IP adresa virtualnog računala ili računala s kojeg je prenijeli u telemetrijskih


Da biste onemogućili telemetrijskih, dodajte sljedeće *Svojstva* u datoteci config klaster: *enableTelemetry: false*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>Pregled značajki programa paketa

Ništa.

>[AZURE.NOTE] Novim [GA verziju klaster samostalne za Windows Server (verzija 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), možete nadograditi svoj klaster buduća izdanja automatski ili ručno. Budući da ova značajka nije dostupna u verzijama pretpregled, morat ćete stvaranje klaster pomoću verzije GA i migriranje podataka i aplikacije iz skupine pretpregled. Ostanite definirati dodatne informacije o toj značajci.


## <a name="next-steps"></a>Daljnji koraci
- [Konfiguriranje postavki za samostalne Windows klaster](service-fabric-cluster-manifest.md)
- [Dodavanje i uklanjanje čvorove klaster za servis tkanina samostalni](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Stvaranje samostalne klaster tkanina servisa Azure VMs sa sustavom Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Sigurne samostalne klaster u sustavu Windows pomoću sigurnost sustava Windows](service-fabric-windows-cluster-windows-security.md)
- [Sigurne samostalne klaster u sustavu Windows pomoću X509 certifikata](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
