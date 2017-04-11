<properties
   pageTitle="Konfiguriranje vremenskog ograničenja na neaktivnosti TCP raspoređivača opterećenja | Microsoft Azure"
   description="Konfiguriranje TCP raspoređivača opterećenja neaktivnosti vremenskog ograničenja"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfiguriranje postavki neaktivnosti vremenskog ograničenja TCP za Azure opterećenja

Njegov zadanoj konfiguraciji Azure opterećenja ima u neaktivan vremenskog ograničenja postavku 4 minuta. Ako je razdoblje neaktivnosti dulje od vrijednost vremenskog ograničenja, nema jamstva koje sesiju TCP i HTTP se održavaju između klijenta i servis u oblaku.

Kada je veza Zatvoreno, klijentska aplikacija dobiti sljedeću poruku o pogrešci: "je zatvoren podlozi veze: veze koje trebalo biti zadržane aktivnosti zatvaranja poslužitelj."

Uobičajeno je da biste koristili TCP koji je produžite. Praksa zadržava vezu koji aktivni za dulje razdoblje. Dodatne informacije potražite u ovim se [primjerima .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Produžite omogućena, pakete šalju neaktivnosti za vezu. Ove produžite pakete provjerite je li vrijednost neaktivnosti vremenskog ograničenja nikad ne dosegne i veza se održava dugo.

Ova postavka funkcionira samo dolaznih veza. Da biste izbjegli gubitak veze, morate konfigurirali TCP produžite interval manje od postavku neaktivnosti vremenskog ograničenja ili povećati vrijednost neaktivnosti vremenskog ograničenja. Za podršku takvih scenariji dodali smo podrška za konfigurirati Prekoračenje vremena neaktivnosti. Sada možete postaviti ga za trajanje od 4 do 30 minuta.

TCP produžite funkcionira i za scenariji kojima trajanja baterija nije ograničenja. Ne preporučuje se za mobilne aplikacije. Korištenje TCP koji je produžite u mobilnu aplikaciju možete potrošiti baterija uređaja brže.

![TCP vremenskog ograničenja](./media/load-balancer-tcp-idle-timeout/image1.png)

U sljedećim se odjeljcima opisuju kako promijeniti postavke neaktivnosti vremenskog ograničenja na virtualnim računalima sustava i servise u oblaku.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Konfiguriranje TCP vremenskog ograničenja za instancu razinom javnu IP 15 minuta

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`nije obavezno. Ako ne postavite, vremenskog ograničenja zadani je 4 minuta. Raspon prihvatljiva isteklo je 4 do 30 minuta.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Postavljanje neaktivnosti vremenskog ograničenja prilikom stvaranja krajnje Azure na virtualnog računala

Da biste promijenili postavke vremenskog ograničenja za krajnje točke, koristite sljedeće:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Da biste dohvatili konfiguraciju neaktivnosti vremensko ograničenje, koristite sljedeću naredbu:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Postavljanje vremenskog ograničenja TCP na skupu uravnoteženja krajnje točke

Ako su krajnje točke dio skupa uravnoteženja krajnje točke, TCP vremensko ograničenje mora biti postavljeno na stranici Postavljanje krajnje uravnoteženja. Ako, na primjer:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Promjena postavki vremenskog ograničenja za servise u oblaku

Azure SDK možete koristiti da biste ažurirali servis u oblaku. Provjerite postavke krajnja točka za servise u oblaku u datoteci .csdef. Ažuriranje TCP vremenskog ograničenja za implementaciju servis u oblaku zahtijeva implementaciju nadogradnje. Iznimka je ako je samo za javnu IP navedena TCP vremenskog ograničenja. Javnu IP postavke vidljive u datoteci .cscfg, a možete ih ažurirati putem implementaciju ažuriranja i nadogradnje.

.Csdef promjene postavki krajnjoj točki su:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Promjena .cscfg za postavke vremenskog ograničenja na javnu IP-ovi su:

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

## <a name="rest-api-example"></a>Primjer REST API-JA

Prekoračenje vremena neaktivnosti TCP možete konfigurirati pomoću Upravljanje servisom API-JA. Provjerite je li u `x-ms-version` je zaglavlje postavljeno na verziju `2014-06-01` ili noviji. Ažurirajte konfiguraciju navedeni uravnoteženja unos krajnjih točaka na svim računalima virtualne u implementacije.

### <a name="request"></a>Zahtjev

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Odgovor

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Daljnji koraci

[Pregled raspoređivača Interna učitavanja](load-balancer-internal-overview.md)

[Početak rada konfiguriranje opterećenja na mjesto na Internetu](load-balancer-get-started-internet-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)
