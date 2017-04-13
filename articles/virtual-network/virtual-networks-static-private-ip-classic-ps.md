<properties 
   pageTitle="Upute za postavljanje statičke IP privatni klasičnog načina pomoću komponente PowerShell | Microsoft Azure"
   description="Razumijevanje statične privatne IP-ovi (DIPs) i upravljanje njima u klasičan način rada i PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Kako postaviti statičke privatne IP adrese (klasični) u ljusci PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Upravljanje statičke privatne IP adrese u modelu implementacije Voditelj resursa](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ispod naredbe ljuske PowerShell uzorka očekivati okruženju jednostavne već stvorili. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje opisane u [Stvaranje na VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)okruženje za testiranje.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Kako provjeriti je li određene IP adrese dostupna
Da biste provjerili je li adresa IP *192.168.1.101* dostupne u VNet pod nazivom *TestVNet*, pokrenite sljedeću naredbu komponente PowerShell i provjerite je li vrijednost za *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Očekivani izlaz:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kako odrediti statičke privatne IP adrese prilikom stvaranja na VM
Ispod skriptu PowerShell stvara novi servis u oblaku pod nazivom *TestService*, a zatim dohvaća sliku iz Azure, stvara VM pod nazivom *DNS01* u novi servisa u oblaku pomoću dohvaćene slika, postavlja VM u podmreži pod nazivom *sučelju*i postavlja *192.168.1.7* kao statičke privatne IP adrese za na VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Očekivani izlaz:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Upute za dohvaćanje statične privatne IP adresa informacija o na VM
Da biste pogledali statički privatne podataka IP adrese za VM stvorene pomoću skripte iznad, pokrenite sljedeću naredbu komponente PowerShell i pridržavajte vrijednosti za *IPAdresa*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Očekivani izlaz:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kako ukloniti statičke privatne IP adrese iz programa VM
Da biste uklonili privatne statičke IP adrese dodati VM u skripti iznad, pokrenite sljedeću naredbu komponente PowerShell:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Očekivani izlaz:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kako dodati statične privatne IP adrese postojeće VM
Da biste dodali u statični privatne IP adresu da biste VM stvoren pomoću skripte iznad runt on naredbe:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Očekivani izlaz:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rezervirane javnu IP](virtual-networks-reserved-public-ip.md) adrese.
- Informacije o adresama u [instancu razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Pogledajte [Rezervirana IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx).
