<properties 
   pageTitle="Kako postaviti statičke privatne IP adrese u Azure Voditelj resursa pomoću komponente PowerShell | Microsoft Azure"
   description="Razumijevanje statičke privatne IP adrese i upravljanje njima u Azure Voditelj resursa pomoću komponente PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-resource-manager-by-using-powershell"></a>Kako postaviti statičke privatne IP adrese u upravitelju resursa pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [Upravljanje statičke privatne IP adrese u modelu klasični implementacije](virtual-networks-static-private-ip-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Na temelju uzorka PowerShell naredbe ispod očekivati okruženju jednostavne već stvorili scenarij iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje opisane u [Stvaranje na vnet](virtual-networks-create-vnet-arm-ps.md)okruženje za testiranje.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kako odrediti statičke privatne IP adrese prilikom stvaranja na VM
Da biste stvorili VM pod nazivom *DNS01* u *sučelju* podmreži VNet pod nazivom *TestVNet* pomoću statičke IP privatne od *192.168.1.101*, slijedite ove korake:

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Postavljanje varijable za račun za pohranu, mjesto, grupa resursa i vjerodajnice koje će se koristiti. Morat ćete unijeti korisničko ime i lozinku za na VM. Grupa računa i resursa za pohranu mora već postojati.

        $stName = "vnetstorage"
        $locName = "Central US"
        $rgName = "TestRG"
        $cred = Get-Credential -Message "Type the name and password of the local administrator account."

3. Dohvaćanje virtualne mreže i podmreže koju želite stvoriti VM u.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet  
        $subnet = $vnet.Subnets[0].Id

4. Ako je potrebno, stvorite javnu IP adresu da biste pristupili na VM s Interneta.

        $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

5. Stvaranje NIC pomoću statičke privatne IP adrese koje želite dodijeliti na VM. Provjerite je li na IP podmreže rasponu dodajete VM za. Ovo je glavni korak za ovaj članak koju postavljate IP privatne biti statične.

        $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.1.101

6. Stvaranje VM pomoću NIC stvorena iznad.

        $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
        $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01  -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
        $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
        $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
        $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri -CreateOption fromImage
        New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 

    Očekivani izlaz:

        EndTime             : 9/8/2015 2:32:09 PM -07:00
        Error               : 
        Output              : 
        StartTime           : 9/8/2015 2:27:42 PM -07:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK 


## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Upute za dohvaćanje statične privatne IP adresa informacija o na VM
Da biste pogledali statički privatne podataka IP adrese za VM stvorene pomoću skripte iznad, pokrenite sljedeću naredbu komponente PowerShell i pridržavajte vrijednosti za *PrivateIpAddress* i *PrivateIpAllocationMethod*:

    Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG

Očekivani izlaz:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/Te
                           stNIC
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMach
                           ines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkIn
                           terfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtual
                           Networks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicI
                           PAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kako ukloniti statičke privatne IP adrese iz programa VM
Da biste uklonili privatne statičke IP adrese dodati VM u skripti iznad, pokrenite sljedeće naredbe ljuske PowerShell:
    
    $nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
    $nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
    Set-AzureRmNetworkInterface -NetworkInterface $nic

Očekivani izlaz:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/Te
                           stNIC
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMach
                           ines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkIn
                           terfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtual
                           Networks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicI
                           PAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kako dodati statične privatne IP adrese postojeće VM
Da biste dodali u statični privatne IP adresu da biste VM stvoren pomoću skripte iznad runt on naredbe:

    $nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
    $nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
    $nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
    Set-AzureRmNetworkInterface -NetworkInterface $nic

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rezervirane javnu IP](virtual-networks-reserved-public-ip.md) adrese.
- Informacije o adresama u [instancu razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Pogledajte [Rezervirana IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx).