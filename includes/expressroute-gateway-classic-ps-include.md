Morate stvoriti na VNet i podmreži pristupnika najprije prije početka rada na sljedeće zadatke. Potražite u članku [Konfiguracija virtualne mreže pomoću portala za klasični](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) dodatne informacije.   

## <a name="add-a-gateway"></a>Dodavanje pristupnika

Pomoću naredbe dolje da biste stvorili pristupnik. Ne zaboravite zamjenske vrijednosti za vlastitu.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Provjerite je li pristupnik stvorena

Pomoću naredbe dolje da biste potvrdili da su stvorili pristupnik. Ta se naredba dohvaća i ID pristupnika koje je potrebno za ostalih operacija.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Promjena veličine pristupnika

Postoje broj [Pristupnika SKU-ove](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Koristite sljedeću naredbu da biste promijenili Gateway SKU u bilo kojem trenutku.

>[AZURE.IMPORTANT] Ta se naredba ne funkcionira za UltraPerformance pristupnik. Da biste promijenili pristupnikom za pristupnik za UltraPerformance, uklonite postojeće ExpressRoute pristupnika, a zatim stvorite novu UltraPerformance pristupnika. Za vraćanje na stariju verziju sustava gateway pristupnik za UltraPerformance, uklonite UltraPerformance pristupnika, a zatim stvorite novi pristupnik. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Uklanjanje pristupnika

Koristite naredbu dolje da biste uklonili pristupnika

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>