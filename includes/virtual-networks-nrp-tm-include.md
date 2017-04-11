## <a name="traffic-manager-profile"></a>Upravitelj promet profila

Upravitelj promet i njegov resurs krajnju točku podređene omogućiti DNS usmjeravanja za krajnje točke u Azure i izvan nje Azure. Takve raspodjele promet uređena usmjeravanje metode pravila. Upravitelj promet omogućuje i stanje krajnju točku da biste moguće nadzirati, a promet diverted komponenta ovisno o stanju krajnje točke. 

| Svojstvo | Opis |
|---|---|
|**trafficRoutingMethod**| moguće vrijednosti su *performanse*, *Weighted*i *Prioritet* | 
| **dnsConfig** | FQDN profila | 
| **Protokol** | *HTTP* i *HTTPS* protokola za nadzor, su za moguće vrijednosti|
| **Priključak** | nadzor priključak |  
| **Put** | nadzor put |
| **Krajnje točke** |  spremnik za krajnje točke resurse | 

### <a name="endpoint"></a>Krajnja točka 

Krajnje je resurs podređeni promet upravitelj profila. Predstavlja servisa ili krajnjoj točki web koji korisniku se raspodijeljene promet na temelju konfigurirani pravila profila Upravitelja promet resurse. 

| Svojstvo | Opis | 
|---|---| 
| **Vrsta** |  Vrsta krajnje točke, moguće vrijednosti su *pokažite Azure završetka*, *Vanjski krajnjoj točki*i *Ugniježđene funkcije krajnje točke* | 
| **targetResourceId** |  Javna IP adresa servisa ili web-krajnje točke. To može biti Azure ili vanjski krajnjoj točki. | 
| **Težina** | Krajnja točka težina koristi u odjeljku Upravljanje promet. | 
| **Prioritet** | Prioritet krajnju točku koristi za definiranje prebacivanje akcija |

Primjer upravitelja promet u JSON osnovni oblik: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Dodatni resursi

Dodatne informacije pročitajte [dokumentaciju REST API -JA za Upravitelj promet](https://msdn.microsoft.com/library/azure/mt163664.aspx) .
