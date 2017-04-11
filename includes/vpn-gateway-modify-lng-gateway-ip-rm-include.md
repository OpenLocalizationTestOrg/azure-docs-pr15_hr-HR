Da biste izmijenili IP adresa pristupnika, koristite na `New-AzureRmVirtualNetworkGatewayConnection` cmdlet. Pod uvjetom da zadržite naziv pristupnika lokalne mreže točno onako kao postojeći naziv, postavke će prebrisati. Trenutačno cmdlet "Postavljanje" ne podržava izmjena IP adresa pristupnika.

### <a name="gwipnoconnection"></a>Upute za izmjenu pristupnik IP adresa – veza pristupnika

Da biste ažurirali pristupnik IP adresa za vaše lokalne mreže pristupnik koji još ne sadrži veze, koristite primjeru u nastavku. Istodobno možete ažurirati i prefiksi adresu. Postavke koje navedete prebrisat će postojeće postavke. Ne zaboravite da biste koristili postojeći naziv pristupnika za lokalnu mrežu. Ako ne, koji ćete stvoriti novi pristupnik za lokalnu mrežu ne prebrišete postojeće.

Koristite sljedeći primjer zamjenu vrijednosti za vlastitu.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Upute za izmjenu IP adresa pristupnika - postojeće veze pristupnika

Ako veza pristupnika već postoji, najprije morate da biste vezu uklonili. Zatim izmijenite IP adresa pristupnika i ponovno stvorite novu vezu. To će rezultirati neke nedostupnost za VPN veza.


>[AZURE.IMPORTANT] Nemojte brisati pristupnika VPN-a. Ako to učinite, morat ćete ponovno proći kroz korake da biste morali stvarati dokumente, kao i konfigurirajte usmjerivač lokalnog s IP adresom koji će se dodijeliti novostvorenu pristupnika.
 

1. Uklonite vezu. Možete pronaći naziv veze pomoću na `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Promijenite vrijednost GatewayIpAddress. Vaša adresa prefiksi možete izmijeniti i trenutku, ako je potrebno. Imajte na umu da to prebrisat će postojeće postavke lokalne mreže pristupnika. Koristite postojeći naziv pristupnika za lokalne mreže prilikom promjene tako da će prebrisati postavke. Ako ne, koji ćete stvoriti novi pristupnik za lokalnu mrežu ne izmjena postojeće.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Stvaranje veze. U ovom primjeru smo konfigurirate vrstu IPsec vezu. Kada ponovno stvoriti vezu, koristite vrstu veze koja je navedena za konfiguraciju. Vrste dodatnu vezu, potražite u članku stranici [cmdlet ljuske PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .  Da biste dobili naziv VirtualNetworkGateway, možete pokrenuti na `Get-AzureRmVirtualNetworkGateway` cmdlet.

    Postavljanje varijable:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Stvaranje veze:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

