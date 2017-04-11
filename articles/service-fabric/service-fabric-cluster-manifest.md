<properties
   pageTitle="Konfiguriranje svoj klaster samostalne | Microsoft Azure"
   description="U ovom se članku objašnjava kako konfigurirati samostalne ili privatne klaster tkanina servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Konfiguriranje postavki za samostalne Windows klaster

U ovom se članku objašnjava kako konfigurirati klaster servisa tkanina samostalne pomoću datoteke _**ClusterConfig.JSON**_ . Možete koristiti tu datoteku da biste naveli podatke kao što su čvorove tkanina servisa i njihovih IP adrese, različite vrste čvorove klaster, konfiguracije sigurnosti kao i mrežna topologija pomoću domene kvara/nadogradnje za svoj klaster samostalne.

Kada [preuzmite paket servisa tkanina samostalni](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), nekoliko primjera ClusterConfig.JSON datoteke se preuzimaju na računalu rad. Uzorci pojavljuju *DevCluster* u nazivu će vam pomoći da klaster sa sve tri čvorove na istom računalu, kao što su logički čvorove. Iz ove, barem jedan čvor mora biti označene kao primarni čvor. Ovaj klaster korisne su za razvojno ili probno okruženje i nisu podržana kao radnog klaster. Uzorci pojavljuju *MultiMachine* u njihova imena će vam pomoći da na klaster kvalitete radnog sa svakom čvor na zasebnom računalo. Broj primarni čvorove za ove klaster će se temeljiti na [razinu pouzdanosti](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* i *ClusterConfig.Unsecure.MultiMachine.JSON* Demonstracija da biste stvorili nezaštićenu test ili radni klaster odnosno. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* i *ClusterConfig.Windows.MultiMachine.JSON* pokazati kako stvoriti test ili radni klaster zaštićen [Sigurnost sustava Windows](service-fabric-windows-cluster-windows-security.md).

3. *ClusterConfig.X509.DevCluster.JSON* i *ClusterConfig.X509.MultiMachine.JSON* pokazati kako stvoriti test ili radni klaster zaštićen [X509 certifikata temeljenu na](service-fabric-windows-cluster-x509-security.md). 


Sada ćemo pregledajte različite dijelove _**ClusterConfig.JSON**_ datoteke u obliku ispod.

## <a name="general-cluster-configurations"></a>Konfiguracija općih klaster
To obuhvaća određene konfiguracije općenite klaster kao što je prikazano u nastavku isječak JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Bilo koji neslužbeni naziv možete dati svoj klaster servisa tkanina dodjelom **naziva** varijabli. **ClusterConfigurationVersion** je broj verzije svoj klaster potrebno je povećati svaki put kada nadogradite na svoj klaster tkanina servisa. No ostavite **apiVersion** zadana vrijednost.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Čvorovi na klaster
Pomoću sekcija **čvorove** kao u sljedećem isječak možete konfigurirati čvorove na svoj klaster tkanina servisa.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Servis tkanina klaster mora sadržavati najmanje 3 čvorove. U ovom se odjeljku možete dodati više čvorove po postavki. U sljedećoj su tablici objašnjava konfiguriranje postavki za svaki čvor.

|**Konfiguriranje čvor**|**Opis**|
|-----------------------|--------------------------|
|nodeName|Bilo koji neslužbeni naziv možete dati čvor.|
|IPAdresa|Saznajte više IP adresa vaša čvora otvaranjem prozoru za naredbe i upisivanjem `ipconfig`. Imajte na umu IPV4 adresa i njezino dodavanje **IPAdresa** varijabli.|
|nodeTypeRef|Svaki čvor se mogu dodijeliti različite čvor vrsta. [Vrste čvor](#nodetypes) su definirani u odjeljak u nastavku.|
|faultDomain|Domena kvara omogućiti klaster administratora da biste definirali fizičke čvorove koje možda neće uspjeti istovremeno zbog zajedničke fizičke ovisnosti.|
|upgradeDomain|Nadogradnja domene opisuju skupova čvorove koja su isključena radi nadogradnje servisa tkanina pri o istovremeno. Možete odabrati koji će se čvorovi dodijeliti domene koje nadogradnje dok ne ograničen svim zahtjevima za fizičke.| 


## <a name="cluster-properties"></a>Svojstva klaster

U odjeljku **Svojstva** u ClusterConfig.JSON koristi se za konfiguraciju klaster na sljedeći način.

<a id="reliability"></a>
### <a name="reliability"></a>Pouzdanost 
U odjeljku **reliabilityLevel** definira broj primjeraka servisa sustava koji se mogu se izvoditi na primarni čvorove klaster. Time se povećava pouzdanost tih servisa i zato klaster. Možete postaviti ovu varijablu *bronzani*, *srebrni*, *žutu* ili *Platinaste* za kopije 3, 5, 7 ili 9 od tih servisa odnosno. Pogledajte primjer ispod.

    "reliabilityLevel": "Bronze",
    
Imajte na umu da Budući da primarni čvor izvođenja jednog primjerka servisa sustava, koji su vam potrebni najmanje 3 primarni čvorove za *bronzani*, 5 za *Srebrno*7 za *Zlatne* i 9 za razine pouzdanosti *Platinaste* .


### <a name="diagnostics"></a>Dijagnostika
U odjeljku **diagnosticsStore** omogućuje konfiguriranje parametara da biste omogućili za dijagnostiku i otklanjanje poteškoća čvor ili klaster pogreške, kao što je prikazano u sljedećim isječka. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metapodataka** opis vaše klaster Dijagnostika i može se postaviti po postavki. Ove varijable pomoći u prikupljanje ETW zapisnika praćenja, rušenje ispisi kao i mjerača performansi. Dodatne informacije o zapisnicima praćenja ETW pročitajte [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) i [Praćenje ETW](https://msdn.microsoft.com/library/ms751538.aspx) . Sve zapisnike uključujući [pasti ispisi](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) i [mjerača performansi](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) možete upućivati na **connectionString** mapu na vašem računalu. *AzureStorage* možete koristiti i za pohranu Dijagnostika. Potražite u nastavku uzastopna uzorka.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Sigurnost 
Odjeljak **Sigurnost** nužni za klaster tkanina servisa sigurne samostalne. Sljedeći isječak prikazuje dio ovaj odjeljak.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metapodaci** opis svoj klaster sigurne i možete postaviti po postavki. **ClusterCredentialType** i **ServerCredentialType** određivanje vrste vrijednosnice koja će implementirati klaster i čvorovi. Moguće je postaviti za potvrdu temeljenu ili *X509* ili programa Azure Active Directory temeljenu na *Windows* . Ostatak odjeljak **Sigurnost** će se temeljiti tipu sigurnost. Pročitajte [Certifikati temeljenu na samostalne](service-fabric-windows-cluster-x509-security.md) ili [Sigurnost sustava Windows u zasebnim klaster](service-fabric-windows-cluster-windows-security.md) informacije o tome kako ispunite i ostale odjeljka **Sigurnost** .


<a id="nodetypes"></a>
### <a name="node-types"></a>Vrste čvor
U odjeljku **nodeTypes** opisuju vrstu čvorove koja ima svoj klaster. Vrsta barem jedan čvora mora biti naveden za klaster, kao što je prikazano u nastavku isječka. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**Naziv** je neslužbeni naziv za tu vrstu određeni čvor. [Da biste stvorili čvor ove vrste čvor, dodijelite njegov neslužbeni naziv varijabli **nodeTypeRef** za taj čvor kao spomenutih.](#clusternodes) Za svaku vrstu čvor definirati krajnje točke veze koja će se koristiti. Možete odabrati bilo koji broj priključka za te krajnje točke vezu pod uvjetom da nisu u sukobu s bilo koje druge krajnje točke u ovoj grupi. Više čvor klasteru, pojavit će se jedan ili više primarni čvorove (odnosno **isPrimary** postavljen na *true*), ovisno o [**reliabilityLevel**](#reliability). Pročitajte [uvjetima planiranja kapaciteta servisa tkanina klaster](service-fabric-cluster-capacity.md) za podatke **nodeTypes** i **reliabilityLevel** vrijednosti, a zatim Saznajte što su primarni i vrste koje nisu primarni čvor. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Krajnje točke za konfiguriranje vrste čvor

- *clientConnectionEndpointPort* je priključak koristi klijent za povezivanje klaster, prilikom korištenja API-ji klijenta. 
- *clusterConnectionEndpointPort* je priključak na kojoj se čvorove komunikaciju.
- *leaseDriverEndpointPort* je priključak koji se koristi klaster Zakup upravljački program da biste saznali jesu li li čvorove aktivan. 
- *serviceConnectionEndpointPort* je priključak koristi aplikacija i servisa u uveden u čvor, možete komunicirati s klijent za servis tkanina na tom određeni čvor.
- *httpGatewayEndpointPort* je priključak koristi tkanina Explorer servisa za povezivanje s klaster.
- *ephemeralPorts* su [dinamički priključke koristi OS-a](https://support.microsoft.com/kb/929851). Servis tkanina koristit dio kao priključke aplikacije i preostalih bit će dostupan za os-a. On će i mapirajte ovaj raspon postojeći raspon koje su prisutne u OS, pa svrhe, možete koristiti raspona u ogledne datoteke JSON. Morate da biste bili sigurni da je razlika između početka i završetka priključke barem 255. 
- *applicationPorts* su priključci koji će se koristiti tkanina servisa aplikacija. To mora biti podskup na *ephemeralPorts*, dovoljno da prekrije obavezne krajnjoj točki aplikacija. Servis tkanina koristit će ih kad god novi priključci su potrebni, kao i voditi brigu o otvaranju Vatrozid za priključke. 
- *reverseProxyEndpointPort* je krajnje neobavezno povratnog proxy. Dodatne informacije potražite u članku [Obrnuti Proxy tkanina za usluge](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Ostale postavke
U odjeljku **fabricSettings** omogućuje postavljanje korijenskog direktorija za servis tkanina podataka i zapisnika. Možete prilagoditi te samo tijekom stvaranja početnog klaster. Potražite u nastavku uzastopna uzorka ovog odjeljka.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Preporučujemo da pomoću koje nisu OS pogon kao FabricDataRoot i FabricLogRoot kao pruža dodatne pouzdanosti protiv OS ruši. Imajte na umu da Ako prilagođavate samo korijensko podataka, zatim korijenski zapisnika će se nalaziti jednu razinu ispod korijenski podataka.


## <a name="next-steps"></a>Daljnji koraci

Nakon što dodate cijeli dokument ClusterConfig.JSON konfiguriran u skladu sa postavu klaster samostalni, možete implementirati svoj klaster pomoću navedenih u članku [Stvaranje programa klaster tkanina servisa Azure na lokalni ili u oblaku](service-fabric-cluster-creation-for-windows-server.md) , a zatim nastavite s [vizualizacija svoj klaster pomoću programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md).


