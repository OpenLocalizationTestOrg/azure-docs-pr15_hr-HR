<properties
   pageTitle="Stvaranje internog opterećenja pomoću EŽA Azure u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako stvoriti Interna opterećenja pomoću EŽA Azure u upravitelju resursa"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Stvaranje internog opterećenja pomoću EŽA Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Implementacija rješenja pomoću EŽA Azure

Sljedeći koraci pokazati kako stvoriti raspoređivača opterećenja na mjesto na Internetu pomoću upravitelja resursa Azure EŽA. Pomoću upravitelja Azure resursa, svaki resurs je stvorio i pojedinačno konfigurirani, a zatim stavite zajedno da biste stvorili resurs.

Morate stvoriti i konfigurirati sljedeće objekte za implementaciju raspoređivača opterećenja:

- **Konfiguracija Front-End IP**: sadrži javnu IP adrese za dolazne mrežni promet
- **Skupna pozadinsku adresa**: sadrži sučelje mreže (NIC-ovi) koja omogućuju virtualnim strojevima mrežni promet primanje raspoređivača opterećenja
- **Pravila za ujednačavanje opterećenja**: sadrži pravila koja mapiranje javno priključak na raspoređivača opterećenja priključak u pozadinskoj adresa
- **NAT ulazna pravila**: sadrži pravila koja mapiranje javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u pozadinskoj adresa
- **Probes**: sadrži probes stanja koji se koriste za Provjera dostupnosti virtualnim strojevima instanci u pozadinskoj adresa

Dodatne informacije potražite u članku [Upravljanje resursima Azure podrške za raspoređivača opterećenja](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Postavljanje EŽA da biste koristili Voditelj resursa

1. Ako još niste koristili Azure EŽA, pročitajte članak [instalirati i konfigurirati EŽA Azure](../../articles/xplat-cli-install.md). Slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokrenite naredbu **azure config način** da biste prešli u način Voditelj resursa, na sljedeći način:

        azure config mode arm

    Očekivani izlaz:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Stvaranje internog opterećenja korak po korak

1. Prijavite se u Azure.

        azure login

    Kada se to od vas zatraži, unesite vjerodajnice za Azure.

2. Promijenite Alati naredba Voditelj resursa Azure način.

        azure config mode arm

## <a name="create-a-resource-group"></a>Stvaranje grupa resursa

Resursi za sve u Azure Voditelj resursa su vezane uz grupu resursa. Ako to već niste napravili još, stvorite grupu resursa.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Stvaranje skupa raspoređivača učitavanja interni

1. Stvaranje internog opterećenja

    U sljedećem scenariju grupu resursa pod nazivom nrprg se stvara u regiji Istočni SAD-a.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Svi resursi za interne učitavanja balancers, kao što su virtualne mreže i podmreže virtualne mreže mora biti u istoj grupi resursa i na istom području.

2. Stvaranje pristupne IP adresa za interne opterećenja.

    IP adresa koji koristite mora biti unutar raspona podmreže virtualne mreže.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Stvorite na skupna pozadinsku adresa.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Kada definirate sučelja IP adresa i na skupna pozadinsku adresa, možete stvoriti pravila raspoređivača opterećenja, Ulazna pravila NAT i probes prilagođene stanja.

4. Stvorite pravilo raspoređivača opterećenja za interne opterećenja.

    Kada slijedite prethodne korake, naredba stvara pravilo raspoređivača opterećenja za slušanje priključak 1433 u sučelja i pošaljete uravnoteženja mrežni promet na skupna pozadinsku adresa, koristi priključak 1433.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Stvaranje ulazna pravila NAT.

    Ulazna pravila NAT se koriste za stvaranje krajnje točke u opterećenja koji idite na instancu komponente određene virtualnog računala. Prethodne korake stvoren dva NAT pravila za udaljene radne površine.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Stvaranje stanja probes za raspoređivača opterećenja.

    Stanje probni provjerava sve instance virtualnog računala da biste provjerili mogu poslati mrežni promet. Instanca virtualnog računala s probni neuspjele provjere je uklonjena iz raspoređivača opterećenja dok dolazi vratite u mrežni i provjere probni određuje je li dobar.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Platforme Microsoft Azure koristi statički javno usmjeravati IPv4 adresa raznih scenariji za administratora. IP adresa nije 168.63.129.16. Ovu IP adresu ne trebaju biti blokirane vatrozidi, tako da jer to može izazvati neočekivano ponašanje.
    >Vezana uz Azure Interna učitavanja opterećenja ovu IP adresu koristi prateći probes iz raspoređivača opterećenja za određivanje stanja za virtualnim strojevima u skupu uravnoteženja. Ako sigurnosne grupe mreže se koristi da biste ograničili promet Azure virtualnim strojevima u skupu interno raspoređivačem ili primjenjuje se na podmreži virtualne mreže, provjerite je li pravila za sigurnost mreže dodati dopušta promet iz 168.63.129.16.

## <a name="create-nics"></a>Stvaranje NIC-ovi

Morate stvoriti NIC-ovi (ili izmijeniti postojeće) i povezati ih NAT pravila, pravila raspoređivača opterećenja i probes.

1. Stvaranje programa NIC pod nazivom *lb-nic1-biti*, i povezati je s NAT pravilo *rdp1* i pozadinske adresu skup *beilb* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Stvaranje programa NIC pod nazivom *lb-nic2-biti*, i povezati je s NAT pravilo *rdp2* i pozadinske adresu skup *beilb* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Stvaranje virtualnog računala pod nazivom *DG1*i povezati je s NIC pod nazivom *lb-nic1-se*. Račun za pohranu naziva *web1nrp* se stvara prije izvođenja sljedeću naredbu:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs u raspoređivača opterećenja moraju biti u istoj postavljanje dostupnosti. Korištenje `azure availset create` da biste stvorili raspoloživost postavite.

4. Stvaranje virtualnog računala (VM) pod nazivom *DB2*i povezati je s NIC pod nazivom *lb-nic2-se*. Račun za pohranu naziva *web1nrp* stvorena prije pokretanjem sljedeće naredbe.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Brisanje raspoređivača opterećenja

Da biste uklonili raspoređivača opterećenja, koristite sljedeću naredbu:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina raspodjele raspoređivača učitavanja pomoću afiniteta izvorni IP](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
