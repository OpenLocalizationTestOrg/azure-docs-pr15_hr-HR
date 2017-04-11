## <a name="how-to-create-a-vnet-using-powershell"></a>Kako stvoriti VNet pomoću komponente PowerShell
Da biste stvorili na VNet pomoću komponente PowerShell, slijedite korake u nastavku.

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../articles/powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.
    
2. Ako je potrebno, stvorite novu grupu resursa, kao što je prikazano u nastavku. Za naše scenarij Stvorite grupu resursa pod nazivom *TestRG*. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](../articles/resource-group-overview.md).

        New-AzureRmResourceGroup -Name TestRG -Location centralus

    Očekivani izlaz:
    
        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG   

3. Stvorite novi VNet pod nazivom *TestVNet*, kao što je prikazano u nastavku.

        New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
            -AddressPrefix 192.168.0.0/16 -Location centralus   
        
    Očekivani izlaz:

        Name                : TestVNet
        ResourceGroupName   : TestRG
        Location            : centralus
        Id                  : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState       : Succeeded
        Tags                    : 
        AddressSpace            : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                 }
        DhcpOptions             : {}
        Subnets                 : []
        VirtualNetworkPeerings  : []

4. Spremanje objekta virtualne mreže u varijablu, kao što je prikazano u nastavku.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    
    >[AZURE.TIP] Možete kombinirati korake 3 i 4 tako da pokrenete **$vnet = novo AzureRmVirtualNetwork - ResourceGroupName TestRG-192.168.0.0/16 TestVNet ime - AddressPrefix-centralus mjesto**.

5. Dodajte podmreži nova varijabla VNet, kao što je prikazano u nastavku.

        Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
        
    Očekivani izlaz:

        Name                : TestVNet
        ResourceGroupName   : TestRG
        Location            : centralus
        Id                  : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState   : Succeeded
        Tags                :
        AddressSpace        : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions         : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings  : []

6. Ponovite koraku 5 za svaku podmreži koju želite stvoriti. Naredba ispod stvara podmreži *pozadinskog* za naše scenarij.

        Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24

7. Iako stvoriti podmreže, oni trenutno postoje samo u lokalnu varijablu koja se koristi za dohvaćanje VNet koje stvorite u koraku 4. Da biste spremili promjene Azure, pokrenite cmdlet **Skup AzureRmVirtualNetwork** , kao što je prikazano u nastavku.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet 
        
    Očekivani izlaz:

        Name                : TestVNet
        ResourceGroupName   : TestRG
        Location            : centralus
        Id                  : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState   : Succeeded
        Tags                :
        AddressSpace        : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions         : {
                                  "DnsServers": []
                                }
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []
