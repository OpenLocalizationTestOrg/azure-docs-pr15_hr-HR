Upute za taj zadatak koristite VNet na temelju vrijednosti u nastavku. Dodatne postavke i nazive i strukturiranih na ovom popisu. Ne koristimo ovaj popis izravno u bilo kojem od koraka, iako dodat ćemo varijable na temelju vrijednosti na popisu. Popis koji želite koristiti kao referenca, možete kopirati zamjenjuju vrijednosti vlastitu.

Konfiguriranje referentni popis:
    
- Virtualna Network Name = "TestVNet"
- Virtualne mreže adresni prostor = 192.168.0.0/16
- Grupa resursa = "TestRG"
- Naziv Subnet1 = "Sučelju" 
- Subnet1 adresni prostor = "192.168.0.0/16"
- Naziv pristupnika podmreže: "GatewaySubnet" uvijek morate imenovati podmreži pristupnika *GatewaySubnet*.
- Pristupnik podmreže adresni prostor = "192.168.200.0/26"
- Regija = "Istočni sad"
- Naziv pristupnika = "GW"
- Naziv pristupnika IP = "GWIP"
- Konfiguraciju pristupnika IP naziv = "gwipconf"
-  Vrsta = "ExpressRoute" ta je vrsta potrebni za konfiguriranje programa ExpressRoute.
- Javnu IP naziv pristupnika = "gwpip"


## <a name="add-a-gateway"></a>Dodavanje pristupnika

1. Povezivanje s Azure pretplatu. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Deklariranje varijabli za ovu vježbu. U ovom se primjeru koristit će korištenje varijabli u primjeru u nastavku. Ne zaboravite urediti u skladu s vizualnim postavke koje želite koristiti. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Spremanje objekta virtualne mreže kao varijabla.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Dodajte podmreži pristupnika virtualne mreže. Pristupnik podmreže mora imati naziv "GatewaySubnet". Želite stvoriti pristupnik koji je /27 ili veća (/ 26, / 25, itd.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Postavljanje konfiguracije.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Pohranite podmreže pristupnika kao varijabla.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Zahtjev javna IP adresa. IP adresa je zatraženo prije stvaranja pristupnika. Ne možete navesti IP adresu koja želite koristiti; dinamički dodijeliti. Koristit ćete ovu IP adresu u sljedećem odjeljku konfiguracije. Na AllocationMethod mora biti dinamičkih.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Stvorite konfiguraciju za vaš pristupnik. Konfiguraciju pristupnika definira podmreži i javnu IP adresu da biste koristili. U ovom ćete koraku određujete konfiguracije koji će se koristiti prilikom stvaranja pristupnika. Ovaj korak stvorite zapravo objekt pristupnika. Koristite uzorak u nastavku da biste stvorili konfiguraciju pristupnika. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Stvaranje pristupnika. U ovom ćete koraku **-GatewayType** je osobito važno. Morate koristiti vrijednost **ExpressRoute**. Imajte na umu da nakon pokretanja tih cmdleta pristupnika možete poduzeti 20 minuta ili više da biste stvorili.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Provjerite je li pristupnik stvorena

Pomoću naredbe dolje da biste potvrdili da su stvorili pristupnik.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Promjena veličine pristupnika

Postoje broj [Pristupnika SKU-ove](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Koristite sljedeću naredbu da biste promijenili SKU pristupnika u bilo kojem trenutku.

>[AZURE.IMPORTANT] Ta se naredba ne funkcionira za UltraPerformance pristupnik. Da biste promijenili pristupnikom za pristupnik za UltraPerformance, najprije uklonite postojeće ExpressRoute pristupnika, a zatim stvorite novu UltraPerformance pristupnika. Za vraćanje na stariju verziju pristupnikom iz pristupnik za UltraPerformance, uklonite UltraPerformance pristupnika i stvaranje novog pristupnika.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Uklanjanje pristupnika

Koristite naredbu dolje da biste uklonili pristupnika

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
