### <a name="to-verify-your-connection-by-using-powershell"></a>Da biste provjerili veza pomoću komponente PowerShell

Možete provjeriti je li uspjelo vezu pomoću na `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet, sa ili bez `-Debug`. 

1. Koristite sljedeći primjer cmdlet konfiguriranje vrijednosti koje odgovaraju vlastitu. Ako se to od vas zatraži, odaberite "A" da biste pokrenuli "Sve". U ovom primjeru `-Name` se odnosi na naziv veze koje ste stvorili i želite testirati.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Nakon cmdlet završi, prikaz vrijednosti. U primjeru u nastavku prikazuje stanje veze kao "Povezan", pa možete vidjeti ingress i izlazne bajtova.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Da biste provjerili vezu pomoću portala za Azure

Na portalu Azure možete prikazati stanje veze tako da odete vezu. Da biste to učinili na više načina. Na sljedeći način prikažite jedan od načina otvorite vezu i provjerite.

1. [Portal za Azure](http://portal.azure.com)kliknite **Resursi za sve** , a zatim otvorite pristupnikom virtualne mreže.
2. Na plohu za pristupnik za virtualne mreže, kliknite **veze**. Možete vidjeti status svake veze.
3. Kliknite naziv vezu koju želite provjeriti da biste otvorili **Essentials**. Osnove, možete prikazati dodatne informacije o vezu. **Status** je 'Je uspjelo' i "Povezano" kada ste uspješno veze.

    ![Provjerite vezu](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)