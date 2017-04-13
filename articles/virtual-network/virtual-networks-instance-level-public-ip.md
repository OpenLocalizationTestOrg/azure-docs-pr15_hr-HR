<properties 
   pageTitle="Instancu razinu javnu IP (ILPIP) | Microsoft Azure"
   description="Razumijevanje ILPIP (TOČAKA) i upravljanje njima"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Instance razine javnu IP pregled
Instance razine javnu IP (ILPIP) je javnu IP adresa koje možete dodijeliti izravno u vašoj instanci VM ili uloge, a ne u oblaku koje se nalaze na instancu VM ili uloga u. To ne odvija VIP (virtualne IP) koji je dodijeljen servis u oblaku. Umjesto toga je dodatne IP adresa koje možete koristiti da biste se povezali izravno u vašoj instanci VM ili uloga.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](virtual-network-ip-addresses-overview-arm.md). 

Provjerite je li razumjeli kako funkcioniraju [IP adresa](virtual-network-ip-addresses-overview-classic.md) u Azure.

>[AZURE.NOTE] U prošlosti programa ILPIP je se nazivaju TOČAKA kratica za javnu IP. 

![Razlika između ILPIP i VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Kako je prikazano na slici 1, servis u oblaku je pristupiti pomoću VIP, dok pojedinačne VMs je obično moguće pristupiti pomoću VIP:&lt;broj priključka&gt;. Dodjelom programa ILPIP određene VM VM može se pristupiti izravno pomoću tog IP adresa.

Kada stvorite servis u oblaku u Azure, odgovarajući zapisi DNS-A automatski se stvaraju da biste omogućili pristup servisu putem na potpuno kvalificirani naziv domene (FQDN) umjesto stvarnih VIP. Isti postupak je ILPIP, dopuštanja pristupa instancu VM ili uloga po FQDN umjesto na ILPIP. Na primjer, ako stvarate neki servis u oblaku pod nazivom *contosoadservice*i konfiguriranje uloga web naziva *contosoweb* s dvije instance, Azure će registrirati sljedećeg A zapisa za instance:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Možete dodijeliti samo jedan ILPIP za svaku instancu VM ili uloga. Koristite najviše 5 ILPIP korisnika po pretplati. Trenutačno ILPIP nije podržana za višestruki NIC VMs.

## <a name="why-should-i-request-an-ilpip"></a>Zašto zahtjev je ILPIP?
Ako želite da biste se mogli povezati na vašoj instanci VM ili uloga tako da je IP adresa dodijeljena izravno, umjesto korištenja oblaka servisa VIP:&lt;broj priključka&gt;, zahtjev je ILPIP za vaše VM ili vaša uloga instance.
- **Pasivni FTP** - tako da vas se ILPIP na VM, možete dobiti promet na gotovo bilo kojeg priključak nećete imati otvorite krajnje prima promet. Time se omogućuje scenariji kao što su pasivni FTP gdje su priključke odabrali dinamički.
- **Izlaznih IP** - odlazni promet potječu na VM prelazi s ILPIP kao izvor i to služi kao jedinstvena identifikacija VM za vanjske entiteti.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Upute za upućivanje zahtjeva za ILPIP tijekom stvaranja VM
Ispod skriptu PowerShell stvara novi servis u oblaku pod nazivom *FTPService*, a zatim dohvaća sliku iz Azure, i stvara VM pod nazivom *FTPInstance* pomoću dohvaćene slike, postavlja VM za korištenje programa ILPIP i dodaje u VM nove usluge:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Upute za dohvaćanje ILPIP informacija o na VM
Da biste pogledali podatke ILPIP za VM stvorene pomoću skripte iznad, pokrenite sljedeću naredbu komponente PowerShell i pridržavajte vrijednosti za *PublicIPAddress* i *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Kako ukloniti programa ILPIP iz programa VM
Da biste uklonili ILPIP dodali VM u skripti iznad, pokrenite sljedeću naredbu komponente PowerShell:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Kako dodati programa ILPIP postojeće VM
Da biste dodali programa ILPIP VM stvoren pomoću skripte iznad, pokrenite sljedeću naredbu:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Upute za povezivanje programa ILPIP da biste na VM pomoću servisa konfiguracijska datoteka
Pomoću servisa konfiguracije (CSCFG) datoteke možete povezati i programa ILPIP da biste na VM. Ogledna xml ispod prikazuje kako konfigurirati servis u oblaku za korištenje programa ILPIP pod nazivom *MyPublicIP* uloga instance: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Daljnji koraci

- Objašnjenje mogućnosti [adresiranja IP](virtual-network-ip-addresses-overview-classic.md) funkcioniranje u modelu klasični implementacije.

- Saznajte više o [Rezervirana IP-ovi](virtual-networks-reserved-public-ip.md).
 