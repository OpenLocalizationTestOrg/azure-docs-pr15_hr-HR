<properties
   pageTitle="Stvaranje programa Interna opterećenja pomoću komponente PowerShell u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako stvoriti na interni opterećenja pomoću komponente PowerShell u upravitelju resursa"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-powershell"></a>Stvaranje programa Interna opterećenja pomoću komponente PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Sljedeći koraci objašnjavaju kako stvoriti na interni raspoređivača opterećenja Voditelj resursa Azure pomoću komponente PowerShell. Pomoću upravitelja Azure resursa, stavke za stvaranje internog opterećenja su pojedinačno konfigurirani i zatim kombiniranje dozvola radi stvaranja raspoređivača opterećenja.

Morate stvoriti i konfigurirati sljedeće objekte za implementaciju raspoređivača opterećenja:

- Prednje završetka konfiguraciju IP - će konfigurirati privatne IP adrese za dolazne mrežni promet
- Skupna adresa pozadinskog - će konfigurirati sučelje mreže koju će primiti rasporediti opterećenje prometa koji dolaze iz Skupna IP sučelje
- Pravila - izvor i konfiguriranje lokalnog priključka za raspoređivača opterećenja za ujednačavanje opterećenja.
- Probes – konfigurira probni status stanja instanci virtualnog računala.
- Ulazna pravila NAT - konfigurira pravila priključka da biste izravno pristupili instance virtualnog računala.

Dodatne informacije o učitavanja možete postići komponente opterećenja pomoću upravitelja Azure resursa na [Azure Voditelj resursa podrške za opterećenja](load-balancer-arm.md).

Sljedeći koraci objašnjavaju kako konfigurirati opterećenja između dva virtualnih računala.


## <a name="setup-powershell-to-use-resource-manager"></a>Postavljanje ljuske PowerShell za korištenje Voditelj resursa

Provjerite je li imati najnoviju verziju radnog modul Azure za PowerShell i pravilno postavili PowerShell dodatno da biste pristupili Azure pretplatu.

### <a name="step-1"></a>Korak 1

        Login-AzureRmAccount

### <a name="step-2"></a>Korak 2

Provjera pretplate za račun

        Get-AzureRmSubscription

Zatražit će se za provjeru autentičnosti s vjerodajnice.<BR>

### <a name="step-3"></a>Korak 3

Odabir pretplate Azure da biste koristili. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Stvaranje grupa resursa za opterećenja

### <a name="step-4"></a>Korak 4

Stvorite novu grupu resursa (preskoči ovaj korak ako koristi postojeću grupu resursa)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. Koristi se kao zadano mjesto za resurse u toj grupi resursa. Provjerite je li sve naredbe za stvaranje raspoređivača opterećenja će koristiti isti grupu resursa.

U primjeru iznad koju smo stvorili grupu resursa pod nazivom "NRP ru" i mjesto "Zapad NAM".

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Stvaranje virtualne mreže i privatni IP adresa za Skupna IP sučelje


### <a name="step-1"></a>Korak 1

Stvara podmreže za virtualne mreže i dodjeljuje varijabli $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Stvaranje virtualne mreže:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Stvara virtualne mreže i dodaje podmreže lb-podmreže-biti virtualne mreže NRPVNet i dodjeljuje varijabli $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Stvaranje prednje resurse IP i skupna pozadinskog adresa

Postavljanje Skupna IP za sučelje za na dolazne učitavanja opterećenja mrežni promet i pozadinskog skupna adresa prima opterećenje raspoređen promet.

### <a name="step-1"></a>Korak 1

Stvaranje grupe aplikacija IP sučelje za korištenje privatne IP adrese 10.0.2.5 za 10.0.2.0/24 podmreže koji će biti dolazne mrežni promet krajnjoj točki.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Korak 2

Postavljanje skupna adresa pozadinskih koja koristi promet primanje Skupna IP sučelja:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Stvaranje pravila LB, NAT pravila, a zatim probni i raspoređivača učitavanja

Nakon stvaranja Skupna IP sučelje i skupna adresa pozadinskog, morat ćete stvoriti pravila koja će pripadati resursa raspoređivača opterećenja:

### <a name="step-1"></a>Korak 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Gornji primjer stvara sljedećih stavki:

- NAT pravilo koje će sve dolazne promet priključak 3441 otvorite priključak 3389.
- drugo NAT pravilo koje će sve dolazne promet priključak 3442 otvorite priključak 3389.
- pravilo raspoređivača opterećenja koja učitava saldo sve dolazne promet na javno priključak 80 Lokalni priključak 80 adresu u pozadini.
- Probni pravilo koje će provjeriti stanje status put "HealthProbe.aspx"



### <a name="step-2"></a>Korak 2

Stvaranje opterećenja zbrajanjem svih objekata (NAT pravila, pravila raspoređivača opterećenja, probni konfiguracije):

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Stvaranje sučelje mreže

Nakon stvaranja interne opterećenja, morate definirati koje sučelje mreže će primati dolazne rasporediti opterećenje mrežnog prometa, NAT pravila i probni. Mrežnog sučelja u tom slučaju pojedinačno konfigurirani i mogu biti kasnije dodijeljeni virtualnog računala.


### <a name="step-1"></a>Korak 1


Traženje resursa virtualne mreže i podmreže da biste stvorili sučelje mreže:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


U ovom koraku stvarate mrežno sučelje koje pripadaju grupe aplikacija pozadinskih raspoređivača opterećenja i pridruživanje prvo pravilo NAT za RDP za ovo sučelje mreže:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Korak 2

Stvaranje drugog mrežnog sučelja naziva LB-Nic2-biti:

U ovom koraku stvarate drugog mrežnog sučelja, stvoriti dodijelite isti skup pozadinskih raspoređivača opterećenja i drugo pravilo NAT zastavicom za RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Rezultat prikazat će se sljedeće:

    $backendnic1

Očekivani izlaz:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Korak 3

Koristite naredbu Dodaj AzureRmVMNetworkInterface da biste dodijelili NIC-a virtualnog računala.

Možete pronaći korak po korak upute za stvaranje virtualnog računala i dodjela NIC praćenja u dokumentaciji: [Stvaranje programa VM Azure pomoću komponente PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Ako već imate virtualnog računala stvorili, možete dodati mrežno sučelje sa sljedećim koracima:

#### <a name="step-1"></a>Korak 1

Učitavanje resursa raspoređivača opterećenja u varijablu (Ako dosad niste koji još). Varijabla koristi se zove $lb i korištenje istog naziva resursa raspoređivača opterećenja stvorena iznad.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Korak 2

Učitavanje konfiguracije pozadinskog tjednog prikaza kalendara.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Korak 3

Učitavanje već stvoreni mrežnog sučelja u varijablu. Naziv varijable koristi je $nic. Naziv mreže sučelja koji se koristi se ne razlikuje od gornji primjer.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Korak 4

Promjena konfiguracije pozadinskog mrežnog sučelja.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Korak 5

Spremanje objekta sučelja mreže.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Kada dodate mrežno sučelje resurse za pozadinskog raspoređivača opterećenja se pokreće prima mrežni promet na temelju pravila za resursa raspoređivača opterećenja za ujednačavanje opterećenja.

## <a name="update-an-existing-load-balancer"></a>Ažuriranje postojećih opterećenja


### <a name="step-1"></a>Korak 1

Objekt raspoređivača opterećenja pomoću opterećenja iz gornji primjer dodijeliti varijabla $slb pomoću Get-AzureRmLoadBalancer

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Korak 2

U sljedećem primjeru ćete dodati novo pravilo dolazni NAT pomoću priključak 81 u sučelje i priključak 8181 za skup pozadinskih za postojeće opterećenja

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Korak 3

Spremanje nove konfiguracije pomoću skupa AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Uklanjanje raspoređivača opterećenja

Koristite naredbu Ukloni AzureRmLoadBalancer da biste izbrisali na prethodno stvorena opterećenja pod nazivom "NRP-LB" u grupu resursa pod nazivom "NRP ru"

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Možete koristiti neobavezna prebacivanje – prisilno da biste izbjegli upita za brisanje.



## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)