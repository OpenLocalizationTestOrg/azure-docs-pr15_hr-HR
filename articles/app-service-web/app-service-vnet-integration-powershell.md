<properties
    pageTitle="S aplikacijom virtualne mreže pomoću komponente PowerShell"
    description="Upute za povezivanje i rad s virtualne mreže pomoću komponente PowerShell"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>S aplikacijom virtualne mreže pomoću komponente PowerShell #

## <a name="overview"></a>Pregled ##

U Azure aplikacije servisa Azure virtualne mreže (VNet) možete povezati aplikacije (web, mobile ili API-JA) u svoju pretplatu. Ta značajka zove VNet integracija. Značajka integracije VNet ne treba zamijeniti pomoću značajke aplikacije servisa okruženje, koji omogućuje vam pokretanje instance komponente aplikacije servisa za Azure u virtualne mreže.

Značajka integracije VNet ima korisničko sučelje (UI) na novom portalu koje možete koristiti za integraciju s virtualne mreže koje su uvedene pomoću modela uvođenje klasičnog ili model implementacije Azure Voditelj resursa. Ako želite da biste saznali više o značajki, potražite u članku [Integrate aplikacije s Azure virtualne mreže](web-sites-integrate-with-vnet.md).

Ovaj je članak ne o tome kako koristiti korisničko Sučelje, ali umjesto o tome da biste omogućili integraciju pomoću komponente PowerShell. Jer naredbe za svaki model implementacije razlikuju, u ovom se članku sadrži odjeljak za svaki model implementacije.  

Prije nego što nastavite s ovog članka, bili sigurni da imate:

- Najnoviji Azure PowerShell SDK instaliran. Sustav možete instalirati s platformu Installer Web.
- Aplikacije programa Azure aplikacije servisa za izvodi u standardnom ili Premium SKU-om.

## <a name="classic-virtual-networks"></a>Klasični virtualne mreže ##

U ovom se odjeljku objašnjavaju triju zadataka za virtualne mreže koje koriste model klasični implementacije:

1. Povezivanje aplikacije već postojeća virtualne mreže koji je pristupnika i konfiguriran za povezivanje točke na web.
1. Ažuriranje podataka za integraciju virtualne mreže za aplikacije.
1. Pokrenite aplikaciju prekid virtualne mreže.

### <a name="connect-an-app-to-a-classic-vnet"></a>Povezivanje aplikacije klasični VNet ###

Da biste povezali aplikacije virtualne mreže, slijedite ove tri koraka:

1. Deklariranje web App da ona će biti uključivanja određeni virtualne mreže. Aplikaciju generirat će certifikat koji će biti odobren virtualne mreže za povezivanje točke na web.
1. Prijenos na web-aplikacije certifikat virtualne mreže i dohvaćanje VPN paketa točke web URI.
1. Ažurirajte virtualne mrežne veze web-aplikaciji paketa točke web URI.

Koraci za prvo i treće su potpuno skriptama, ali drugi korak zahtijeva jednokratan, ručno akcija putem portala ili pristup za izvođenje **STAVITI** ili **ZAKRPU** akcija na krajnjoj Voditelj resursa Azure virtualne mreže. Obratite se podršci za Azure da bi to omogućena. Prije nego što počnete, provjerite jeste li povezani s klasični virtualne mreže koji ima mjesta točke povezivanje već omogućeno i distribuiranih pristupnika. Da biste stvorili pristupnik i omogući povezivanje točke na web, morate koristiti portal kao što je opisano pri [stvaranju pristupnik VPN][createvpngateway].

Klasični virtualne mreže mora biti iste pretplate na kao aplikacije servisa za plan koja sadrži aplikacije koje su Integracija sa.

##### <a name="set-up-azure-powershell-sdk"></a>Postavljanje Azure PowerShell SDK #####

Otvorite prozor PowerShell i postaviti račun za Azure i pretplate pomoću:

    Login-AzureRmAccount

Naredba će se otvoriti upit da biste dobili Azure vjerodajnice. Kada se prijavite, koristite neku od sljedećih naredbi odaberite pretplatu u koju želite koristiti. Provjerite je li koristite pretplatu u koju se nalazite virtualne mreže i aplikacije servisa za planiranje.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

ili

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Varijable koji se koristi u ovom članku #####

Da biste pojednostavnili naredbe, ne možemo postavit **$Configuration** PowerShell tjednog prikaza kalendara u konfiguraciji određene.

Postavite tjednog prikaza kalendara na sljedeći način u ljusci PowerShell sa sljedećih parametara:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Mjesto aplikacije mora biti mjesto bez razmake. Na primjer, Zapad sad je westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Na sljedeću stavku je mjesto staviti certifikata. Mora biti snimanje put na lokalnom računalu. Provjerite jeste li uključili .cer na kraju.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Da biste vidjeli što postavite, upišite **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Nastavak ovog odjeljka podrazumijeva tjednog prikaza kalendara stvorena samo opisano e-pošta.

