<properties
    pageTitle="Stvaranje internetsku nasuprotne opterećenja s IPv6 u Azure Voditelj resursa pomoću EŽA Azure | Microsoft Azure"
    description="Saznajte kako stvoriti internetsku nasuprotne opterećenja s IPv6 u Azure Voditelj resursa pomoću EŽA Azure"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure opterećenja, dvostruki snop, javnu ip, izvorni ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Stvaranje internetsku nasuprotne opterećenja s IPv6 u Azure Voditelj resursa pomoću EŽA Azure

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure EŽA](./load-balancer-ipv6-internet-cli.md)
- [Predložak](./load-balancer-ipv6-internet-template.md)

Azure opterećenja je opterećenja za sloj-4 (TCP, UDP). Raspoređivača opterećenja nudi visoke dostupnosti raspodijeliti dolazne promet među instanci dobar servisa u oblaku servise ili virtualnim strojevima u skupu raspoređivača opterećenja. Azure opterećenja također možete prikazati tih servisa na više priključaka, više IP adresa ili i jedno i drugo.

## <a name="example-deployment-scenario"></a>Primjer implementacije scenarija

Na sljedećem su dijagramu ilustrira rješenja za ujednačavanje opterećenja uvodi pomoću predloška za primjer opisane u ovom članku.

