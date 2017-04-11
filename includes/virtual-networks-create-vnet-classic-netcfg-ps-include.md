## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Kako stvoriti VNet pomoću mreže konfiguracijskoj datoteci iz komponente PowerShell

Azure koristi xml datoteku da biste definirali VNets sve dostupne na pretplatu. Možete preuzeti ove datoteke i uređivati možete mijenjati ili brisati postojeće VNets i stvarati nove. U ovom dokumentu će upute za preuzimanje datoteke, se nazivaju mrežne datoteke koje konfiguracije (ili netcgf) i uređivanje da biste stvorili novi VNet. Provjerite [sheme konfiguracije Azure virtualne mreže](https://msdn.microsoft.com/library/azure/jj157100.aspx) da biste saznali više o datoteci konfiguracije mreže.

Da biste stvorili VNet pomoću datoteke netcfg pomoću komponente PowerShell, slijedite korake u nastavku.

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../articles/powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.
2. S konzole Azure PowerShell pomoću cmdleta **Get-AzureVnetConfig** da biste preuzeli konfiguracijska datoteka mrežu tako da pokrenete naredbu ispod. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Očekivani izlaz:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Otvorite datoteku koju ste spremili u koraku 2 pomoću bilo koju aplikaciju uređivač XML-a ili tekst, a zatim potražite na **<VirtualNetworkSites>** element. Ako imate neki mreža već stvorili, svaku prikazat će se kao vlastitu **<VirtualNetworkSite>** element.
4. Da biste stvorili virtualne mreže opisani u ovom scenariju, dodajte sljedeće XML samo u odjeljku s **<VirtualNetworkSites>** elementa:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Spremite datoteku konfiguracije mreže.
10. S konzole Azure PowerShell pomoću cmdleta **Skup AzureVnetConfig** prenijeti datoteku konfiguracije mrežu tako da pokrenete naredbu u nastavku. Obratite pozornost na Izlaz u odjeljku naredbe, trebali biste vidjeti **je uspio** u odjeljku **OperationStatus**. Ako je to jest nije slučaj, provjerite xml datoteke pogrešaka.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Evo očekivanog izlaza iznad naredbe:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Provjerite je li novu mrežu dodan tako da pokrenete naredbu ispod pomoću cmdleta **Get-AzureVnetSite** konzoli Azure PowerShell. 

        Get-AzureVNetSite -VNetName TestVNet

    Evo očekivanog izlaza iznad naredbe:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded