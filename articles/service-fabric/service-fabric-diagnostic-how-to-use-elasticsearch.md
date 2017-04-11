<properties
   pageTitle="Korištenje Elasticsearch kao tkanina usluge iz trgovine praćenje aplikacije | Microsoft Azure"
   description="U članku se opisuje kako tkanina servisa aplikacije mogu koristiti Elasticsearch i Kibana za pohranu, indeks i pretraživati aplikacije kašnjenja (zapisnika)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Korištenje Elasticsearch kao praćenje aplikacije servisa tkanina pohrana
## <a name="introduction"></a>Uvod
U ovom se članku opisuje kako [Tkanina servisa Azure](https://azure.microsoft.com/documentation/services/service-fabric/) aplikacije mogu koristiti **Elasticsearch** i **Kibana** za pohranu praćenje aplikacije, indeksiranja i pretraživanja. [Elasticsearch](https://www.elastic.co/guide/index.html) je Otvori izvor, raspodijeljeno i skalabilni u stvarnom vremenu pretraživanja i analitiku mehanizam koje je dobro prikladniji za taj zadatak. Moguće je instalirati na Windows i Linux virtualnim strojevima izvodi u Microsoft Azure. Elasticsearch mogu učinkovito procesa *strukturiranih* kašnjenja proizvodi pomoću tehnologije kao što su **Događaj praćenje za sustav Windows (ETW)**.

ETW koristi servis tkanina runtime dijagnostičke informacije za izvora (kašnjenja). Je preporučeni je način za to izvora dijagnostičke informacije, aplikacijama tkanina servisa. Korištenje iste mehanizam omogućuje za korelacije između runtime navedeni i aplikacije navedeni kašnjenja i čini vam za otklanjanje poteškoća. Servis tkanina predlošcima projekata u Visual Studio obuhvaćaju (koja se temelji na predmete.NET **EventSource** ) API zapisivanje koji emits ETW kašnjenja po zadanom. Općenito pregled tkanina servisa aplikacija praćenje pomoću ETW potražite u članku [praćenja i dijagnosticiranje services u postavi za razvoj lokalnog računala](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Za kašnjenja koja će se prikazivati u Elasticsearch, koje su im potrebne zabilježene pri čvorove klaster tkanina servisa u stvarnom vremenu (pokrenutom aplikacije) i poslana krajnje Elasticsearch. Postoje dvije glavne mogućnosti za praćenje snimanje:

+ **U tijeku hvatanje praćenja**  
Aplikaciju ili preciznije, proces servisa je zadužen za slanje dijagnostičkih podataka i praćenje Store (Elasticsearch).

+ **Dohvaćanje-postupak praćenja**  
Zasebnom agent je dohvaćanje kašnjenja proces servisa ili procesa i slanjem spremište praćenja.

U nastavku ćemo opisuje kako postaviti Elasticsearch na Azure, raspravljati stručnjaka i protiv i snimite mogućnosti, a objašnjavaju kako Konfiguriranje servisa za servis tkanina slanje podataka u Elasticsearch.


## <a name="set-up-elasticsearch-on-azure"></a>Postavljanje Elasticsearch na Azure
Većina jasan način da biste postavili servis Elasticsearch Azure se putem [**Voditelj resursa Azure predložaka**](../azure-resource-manager/resource-group-overview.md). Brzi početak rada Azure predložaka spremište dostupna potpun [Voditelj resursa za brzi početak rada Azure predložak za Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) . Ovaj predložak koristi računi zasebnom prostora za pohranu za skaliranje jedinice (grupa čvorove). Također možete Dodjela zasebnom klijenta i poslužitelja čvorove s različitim konfiguracijama i različite brojeve podataka diskova priložene.

Ovdje, koristimo drugi predložak pod nazivom **ES MultiNode** [spremište Azure dijagnostičke alate](https://github.com/Azure/azure-diagnostics-tools). Ovaj predložak je lakše koristiti i stvara sustava Elasticsearch klaster zaštićen HTTP osnovnu provjeru autentičnosti. Prije nego što nastavite, preuzmite spremište iz GitHub na vaše računalo (po kloniranje spremište ili preuzimanje datoteke zip). Predložak ES MultiNode nalazi se u mapi s istim nazivom.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Priprema računala da pokreću skripte Elasticsearch instalacije
Da biste koristili predložak ES MultiNode najjednostavnije kroz navedeni skriptu Azure PowerShell pod nazivom `CreateElasticSearchCluster`. Da biste koristili ovu skriptu, morate instalirati PowerShell moduli i alat s nazivom **openssl**. Drugu mogućnost je potrebno za stvaranje ključa za SSH koje je moguće koristiti za daljinsko administriranje svoj klaster Elasticsearch.

`CreateElasticSearchCluster`Skripta namijenjena jednostavnost s predloškom ES MultiNode s računala za Windows. Nije moguće koristiti predložak na računalo koje nisu u sustavu Windows, no taj scenarij izlazi iz ovog članka.

1. Ako još niste ih već instalirali, instalirajte [**modula Azure PowerShell**](http://aka.ms/webpi-azps). Kada se to od vas zatraži, kliknite **Pokreni**, zatim **instalirajte**. Potreban je Azure PowerShell 1.3 ili novija verzija.

2. Alat za **openssl** je sve obuhvaćeno raspodjele [**Brojka za Windows**](http://www.git-scm.com/downloads). Ako ste još niste učinili, instalirajte [Brojka za Windows](http://www.git-scm.com/downloads) . (Zadane mogućnosti instalacije su u redu).

3. Uz pretpostavku da je brojka je instaliran, ali neće biti uvršteni u putu sustava, otvorite prozor Microsoft Azure PowerShell i izvršite sljedeće naredbe:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Zamjena na `<Git installation folder>` lokacijom brojka na vašem računalu Zadana je vrijednost **"C:\Program Files\Git"**. Imajte na umu znak točka sa zarezom na početku prvi put.

4. Provjerite da ste prijavljeni na Azure (putem [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet) i koju ste odabrali pretplatu u koju treba koristiti za stvaranje svoj klaster Elastic pretraživanja. Možete provjeriti da točan pretplate odabran pomoću `Get-AzureRmContext` i `Get-AzureRmSubscription` cmdleta.

5. Ako to još niste učinili, promijenite trenutnog direktorija ES MultiNode mapu.

### <a name="run-the-createelasticsearchcluster-script"></a>Pokrenuti skriptu CreateElasticSearchCluster
Prije nego što pokrenete skriptu, otvorite na `azuredeploy-parameters.json` datoteke i provjerite je li ili unesite vrijednosti za parametre skripte. Dostupna je sljedećih parametara:

|Naziv parametra           |Opis|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Naziv koji se koristi da biste stvorili javno vidljive DNS naziv za klaster Elastic pretraživanja (dodavanjem domene Azure regije navedene naziv). Ako, na primjer, ako ovu vrijednost parametra je "myBigCluster" i odabranom Azure regija je Zapad SAD-a, dobivene DNS naziv za klaster je myBigCluster.westus.cloudapp.azure.com. <br /><br />Ovaj naziv služi i kao naziv korijenske za mnoge artefakte pridružene klaster Elastic pretraživanja, kao što su nazivi čvor podataka.|
|adminUsername           |Naziv računa administrator za upravljanje klaster Elastic pretraživanja (odgovarajuće tipke SSH automatski se generiraju).|
|dataNodeCount           |Broj čvorove u skupini Elastic pretraživanja. Trenutna verzija skripte ne razlikuje između podataka i upita čvorove; sve čvorove reproducirati i uloge. Po zadanom je 3 čvorove.|
|dataDiskSize            |Veličina podataka diskova (GB) dodijeljen za svaki čvor podataka. Svaki čvor prima 4 diskova podataka, isključivo trakom za uslugu Elastic pretraživanja.|
|regija                  |Naziv Azure regije gdje mora biti smješten klaster Elastic pretraživanja.|
|esUserName              |Korisničko ime korisnika koji je konfiguriran da biste imali pristup ES klaster (primjenjuje Osnovna provjera autentičnosti s HTTP-a). Lozinka nije dio datoteke parametre i mora se navesti kada `CreateElasticSearchCluster` skripte se poziva.|
|vmSizeDataNodes         |Veličina Azure virtualnog računala za čvorove klaster Elastic pretraživanja. Po zadanom je Standard_D2.|

Sada ste spremni pokrenuti skriptu. Problema sljedeću naredbu:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

gdje 

|Naziv parametra skripte    |Opis|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Naziv grupe Azure resursa koja će sadržavati sve resurse klaster Elastic pretraživanja.|
|`<azure-region>`         |Naziv Azure regije gdje treba stvoriti klaster Elastic pretraživanja.|         
|`<es-password>`          |Lozinka za korisnika Elastic pretraživanja.|

>[AZURE.NOTE] Ako nailazite na NullReferenceException iz cmdlet Test AzureResourceGroup, ste zaboravili da biste se prijavili Azure (`Add-AzureRmAccount`).

Ako dođe do pogreške s pokretanjem skripte i odrediti da je pogreška uzrok vrijednost parametra pogrešan predložak, ispravite parametar datoteku i ponovno pokrenuti skriptu pod nazivom resursa za različite grupe. Možete i ponovno koristiti isti naziv grupe resursa, a skripte očistiti staru dodavanjem u `-RemoveExistingResourceGroup` parametar skripte poziva.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Rezultat izvođenja CreateElasticSearchCluster skripte
Nakon pokretanja na `CreateElasticSearchCluster` skripti sljedeće glavni artefakte stvorit će se. U ovom primjeru pretpostavimo da ste koristili "myBigCluster" kao vrijednost na `dnsNameForLoadBalancerIP` parametar i je li područje u kojem ste stvorili klaster Zapad SAD-a.

|Artefakt|Naziv, mjesto i napomene|
|----------------------------------|----------------------------------|
|SSH ključ za daljinsko administriranje |datoteka myBigCluster.key (u direktoriju iz kojeg se pokreće na CreateElasticSearchCluster). <br /><br />Ključni datoteku poslužite da biste se povezali čvor administrator i (putem čvor administratore) za čvorove podataka u klasteru.|
|Administrator čvor                        |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Namjenski VM za daljinsko Elasticsearch klaster administriranje – samo onaj koji omogućuje vanjske veze SSH. Pokreće se na istom virtualne mreže kao sve čvorove klaster Elasticsearch, ali se ne pokreće servisi Elasticsearch.|
|Čvorovi podataka                        |myBigCluster1... myBigCluster*N* <br /><br />Čvorovi podatke koji su pokrenuti Elasticsearch i Kibana servise. Možete se povezati putem SSH svaki čvor, ali samo putem čvor administrator.|
|Elasticsearch klaster             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Primarni krajnja točka za klaster Elasticsearch (bilješke sufiks /es). Je zaštićena upravljanjem Osnovna provjera autentičnosti HTTP (vjerodajnice nisu navedeni parametri esUserName/esPassword predloška ES MultiNode). Klaster također sadrži zaglavlje dodatak instalirali (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) za administraciju osnovni klaster.|
|Servis za Kibana                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Servis Kibana postavljena tako da biste prikazali podatke iz stvoreni skupine Elasticsearch. Je zaštićena upravljanjem iste vjerodajnice za provjeru autentičnosti kao klaster sam.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>U – postupak nasuprot hvatanje-postupak praćenja
U uvodu, ne možemo spomenuti dva osnovnim načinima prikupljanje dijagnostičkih podataka: u tijeku i postupak. Svaka ima prednosti i slabosti.

Prednosti **u tijeku praćenje hvatanje** obuhvaćaju sljedeće:

1. *Jednostavnu konfiguraciju i implementaciju*

    * Konfiguracija dijagnostičkih podataka zbirke je samo dio konfiguracije aplikacije. Jednostavno je uvijek sinkroniziranosti je "" s ostatkom aplikacije.

    * Konfiguracija po aplikaciji ili po servis je jednostavno može postići.

    * Dohvaćanje-postupak praćenja obično zahtijeva zasebne implementacije i konfiguriranje dijagnostičkog agent koji je vrlo administrativnih zadataka i izvor potencijalne pogreške. Agent za određenu tehnologiju često omogućuje samo jedna pojava agent za virtualnog računala (čvora). To znači da tu konfiguraciju za skup dijagnostičkih konfiguracije je zajednički koristiti sve aplikacije i servise taj čvor.

2. *Fleksibilnost*

    * Aplikaciju možete poslati podatke mjesto na kojem je potrebno da biste prešli, dok god postoji biblioteci klijent koji podržava ciljano podatkovnom sustavu prostora za pohranu. Novi primatelji mogu se dodati kao željeni.

    * Složena pravila snimke, filtriranja i prikupljanje podataka može se implementirati.

    * Primatelji podataka koje podržava agenta često ograničen-postupak praćenja snimanje. Extensible su neke agenata.

3. *Pristup podacima interne i kontekst*

    * Dijagnostičke podsustav izvodi unutar postupku aplikacija i servisa možete jednostavno dodavanje kašnjenja s informacijama o kontekstu.

    * U pristup postupak podataka mora biti poslana agent putem neke mehanizam inter-procesa komunikacije, kao što su događaj praćenje za Windows. U ovom mehanizam možete nametnuti dodatna ograničenja.

Prednosti **-postupak praćenja hvatanje** obuhvaćaju sljedeće:

1. *Ako ste omogućuje prikupljanje rušenje i praćenje aplikacije*

    * Praćenje u tijeku hvatanje možda neće biti uspješna ako aplikacija ne pokreće ili ruši. Agent neovisno ima mnogo bolji izgledi hvatanje presudne informacije o otklanjanju poteškoća.<br /><br />

2. *Dospijeće, odličnim i dokazana performansi*

    * Agent razvio dobavljača platformu (kao što je Microsoft Azure Dijagnostika agent) je primjenjuju stroge testiranje te Bitka utvrđivanje.

    * S praćenja u tijeku snimanje, potrebno je poduzeti da biste bili sigurni da aktivnost slanje dijagnostičkih podataka iz proces aplikacija ne ometaju glavni zadaci aplikacije ili predstavljanje tempiranja ili performanse problema. Agent za samostalno izvodi manje podložni te probleme i posebno dizajnirane da biste ograničili njegov utjecaj na sustav.

Nije moguće kombinirati i prednosti oba pristupa. Uistinu, možda je najbolje rješenje za mnoge aplikacije.

Ovdje, koristimo **Microsoft.Diagnostic.Listeners biblioteke** i praćenje u tijeku hvatanje slanje podataka iz servisa tkanina aplikacije programa Elasticsearch klaster.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Slanje dijagnostičkih podataka u Elasticsearch pomoću slušače biblioteke
Biblioteka Microsoft.Diagnostic.Listeners je dio aplikacije za servis tkanina PartyCluster uzorka. Da biste koristili:

1. Preuzimanje [oglednih PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) iz GitHub.

2. Kopirajte projekata Microsoft.Diagnostics.Listeners i Microsoft.Diagnostics.Listeners.Fabric (cijele mape) iz imenika uzorka PartyCluster u mapu rješenje aplikacije koja bi trebao se podaci poslati Elasticsearch.

3. Otvorite rješenje cilj, desnom tipkom miša kliknite čvor rješenje u pregledniku rješenja i odaberite **Dodaj postojeći projekt**. Dodajte projekta Microsoft.Diagnostics.Listeners rješenje. Ponovite isti Microsoft.Diagnostics.Listeners.Fabric projekta.

4. Dodavanje reference projekta iz vaše project(s) servisa dva dodane projektima. (Svakog servisa koji bi trebao slanje podataka u Elasticsearch trebalo pozivati Microsoft.Diagnostics.EventListeners i Microsoft.Diagnostics.EventListeners.Fabric).

    ![Project reference na biblioteke Microsoft.Diagnostics.EventListeners i Microsoft.Diagnostics.EventListeners.Fabric][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Dostupnost servisa tkanina Općenito izdanje i Microsoft.Diagnostics.Tracing Nuget paketa
Aplikacija načinjene pomoću servisa tkanina Općenito dostupnost izdanje (2.0.135, objavio ožujak 31, 2016) ciljne **.NET Framework 4.5.2**. Ova verzija je najveće verzija .NET Framework podržava Azure trenutku izdanje GA. Nažalost, ova verzija framework nedostaju određene API-ji EventListener koju je potrebno Microsoft.Diagnostics.Listeners biblioteke. Budući da EventSource (komponenta koja je na temelju zapisivanje API-ji u aplikacijama tkanina) i EventListener čvrsto povezano, svaki projekt koji koristi biblioteka Microsoft.Diagnostics.Listeners morate koristiti zamjenski implementacija EventSource. Ova implementacija osigurava **Microsoft.Diagnostics.Tracing Nuget paketa**, autor Microsoft. Paket nije potpuno kompatibilan s EventSource obuhvaćeno framework, tako da nema promjena kod mora biti potrebno osim promjene referencirani prostor naziva.

Da biste počeli koristiti implementaciju Microsoft.Diagnostics.Tracing klase EventSource, slijedite ove korake za svaki projekt usluge koje su potrebne za slanje podataka u Elasticsearch:

1. Desnom tipkom miša kliknite na projektu servisa, a zatim odaberite **Upravljanje Nuget paketa**.

2. Prijeđite na izvor paket nuget.org (Ako već nije potvrđen), a zatim potražite "**Microsoft.Diagnostics.Tracing**".

3. Instalacija na `Microsoft.Diagnostics.Tracing.EventSource` paket (i njezine ovisnosti).

4. Otvorite datoteku **ServiceEventSource.cs** ili **ActorEventSource.cs** u projektu servisa i zamijeni u `using System.Diagnostics.Tracing` uputa pri vrhu datoteke s na `using Microsoft.Diagnostics.Tracing` uputa.

Ove korake neće biti potrebno kada **.NET Framework 4.6** podržava Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch ga slušatelj instancijacije i konfiguracija
U posljednjem koraku za slanje dijagnostičkih podataka Elasticsearch će stvoriti instancu `ElasticSearchListener` i konfigurirati vezu podacima Elasticsearch. Ga slušatelj automatski se pohranjuju Svi događaji potenciju putem EventSource klase definirano u programu project servisa. Ga mora biti aktivnosti tijekom života servisa, pa je najbolje mjesto ga stvoriti u Inicijalizacija kod usluge. Evo kako može izgledati kod Inicijalizacija bez praćenja stanja servisa nakon potrebne promjene (dodatke pokazali u komentarima počevši od `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Podatkovne veze Elasticsearch se mora staviti u posebnom odjeljku, a ne u datoteci konfiguracije servisa (**PackageRoot\Config\Settings.xml**). Naziv sekcije mora odgovarati proslijeđena vrijednost u `FabricConfigurationProvider` Graditelj, na primjer:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Vrijednosti `serviceUri`, `userName` i `password` parametre koje odgovaraju Elasticsearch klaster krajnjoj točki adresu, Elasticsearch korisničko ime i lozinku, odnosno. `indexNamePrefix`je prefiks za kazala Elasticsearch; Biblioteka Microsoft.Diagnostics.Listeners svakodnevno stvara novi indeks za svoje podatke.

### <a name="verification"></a>Provjera
To je to! Sada kad god se pokreće servis, pokretanja slanja kašnjenja sa servisom Elasticsearch naveden u konfiguraciji. Možete provjeriti to tako da otvorite korisničkog Sučelja Kibana povezan s Elasticsearch instancu cilj. U našem primjeru adresu stranice je http://myBigCluster.westus.cloudapp.azure.com/. Provjerite indeksi s prefiksom naziv za na `ElasticSearchListener` instanci su uistinu stvorili i popunjavaju podacima.

![Prikazuje Kibana PartyCluster aplikacije događaja][2]

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o dijagnosticiranje i nadzor servisa tkanina servisa](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