##### <a name="declare-the-virtual-network-to-the-app"></a>Deklariranje virtualne mreže aplikaciju #####

Koristite sljedeću naredbu da biste aplikaciju da ga će koristiti ovu virtualne mreže. Time će aplikaciju da biste generirali potrebne potvrde:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Ako se ta naredba potvrdi, **$vnet** imat **Svojstva** tjednog prikaza kalendara u njoj. Varijabla **Svojstva** smiju sadržavati otisak prsta certifikat i podatke o certifikatu.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Prijenos na web-aplikacije certifikat virtualne mreže #####

Ručno, jednokratni korak nužan za svaku pretplate te kombinacije virtualne mreže. To jest, ako se povezujete aplikacije u pretplati odgovora odgovora virtualne mreže, morat ćete učiniti ovaj korak samo kada bez obzira na to koliko aplikacije konfiguriranje. Ako dodajete nove aplikacije na drugu virtualne mreže, morat ćete ponovno to učiniti. Razlog za to je skup potvrde se generira na razini pretplate u aplikacije servisa za Azure i postavljanje generira se jednom za svaki se virtualne mreže aplikacija će se povezati s.

Potvrda će već postavljene ako ste pratili korake u nastavku ili ako je integriran s istom virtualne mreže pomoću portala za.

U prvi je korak da biste generirali .cer datoteka. Drugi korak je da biste prenijeli .cer datoteka na virtualne mreže. Da biste generirali .cer datoteka iz API poziva u prethodnom koraku, pokrenite sljedeće naredbe.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Certifikat će se pronaći na mjestu **$Configuration.GeneratedCertificatePath** određuje.

Da biste ručno dodali certifikata, koristiti [portal za Azure] [ azureportal] i **Pregled virtualne mreže (klasični)** > **VPN veze** > **točke-na-web-mjesta** > **Upravljanje certifikata**. Na tom mjestu, prenesite certifikata.

##### <a name="get-the-point-to-site-package"></a>Dohvaćanje paketa točke web #####

Sljedeći korak u postavljanju virtualne mrežne veze na web-aplikacijama je da biste dobili pakiranje točke na web i podijelite web-aplikaciju programa.

Sljedeći predložak spremiti datoteku s nazivom GetNetworkPackageUri.json negdje na vašem računalu, na primjer, C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Postavljanje ulaznih parametara:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Poziv skripte:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Varijabla **$output. Outputs.packageUri** sada sadrže paketa URI koji se na web-aplikaciju.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Prijenos paketa mjesta točke u aplikaciju #####

U posljednjem koraku je pružanje aplikaciju s njim. Jednostavno pokrenite sljedeću naredbu:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Ako se poruke traži da potvrdite da su prebrisivanja postojećeg resursa, obavezno ga omogućite i.

Nakon uspješnog ta naredba aplikacije sad trebao biti povezan virtualne mreže. Da biste potvrdili uspjeh, idite na konzoli za aplikacije i upišite sljedeće:

    SET WEBSITE_

Ako postoji varijabla okruženja naziva WEBSITE_VNETNAME koja ima vrijednost koja odgovara nazivu cilj virtualne mreže, sve konfiguracije ste je uspio.

### <a name="update-classic-vnet-integration-information"></a>Ažuriranje podataka za integraciju klasični VNet ###

Da biste ažurirali ili ponovnog sinkroniziranja podatke, samo ponovite korake koji ste pratili prilikom stvaranja integraciju sa servisom lakim. Ti koraci su:

1. Definiranje konfiguracijske informacije.
1. Deklariranje virtualne mreže aplikaciju.
1. Nabavite paket točke na web.
1. Prijenos paketa mjesta točke u aplikaciju.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Prekid aplikacije klasični VNet ###

Da biste isključili aplikaciju, morate informacije o konfiguraciji koji je postavljen tijekom Integracija virtualne mreže. Korištenje tih podataka postoji zatim jednoj naredbi aplikacije prekid virtualne mreže.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Voditelj resursa virtualne mreže ##

Voditelj resursa virtualne mreže imaju Azure API-ji Voditelj resursa, koji Pojednostavnite neki procesa u usporedbi s klasični virtualne mreže. Imamo skriptu koja će vam pomoći u obavljanju sljedeće zadatke:

- Stvaranje resursima virtualne mreže i integracija aplikacije s njim.
- Stvaranje pristupnika za, konfiguriranje točke mjesta povezivanja u već postojeća resursima virtualne mreže i zatim integrirane aplikacije je.
- Pokrenite aplikaciju integrirati s već postojeća resursima virtualne mreže pristupnika i točke-na-web-mjesta omogućena za povezivanje.
- Pokrenite aplikaciju prekid virtualne mreže.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Voditelj resursa VNet aplikacije servisa Integracija skripte ###

