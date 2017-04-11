### <a name="noconnection"></a>Kako dodati ili ukloniti prefiksi – veza pristupnika

- **Dodavanje** dodatnih adresa prefiksi na lokalni mrežni pristupnik koji ste stvorili, ali koje ne još uvijek nema pristupnika veze, koristite primjeru u nastavku. Ne zaboravite da biste promijenili vrijednosti u vlastiti.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Da biste uklonili** adresu prefiks koji iz lokalne mreže pristupnika koji nema VPN vezu, koristite primjeru u nastavku. Izostavite prefiksi koja vam više nije potrebna. U ovom primjeru smo više nije potrebna prefiks 20.0.0.0/24 (iz prethodnog primjera), pa ćemo ažurirati lokalnoj mreže pristupnika i isključiti tu prefiks.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Kako dodati ili ukloniti prefiksi - postojeće veze pristupnika

Ako ste stvorili vezu za pristupnika, a želite dodati ili ukloniti prefiksa IP adresa koje se nalaze u lokalnoj mreži pristupnikom, morat ćete poduzeti sljedeće korake redoslijedom. To će rezultirati neke nedostupnost za VPN veza. Kada se ažurira svoje prefiksi ćete najprije uklonite vezu, izmijenite prefiksa i stvorite novu vezu. U primjerima koji slijede, svakako promijenite vrijednosti u vlastiti.

>[AZURE.IMPORTANT] Nemojte brisati pristupnika VPN-a. Ako to učinite, morat ćete ponovno proći kroz korake da biste morali stvarati dokumente, kao i konfigurirajte usmjerivač lokalnog s novim postavkama.
 
1. Uklonite vezu.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Izmjena prefiksi adresa za pristupnik za lokalnu mrežu.

    Postavljanje varijable za na LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Izmjena prefiksa.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Stvaranje veze. U ovom primjeru smo konfigurirate vrstu IPsec vezu. Kada ponovno stvoriti vezu, koristite vrstu veze koja je navedena za konfiguraciju. Vrste dodatnu vezu, potražite u članku stranici [cmdlet ljuske PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .

    Postavljanje varijable za na VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Stvaranje veze. Imajte na umu da ovaj primjer koristi varijabla $local koji ste postavili u prethodnom koraku.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
