<properties
   pageTitle="Arhitektura referenca Azure - IaaS: Stvaranje skupa stabala resursa u servisu Active Directory u Azure | Microsoft Azure"
   description="Kako stvoriti pouzdano domene servisa Active Directory u Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Stvaranje skupa stabala resursa u Active Directory Directory Services (ZBRAJA) u Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje kako stvoriti programa domene servisa Active Directory u Azure koja je neovisan, no pouzdanih, domena u vaše lokalne skupa stabala.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. Ovu referenca arhitektura koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Tvrtka ili ustanova koja se pokreće Active Directory (AD) lokalnog možda skupa stabala je comprising mnogo različitih domena. Na primjer, možete stvoriti pojedinačne domenama za različite odjele ili suborganizations ili nove domene možda dodani kao rezultat acquisition ili spajanja drugim tvrtkama ili ustanovama. Domene možete koristiti za davanje odvajanja područja funkcionalnosti mora biti zadržane odvojeni, vjerojatno sigurnosnih razloga, ali možete omogućiti zajedničko korištenje informacija između domene uspostavljanjem pouzdani odnosi.

Tvrtkom ili ustanovom koja koristi zasebnom domene možete iskoristiti Azure po relocating jedan ili više od tih domena u oblak. Osim toga, tvrtkom ili ustanovom možda želite zadržati sve resurse oblaka logično razlikuju od onih za te čuvanju lokalnog i pohranili podatke o oblaka resursa u imeniku vlastite domene i održava u oblaku.

Možete implementirati servisa Active Directory u Azure na nekoliko različitih načina, kao što je opisano u člancima [Proširivanje servisa Active Directory za Azure] [ extending-ad-to-azure] i [Implementaciju Azure Active Directory][implementing-aad]. Ovaj dokument usredotočuje se na jednu određenu scenarij: stvaranje domene u oblaku koje se razlikuju od onih za sve domene održava lokalnog, no koji imaju odnos pouzdanosti lokalne domene u sustavu. 

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Održavanje sigurnosti odvojenosti objekata i identiteti održava u oblaku.

