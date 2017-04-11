<properties
    pageTitle="Azure Voditelj resursa podrške za Upravitelj promet | Microsoft Azure "
    description="Pomoću ljuske PowerShell za upravitelja promet s Azure Voditelj resursa"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure Voditelj resursa podrške za Azure promet Manager

Azure Voditelj resursa je Preferirani upravljanje sučelje za usluge u Azure. Azure profili promet Manager možete se upravlja pomoću utemeljen na Voditelj resursa Azure API-ji i alata.

## <a name="resource-model"></a>Model resursa

Azure Upravitelj promet je konfiguriran pomoću skup postavki profila promet Upravitelj naziva. Ovaj profil sadrži DNS postavke, postavke za usmjeravanje prometa, nadzor postavke krajnjoj točki i popis krajnje točke servisa usmjeravanja promet.

Svaki profil promet Upravitelj predstavlja resursa vrste 'TrafficManagerProfiles'. Na razini REST API-JA URI za svaki profil je kako slijedi:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Usporedba s API klasični Upravitelj promet Azure

Podrška za Azure Voditelj resursa za Upravitelj promet koristi različitu terminologiju od model klasični implementacije. U sljedećoj su tablici prikazane su razlike između Voditelj resursa i klasični uvjete:

| Voditelj resursa termina | Klasični termina |
|-----------------------|--------------|
| Metodu promet usmjeravanja | Način za ujednačavanje opterećenja |
| Način prioritet | Prebacivanje način |
| Ponderirani način | Kružno način |
| Način performansi | Način performansi |

Na temelju povratnih informacija od klijenata, ne možemo promijeniti terminologija možete poboljšati preglednosti i smanjiti uobičajenih misunderstandings. Nema razlike u funkcijama.

## <a name="limitations"></a>Ograničenja

Kada se pozivate krajnje vrste 'AzureEndpoints' za web-aplikacije, krajnje točke promet Manager možete referencirati samo zadani (Proizvodnja) [vremensko razdoblje web-aplikacije](../app-service-web/web-sites-staged-publishing.md). Prilagođeni slobodnih nisu podržani. Kao zaobilazno rješenje, prilagođene slobodnih može konfigurirati pomoću vrsta "ExternalEndpoints".

## <a name="setting-up-azure-powershell"></a>Postavljanje Azure PowerShell

Ove upute koristite Microsoft Azure PowerShell. U sljedećem članku objašnjava kako instalirati i konfigurirati Azure PowerShell.

- [Kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md)

Primjeri u ovom se članku pretpostavlja da imate postojeću grupu resursa. Možete stvoriti grupu resursa pomoću sljedeće naredbe:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure Voditelj resursa zahtijeva svih grupa resursa na mjestu. To mjesto služi kao zadanu za resurse stvorene u toj grupi resursa. Međutim, jer upravitelj promet profila Resursi su globalni ne regionalne, odabir resursa grupi mjesto ima bez utjecaja na Azure promet Manager.

## <a name="create-a-traffic-manager-profile"></a>Stvaranje profila Upravitelja promet

Da biste stvorili promet upravitelj profila, koristite cmdlet novo AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

U sljedećoj su tablici opisani parametri:

| Parametar | Opis |
|-----------|-------------|
| Ime | Naziv resursa promet upravitelj resursa profila. Profili u istoj grupi resursa moraju imati jedinstvene nazive. Ovaj naziv razlikuje se od DNS naziv koji se koristi za upite DNS-a.|
| ResourceGroupName | Naziv grupe resursa koja sadrži resurs profila.|
| TrafficRoutingMethod | Određuje način promet usmjeravanje upotrijebljena za određivanje koji krajnje točke, vraća se u odgovoru DNS upit. Moguće vrijednosti su 'Performanse', 'Weighted' ili "Prioritet".|
| RelativeDnsName | Određuje naziv glavnog računala dio naziva DNS-a nudi ovaj profil Upravitelj promet. Tu vrijednost u kombinaciji s nazivom domene DNS-a za Azure promet Upravitelj obrasca na potpuno kvalificirani naziv domene (FQDN) profila. Na primjer, postavljanje vrijednosti "contoso" postaje "contoso.trafficmanager.net."|
| TTL | Određuje na DNS vrijeme važenja (TTL), u sekundama. U ovom TTL obavještava lokalni DNS resolvers i klijenti DNS koliko odgovora DNS predmemoriju za ovaj profil Upravitelj promet.|
| MonitorProtocol | Određuje protokol za praćenje stanja krajnjoj točki. Moguće vrijednosti su 'HTTP' i 'HTTPS'.|
| MonitorPort | Određuje TCP priključak koji se koristi za praćenje stanja krajnjoj točki.|
| MonitorPath | Navodi put odnosu naziv domene krajnja točka za isprobati za stanje krajnjoj točki.|

