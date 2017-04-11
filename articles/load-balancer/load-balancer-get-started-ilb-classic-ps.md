<properties
   pageTitle="Stvaranje programa Interna opterećenja pomoću komponente PowerShell u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako stvoriti na interni opterećenja pomoću komponente PowerShell u modelu klasični implementacije"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Početak rada prilikom stvaranja za interne opterećenja (klasični) pomoću komponente PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Stvaranje programa Interna opterećenja za virtualnim strojevima

Da biste stvorili skupa opterećenja Interna opterećenja i poslužitelje koji će se slati svojim promet na njega, to morate učiniti sljedeće:

1. Stvaranje instance komponente Interna učitati opterećenja koji će biti krajnje promet se rasporediti na poslužiteljima skupa uravnoteženja opterećenje.

1. Dodavanje krajnje točke odgovara virtualnim strojevima koje će primati dolazne promet.

1. Konfiguriranje s poslužiteljima koji će slanja promet se rasporediti da biste poslali njihove promet na adresu virtualne IP (VIP) instance Interna učitati opterećenja opterećenje.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Korak 1: Stvaranje internog opterećenja instance

Za postojeće oblaku ili servis u oblaku u uveden u odjeljku regionalne virtualne mreže, možete stvoriti instancu Interna opterećenja s sljedeće naredbe komponente Windows PowerShell:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Imajte na umu da ova koristite cmdlet za [Dodavanje AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell koristi skup parametara DefaultProbe. Dodatne informacije o skupovima parametara za dodatna potražite u članku [Dodavanje AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Korak 2: Dodavanje krajnje točke na instancu Interna opterećenja

Evo jednog primjera:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Korak 3: Konfiguriranje poslužitelja da biste poslali njihove promet na interni opterećenja krajnju točku

Imate konfiguraciju poslužitelja čije promet će biti rasporediti koristiti novu IP adresu (VIP) instance Interna učitati opterećenja opterećenje. To je adresa na kojoj se instancu Interna opterećenja priključuje. U većini slučajeva morate samo dodavanje ili promjenu DNS zapisa za VIP instance Interna opterećenja.

Ako ste odredili IP adresu tijekom stvaranja instance Interna opterećenja, već imate na VIP. U suprotnom, vidjet ćete VIP sljedeće naredbe:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Da biste koristili te naredbe, unijeli vrijednosti i uklonite na < i >. Evo jednog primjera:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Prikaz naredbe Get-AzureInternalLoadBalancer, imajte na umu IP adresa i unesite potrebne promjene s poslužitelja ili DNS zapisa da biste bili sigurni da će promet poslati na VIP.

>[AZURE.NOTE]Platforme Microsoft Azure koristi statički javno usmjeravati IPv4 adresa raznih scenariji za administratora. IP adresa nije 168.63.129.16. Ovu IP adresu ne trebaju biti blokirane vatrozidi, tako da jer on može izazvati neočekivano ponašanje.
>Vezana uz Azure Interna učitati opterećenja, ovu IP adresu koristi prateći probes iz raspoređivača opterećenja za određivanje stanja za virtualnim strojevima u skupu rasporediti opterećenje. Ako sigurnosne grupe mreže se koristi da biste ograničili promet Azure virtualnim strojevima u skupu interno raspoređivačem ili primjenjuje se na podmreži virtualne mreže, provjerite je li pravila za sigurnost mreže dodati dopušta promet iz 168.63.129.16.


## <a name="example-of-internal-load-balancing"></a>Primjer interni opterećenja

Da biste se pomicali kroz Završi proces na kraj stvaranja skupa uravnoteženja za dva primjer konfiguracije, potražite u sljedećim odjeljcima.

### <a name="an-internet-facing-multi-tier-application"></a>Putem programa za internetsku nasuprotne, aplikacije više razina

Želite li omogućiti rasporediti opterećenje baze podataka usluge za skup poslužiteljima web-mjesto na Internetu. Oba skupa poslužitelje nalaze se u jednom Azure u oblaku. Promet poslužitelja web TCP priključka 1433 moraju se distribuirati između dva virtualnim strojevima u sloju baze podataka. Slika 1 prikazuje konfiguracije.

![Interna uravnoteženja skup za sloju baze podataka](./media/load-balancer-internal-getstarted/IC736321.png)


Konfiguracija sastoji se od sljedećeg:

- Postojeće servisa u oblaku hosting virtualnim strojevima pod nazivom mytestcloud.

- Dva postojeću bazu podataka poslužitelja imenovane su DG1, DB2.

- Web-poslužiteljima u sloju web se povezuje s poslužiteljima baze podataka u sloju baze podataka pomoću privatne IP adrese. Druga mogućnost je korištenje vlastitim DNS za virtualne mreže i ručno registrirali A zapis za skup opterećenja Interna Učitaj.

Sljedeće naredbe konfiguracija nove Interna opterećenja instance pod nazivom **ILBset** i dodavanje krajnje točke virtualnih računala koja odgovara poslužitelje dva baza podataka:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Uklanjanje konfiguracije za interne opterećenja

Da biste uklonili virtualnog računala kao krajnje s instancu raspoređivača učitavanja Interna, koristite sljedeće naredbe:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Da biste koristili te naredbe, unijeli vrijednosti, uklonite na < i >.

Evo jednog primjera:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Da biste uklonili instancu opterećenja Interna Učitaj na neki servis u oblaku, koristite sljedeće naredbe:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Da biste koristili te naredbe, unesite vrijednost i ukloniti na < i >.

Evo jednog primjera:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Dodatne informacije o cmdletima opterećenja Interna učitavanja


Da biste dobili dodatne informacije o cmdletima za interne opterećenja, upit komponente Windows PowerShell pokrenite sljedeće naredbe:

- Pomoć novi AzureInternalLoadBalancerConfig-puni

- Pomoć dodavanje AzureInternalLoadBalancer-puni

- Pristup pomoći Get-AzureInternalLoadbalancer-puni

- Pomoć Ukloni AzureInternalLoadBalancer-puni

## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina distribucije raspoređivača učitavanja pomoću afiniteta izvorni IP](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)