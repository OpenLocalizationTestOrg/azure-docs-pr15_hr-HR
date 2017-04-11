## <a name="deploy-the-arm-template-by-using-powershell"></a>Uvođenje predloška ARM pomoću komponente PowerShell

Da biste implementirali ARM predložak koji ste preuzeli pomoću komponente PowerShell, slijedite korake u nastavku.

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../articles/powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.

3. Ako je potrebno, pokrenite na **`New-AzureRmResourceGroup`** cmdlet da biste stvorili novu grupu resursa. Naredba ispod stvara grupu resursa pod nazivom *TestRG* u regiji *Središnje sad* azure. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](../articles/resource-group-overview.md).

        New-AzureRmResourceGroup -Name TestRG -Location centralus
        
    Evo očekivanog izlaza iznad naredbe:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

4. Pokretanje u **`New-AzureRmResourceGroupDeployment`** cmdlet za implementaciju novi VNet pomoću predloška i parametar datoteke možete preuzeti i izmijeniti iznad.

        New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
            -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
            
    Evo očekivanog izlaza iznad naredbe:
        
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : 8/14/2015 9:40:00 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
        
        Outputs           :

5. Pokretanje u **`Get-AzureRmVirtualNetwork`** cmdlet da biste pogledali svojstva novi VNet, kao što je prikazano u nastavku.


        Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
        
    Evo očekivanog izlaza iznad naredbe:
        
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]