Cmdlet stvara profil Upravitelj promet u Azure i vraća odgovarajući objekt profila PowerShell. Sada profil sadržavati bilo koju krajnje točke. Dodatne informacije o dodavanju krajnje točke promet upravitelj profila potražite u članku [Dodavanje krajnje točke Upravitelj promet](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Početak promet upravitelj profila

Da biste dohvatili postojećeg objekta promet upravitelj profila, koristite cmdlet Get-AzureRmTrafficManagerProfle:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Ovaj cmdlet vraća objekt promet upravitelj profila.

## <a name="update-a-traffic-manager-profile"></a>Ažuriranje profila Upravitelja promet

Izmjena profila promet Upravitelj slijedi 3 koraka:

1. Dohvaćanje profil koristeći Get-AzureRmTrafficManagerProfile ili koristiti profil koji je vratio novo AzureRmTrafficManagerProfile.
2. Izmjena profil. Dodavanje i uklanjanje krajnje točke ili promjena parametara krajnja točka ili profila. Te promjene su izvanmrežno operacije. Samo želite promijeniti lokalni objekt u memoriji koja predstavlja profil.
3. Zapiši promjene pomoću cmdleta skup AzureRmTrafficManagerProfile.

Sva svojstva profila mogu se promijeniti osim RelativeDnsName profila. Da biste promijenili u RelativeDnsName, morate izbrisati profil i novi profil s novim nazivom.

Sljedeći primjer pokazuje kako promijeniti TTL profila:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Postoje tri vrste krajnje točke Upravitelj promet:

1. **Azure krajnje točke** su usluga koje se nalaze u Azure
2. **Vanjski krajnje točke** su usluga koje se nalaze izvan Azure
3. **Ugniježđene krajnje točke** koriste se za sastavljanje ugniježđene hijerarhije promet upravitelj profila. Ugniježđene krajnje točke omogućiti naprednih promet usmjeravanje konfiguracija za složene aplikacije.

U svakom slučaju tri krajnje točke možete dodati na dva načina:

1. Korištenje u 3 koraka prethodno opisano. Prednosti ove metode je da nekoliko krajnjoj točki promjene može unijeti u jednom ažuriranja.
2. Pomoću cmdleta New-AzureRmTrafficManagerEndpoint. Ovaj cmdlet dodaje krajnje točke u postojeći profil promet upravitelja u jednoj operaciji.

## <a name="adding-azure-endpoints"></a>Dodavanje Azure krajnje točke

Azure krajnje točke reference usluga koje se nalaze u Azure. Podržani su tri vrste Azure krajnje točke:

1. Azure web-aplikacije
2. "Klasični cloud servisima (koji se može sadržavati PaaS servisa ili IaaS virtualnim strojevima)
3. Azure PublicIpAddress resursa (koji se može priložiti raspoređivača opterećenja ili virtualnog računala NIC). Na PublicIpAddress mora imati naziv DNS dodijeljeni koja će se koristiti u upravitelju promet.

U svakom slučaju:

- Servis navedena je parametar 'targetResourceId' Dodaj AzureRmTrafficManagerEndpointConfig ili novo AzureRmTrafficManagerEndpoint.
- 'Cilj' i 'EndpointLocation' IMPLICITNIH je tako da na TargetResourceId.
- Odredite 'težinu' nije obavezno. Težina koriste se samo ako profil je konfiguriran za korištenje metodu promet usmjeravanje "Weighted". U suprotnom se zanemaruju. Ako navedeni, vrijednost mora biti broj između 1 i 1000. Zadana vrijednost je "1".
- Odredite 'prioritet' nije obavezno. Prioriteti se koristiti samo ako profil je konfiguriran za korištenje metodu promet usmjeravanje "Prioritet". U suprotnom se zanemaruju. Valjani su vrijednosti od 1 do 1000 s niže vrijednosti u kojoj stoji da se viši prioritet. Ako je naveden za jednu krajnju točku, mora se navesti za sve krajnje točke. Ako se ispusti, zadanih vrijednosti počevši od "1" primjenjuju se redoslijedom navedene su krajnjih točaka.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Primjer 1: Dodavanje krajnje točke web-aplikacije pomoću Dodaj AzureRmTrafficManagerEndpointConfig

U ovom primjeru smo stvaranje profila Upravitelja promet i dodajte dva krajnje točke web-aplikacije pomoću cmdleta Dodaj AzureRmTrafficManagerEndpointConfig.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Primjer 2: Dodavanje krajnja točka servisa "klasični" oblaka za korištenje novo AzureRmTrafficManagerEndpoint

U ovom primjeru 'klasični krajnje točke u Oblaku dodaje promet upravitelj profila. U ovom primjeru smo navedeni profila pomoću naziva grupa profil i resursa, umjesto prosljeđivanje profila objekta. Podržane su oba pristupa.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Primjer 3: Dodavanje krajnje točke publicIpAddress pomoću novo AzureRmTrafficManagerEndpoint

U ovom primjeru javno resursa IP adresa dodaje se promet upravitelj profila. Na javnu IP adresu mora imati naziv DNS konfiguriran i možete vezana NIC na VM ili raspoređivača opterećenja.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Dodavanje vanjskog krajnje točke

Upravitelj promet koristi vanjski krajnje točke da biste usmjerili promet usluga koje se nalaze izvan Azure. Kao i kod Azure krajnje točke vanjskih krajnje točke možete dodati ili pomoću Dodaj AzureRmTrafficManagerEndpointConfig slijedi skup AzureRmTrafficManagerProfile ili novo AzureRMTrafficManagerEndpoint.

Prilikom određivanja vanjskih krajnje točke:

- Naziv domene krajnjoj točki mora biti definirano pomoću parametar 'Cilj'
- Ako se koristi način za promet usmjeravanje 'Performanse', 'EndpointLocation' je obavezan. U suprotnom nije obavezno. Ta vrijednost mora biti [naziv valjani Azure područja](https://azure.microsoft.com/regions/).
- 'Težina' i 'Prioritet' nisu obavezni.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Primjer 1: Dodavanje vanjskog krajnje točke pomoću Dodaj AzureRmTrafficManagerEndpointConfig i postavljanje AzureRmTrafficManagerProfile

U ovom primjeru smo stvaranje profila Upravitelja promet, dodajte dva vanjskih krajnje točke, a Zapiši promjene.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Primjer 2: Dodavanje vanjskog krajnje točke pomoću novo AzureRmTrafficManagerEndpoint

U ovom primjeru dodat ćemo krajnje vanjskog u postojeći profil. Profil navedeni su nazivi grupa profil i resursa.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>Dodavanje krajnje točke "Ugniježđene"

Svaki profil promet Upravitelj određuje jedan način promet usmjeravanja. No postoje scenariji koje je potrebno više sofisticirane promet usmjeravanje od postupka nudi jedan profil Upravitelj promet. Možete ugnijezditi profili Upravitelj promet za kombiniranje prednosti više postupaka promet usmjeravanja. Ugniježđene profili jednostavan način možete nadjačati zadano ponašanje Upravitelj promet za podršku za veće i složenije implementacijama aplikacije. Više podrobne primjere potražite u članku [ugniježđene funkcije Upravitelj promet profila](traffic-manager-nested-profiles.md).

Ugniježđene krajnje točke konfiguraciji nadređenog profila, korištenje određene krajnje vrste 'NestedEndpoints'. Prilikom određivanja ugniježđene krajnje točke:

- Krajnja točka mora biti naveden pomoću parametar "targetResourceId"
- Ako se koristi način za promet usmjeravanje 'Performanse', 'EndpointLocation' je obavezan. U suprotnom nije obavezno. Ta vrijednost mora biti [naziv valjani Azure područja](http://azure.microsoft.com/regions/).
- 'Težina' i 'Prioritet' nije obavezno, kao Azure krajnje točke.
- Parametar 'MinChildEndpoints' nije obavezno. Zadana vrijednost je "1". Ako broj dostupnih krajnje točke pada ispod prag, profila nadređenog smatra profila podređene 'smanjena i diverts promet na druge krajnje točke u profilu nadređenog.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Primjer 1: Dodavanje pomoću Dodaj AzureRmTrafficManagerEndpointConfig i postavljanje AzureRmTrafficManagerProfile ugniježđene krajnje točke

U ovom se primjeru ćemo stvoriti novi podređeni Upravitelj promet i nadređenog profile, dijete kao ugniježđene krajnjoj točki dodati nadređenog i Zapiši promjene.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Za ispušteno da bude kraće u ovom primjeru smo jeste li dodati druge krajnje točke profili dijete ili nadređenu.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Primjer 2: Dodavanje pomoću novo AzureRmTrafficManagerEndpoint ugniježđene krajnje točke

U ovom primjeru dodat ćemo postojeći profil podređeni kao ugniježđene krajnje točke u postojeći profil nadređenog. Profil navedeni su nazivi grupa profil i resursa.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Ažuriranje krajnjoj točki Upravitelj promet

Da biste ažurirali krajnje postojeće Upravitelj promet na dva načina:

1. Početak profil promet Upravitelj koristeći Get-AzureRmTrafficManagerProfile, ažuriranje svojstava krajnjoj točki unutar profil i Zapiši promjene pomoću skupa AzureRmTrafficManagerProfile. Ta metoda ima prednost može ažurirati više krajnje točke u jednoj operaciji.
2. Početak krajnju točku Upravitelj promet pomoću Get-AzureRmTrafficManagerEndpoint, ažuriranje svojstava krajnjoj točki i Zapiši promjene pomoću skupa AzureRmTrafficManagerEndpoint. Ta je metoda jednostavniji, jer ne zahtijeva indeksiranje u polju krajnje točke u profilu.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Primjer 1: Ažuriranje krajnje točke pomoću Get-AzureRmTrafficManagerProfile i postavljanje AzureRmTrafficManagerProfile

U ovom primjeru, ne možemo izmijeniti prioritet na dva krajnje točke u postojeći profil.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Primjer 2: Ažuriranje krajnje pomoću Get-AzureRmTrafficManagerEndpoint i postavljanje AzureRmTrafficManagerEndpoint

U ovom primjeru smo izmijeniti debljine jedan krajnje točke u postojeći profil.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Omogućivanje i onemogućivanje krajnje točke i profile

Upravitelj promet omogućuje pojedinačne krajnje točke da biste se omogućuje i onemogućuje, kao i dopuštanje Omogućivanje i onemogućivanje cijelu profila.
Ove promjene može unijeti postavljanjem početak/ažuriranje/resursi krajnja točka ili profila. Kako biste pojednostavili te uobičajene operacije, oni podržani su i putem namjenski cmdleta.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Primjer 1: Omogućivanje i onemogućivanje promet upravitelj profila

Da biste omogućili promet upravitelj profila, koristite Omogući AzureRmTrafficManagerProfile. Profil možete navesti pomoću profila objekta. Objekt profil možete proslijediti putem kanal, ili pomoću na "-TrafficManagerProfile' parametara. U ovom primjeru smo navedite profil prema nazivu grupe profil i resursa.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Da biste onemogućili promet upravitelj profila:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Cmdlet Onemogući AzureRmTrafficManagerProfile Zatraži potvrdu. Ovaj upit se mogu prikazati pomoću na ' – prisilno ' parametara.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Primjer 2: Omogućivanje i onemogućivanje krajnjoj točki Upravitelj promet

Da biste omogućili krajnjoj točki Upravitelj promet, koristite Omogući AzureRmTrafficManagerEndpoint. Dva su načina za određivanje točki

1. Korištenje TrafficManagerEndpoint objekt poslao kanal ili korištenje u "-TrafficManagerEndpoint' parametara
2. Koristi naziv krajnje točke, krajnjoj točki vrsta, naziv profila i naziv grupe resursa:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Isto tako, da biste onemogućili krajnjoj točki Upravitelj promet:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Kao i u slučaju onemogući AzureRmTrafficManagerProfile cmdlet Onemogući AzureRmTrafficManagerEndpoint Zatraži potvrdu. Ovaj upit se mogu prikazati pomoću na ' – prisilno ' parametara.

## <a name="delete-a-traffic-manager-endpoint"></a>Brisanje krajnje točke Upravitelj promet

Da biste uklonili pojedinačne krajnje točke, koristite cmdlet Ukloni AzureRmTrafficManagerEndpoint:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Ovaj cmdlet Zatraži potvrdu. Ovaj upit se mogu prikazati pomoću na ' – prisilno ' parametara.

## <a name="delete-a-traffic-manager-profile"></a>Brisanje promet upravitelj profila

Da biste izbrisali promet upravitelj profila, koristite cmdlet Ukloni AzureRmTrafficManagerProfile navodeći nazive grupa profil i resursa:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Ovaj cmdlet Zatraži potvrdu. Ovaj upit se mogu prikazati pomoću na ' – prisilno ' parametara.

Profil će se izbrisati mogu se odrediti i pomoću profila objekta:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Piped možete se i u ovom nizu:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Daljnji koraci

[Nadzor upravitelja promet](traffic-manager-monitoring.md)

[Razmatranja promet Upravitelj performansi](traffic-manager-performance-considerations.md)
