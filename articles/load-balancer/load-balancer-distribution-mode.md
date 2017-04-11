<properties
   pageTitle="Konfiguriranje način distribucije opterećenja | Microsoft Azure"
   description="Kako konfigurirati Azure učitavanja opterećenja raspodjele način za podršku afinitet izvorni IP"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Konfiguriranje načina distribucije za opterećenja

## <a name="hash-based-distribution-mode"></a>Način raspršivanje distribucije

Algoritam zadani raspodjele je 5-n-torke (izvorni IP, izvorni Priključak odredišni IP, odredišni priključak protokol vrsta) raspršivanje da biste mapirali promet dostupni poslužitelji. Pruža stickiness samo unutar sesije prijenosa. Pakete u istoj sesiji bit će preusmjereni na istoj instanci podatkovnog centra IP (DIP) iza krajnju točku rasporediti opterećenje. Kada klijent pokrene novu sesiju iz iste IP izvora, izvorni Priključak promijeniti te uzrokuje promet da biste prešli na drugu krajnjoj točki DIP.

![Raspršivanje temelji opterećenja](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Slika 1-5-n-torke raspodjele

## <a name="source-ip-affinity-mode"></a>Način rada s izvorom IP afinitet

Imamo drugi raspodjele nazvano izvorni IP afinitet (poznat i kao sesija afinitet ili afinitet IP klijent). Azure opterećenja moguće je konfigurirati za korištenje 2-n-torke (IP izvora, odredišni IP) ili 3-n-torke (IP izvora, IP odredište Protocol) da biste mapirali promet dostupni poslužitelji. Pomoću afiniteta izvorni IP veze pokrenut na istom računalu klijentskog odlazak krajnju točku iste DIP.

Sljedeći dijagram prikazuje 2-n-torke konfiguracije. Obratite pozornost na to načinom 2-n-torke kroz na opterećenja za virtualnog računala 1 (VM1) koji se zatim sigurnosno VM2 i VM3.

![sesije afinitet](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Slika 2-2-n-torke raspodjele

Izvorni IP afinitet rješava programa nekompatibilnost između Azure opterećenja i pristupnik udaljene radne površine (RD). Sada možete sastaviti farma sustava RD pristupnika u jednu u oblaku.

Drugi scenarij slučaja koristi se prijenos medijskog sadržaja kojima će se dogoditi prijenos podataka putem UDP, ali ravnini kontrole se postiže putem TCP:

- Klijent prvi put započinje TCP sesije rasporediti opterećenje javnu adresu, dobiti usmjereni na određene DIP, kanala je preostalo aktivni praćenje stanja veze
- Nove sesije UDP iz istog klijentskom računalu pokrenut isti krajnjoj javno rasporediti opterećenje, očekivanja ovdje je da ovu vezu je i usmjereni na isti krajnje DIP kao prethodne TCP veza tako da se prijenos medijskog sadržaja možete izvršiti na visok propusnost i zadržavanje kanala kontrola putem TCP.

>[AZURE.NOTE] Kada uravnoteženja skup promjene (uklanjanjem ili dodavanje virtualnog računala), raspodjele zahtjevi klijenta je recomputed. Ne ovise o nove veze iz postojeće klijenti koje završavaju na istom poslužitelju. Uz to, pomoću izvorni IP način distribucije afinitet može uzrokovati nejednaki raspodjele prometa. Klijenti koji se izvodi iza proxyji moguće je vidjeti kao jedan jedinstveni klijentske aplikacije.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfiguriranje postavki afinitet izvorni IP za učitavanje opterećenja

Za virtualnim strojevima PowerShell možete koristiti da biste promijenili postavke vremenskog ograničenja:

Dodavanje krajnje Azure virtualnog računala i postavljanje načina za raspodjelu raspoređivača učitavanja

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution moguće je postaviti za sourceIP za 2-n-torke (IP izvora, odredišni IP) za ujednačavanje opterećenja, sourceIPProtocol 3-n-torke (IP izvora, IP odredište protocol) opterećenja ili ništa ako želite da se zadano ponašanje 5-n-torke opterećenja.

Dohvaćanje programa krajnjoj točki učitavanja opterećenja način konfiguracija raspodjele, koristite sljedeće:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Ako nema LoadBalancerDistribution element Azure opterećenja koristi algoritam zadani 5-n-torke.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Postavljanje načina za raspodjelu na krajnjoj točki skupu rasporediti opterećenje

Ako su krajnje točke dio skupa krajnjoj točki rasporediti opterećenje, način distribucije mora biti postavljeno na stranici Postavljanje krajnje točke rasporediti opterećenje:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Konfiguracija servisa oblaku da biste promijenili način distribucije.

Azure SDK za .NET 2,5 (da biste se izdavati u studenom) možete koristiti da biste ažurirali servis u Oblaku. Postavke krajnje točke za servise u Oblaku su stvorene u .csdef. Da biste ažurirali način raspodjele raspoređivača učitavanja implementacije servise u Oblaku, potreban je implementaciju nadogradnje.
Evo primjera .csdef promjene postavki krajnja točka:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
      <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
      </PublicIPs>
    </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="api-example"></a>Primjer API-JA

Možete konfigurirati raspodjele raspoređivača opterećenja pomoću Upravljanje servisom API-JA. Provjerite je li da biste dodali u `x-ms-version` je zaglavlje postavljeno na verziju `2014-09-01` ili noviji.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Ažurirajte mogućnost konfiguracija skupa navedeni uravnoteženja u instalaciji

#### <a name="request-example"></a>Primjer zahtjev

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Vrijednost LoadBalancerDistribution može biti sourceIP 2-n-torke afinitet, sourceIPProtocol za 3-n-torke afinitet ili ništa (za bez afinitet. odnosno 5-n-torke)

#### <a name="response"></a>Odgovor

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Daljnji koraci

[Pregled opterećenja Interna učitavanja](load-balancer-internal-overview.md)

[Početak rada konfiguriranje internetsku nasuprotne opterećenja](load-balancer-get-started-internet-arm-ps.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
