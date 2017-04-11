<properties
   pageTitle="Stvaranje i izmjena je elektronička ExpressRoute pomoću model klasični implementacije i PowerShell | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz korake za stvaranje i dodjeljivanje je elektronička ExpressRoute. U ovom se članku se objašnjava da biste provjerili status, ažuriranje ili brisanje i deprovision vaše elektronička."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Stvaranje i izmjena je elektronička ExpressRoute

> [AZURE.SELECTOR]
[Azure portala - resursima](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell – resursima](expressroute-howto-circuit-arm.md)
[PowerShell – klasični](expressroute-howto-circuit-classic.md)

U ovom se članku vodit će vas kroz korake da biste stvorili programa Azure ExpressRoute elektronička pomoću cmdleta ljuske PowerShell i model klasični implementacije. U ovom se članku i vidjet ćete kako provjeriti stanje, ažuriranje ili brisanje pa deprovision je elektronička ExpressRoute.

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Prije početka

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Pregledajte preduvjeti i članci tijeka rada

Pripazite da pregledate [preduvjeti](expressroute-prerequisites.md) i [tijekove rada](expressroute-workflows.md) prije nego što počnete konfiguracije.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. instalirajte najnoviju verziju modula Azure PowerShell 

Slijedite upute u [načinu instaliranja i konfiguriranja Azure PowerShell](../powershell-install-configure.md) postupne upute o tome kako konfigurirati računalo da biste koristili modula Azure PowerShell.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. prijavite se na račun za Azure i odaberite pretplatu

1. Pokrenite sljedeći cmdlet pomoću komponente Windows PowerShell odzivnik povećane:

        Add-AzureAccount
2. Na zaslon za prijavu u koji se pojavljuje, prijavite na račun.

3. Dobit ćete popis pretplate.

        Get-AzureSubscription
4. Odaberite pretplatu u koju želite koristiti.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Stvaranje i Dodjela je elektronička ExpressRoute

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. uvezite PowerShell module za ExpressRoute

 Ako već niste učinili, morate uvesti modula Azure i ExpressRoute u sesiju ljuske PowerShell za pokretanje pomoću cmdleta ExpressRoute. Uvezite module s mjesta na kojemu su instalirani da biste na lokalnom računalu. Ovisno o načina koji se koristi da biste instalirali module, mjesto mogu se razlikovati od prikazano u sljedećem primjeru. Prema potrebi izmijenite primjera.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. dohvatiti popis podržanih davatelje usluga, mjesta i bandwidths

Stvorite je elektronička ExpressRoute, morate na popisu podržanih povezivanje davatelji, mjesta i mogućnosti propusnosti.

Cmdlet ljuske PowerShell `Get-AzureDedicatedCircuitServiceProvider` vraća ove informacije koje će se koristiti u noviji korake:

    Get-AzureDedicatedCircuitServiceProvider

Provjerite nalazi li se vaš davatelj usluga za povezivanje dva. Zabilježite sljedeće informacije jer morat ćete ga kasnije prilikom stvaranja je elektronička:

- Ime

- PeeringLocations

- BandwidthsOffered

Sada ste spremni stvoriti je elektronička ExpressRoute.         

### <a name="3-create-an-expressroute-circuit"></a>3. stvaranje je elektronička ExpressRoute

Sljedeći primjer pokazuje kako stvoriti 200 Mbps elektronička ExpressRoute kroz Equinix u Silicon udubljenje. Ako koristite nekog drugog davatelja usluge i različite postavke, zamijenite te podatke kada vaš zahtjev.

>[AZURE.IMPORTANT] Vaš elektronička ExpressRoute će se naplatiti od trenutka izdavanja ključa usluge. Provjerite je li kada davatelj usluga za povezivanje je spremna za dodjelu resursa u elektronička izvođenje ovog postupka.


Slijedi primjer traženje novog ključa servisa:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Ili, ako želite da biste stvorili je elektronička ExpressRoute dodatak premium, u sljedećem primjeru. Pogledajte [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md) za dodatne pojedinosti o dodatak premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Odgovor će sadržavati ključa usluge. Detaljne opise sve parametre možete dobiti pokretanjem sljedeće:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. popis svih ExpressRoute krugova

