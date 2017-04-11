## <a name="virtual-network"></a>Virtualne mreže
Virtualna mreža (VNET) i podmreže resurse olakšali definiranje granicu sigurnosti za radnih opterećenja izvodi u Azure. U VNet je određene argumentima zbirka razmake adresu definiran kao CIDR blokova. 

>[AZURE.NOTE] Mrežni administratori upoznati s CIDR notacije. Ako niste upoznati s CIDR, [Saznajte više o tome](http://whatismyipaddress.com/cidr).

![VNet s više podmreže](./media/resource-groups-networking/Figure4.png)

VNets sadrže sljedeća svojstva.

|Svojstvo|Opis|Ogledne vrijednosti|
|---|---|---|
|**addressSpace**|Skup prefiksi adresa koji čine VNet notacijom CIDR|192.168.0.0/16|
|**podmreže**|Skup podmreže koji čine na VNet|u odjeljku [podmreže](#Subnets) ispod.|
|**IPAdresa**|IP adresa dodijeliti objektu. To je svojstvo samo za čitanje.|104.42.233.77|

### <a name="subnets"></a>Podmreže
Podmreži je resurs dijete od na VNet, a omogućuje definiranje segmenata adresu razmake unutar bloka CIDR, pomoću prefiksi IP adresa. NIC-ovi možete dodati podmreže i povezani VMs, koja omogućuje povezivanje za različite radnih opterećenja.

Podmreže sadrže sljedeća svojstva. 

|Svojstvo|Opis|Ogledne vrijednosti|
|---|---|---|
|**addressPrefix**|Jedna adresa prefiks koji čine podmreže notacijom CIDR|192.168.1.0/24|
|**networkSecurityGroup**|NSG primjenjuje se na podmreži|u odjeljku [NSGs](#Network-Security-Group)|
|**routeTable**|Usmjeravanje tablicu primjenjuje se na podmreži|u odjeljku [UDR](#Route-table)|
|**ipConfigurations**|Zbirka IP configruation objekata koristi NIC-ovi s podmreži|u odjeljku [UDR](#Route-table)|


Ogledna VNet u JSON OSNOVNI oblik:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Dodatni resursi

- Saznajte više o [VNet](../articles/virtual-network/virtual-networks-overview.md).
- Pročitajte [dokumentaciju referenca REST API -JA](https://msdn.microsoft.com/library/azure/mt163650.aspx) za VNets.
- Pročitajte [dokumentaciju referenca REST API -JA](https://msdn.microsoft.com/library/azure/mt163618.aspx) za podmreže.