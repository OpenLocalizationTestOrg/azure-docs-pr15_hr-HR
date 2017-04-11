<properties
  pageTitle="Servis u Oblaku povezati kontroler prilagođene domene | Microsoft Azure"
  description="Saznajte kako se povezati na web-tempiranja uloge za prilagođenu domenu AD pomoću komponente PowerShell i nastavak domene servisa Active Directory"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Povezivanje Azure oblaka servisa uloge prilagođeni kontroler domene AD smješten u Azure

Ne možemo će najprije postavite virtualne mreže (VNet) u Azure. Zatim dodat ćemo kontroler o domene Active Directory u (hostirane na programa Azure virtualnog računala) da biste na VNet. Zatim ćemo će dodati postojeći uloge servisa oblaka unaprijed stvoreni VNet i naknadno povezati s kontrolerom domene.

Prije nego što smo početak nekoliko stvari koje treba imati na umu:

1.  Pomoću ovog praktičnog vodiča koristi PowerShell, pa vas molimo da provjerite ima li Azure PowerShell instalirana i spremna za slanje. Pomoć pri postavljanju Azure PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

2.  Na Web-tempiranja ulogu i kontroler domene AD instance moraju biti u VNet.

Slijedite ovaj Postupni vodič, a Ako naiđete na probleme, napišite nam komentar ispod. Netko će se vratiti na vas (da, ne možemo čitanje komentara).

3. Mreža za koju se pozivaju oblaka servisa <mark>mora biti</mark> **Klasični virtualne mreže**.

## <a name="create-a-virtual-network"></a>Stvaranje virtualne mreže

Možete stvoriti virtualne mreže u Azure pomoću portala za Azure klasični ili PowerShell. Za ovaj vodič koristit ćemo PowerShell. Da biste stvorili virtualne mreže pomoću portala za Azure klasični, potražite u članku [Stvaranje virtualne mreže](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Stvaranje virtualnog računala

Nakon što ste dovršili postavljanje virtualne mreže, morat ćete stvoriti programa kontroler domene servisa Active Directory. Za ovaj vodič smo će biti postavljanje domene AD kontroler na programa Azure virtualnog računala.

Da biste to učinili, stvorite virtualnog računala putem komponente PowerShell pomoću naredbi u nastavku:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Promicanje virtualnog računala s kontrolerom domene
Da biste konfigurirali virtualnog računala kao programa kontroler domene servisa Active Directory, morat ćete se prijaviti u na VM i konfigurirajte je.

Da biste se prijavili na VM, možete dobiti datoteku RDP putem komponente PowerShell, koristite naredbe u nastavku.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Kada ste prijavljeni u VM, postavljanje virtualnog računala kao programa kontroler domene AD slijedeći Postupni vodič za [Postavljanje klijentu kontroler domene servisa Active Directory](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Dodavanje servis u Oblaku virtualne mreže

Ćete morati dodati servis za implementaciju sustava oblaka VNet koji ste upravo stvorili. Da biste to učinili, izmijenite svoje cscfg servisa oblaka dodavanjem odjeljke vaše cscfg pomoću Visual Studio ili uređivač po izboru.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Zatim sastavljanje projekta oblaka services i implementacija Azure. Da biste dobili pomoć pri implementaciji servisa pakiranju oblaka za Azure, potražite [u](cloud-services-how-to-create-deploy.md#deploy) članku Stvaranje i uvođenje servis u Oblaku

## <a name="connect-your-webworker-roles-to-the-domain"></a>Povezivanje vašeg web-tempiranja uloge domeni

Kada je servis projekta oblaka implementiran na Azure, vaša uloga instance povezati prilagođenu domenu AD korištenje nastavka AD domene. Da biste dodali nastavak domene AD postojeće uvođenja usluge oblaka i pridruživanje prilagođenu domenu, izvršiti sljedeće naredbe u ljusci PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

I to je.

Koje oblaka servisa trebala bi biti spojena sada upravljaču prilagođenu domenu. Ako želite da biste saznali više o različite mogućnosti dostupne za konfiguriranje domene AD nastavak, upotrijebite pomoć PowerShell kao što je prikazano u nastavku.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