![Scenarij raspoređivača učitavanja](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

U ovom scenariju ćete stvoriti u sljedećim resursima Azure:

- dva virtualnim strojevima (VMs)
- virtualne mreže sučelje za svaki VM IPv4 i IPv6 adrese dodijeljeno
- mjesto na Internetu raspoređivača opterećenja s na IPv4 i IPv6 javnu IP adresa
- Dostupnost postavljena na koji sadrži dvije VMs
- dva učitavanje ujednačavanje pravila za mapiranje javno VIPs privatne krajnje točke

## <a name="deploying-the-solution-using-the-azure-cli"></a>Implementacija rješenja pomoću EŽA Azure

Na sljedeći način prikažite kako stvoriti internetsku nasuprotne opterećenja Voditelj resursa Azure pomoću EŽA. Pomoću upravitelja Azure resursa, svaki resurs je stvorio i pojedinačno konfigurirani, a zatim stavite zajedno da biste stvorili resurs.

Da biste implementirali raspoređivača opterećenja, stvaranje i konfiguriranje sljedeće objekte:

- Pristupne Konfiguracija IP - sadrži javnu IP adresa za dolazne mrežni promet.
- Skupna pozadinsku adresa – sadrži sučelje mreže (NIC-ovi) za virtualnim strojevima mrežni promet primanje raspoređivača opterećenja.
- Opterećenja pravila - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak u pozadinskoj adresu.
- Ulazna pravila NAT - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u pozadinskoj adresu.
- Probes – sadrži stanje probes koristi da biste provjerili dostupnost virtualnim strojevima instanci u pozadinskoj adresu.

Dodatne informacije potražite u članku [Upravljanje resursima Azure podrške za raspoređivača opterećenja](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Postavljanje okruženja EŽA da biste koristili Voditelj resursa za Azure

Primjerice, ne možemo izvodi EŽA Alati u prozoru za naredbe ljuske PowerShell. Ne možemo su pomoću cmdleta ljuske PowerShell Azure, ali koristimo mogućnosti skriptiranja PowerShell korisnika za poboljšanje čitljivosti i ponovno korištenje.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokrenite naredbu **azure config način** da biste prešli u način upravljanja resursima.

        azure config mode arm

    Očekivani izlaz:

        info:    New mode is arm

3. Prijavite se u Azure i dobit ćete popis pretplata.

        azure login

    Unesite vjerodajnice Azure kada se to od vas zatraži.

        azure account list

    Odaberite pretplatu koju želite koristiti. Zabilježite Id pretplate za sljedeći korak.

4. Postavljanje varijable ljuske PowerShell za korištenje s naredbama EŽA.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Stvaranje grupe resursa, raspoređivača opterećenja, virtualne mreže i podmreže

1. Stvaranje grupa resursa

        azure group create $rgName $location

2. Stvaranje raspoređivača opterećenja

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Stvaranje virtualne mreže (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Stvorite dva podmreže u ovom VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Stvaranje javne IP adrese za sučelja skup

1. Postavljanje varijable PowerShell

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Stvaranje pristupne Skupna IP javnu IP adresa.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Raspoređivača opterećenja koristi oznaku domene javnu IP kao njegov FQDN. To promjenu klasični implementacije koja koristi servis u oblaku naziv kao raspoređivača opterećenja FQDN.
    >U ovom se primjeru FQDN je *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Stvaranje sučelja i stražnje grupe

U ovom se primjeru stvara sučelja Skupna IP koja prima dolazne mrežnog prometa na raspoređivača opterećenja i skupna pozadinsku IP gdje sučelja skup šalje rasporediti opterećenje mrežni promet.

1. Postavljanje varijable PowerShell

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Stvaranje pristupne Skupna IP zastavicom javnu IP stvorili u prethodnom koraku, a raspoređivača opterećenja.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Stvorite probni, NAT pravila i LB pravila

U ovom se primjeru stvara sljedećih stavki:

- Probni pravila za provjeru za povezivanje s priključkom TCP 80
- NAT pravilo da biste preveli sve dolazne promet na priključak 3389 priključak 3389 za RDP<sup>1</sup>
- NAT pravilo da biste preveli sve dolazne promet na priključak 3391 priključak 3389 za RDP<sup>1</sup>
- pravilo raspoređivača opterećenja za saldo sve dolazne promet na priključak 80 priključak 80 na adrese u pozadinskoj.

<sup>1</sup> NAT pravila povezane su s instancom određene virtualnog računala iza raspoređivača opterećenja. Mrežni promet stiže na priključak 3389 šalju određene virtualnog računala i priključak povezana s pravilom NAT. Morate navesti protocol (UDP ili TCP) za NAT pravilo. Oba protokoli ne može biti dodijeljena isti priključak.

1. Postavljanje varijable PowerShell

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Stvaranje na probni

    Sljedeći primjer stvara na TCP probni koji provjerava za povezivanje s priključkom pozadinske TCP 80 svakih 15 sekundi. Ga će označiti resursa pozadinske nedostupne nakon dva uzastopna neuspješna pokušaja.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Stvaranje ulaznog NAT pravila koja omogućuju RDP veze na resurse pozadinske

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Stvaranje raspoređivača opterećenja pravila koji šalju promet na različite pozadinske priključke ovisno o tome koje sučelje primio zahtjev

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Provjerite postavke

        azure network lb show --resource-group $rgName --name $lbName

    Očekivani izlaz:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Stvaranje NIC-ovi

Stvaranje NIC-ovi i povezati ih NAT pravila, pravila raspoređivača opterećenja i probes.

1. Postavljanje varijable PowerShell

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Stvorite NIC za svaki pozadinske i dodajte je konfiguracije IPv6.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Stvaranje pozadinske VM resursa i priložite svaki NIC

Da biste stvorili VMs, morate imati račun za pohranu. Opterećenja, na VMs moraju biti članovi skupa dostupnost. Dodatne informacije o stvaranju VMs potražite u članku [Stvaranje programa VM Azure pomoću komponente PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. Postavljanje varijable PowerShell

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] U ovom se primjeru koristi korisničko ime i lozinku za VMs u običan tekst. Odgovarajući, potrebno je poduzeti prilikom korištenja vjerodajnice u isključite. Sigurnije način obrade vjerodajnice u ljusci PowerShell potražite u članku cmdlet [Get-vjerodajnica](https://technet.microsoft.com/library/hh849815.aspx) .

2. Stvaranje spremišta račun i dostupnosti skupa

    Postojeći račun za pohranu možete koristiti prilikom stvaranja na VMs. Sljedeća naredba stvara novi račun za pohranu.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Nakon toga Stvaranje skupa dostupnost.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Stvaranje virtualnim strojevima sa pridružene mrežne kartice

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-cli.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