Možete pokrenuti na `Get-AzureDedicatedCircuit` naredba dobit ćete popis krugova ExpressRoute koji ste stvorili:


    Get-AzureDedicatedCircuit

Odgovor bit će nešto slično kao u sljedećem primjeru:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Ove informacije u bilo kojem trenutku možete dohvatiti pomoću na `Get-AzureDedicatedCircuit` cmdlet. Uputite poziv bez parametre navodi sve krugova. Ključ servisa će biti naveden u polju *ServiceKey* .

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Detaljne opise sve parametre možete dobiti pokretanjem sljedeće:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. poslati ključa usluge povezivanja davatelju za dodjelu resursa


*ServiceProviderProvisioningState* nudi informacije o trenutnom stanju aktiviranja na strani davatelja usluga. *Status* nudi stanje na strani Microsoft. Dodatne informacije o elektronička dodjeljivanje stanja potražite u članku [Tijekovi rada](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Prilikom stvaranja novog elektronička ExpressRoute na elektronička bit će u stanju sljedeće:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Na elektronička bit će se sljedeće stanje kada je u tijeku omogućiti za koje je davatelj usluga za povezivanje:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Elektronička se ExpressRoute moraju biti u sljedeće stanje umjesto vas da biste mogli koristiti:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. povremeno status i stanje ključa elektronička

To obavještava kada vaš davatelj usluga omogućio vaše elektronička. Nakon što u elektronička konfiguriran, *ServiceProviderProvisioningState* će se prikazivati kao *Provisioned* kao što je prikazano u sljedećem primjeru:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. stvaranje konfiguraciju usmjeravanje

Pogledajte na [ExpressRoute elektronička usmjeravanje konfiguracije (Stvaranje i izmjena elektronička peerings)](expressroute-howto-routing-classic.md) članak detaljne upute.

>[AZURE.IMPORTANT] Ove se upute odnose samo na krugova koje su stvorene pomoću davateljima usluga koje nude usluge za povezivanje 2 sloja. Ako koristite usluga koje nudi upravljanih slojeva 3 services (obično je IP VPN, kao što je MPLS), vaš davatelj usluga za povezivanje će konfigurirati i upravljati njima usmjeravanje umjesto vas.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. povezati virtualne mreže je elektronička ExpressRoute

Nakon toga veza virtualne mreže vaše elektronička ExpressRoute. Pogledajte [Povezivanje ExpressRoute krugova virtualne mrežama](expressroute-howto-linkvnet-classic.md) za detaljne upute. Ako vam je potrebna da biste stvorili virtualne mreže pomoću model klasični implementacije za ExpressRoute, pročitajte članak [Stvaranje virtualne mreže za ExpressRoute](expressroute-howto-vnet-portal-classic.md) upute.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Početak status je elektronička ExpressRoute

Ove informacije u bilo kojem trenutku možete dohvatiti pomoću na `Get-AzureCircuit` cmdlet. Uputite poziv bez parametre navodi sve krugova.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Informacije o određenim elektronička ExpressRoute možete dobiti prosljeđivanjem ključa usluge kao parametar poziv:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Detaljne opise sve parametre možete dobiti pokretanjem sljedeće:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Izmjena je elektronička ExpressRoute

Neka svojstva programa ExpressRoute elektronička možete izmijeniti bez utjecaja povezivanje.

Možete učiniti sljedeće s bez nedostupnost:

- Omogućite ili onemogućite dodatak premium ExpressRoute za svoje sklopovske ExpressRoute.
- Povećanje propusnosti vaše elektronička ExpressRoute. Imajte na umu da prijelaz na stariju propusnosti je elektronička nije podržana.
- Promjena mjerne plana iz podataka s ograničenim prometom neograničeno podataka. Imajte na umu da promjena mjerne tarifu iz neograničeno podataka s ograničenim prometom podataka nije podržana.
- Možete omogućiti i onemogućiti *Dopusti klasični operacije*.

Pogledajte [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md) za dodatne informacije o ograničenjima i ograničenja.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Da biste omogućili dodatak premium ExpressRoute

Možete omogućiti dodatak premium ExpressRoute za svoje postojeće elektronička pomoću cmdleta komponente PowerShell sustava sljedeće:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Vaša elektronička imat će značajke ExpressRoute premium dodatak omogućen. Imajte na umu da ne možemo počet će naplate za mogućnost dodatak premium čim uspješno pokrenuta naredbu.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Da biste onemogućili premium dodatak ExpressRoute

>[AZURE.IMPORTANT] Ovaj postupak možete neće uspjeti ako koristite resursa koje su veće od što je dopušteno za standardne elektronička.

Imajte na umu sljedeće:

- Koje morate provjerite je li broj virtualne mreže povezan s instalacijom manje od 10 prije nego što ste prijeći na nižu od premium Standard. Ako je ovo nemojte učiniti zahtjevom za ažuriranje neće uspjeti te će se naplatiti stope premium.

- Sve virtualne mreže u drugim regijama Geopolitički morate prekinuti vezu. Ako je ovo nemojte učiniti zahtjevom za ažuriranje neće uspjeti te će se naplatiti stope premium.

- Usmjeravanje tablici mora biti manja od 4000 usmjerava za privatne peering. Ako je vaš Veličina tablice usmjeravanje veći od 4000 usmjerava, sesiju BGP će ispustite i neće biti reenabled dok je broj oglašenu prefiksi dolazi ispod 4000.

Dodatak premium ExpressRoute za svoje postojeće elektronička možete onemogućiti pomoću cmdleta komponente PowerShell sustava sljedeće:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Da biste ažurirali elektronička propusnosti ExpressRoute

Pročitajte [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md) za mogućnosti podržani propusnosti za davatelja usluga. Možete odabrati bilo koje veličine koja je veća od veličine vaše postojeće elektronička dok god fizičke priključak (na kojem vaš elektronička je stvorena) omogućuje.

>[AZURE.IMPORTANT] Ne možete smanjiti propusnosti programa ExpressRoute elektronička bez prekidu. Prijelaz na stariju propusnosti ćete morati deprovision elektronička ExpressRoute i reprovision nove ExpressRoute elektronička.

Kada odlučite koju veličinu morate, koristite sljedeću naredbu da biste promijenili veličinu vaše elektronička:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Vaša elektronička će imati je veličine prema gore na strani Microsoft. Obratite se davatelju povezivanje da biste ažurirali konfiguracije kod sebe da odgovara ta promjena. Imajte na umu da ne možemo počet će vam naplata za mogućnost ažurirane propusnosti od ove točke na.

Ako se prikaže sljedeća pogreška kada povećanje propusnosti elektronička, to znači da nema nema dovoljno propusnosti ostaje na fizičke priključak na kojem je stvorena na postojeće elektronička. Morate izbrisati ovaj elektronička i stvoriti nove elektronička veličine vam je potrebna. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning i brisanje je elektronička ExpressRoute

Imajte na umu sljedeće:

- Virtualne mreže s elektronička ExpressRoute za ovaj postupak da morate prekinuti vezu. Provjerite koristite li virtualne mreže koji su povezani s instalacijom Ako ovaj postupak ne uspije.

- Ako je ExpressRoute elektronička servisa davatelja dodjele resursa stanje **Provisioning** ili **Provisioned** morate raditi s vaš davatelj usluga za deprovision elektronička kod sebe. Nastavit ćemo rezervirati resurse i fakturu dok davatelj usluga dovršava deprovisioning na elektronička i obavještava us.

- Ako davatelj usluga sadrži pretplati su uklonjeni resursi za elektronička (stanje dodjele resursa servisa davatelja postavljeno je na **nisu dodijeljeni resursi**) na elektronička moći ćete izbrisati. Time će se zaustaviti naplatom za na elektronička.

Vaša elektronička ExpressRoute možete izbrisati tako da pokretanjem sljedeće naredbe:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Daljnji koraci

Kada stvorite vaše elektronička, provjerite je li da to učinite sljedeće:

- [Stvaranje i izmjena usmjeravanja za vaše elektronička ExpressRoute](expressroute-howto-routing-classic.md)
- [Povezivanje virtualne mreže vaše elektronička ExpressRoute](expressroute-howto-linkvnet-classic.md)
