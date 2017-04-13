<properties 
   pageTitle="Kako postaviti statičke IP Interna privatne"
   description="Razumijevanje statične interne IP-ovi (DIPs) i upravljanje njima"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Kako postaviti na statičke IP adrese interne privatne pomoću komponente PowerShell (Classic)
U većini slučajeva, nećete morati navesti statičke interne IP adrese za virtualnog računala. VMs u virtualne mreže automatski će primiti internu IP adresu iz raspona koji navedete. No u određenim slučajevima, određivanje statičke IP adrese za određeni VM smisla. Na primjer, ako je vaš VM namjeravate pokrenuti DNS-a ili bit će kontroler domene. Statički interne IP adresa ostaje s VM čak i do zaustavljanja/deprovision stanje. 

> [AZURE.IMPORTANT] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa i classic](../resource-manager-deployment-model.md). U ovom se članku opisuje pomoću klasične implementacije modela. Microsoft preporučuje da najčešće novi implementacijama koristiti [model implementacije Voditelj resursa](virtual-networks-static-private-ip-arm-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Kako provjeriti je li određene IP adrese dostupna
Da biste provjerili je li adresa IP *10.0.0.7* dostupne u vnet pod nazivom *TestVnet*, pokrenite sljedeću naredbu komponente PowerShell i provjerite je li vrijednost za *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Ako želite da biste testirali naredbu iznad u sigurnom okruženju slijedite smjernice za [Stvaranje virtualne mreže](virtual-networks-create-vnet-classic-portal.md) da biste stvorili na vnet pod nazivom *TestVnet* i provjerite je li koristi *10.0.0.0/8* adresni prostor.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Kako odrediti statičke IP Interna prilikom pisanja na VM
Ispod skriptu PowerShell stvara novi servis u oblaku pod nazivom *TestService*, zatim dohvaća sliku iz Azure, a zatim stvara VM pod nazivom *TestVM* u novi servisa u oblaku pomoću dohvaćene slike, postavlja VM u podmreže pod nazivom *podmreže 1*i postavlja *10.0.0.7* kao statičke IP Interna na VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Upute za dohvaćanje statične interne IP informacija o na VM
Da biste pogledali statički interne IP podatke za VM stvorene pomoću skripte iznad, pokrenite sljedeću naredbu komponente PowerShell i pridržavajte vrijednosti za *IPAdresa*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Kako ukloniti statičke IP Interna iz programa VM
Da biste uklonili statičke IP Interna dodali VM u skripti iznad, pokrenite sljedeću naredbu komponente PowerShell:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Kako dodati statičke IP Interna postojeće VM
Da biste dodali u statični interne IP da biste na VM stvoren pomoću skripte iznad runt on naredbe:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Daljnji koraci

[Rezervirane IP](virtual-networks-reserved-public-ip.md)

[Instance razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Rezervirane IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
