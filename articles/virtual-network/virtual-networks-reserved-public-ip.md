<properties
   pageTitle="Rezervirane IP | Microsoft Azure"
   description="Razumijevanje Rezervirana IP-ovi i upravljanje njima"
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

# <a name="reserved-ip-overview"></a>Pregled Rezervirana IP
IP adrese u Azure svrstati u jednu od dviju kategorija: dinamične i rezervirane. Javnu IP adrese upravlja Azure nisu dinamički prema zadanim postavkama. To znači da IP adresa za servise u oblaku za dani (VIP) ili da biste pristupili VM ili uloga instancu komponente izravno (ILPIP) možete promijeniti s vremena na vrijeme kada su resursi isključivanja ili deallocated.

Da biste spriječili promjena IP adrese, možete rezervirati IP adresa. Rezervirane IP-ovi može se koristiti samo kao VIP, jamči da IP adresa servisa u oblaku će biti iste čak i kao što su isključivanje resursi deallocated. Osim toga, možete pretvoriti postojeće dinamičke IP-ovi koriste kao VIP Rezervirana IP adresu.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako rezervirati statične javna IP adresa pomoću [model implementacije Voditelj resursa](virtual-network-ip-addresses-overview-arm.md).

Provjerite je li razumjeli kako funkcioniraju [IP adresa](virtual-network-ip-addresses-overview-classic.md) u Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Kada moram Rezervirana IP?
- **Želite biti sigurni da je u IP rezervirana u svoju pretplatu**. Ako želite rezervirati IP adresu koja će biti objavljeni iz pretplate u odjeljku sve okolnostima, trebali biste koristiti rezervirane javnu IP.  
- **Želite da se IP da biste bili u oblaku čak i u zaustavljena ili deallocated stanju (VMs)**. Ako želite na servisu za može pristupiti pomoću IP adresu koja će promijeniti čak i kada VMs u servisu oblaka su tabulatora ili deallocated.
- **Želite biti sigurni da odlazni promet od Azure koristi predvidljivi IP adresa**. Možda ćete vatrozida lokalnog konfigurirano tako da dopušta promet s određenim IP adresa. Po rezervirati IP će znati IP adresu izvora i nećete morati ažurirati pravila vatrozida zbog programa IP promjena.

## <a name="faq"></a>NAJČEŠĆA PITANJA
1. Mogu li koristiti Rezervirana IP za sve servise Azure?  
  - Rezervirane IP-ovi se može koristiti samo za VMs i vidljiva kroz na VIP oblaka servisa instancu uloge.
1. Koliko Rezervirana IP-ovi mogu imati?  
  - Trenutno sve pretplate za Azure imaju dozvolu za korištenje 20 Rezervirana IP-ovi. Međutim, možete zatražiti dodatne rezervirane IP-ovi. Posjetite stranicu [pretplate i ograničenja servisa](../azure-subscription-service-limits.md) da biste saznali više.
1. Je li naknadu za rezervirana IP-ovi?
  - Informacije o cijenama potražite [Rezervirana IP adresa cijene pojedinosti](http://go.microsoft.com/fwlink/?LinkID=398482) .
1. Kako se rezervirati IP adresu?
  - Koristite PowerShell ili [Azure upravljanja REST API -JA](https://msdn.microsoft.com/library/azure/dn722420.aspx) rezervirati IP adresu unutar određenog područja. Rezervirana IP adresa pridružen je za vašu pretplatu. Ne možete rezervirati IP adresu pomoću portala za upravljanje.
1. Mogu li koristiti to s grupom afinitet temelji VNets?
  - Rezervirane IP-ovi podržani su samo u regionalnim VNets. Nije podržana za VNets koje su vezane uz afinitet grupe. Dodatne informacije o povezivanju VNet područja ili grupe sustava afinitet potražite u članku [o regionalnim VNets i afinitet grupe](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Kako upravljati rezervirane VIPs

Prije korištenja Rezervirana IP-ovi, morate je dodati u pretplatu. Da biste stvorili Rezervirana IP skup javnu IP adrese dostupna na mjestu *Središnje SAD -a* , pokrenite sljedeću naredbu komponente PowerShell:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Obratite pozornost na to, no da ne možete navesti što IP se rezervirana. Da biste vidjeli što IP adrese rezerviranih u vašoj pretplati, pokrenite sljedeću naredbu komponente PowerShell i obratite pozornost na to vrijednosti za web-mjesto *ReservedIPName* i u okvir za *adresu*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Kada je rezervirana IP, ona ostaje povezan pretplatu dok se ne izbrišete. Da biste izbrisali Rezervirana IP gore navedenoj sintaksi, pokrenite sljedeću naredbu komponente PowerShell:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Kako rezervirati IP adrese postojeće oblaku

IP adrese postojeće oblaku možete rezervirati dodavanjem parametara *- Naziv servisa* . Da biste rezervirati IP adresu neki servis u oblaku *TestService* mjestu *Središnje SAD -a* , pokrenite sljedeću naredbu komponente PowerShell:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Kako se pridružiti Rezervirana IP na novi servis u oblaku
U nastavku skriptu stvara novi Rezervirana IP, a zatim povezuje na nove servise u oblaku pod nazivom *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Kada stvorite Rezervirana IP će se koristiti za servise u oblaku, i dalje morat ćete odnose se na VM pomoću *VIP:&lt;broj priključka >* za dolazni komunikaciju. Rezervirati IP ne znači da ne možete se povezati s VM izravno. Rezervirane IP se dodjeljuje u oblaku koja se VM implementiran. Ako želite izravno povezati VM po IP morate konfigurirati instancu razinom javnu IP. Vrsta javnu IP (naziva se na ILPIP) koji je dodijeljen izravno na VM je javnu IP instancu razinu. Ne može biti rezervirana. Dodatne informacije potražite u članku [Instancu razini javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Kako ukloniti Rezervirana IP iz izvodi implementacije
Da biste uklonili Rezervirana IP dodaje novi servis stvorene u skripti iznad, pokrenite sljedeću naredbu komponente PowerShell:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Rezervirane IP uklanjanjem izvodi implementacije uklonite rezervacija iz pretplate. Samo se oslobađa IP koriste neki drugi resurs za pretplatu.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Kako se pridružiti Rezervirana IP da biste izvodi implementacije
Ispod skripte stvara nove servise u oblaku pod nazivom *TestService2* s novi VM pod nazivom *TestVM2*i povezuje postojeće Rezervirana IP pod nazivom *MyReservedIP* na servis u oblaku.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Kako se pridružiti Rezervirana IP na servis u oblaku putem servisa konfiguracijska datoteka
Pomoću servisa konfiguracije (CSCFG) datoteke možete povezati i Rezervirana IP na servis u oblaku. Ogledna xml ispod prikazuje kako konfigurirati servis u oblaku da biste koristili rezervirane VIP pod nazivom *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Daljnji koraci

- Objašnjenje mogućnosti [adresiranja IP](virtual-network-ip-addresses-overview-classic.md) funkcioniranje u modelu klasični implementacije.

- Saznajte više o [rezervirane privatne IP adrese](virtual-networks-reserved-private-ip.md).

- Informacije o [adresama u instancu razinu javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md).
