<properties
   pageTitle="Servis tkanina povratnog Proxy | Microsoft Azure"
   description="Korištenje servisa tkanina povratnog proxy za komunikaciju na microservices s unutrašnje i vanjske klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Servis tkanina povratnog Proxy

Servis tkanina povratnog proxy je tkanina ugrađen u servis povratnog proxy koji omogućuje adresiranja microservices u skupini tkanina servis koji izlažu HTTP krajnje točke.

## <a name="microservices-communication-model"></a>Model Microservices komunikacije

Microservices u tkanina usluga obično se izvoditi na podskup VM korisnika u klasteru i možete prijeći s jedne VM na drugu različitih razloga. Da bi se krajnje točke za microservices dinamički možete promijeniti. Uobičajeni uzorak komunikaciju s microservice je petlje Razriješi ispod,

1. Razrješavanje mjesto servisa prethodno putem servisa imenovanja.
2. Povezivanje sa servisom za.
3. Uzrok pogreške vezu i ponovno riješiti mjesto servisa kada je to potrebno.

Ovaj postupak obično uključuje prelamanje klijentsko komunikacije biblioteke u petlji pokušaj koji implementira servis pravila razlučivost i pokušajte ponovno.
Dodatne informacije o ovoj temi potražite u članku [komunikacije sa servisima](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Obavještavanje putem SF povratnog proxy
Servis tkanina povratnog proxy pokreće se na sve čvorove u klasteru. Izvodi postupka razlučivost cijelu servisa u ime klijentskog računala i prosljeđuje zahtjev klijenta. Tako da klijenti pokrenuti na klaster mogu koristiti nijednu biblioteku za klijentsko HTTP komunikacije razgovarati s servis za ciljnu putem SF povratnog proxy izvodi lokalno na istom čvor.

![Internu komunikaciju][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Komuniciranje Microservices iz izvan klaster
Vanjska komunikacija modela zadano za microservices je **slaganja** gdje se svaki servis prema zadanim postavkama nije moguć izravno iz vanjski klijenti. [Azure opterećenja](../load-balancer/load-balancer-overview.md) je mreža granice između microservices i vanjski klijenti koji izvršava prevođenja mrežnih adresa i prosljeđuje vanjskog zahtjeva za interne **IP:port** krajnje točke. Da biste omogućili krajnje točke na microservice izravno pristupiti vanjski klijenti, opterećenja Azure najprije morate ga konfigurirati za prosljeđivanje promet za svaki priključak usluga u klasteru koristi. Osim toga, većina microservices (esp. s praćenjem stanja microservices) ne live na sve čvorove klaster, a oni prebacivanje čvorove na prebacivanje, tako da u tim slučajevima opterećenja Azure učinkovito ne može odrediti cilj čvora na replike nalaze da preusmjerava promet na.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Da Microservices se obrati izravnom SF obrnutim proxy iz izvan klaster

Umjesto konfiguriranja pojedinačnih servisnih priključke u azure opterećenja, samo proxy priključak SF obrnuti moguće je konfigurirati u Azure raspoređivača opterećenja. Time se omogućuje klijenti izvan klaster dosegne servisa unutar klaster putem povratnog proxy bez dodatnih konfiguracije.

![Vanjska komunikacija][0]

>[AZURE.WARNING] Konfiguriranje povratnog proxy priključak na raspoređivača opterećenja, čini micro services u klaster koji izlažu krajnje HTTP-a, da biste se addressible iz izvan klaster.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>Oblikovanje URI adresiranja servisa putem povratnog proxy

Povratnog proxy koristi određenom obliku URI da biste odredili koji servis particije treba proslijediti dolazni zahtjev:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http(s):** Da biste prihvatili HTTP ili HTTPS promet moguće je konfigurirati povratnog proxy. U slučaju promet HTTPS SSL prekid pojavljuje se na povratnog proxy. Zahtjevi za koje prosljeđuju povratnog proxy servisima u klasteru su putem HTTP-a.
 - **Klaster FQDN | interne IP:** Za vanjski klijenti povratnog proxy može konfigurirati tako da bude dostupno putem klaster domene (primjerice, mycluster.eastus.cloudapp.azure.com). Prema zadanim postavkama povratnog proxy pokreće svaki čvor, pa za interne promet je proslijediti na localhost ili bilo koju Interna čvor IP (npr., 10.0.0.1).
 - **Priključka:** Priključka koji je naveden za povratnog proxy. Npr: 19008.
 - **ServiceInstanceName:** To je naziv instance u potpunosti kvalificirana distribuiranih servisa pružanja usluge pokušavate dosegne na "tkanina: /" shemu. Na primjer, dosegne servisa *tkanina: / myapp/myservice/*, koristit ćete *myapp/myservice*.
 - **Put sufiks:** Ovo je stvarni put URL-a za servis koji želite povezati. Na primjer, *myapi/vrijednosti/dodavanje/3*
 - **PartitionKey:** Particioniranom servisa, to je tipku izračunatom particije particije koji želite kontaktirati. Imajte na umu da je to *nije* particija GUID ID-a. Taj parametar nije potrebna za servise pomoću sheme jednočlana particije.
 - **PartitionKind:** Shema particija servisa. To se može biti 'Int64Range' ili "Pod nazivom". Taj parametar nije potrebna za servise pomoću sheme jednočlana particije.
 - **Vremenskog ograničenja:**  Određuje vremenskog ograničenja za http zahtjev stvorenu obrnutu proxy poslužitelj sa servisom ime zahtjev klijenta. Zadane vrijednosti za to je 60 sekundi. Ovo je neobavezan parametar.

### <a name="example-usage"></a>Primjer korištenja

Na primjer, pogledajmo servisa **tkanina: / MyApp/MyService** koji otvara HTTP ga slušatelj na sljedeći URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Pomoću sljedećih resursa:

 - `/index.html`
 - `/api/users/<userId>`

Ako se servis koristi shema particioniranja jednočlana, parametrima niza upita *PartitionKey* i *PartitionKind* nisu potrebni i usluge mogu pristupiti putem pristupnika kao:

 - Vanjsko:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Interno:`http://localhost:19008/MyApp/MyService`

Ako se servis koristi shema particioniranja Uniform Int64, parametrima niza upita *PartitionKey* i *PartitionKind* mora koristiti dosegne particija servisa:

 - Vanjsko:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Interno:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Dosegne resursi koji prikazuje servis, jednostavno nakon naziva servisa u URL-u umetnuti put resursa:

 - Vanjsko:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Interno:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Pristupnik zatim proslijediti te zahtjeve URL servisa:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Posebni postupak za zajedničko korištenje priključak services

Pristupnik aplikacija pokušava ponovno riješiti adresa servisa i pokušajte ponovno zahtjev kad je servis nije moguće dohvatiti. To je jedna od glavnih prednosti pristupnika, kao kod klijent ne treba implementirati vlastitu razlučivost servisa i riješili petlje.

Općenito kada servis nije moguće dohvatiti znači da instanca servisa ili replike se premješta u različitim čvor kao dio njegova normalni životnog ciklusa. Kada se to dogodi, pristupnika primiti mrežne veze pogreške koja označava krajnje više nije otvorena na izvorno riješi adresu.

Međutim, replike ili instanci servisa možete zajednički koristiti proces glavnog računala te mogu omogućiti zajedničko korištenje priključak kada na poslužitelju web-mjesto utemeljeno na http.sys uključujući:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [WebListener Core platforme ASP.NET](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

U tom slučaju je vjerojatno web-poslužitelj nalazi se u matični proces i odgovaranje na zahtjeve, ali instancu riješi servisa ili replike više nije dostupan na glavnom računalu. U ovom slučaju pristupnika primit će je HTTP 404 odgovor na web-poslužitelju. Kao rezultat je HTTP 404 sastoji se od dva različita značenja:

 1. Servis adresa ispravna, ali resurs zatražio korisnik ne postoji.
 2. Adresa servisa nije valjana, a resursa zatražio korisnik zapravo možda postoji na drugi čvor.

U slučaju da prvi, to je normalno HTTP 404 koji se smatra pogreška korisničkog. Međutim, u drugom slučaju korisnik zatražio je resurs koji ne postoji, ali je pristupnika nije moguće pronaći jer sam servis premještena, u tom slučaju pristupnika potrebno ponovno razriješiti adresu, a zatim pokušajte ponovno.

Pristupnik stoga mora način da biste razlikovali te dva slučaja. Da biste omogućili tu razliku potreban je savjet s poslužitelja.

 - Prema zadanim postavkama pristupnik aplikacije pretpostavlja slučaj #2 i pokuša ponovno riješiti i ponovno poslao zahtjev.
 - Da biste naznačili slučaj #1 pristupnik za aplikaciju, servis mora vratiti sljedeće HTTP zaglavlje odgovor:

`X-ServiceFabric : ResourceNotFound`

Ovo zaglavlje HTTP odgovor označava normalni HTTP 404 situacija u kojima traženi resurs ne postoji, a pristupnika će pokušati ponovno riješiti adresa servisa.

## <a name="setup-and-configuration"></a>Postavljanje i konfiguriranje
Tkanina povratnog proxy servis je moguće omogućiti za klaster putem [predloška Azure Voditelj resursa](./service-fabric-cluster-creation-via-arm.md).

Nakon što dodate predložak za klaster koju želite uvesti (u programu Word ili tako da stvorite predložak Upravitelj prilagođene resursa) povratnog proxy mogu omogućiti u predlošku sljedeće korake.

1. Definiranje priključak za povratnog proxy u [odjeljku parametara](../resource-group-authoring-templates.md) predloška.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Navedite priključak za svaku nodetype objekata u **klaster** [odjeljak vrste resursa](../resource-group-authoring-templates.md)

    Za apiVersion, prije no što "2016-09-01' priključak je označena parametrom naziv ***httpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Za apiVersion korisnika na ili nakon "2016-09-01' priključak je označena parametrom naziv ***reverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Da biste riješili povratnog proxy iz izvan azure klaster, postavite **pravila raspoređivača opterećenja azure** za priključak navedene u koraku 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Konfiguriranje SSL certifikata na priključak za povratnog proxy, dodali certifikat svojstvo httpApplicationGatewayCertificate u **klaster** [odjeljak vrste resursa](../resource-group-authoring-templates.md)

    Za apiVersion, prije no što "2016-09-01' certifikat je označena parametrom naziv ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Za apiVersion korisnika na ili nakon "2016-09-01' certifikat je označena parametrom naziv ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Daljnji koraci
 - Pogledajte primjer HTTP komunikacije između servisa [uzorka projekt na GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Poziva udaljene procedure s remoting pouzdanog Services](service-fabric-reliable-services-communication-remoting.md)

 - [API koja koristi OWIN pouzdanog Services na webu](service-fabric-reliable-services-communication-webapi.md)

 - [WCF komunikacije pomoću pouzdanog Services](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
