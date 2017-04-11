<properties
   pageTitle="Stvaranje internetsku nasuprotne opterećenja u Voditelj resursa pomoću EŽA Azure | Microsoft Azure"
   description="Saznajte kako stvoriti internetsku nasuprotne opterećenja u Voditelj resursa pomoću EŽA Azure"
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

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Stvaranje programa Interna opterećenja pomoću EŽA Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [Saznajte kako stvoriti internetsku nasuprotne opterećenja pomoću klasične implementacije](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Implementacija rješenja pomoću EŽA Azure

Na sljedeći način prikažite kako stvoriti internetsku nasuprotne opterećenja Voditelj resursa Azure pomoću EŽA. Pomoću upravitelja za Azure resursa svaki resurs stvara i konfigurirali pojedinačno, zatim stavi zajedno da biste stvorili resurs.

Morate stvoriti i konfigurirati sljedeće objekte za implementaciju raspoređivača opterećenja:

- Pristupne Konfiguracija IP - sadrži javnu IP adresa za dolazne mrežni promet.
- Skupna pozadinsku adresa – sadrži sučelje mreže (NIC-ovi) za virtualnim strojevima mrežni promet primanje raspoređivača opterećenja.
- Opterećenja pravila - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak u pozadinskoj adresu.
- Ulazna pravila NAT - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u pozadinskoj adresu.
- Probes – sadrži stanje probes koristi da biste provjerili dostupnost virtualnim strojevima instanci u pozadinskoj adresu.

Dodatne informacije potražite u članku [Upravljanje resursima Azure podrške za opterećenja](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Postavljanje EŽA da biste koristili Voditelj resursa

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokrenite naredbu **azure config način** da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Očekivani izlaz:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Stvaranje virtualne mreže i javnu IP adresa za sučelja Skupna IP

1. Stvaranje virtualne mreže (VNet) pod nazivom *NRPVnet* na mjestu na SAD-a pomoću grupu resursa pod nazivom *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Stvaranje podmreže pod nazivom *NRPVnetSubnet* s blok CIDR 10.0.0.0/24 u *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Stvorite javnu IP adresu pod nazivom *NRPPublicIP* koriste sučelja Skupna IP s DNS naziva *loadbalancernrp.eastus.cloudapp.azure.com*. Naredba u nastavku koristi statički dodijeljeni vrsta i neaktivnosti vremensko ograničenje od 4 minuta.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Raspoređivača opterećenja koristit će se oznaka domene javnu IP kao njegov FQDN. To promjenu klasični implementaciju koji koristi oblaka servisa kao raspoređivača opterećenja potpuno kvalificirani domene naziv (FQDN).
    >U ovom se primjeru FQDN je *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Stvaranje raspoređivača opterećenja

Sljedeća naredba stvara raspoređivača opterećenja pod nazivom *NRPlb* u grupi resursa *NRPRG* *Istočni sad* Azure mjesto.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Stvaranje pristupne Skupna IP i skupna adresa za pozadinskog

U ovom se primjeru pokazuje kako stvoriti sučelja Skupna IP koja prima dolazne mrežnog prometa na raspoređivača opterećenja i Skupna IP pozadinskog gdje sučelja skup šalje rasporediti opterećenje mrežni promet.

1. Stvaranje pristupne Skupna IP zastavicom javnu IP stvorili u prethodnom koraku, a raspoređivača opterećenja.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Postavite na skupna pozadinsku adresa koristi promet primanje sučelja Skupna IP.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Stvaranje pravila LB, NAT pravila i probni

U ovom se primjeru stvara sljedeće stavke.

- NAT pravilo da biste preveli sve dolazne promet na priključak 21 priključak 22<sup>1</sup>
- NAT pravilo da biste preveli sve dolazne promet na priključak 23 priključak 22
- pravilo raspoređivača opterećenja za saldo sve dolazne promet na priključak 80 priključak 80 na adrese u pozadinskoj.
- Probni pravilo da biste provjerili status stanja na stranicu pod nazivom *HealthProbe.aspx*.

<sup>1</sup> NAT pravila povezane su s instancom određene virtualnog računala iza raspoređivača opterećenja. Mrežni promet stiže na priključak 21 šalje se određene virtualnog računala na priključak 22 povezane s tim se pravilom NAT. Morate navesti protocol (UDP ili TCP) za NAT pravilo. Oba protokoli ne može biti dodijeljena isti priključak.

1. Stvaranje pravila za NAT.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Stvorite pravilo raspoređivača opterećenja.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Stvaranje stanja probni.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Provjerite postavke.

        azure network lb show nrprg nrplb

    Očekivani izlaz:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Stvaranje NIC-ovi

Morate stvoriti NIC-ovi (ili izmijeniti postojeće) i povezati ih NAT pravila, pravila raspoređivača opterećenja i probes.

1. Stvaranje NIC pod nazivom *lb-nic1-biti*, i povezati je s *rdp1* NAT, i pravila pozadinske adresu skup *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Očekivani izlaz:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Stvaranje NIC pod nazivom *lb-nic2-biti*, i povezati je s *rdp2* NAT, i pravila pozadinske adresu skup *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Stvaranje virtualnog računala (VM) pod nazivom *webu1*i pridružiti NIC pod nazivom *lb-nic1-se*. Račun za pohranu naziva *web1nrp* stvorena prije izvođenja naredbe u nastavku.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs u raspoređivača opterećenja moraju biti u istoj postavljanje dostupnosti. Korištenje `azure availset create` da biste stvorili raspoloživost postavite.

    Rezultat mora biti sličnu ovoj:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Informativna poruka **je SLIKA bez publicIP konfiguriran** očekuje od NIC-a za opterećenja povezivanja s internetom putem učitavanja opterećenja javnu IP adresu.

    Budući da u *lb-nic1-biti* NIC vezan uz pravilo NAT *rdp1* , možete se povezati s *webu1* koristite RDP putem priključak 3441 na raspoređivača opterećenja.

4. Stvaranje virtualnog računala (VM) pod nazivom *webu2*i pridružiti NIC pod nazivom *lb-nic2-se*. Račun za pohranu naziva *web1nrp* stvorena prije izvođenja naredbe u nastavku.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Ažuriranje postojećih raspoređivača opterećenja

Možete dodati pravila pozivanju postojeće opterećenja. U sljedećem primjeru novo pravilo raspoređivača opterećenja dodaje se postojeće opterećenja **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Brisanje raspoređivača opterećenja

Da biste uklonili raspoređivača opterećenja, koristite sljedeću naredbu:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-cli.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
