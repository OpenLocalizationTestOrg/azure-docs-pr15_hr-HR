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