- Migracija pojedinačne domene s lokalnim u oblak.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističe važne komponente u toj arhitekturi (*kliknite da biste povećali*). Dodatne informacije o elementima zasivljen izlaz čitati [implementacijom arhitekturu mreže sigurne hibridnog u Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementaciju arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Lokalne mreže.** Lokalne mreže sadrži vlastiti skup stabala AD i domenama.

- **AD poslužitelji.** To su kontrolera domena implementacijom directory services (AD DS) izvodi kao VMs u oblaku. Sljedećim poslužiteljima hostira skupa stabala je koja sadrži jednu ili više domena, osim onih iz tih nalazi lokalnog.

- **Jednosmjerna pouzdani odnos.** U primjeru u dijagramu prikazuje jednosmjerna pouzdanost s domenom u oblaka za lokalne domene. Taj odnos lokalnim korisnicima omogućuje pristup resurse na domeni u oblaku, ali ne, Suprotno tome. To je uobičajena slučaja. Međutim, možete stvoriti dvosmjerna pouzdanost Ako korisnici oblaka i zahtijevaju pristup lokalnog resursa.

- **Active Directory podmreže.** Poslužitelji AD DS nalaze se u zasebnom podmreže. Pravila NSG zaštita poslužitelja za AD DS i pružaju Vatrozid za promet iz nepoznatih izvora.

- **Podmreže web razina**, **podmreže sloju tvrtke**i **podataka sloju podmreže**. Ove podmreže glavnog računala poslužitelja i komponenata koje pokreću aplikacije u oblaku. Dodatne informacije potražite u članku [Pokretanje VMs za Arhitektura programa N sloja na Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. Resurse i VMs u ovom podmreže nalaze se u oblak domeni.

- **Azure pristupnika**. Pristupnik za Azure nudi veze između lokalne mreže i Azure VNet. To može biti [VPN vezu] [ azure-vpn-gateway] ili [Azure ExpressRoute][azure-expressroute]. Dodatne informacije potražite u članku [implementacijom arhitekturu mreže sigurne hibridnog u Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Preporuke

U ovom se odjeljku nalaze se popis preporuke na temelju ključne komponente potrebne za implementaciju osnovni arhitektura. Te preporuke pokrivaju:

- ZBRAJA postavke, a

- Pouzdanost konfiguracije odnos.

Preduvjeti dodatne ili različiti od onih koje se ovdje opisuju možda imate. Stavke u ovom odjeljku možete koristiti kao početnu točku za odlučuje kako prilagoditi arhitektura za vlastiti sustav.

### <a name="adds-recommendations"></a>ZBRAJA preporuke

Za određene preporuke o implementacije servisa Active Directory u oblak, pogledajte dokument [Proširivanje servisa Active Directory za Azure][extending-ad-to-azure]. U članku [smjernice za implementaciju sustava Windows Server Active Directory virtualnim računalima sustava na Azure] [ ad-azure-guidelines] sadrži detaljne informacije.

### <a name="trust-recommendations"></a>Preporuke za pouzdanost

Domena lokalne nalaze se u različitim skupa stabala iz domena u oblaku. Da biste omogućili provjera autentičnosti lokalne korisnike u oblaku, domena u oblak moraju vjerovati domenu za prijavu u skup stabala lokalnog. Isto tako, ako je u oblak nudi domenu za prijavu za vanjske korisnike, možda će biti potrebno za lokalni skup stabala koji će biti pouzdanim za domenu oblaka.

Možete uspostaviti trusts na razini skupa stabala stvaranjem [skupa stabala trusts][creating-forest-trusts], ili na razini domene stvaranjem [vanjske trusts][creating-external-trusts]. Skup stabala razine pouzdanosti možete stvoriti odnos između svih domena u dva šuma. Razine sigurnosti programa vanjske domene samo možete stvoriti odnos između dvije navedeni domene. Potrebno je stvoriti samo vanjske domene razine trusts između domena u različitim šuma.

Trusts može biti unidirectional (jednosmjerna) ili Dvosmjeran (dvosmjerni):

- Jednosmjerna pouzdanost korisnicima u jedan domene ili skup stabala (poznatom kao *dolazne* domene ili skup stabala) omogućuje pristup resursima održava u drugoj ( *odlazne* domene ili skup stabala). 

- Dvosmjerna pouzdanost korisnicima omogućuje u domeni ili skup stabala pristup resursi koji se održava u drugi.

U sljedećoj su tablici navedene pouzdanost konfiguracije nekoliko jednostavnih scenarijima za:

| Scenarij | Lokalni pouzdanost | Pouzdanost oblaka |
|----------|-------------------|-------------|
| Lokalni korisnici zahtijevaju pristup resursima u oblaku, ali ne krajnje bilješke | Jednosmjerna, dolazni | Jednosmjerna, odlazne |
| Korisnici u oblaku zahtijevaju pristup resursa koji se nalazi na lokalni, ali ne krajnje bilješke | Jednosmjerna, odlazne | Jednosmjerna, dolazni |
| Korisnici u oblaku i lokalnog potreban pristup resursima održava u oblak i lokalne | Dvosmjerna, dolazne i odlazne | Dvosmjerna, dolazne i odlazne |

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Korištenje su skupa stabala razine trusts. Ako uspostavili skupa stabala razine pouzdanosti između programa lokalnog skupa stabala i skup stabala u oblaku, ova pouzdanost prošireni drugim novi domenama stvorene u oba skupa stabala. Ako koristite domene omogućuju odvojenosti sigurnosnih razloga, razmislite o stvaranju trusts na razini domene samo. Domene razine trusts su koje nisu korištenje.

Specifične za AD sigurnosna pitanja vezana uz, potražite u članku u odjeljku *Sigurnosna pitanja vezana uz* [Proširivanje servisa Active Directory za Azure][extending-ad-to-azure].

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Implementacija najmanje dva kontrolera domena za svaku domenu. Time se omogućuje automatsko replikacije između poslužitelja. Stvaranje raspoloživost za VMs ulozi poslužitelja za AD zadužen za svaku domenu. Provjeriti postoje li barem dva poslužitelja u skupu. 

Uzmite u obzir određivanje jednom ili više poslužitelja u svakoj domeni kao [čekanja operacije matrice] [ standby-operations-masters] u slučaju prekida se veza s poslužiteljem ulozi FSMO uloge.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

AD je automatski podesiva za kontrolera domena koje su dio iste domene. Zahtjevi za raspodijeliti su sve kontrolera unutar domene. Možete dodati druge kontroler domene i automatski sinkronizira s domenom. Konfiguriranje zasebnom opterećenja da biste usmjerili promet kontrolera unutar domene. Svih kontrolera provjerite imaju li dovoljno resursa memorije i prostora za pohranu za rukovanje baze podataka za domenu. Učini sve kontroler domene VMs iste veličine.

## <a name="management-and-monitoring-considerations"></a>Upravljanje i nadzor pitanja vezana uz

Informacije o upravljanju i nadzor pitanja vezana uz potražite u članku ekvivalentan odjeljka [Proširivanje servisa Active Directory za Azure][extending-ad-to-azure]. 

Dodatne informacije potražite u članku [Praćenje servisa Active Directory][monitoring_ad]. Instalirajte alate kao što je [Microsoftova centra za sustave] [ microsoft_systems_center] na poslužitelju nadzora u podmreže upravljanje da biste lakše izvršiti sljedeće zadatke. 

## <a name="solution-components"></a>Komponente rješenja

Skripta za rješenja uzorka, [Uvođenje ReferenceArchitecture.ps1][solution-script], dostupna da možete koristiti implementirati arhitektura koji se nalazi iza preporuke opisane u ovom članku. Ova skripta koristi Voditelj resursa Azure predložaka. Predlošci su dostupni kao skup temeljne sastavne blokove, od kojih svaka izvodi određenu akciju kao što je VNet stvaranju ili konfiguriranju programa NSG. Svrha skriptu je orkestrirali uvođenje predloška.

Predlošci su parametrizirane, s parametrima sadrži zasebne datoteke JSON Možete izmijeniti parametara u te datoteke da biste konfigurirali implementacije u skladu s vlastitim potrebama. Nije potrebno izmijeniti predložaka sami. Ne možete promijeniti sheme objekata u datotekama parametar.

Prilikom uređivanja predložaka, stvaranje objekata koji slijede konvencije imenovanja opisane u [Preporučuje konvencije imenovanja za resurse Azure][naming-conventions].

Uzorak rješenja stvara i konfigurira okruženje sustava u oblaku koji implementira domene pod nazivom *treyresearch.com*. U ovom okruženje sastoji se od ZBRAJA podmreže i poslužitelje, DMZ, web razina, razina tvrtke i komponente sloju za pristup podacima VPN pristupnik i upravljanje sloju. Ogledno rješenje obuhvaća i neobavezna konfiguracije za stvaranje Simulirani lokalnog okruženja s vlastitom domenom *contoso.com*. Rješenje sadrži skripte koje uspostaviti odnos pouzdanosti putem te domene lokalnog korisnicima omogućuje pristup objekata u domeni *treyresearch.com* u oblaku.

U sljedećim se odjeljcima opisuju elemenata na lokalni i konfiguracija u oblaku.

### <a name="on-premises-components"></a>Lokalni komponente

>[AZURE.NOTE] Te komponente nisu glavni fokusa arhitektura opisani u ovom dokumentu i dostupna je samo da bi se dobilo priliku da biste testirali okruženje oblaka sigurno, umjesto stvarnih radnog okruženja. U ovom se odjeljku zbog toga samo navedene ključne parametar datoteke. Promijenite postavke kao što je IP adresa ili veličine na VMs, ali preporučuje se da biste napustili mnoge druge parametara nepromijenjena.

U ovom okruženje sastoji se od skupa AD stabala domenu *contoso.com* . Domene sadrži dva poslužitelja ZBRAJA s IP adresama 192.168.0.4 i 192.168.0.5. Ove dva poslužitelja i pokrenuti DNS servis. Lokalni administratorski račun na oba VMs zove `testuser` lozinkom `AweS0me@PW`. Uz to, konfiguraciju postavlja pristupnik VPN-a za povezivanje s VNet u oblaku. Konfiguracija možete izmijeniti tako da uredite sljedeće JSON datoteke koja se nalazi u [**parametara/lokalnu verziju**] [ on-premises-folder] mape:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Tu datoteku definira prostor adrese mreže za lokalnog okruženja.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Datoteka sadrži konfiguraciju za lokalni VMs ZBRAJA usluge hostiranja. Po zadanom se stvaraju dva *Standardna DS3 v2* VMs.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** i ** [connection.parameters.json][on-premises-connection-parameters]**. Te se datoteke držite postavke za VPN vezu s pristupnikom Azure VPN-a u oblaku, uključujući zajednički ključ koja će se koristiti za zaštitu promet traversing pristupnika.

Preostali datoteke u mapi sadrže informacije o konfiguraciji koristi za stvaranje lokalne domene (*contoso.com*) pomoću ovog infrastrukture. Pomoću njih možete instalirati ZBRAJA, postavljanje DNS-a, a Stvaranje skupa stabala lokalnog.

Rješenje koristi sljedeću skriptu pod nazivom [dolazni trust.ps1][incoming-trust], koji se izvodi na računalo u lokalne domene:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Ova skripta zbraja IP adresa poslužitelja za AD DS u oblak (potražite u sljedećem odjeljku) za lokalni DNS servis, a zatim koristi [netdom] [ netdom] naredbu da biste stvorili dolazne jednosmjerna pouzdanost domene u oblaku (*treyresearch.com*).

### <a name="cloud-components"></a>Oblak komponente

Te komponente obrasca core tu arhitekturu. Postavljanje infrastrukture za domenu *treyresearch.com* i stvaranje odnosa pouzdanost s domenom *contoso.com* lokalnog. [**Parametri/azure**] [ azure-folder] mapa sadrži sljedeće datoteke parametar za konfiguriranje sljedeće komponente:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Tu datoteku definira strukturu na VNet na VMs i druge komponente u oblaku. Uključuje postavke, kao što su ime, adresu prostor, podmreže i adrese neki DNS poslužitelji obavezno. Adrese DNS prikazane u ovom primjeru referenci IP adrese lokalni DNS poslužitelji i zadani Azure DNS poslužitelj. Izmjena te adrese referentni postavki DNS-a ako ne koristite lokalnog okruženja uzorak:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Datoteka konfigurira VMs radi ZBRAJA u oblak. Konfiguracija sastoji se od dva VMs. Promjena administratorskog korisničkog imena i lozinke u na `virtualMachineSettings` odjeljak, a po želji možete promijeniti veličinu VM u skladu s potrebama domene:

    Dodatne informacije potražite u članku [Proširivanje servisa Active Directory za Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [Dodavanje-dodaje-domene-controller.parameters.json][add-adds-domain-controller-parameters]**. Datoteka sadrži postavke za stvaranje *treyresearch.com* domene koje se protežu na poslužiteljima ZBRAJA. Koristi prilagođeni proširenja uspostaviti domene i dodavanje poslužitelje ZBRAJA. Dok ne stvorite Dodatne poslužitelje ZBRAJA (u tom slučaju morate ih dodati u na `vms` polja), mijenjati njihova imena zadanu ili želite stvoriti domene pod drugim nazivom nije potrebno da biste izmijenili tu datoteku.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Datoteka sadrži postavke koje se koriste za stvaranje pristupnika Azure VPN-a u oblaku koja se koristi za povezivanje s lokalne mreže. Izmijenite u `sharedKey` vrijednosti u na `connectionsSettings` sekciju u skladu lokalnog uređaja za VPN-a. Dodatne informacije potražite u članku [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** i ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Te se datoteke konfiguriranje ulaznog (javnih) i izlazni (privatna) strane VMs koje čine DMZ, zaštita poslužitelja u oblak. Dodatne informacije o tih elemenata i njihove konfiguracije potražite u članku [implementacijom DMZ između Azure i Interneta][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, a ** [loadBalancer data.parameters.json][loadBalancer-data-parameters]**. Te datoteke parametara sadrže specifikacije VM za pristup razine web, tvrtke i podataka i konfigurirajte učitavanja balancers za svaki sloju. To su VMs glavno računalo web-aplikacije i baza podataka, a izvođenje radnih opterećenja business za tvrtku ili ustanovu. VMs u sloju web dodaju domene u oblak pomoću postavke određene u ** [web-vm-domene-join.parameters.json] [ web-vm-domain-join-parameters] ** datoteku.

    Svaka datoteka sadrži dva skupa parametara konfiguracije. Na `virtualMachineSettings` odjeljak definira VMs koji hostira ADFS servis u oblaku. Prema zadanim postavkama skripta stvara dvije od tih VMs unutar istog dostupnosti skupa. Sljedeće fragmentirane Prikaži važne dijelove *loadBalancer web.parameters.json* datoteke:

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Veličina i broj VMs u svakom sloju možete izmijeniti u skladu s potrebama.

    Na `loadBalancerSettings` odjeljak pruža opis raspoređivača opterećenja o tim VMs. Raspoređivača opterećenja prosljeđuje prometa koji će se prikazati na web-mjesto priključka 80 (HTTP) i u okvir za priključak 443 (HTTPS) na jednu ili druge na VMs. 

    >[AZURE.NOTE] Pravilo za priključak 80 koristi TCP veza, a ne HTTP-a. To je zato instalacije IIS na web razina je konfiguriran za samo podržava provjeru autentičnosti sustava Windows. Anonimna provjera autentičnosti je onemogućen. Pokušaj *ping* priključak 80 putem HTTP veze ne uspijeva s 401 pogreške (neovlašteno) dok samo putem veze TCP prepoznaje je li priključak active:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Datoteka sadrži konfiguraciju za skočni okvir/upravljanje VMs. Moguće je samo da bi se dobio prijave i administrativni pristup VMs web, tvrtke i razine podataka iz okvira Skok. Prema zadanim postavkama, skripta stvara jednu *Standard_DS1_v2* VM, ali možete izmijeniti ove datoteke da biste stvorili veće ili dodatne VMs ako je radno opterećenje upravljanje vjerojatno će biti vrlo.

Konfiguracija koristi i [Izlazni trust.ps1] [ outgoing-trust] skriptu da biste stvorili jednosmjerna odlazne pouzdanost s domenom *contoso.com* je prikazano u nastavku:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Ova skripta je slična skriptu *dolazni trust.ps1* ranije. Dodaje IP adrese na lokalni AD DS poslužitelji za lokalni DNS servis, a zatim koristi [netdom] [ netdom] naredbu da biste stvorili odlazne odnos pouzdanosti.

## <a name="solution-deployment"></a>Uvođenje rješenja

Rješenje podrazumijeva sljedeće preduvjete:

- Imate postojeću pretplatu Azure u kojem možete stvoriti grupe resursa.

- Ste preuzeli i instalirali najnoviju verziju programa Azure Powershell. U odjeljku [ovdje] [ azure-powershell-download] upute.

Da biste pokrenuli skriptu koja uvodi rješenja:

1. Premještanje praktičan mapu na lokalnom računalu i stvorite sljedeće podmape:

    - Skripte

    - Skripte/parametara

    - Skripte/parametara/lokalnu verziju

    - Parametri/skripte/Azure

2. Preuzimanje [Uvođenja ReferenceArchitecture.ps1] [ solution-script] datoteku u mapu skripti

3. Preuzimanje sadržaja [parametara/lokalnu verziju] [ on-premises-folder] mape u mapu skripte/parametara/lokalnu verziju:

4. Preuzimanje sadržaja [parametara/azure] [ azure-folder] mape u mapu parametara/skripte/Azure.

5. Uređivanje uvođenja ReferenceArchitecture.ps1 datoteke u mapi skripte i promijenite sljedeće retke za određivanje grupa resursa koje treba stvorili ili za držite resursi stvorio skripte:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Uređivanje datoteke parametar u skripti/parametara/lokalnu verziju i skripte/parametara/Azure mape. Ažurirajte reference grupu resursa u te datoteke tako da odgovara nazive grupa resursa dodijeljene varijable u datoteci uvođenja ReferenceArchitecture.ps1. Sljedeća tablica pokazuje koje datoteke parametar referencu koju grupu resursa. Grupa resursa *ra-adfs-radno opterećenje-ru*, *ra-adfs-sigurnost-ru*, *ra-adfs-dodaje-ru*, *ra-adfs-adfs-ru*i *ra-adfs-proxy-ru* koriste se samo u skriptu PowerShell i ne pojavljuju se u parametru datoteke.

  	|Grupa resursa|Parametar datoteke|
  	|--------------|--------------|
  	|Ra-adtrust-lokalnu verziju-ru|parameters\onpremise\connection.PARAMETERS.JSON<br /> parameters\onpremise\virtualMachines-adds.PARAMETERS.JSON<br />parameters\onpremise\virtualNetwork-adds-DNS.PARAMETERS.JSON<br />parameters\onpremise\virtualNetwork.PARAMETERS.JSON<br />parameters\onpremise\virtualNetworkGateway.PARAMETERS.JSON<br />parameters\azure\virtualNetworkGateway.PARAMETERS.JSON
  	|Ra-adtrust-mreže-ru|parameters\onpremise\connection.PARAMETERS.JSON<br />parameters\azure\dmz-Private.PARAMETERS.JSON<br />parameters\azure\dmz-public.PARAMETERS.JSON<br />parameters\azure\loadBalancer-Biz.PARAMETERS.JSON<br />parameters\azure\loadBalancer-Data.PARAMETERS.JSON<br />parameters\azure\loadBalancer-web.PARAMETERS.JSON<br />parameters\azure\virtualMachines-adds.PARAMETERS.JSON<br />parameters\azure\virtualMachines-mgmt.PARAMETERS.JSON<br />parameters\azure\virtualNetwork-adds-DNS.PARAMETERS.JSON<br />parameters\azure\virtualNetwork.PARAMETERS.JSON<br />parameters\azure\virtualNetworkGateway.PARAMETERS.JSON (*dvije pojave*)

    Uz to, postavljanje konfiguracije na informacije o lokalnom i u oblaku komponente, kao što je opisano u [Komponenti rješenja] [ solution-components] sekciju.

7. Otvorite prozor programa Azure PowerShell, premjestite u mapu skripte i pokrenite sljedeću naredbu:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Zamjena `<subscription id>` koristeći Azure pretplate ID.

    Za `<location>`, navedite Azure regiju, kao što su `eastus` ili `westus`.

    Na `<mode>` parametar možete koristiti neku od sljedećih vrijednosti:

    - `Onpremise`, radi stvaranja Simulirani lokalnog okruženja.

    - `Infrastructure`, da biste stvorili infrastrukture VNet i skočni okvir u oblaku.

    - `CreateVpn`, omogućuje stvaranje pristupnika Azure virtualne mreže i povežite ga s lokalne mreže.

    - `AzureADDS`, za sastavljanje VMs ulozi ZBRAJA poslužitelje Implementacija servisa Active Directory te VMs i stvaranje domene u oblaku.

    - `WebTier`, koji stvara web razina VMs i učitavanje opterećenja.

    - `Prepare`, koji izvršava prethodni zadatke. **To je preporučena mogućnost ako su stvaranje posve novog implementacije i nemate postojeću infrastrukturu za lokalni.**

    - `Workload`Da biste stvorili sloju VMs tvrtke i podataka i učitavanje balancers. Ove VMs nisu dio na `Prepare` mogućnost.

    >[AZURE.NOTE] Ako koristite na `Prepare` mogućnost skriptu traje nekoliko sati da biste dovršili.

8.  Ako koristite lokalni konfiguracije uzorak:

    1. Povezivanje s skočni okvir (*ra-adtrust-upravljanje dokumentima-vm1* u grupi resursa *ra-adtrust-sigurnost-ru* ). Prijavite se kao *testuser* lozinkom *AweS0me@PW*.

    2.  U okviru Skok sesija razmjene RDP na prvi VM domenu *contoso.com* (lokalne domene). U ovom VM je IP adresa 192.168.0.4. Korisničko ime koje je *contoso\testuser* lozinkom *AweS0me@PW*.

    3. Preuzimanje [dolazni trust.ps1] [ incoming-trust] skripte i pokretanje da biste stvorili dolazne pouzdanost s *treyresearch.com* domene.

9. Ako koristite lokalni sustavu:

    1. Preuzimanje [dolazni trust.ps1] [ incoming-trust] skripte.

    2. Uređivanje skripte i zamijeni vrijednosti u `$TrustedDomainName` varijable pod nazivom vlastite domene.

    3. Pokrenite skriptu.

10. Iz skočni okvir povezati prvi VM u *treyresearch.com* domain (domena u oblaku). U ovom VM je IP adresa 10.0.4.4. Korisničko ime koje je *treyresearch\testuser* lozinkom *AweS0me@PW*.

11. Preuzimanje [Izlazni trust.ps1] [ outgoing-trust] skripte i pokretanje da biste stvorili dolazne pouzdanost s *treyresearch.com* domene. Ako koristite vlastitu lokalnog računala, moći uređivati skriptu prvi put. Postavljanje na `$TrustedDomainName` varijable naziv lokalne domene i navedite IP adresa poslužitelja za AD DS za tu domenu u na `$TrustedDomainDnsIpAddresses` varijablu.

12. Na računalu na lokalni poduzeti korake navedene u članku [Potvrda pouzdanosti] [ verify-a-trust] da biste odredili hoće li odnos pouzdanosti pravilno konfigurirana između *contoso.com* i *treyresearch.com* domene. Možda ćete morati pričekati nekoliko minuta nakon prethodne korake prije Vjeruj možete provjeriti.

Preostale korake neobavezno pokazati kako odrediti je li pouzdanosti domene funkcioniraju kako želite. Ove korake morate imati pristup računalu razvoj s Visual Studio instaliran.

1.  U prozoru Azure PowerShell pokrenite sljedeću naredbu da biste stvorili web razina ako ne postavite ga prethodno (pomoću na `Prepare` mogućnost):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Ta se naredba stvara web razina i dodaje u VMs *treyresearch.com* domene.

2. Koristite Visual Studio na računalu razvoj, stvorite ASP.NET Web-aplikacije pod nazivom *TreyResearchWebApp*. Korištenje .NET Framework 4.5.2.

3. Odaberite predložak *MVC* i promijenite provjeru autentičnosti za *Provjeru autentičnosti sustava Windows*. Ne stvorite aplikacije servisa u oblaku.

3. Stvaranje i pokretanje aplikacije da biste testirali provjeru autentičnosti. Provjerite prikazuje li se trenutni Windows korisničko ime na traci izbornika na vrhu stranice prema desno.

4. Zatvorite Internet Explorer.

5. U prozoru *Preglednik rješenja* , desnom tipkom miša kliknite TreyResearchWebApp projekta, kliknite *Objavi*.

6. U prozoru *Web objavljivanje* kliknite *Prilagođeno*. Stvorite prilagođeni profil pod nazivom *TreyResearchWebApp*.

7. Na stranici *veze* , postavite *način Objavi* na *Datotečnom sustavu* i navedite mapu pod nazivom *TreyResearchWebApp*, koja se nalazi na određenom mjestu na vašem računalu razvoj.

8. Na stranici *Postavke* postavljanje *konfiguracije* *izdanje*.

9. Na stranici *Pregled* kliknite *Objavi*.

10. Povezivanje sa svakom VM u sloju web shodno (putem okvira za skok) i izvedite sljedeće zadatke. IP adrese web-mjesta sloju VMs su 10.0.1.4 i 10.0.1.5. Korisničko ime za oba VMs je *treyresearch\testuser* lozinkom *AweS0me@PW*:

    1. Kopirajte *TreyResearchWebApp* mapu i njezin sadržaj s računala razvoj *C:\inetpub* mapu.

    2. Korištenju konzole za Upravitelj Internet Information Services (IIS), pomaknite se do *Sites\Default Web-mjesto* na računalu.

    3. U oknu *Akcije* kliknite *Osnovne postavke*pa promijenite fizički put web-mjesta u *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. U oknu s *Prikazom značajke* dvokliknite *provjeru autentičnosti*. Provjerite je li omogućen *Provjeru autentičnosti sustava Windows* i *Anonimna provjera autentičnosti* je onemogućen.

11. s računala na lokalni otvorite Internet Explorer, a zatim potražite na web-mjestu pri http://10.0.1.254 (to je adresa web Razina opterećenja).

12. U dijaloškom okviru *Sigurnost sustava Windows* , unesite vjerodajnice za korisnika u lokalne domene. Navedite korisničko ime koje ne postoje u *treyresearch* domene. Ako koristite Simulirani lokalnog okruženja najprije stvorite korisnika u domeni *contoso* i navedite vjerodajnice za tog korisnika.

13. Kada se pojavi na početnoj stranici, provjerite je li točan domene i korisničko ime koji prikazuju u traku izbornika na vrhu stranice prema desno.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte najbolje postupke za [Proširivanje lokalne domene ZBRAJA Azure][adds-extend-domain]

- Saznajte najbolje postupke za [Stvaranje infrastruktura za ADFS] [ adfs] u Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Sigurne hibridnog mreže arhitektura zasebnom AD domene u sustavu"