Kopirajte sljedeću skriptu i spremiti datoteku. Ako ne želite koristiti skriptu, slobodno Saznajte iz nje da biste saznali kako postaviti stvari s resursima virtualne mreže.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Spremite kopiju skriptu. U ovom članku, on se zove V2VnetAllinOne.ps1, ali možete koristiti neki drugi naziv. Postoje nema argumenata za ovu skriptu. Jednostavno izvođenja. Najprije će učiniti skriptu je od vas zatražiti da se prijavite. Kada se prijavite, skripta može vidjeti detalje o računu i vraća popis pretplata. Ne brojeći zahtjeva za vjerodajnice, izvođenja početne skripte izgleda ovako:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Probne pretplate (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) Testiranje MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Ljubičasto probne pretplate (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Odaberite neku mogućnost: 3

    Račun: ccompy@microsoft.com okruženja: AzureCloud pretplate: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klijentu: 722278f-fef1-499f-91ab-2323d011db47

    Unesite grupu resursa aplikacije: hcdemo ru unesite naziv aplikacije: v2vnetpowershell što želite učiniti?

    1) Dodavanje NOVOG virtualne mreže aplikacije
    2) Dodavanje POSTOJEĆEG virtualne mreže aplikacije
    3) Uklanjanje virtualne mreže iz aplikacije

Ostatak u ovom se odjeljku objašnjava svaki od tih tri mogućnosti.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Stvaranje resursima VNet i integracija s njom ###

Da biste stvorili novi virtualne mreže koja koristi model implementacije Voditelj resursa i integraciju s aplikacijom, odaberite **1) dodavanje NOVOG virtualne mreže u aplikaciju programa**. To će vas za naziv virtualne mreže. U slučaju, kao što je vidljivo u sljedeće postavke se koristi naziv v2pshell.

Skripta daje detalje o virtualne mreže koja je stvorena. Mogu li mogu promijeniti neke od vrijednosti. U izvođenja u ovom primjeru sam stvorio virtualne mreže koja ima sljedeće postavke:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Ako želite promijeniti neke od vrijednosti, upišite **Y** i unesite željene promjene. Kada ste zadovoljni s postavkama virtualne mreže, upišite **N** ili samo pritisnite tipku Enter kada se pojavi upit o promjeni postavki. Iz nje na do završetka skripta se neke od koje it' i's način dok se pokreće da biste stvorili pristupnik virtualne mreže. Taj korak može proći u sat. Postoji bez pokazatelj tijeka tijekom ovoj fazi, ali skripta poslat će vam obavijest kada je stvorena pristupnika.

Kada se dovrši skriptu, će biti označen **Dovršeno**. Sada će imati resursima virtualne mreže koji sadrži naziv i postavke koje ste odabrali. U ovom virtualne mreže i integraciju s aplikacijom.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integriranje aplikacije s već postojeća VNet Voditelj resursa ###

Kada ste Integracija sa već postojeća virtualne mreže ako unesete resursima virtualne mreže koji nema pristupnika ili povezivanje točke na web, skripta postavit će koji. Ako na VNET već ima tih značajki postavljanje, skripta odlazak izravno integraciju sa servisom aplikacije. Da biste započeli postupak, samo odaberite **2) POSTOJEĆE virtualne mreže dodati aplikaciju**.

Ta mogućnost funkcionira samo ako ste već postojeća resursima virtualne mreže koji se nalazi u okviru iste pretplate kao aplikacije. Kada odaberete željenu mogućnost, prikazat će se upit o popis virtualne mreže Voditelj resursa.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Odaberite neku mogućnost: 5

Jednostavno odaberite virtualne mreže koju želite integrirati s. Ako već imate pristupnik koji sadrži točke mjesta povezivanje omogućena, skripta jednostavno integrirati aplikacije s virtualne mreže. Ako nemate pristupnika, morat ćete navesti podmreže pristupnika. Vašoj podmreži pristupnika mora biti u prostoru virtualne mreže adresu, a ne može biti u bilo kojem podmreže. Ako imate virtualne mreže bez pristupnika i pokrenite ovaj korak, što će izgledati ovako:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

U ovom primjeru sam stvorio pristupnika virtualne mreže koja ima sljedeće postavke:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Želite li promijeniti neke od tih postavki, to možete učiniti. U suprotnom, pritisnite Enter, a skriptu će stvoriti pristupnikom i priložite aplikacije virtualne mreže. Vrijeme stvaranja pristupnika i dalje jedan sat kroz, pa provjerite koji Imajte na umu. Po završetku sve skripte će biti označen **Dovršeno**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Prekid veze aplikacije s resursima VNet ###

Aplikacije za prekid veze s mrežom virtualne ne zapisivati pristupnika ili onemogućite točke mjesta povezivanja. Možda, koristite ga za nešto drugo. Ga i ne prekinuti vezu iz drugih aplikacija osim one koje ste naveli. Da biste izvršili tu akciju, odaberite **3) uklonite virtualne mreže iz aplikacije**. Kada to učinite, prikazat će se ovako:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Iako skriptu glasi Izbriši, izbrišite virtualne mreže. Samo se ukida integraciju sa servisom. Kada potvrdite da je to što želite učiniti naredbu vrlo brzo obrađuju i obavijestit će vas **True** kad je to učiniti.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
