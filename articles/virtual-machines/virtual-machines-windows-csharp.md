<properties
    pageTitle="Implementacija Azure resursima pomoću C# | Microsoft Azure"
    description="Saznajte kako koristiti C# i upravljanja resursima Azure da biste stvorili Microsoft Azure resursi."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Implementacija resurse Azure pomoću C# 

U ovom se članku objašnjava da biste stvorili Azure resursima pomoću C#.

Najprije morate obavezno dovršite sljedeće zadatke:

- Instalirajte [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Provjerite je li instalaciju paketa [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ili [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Početak [token za provjeru autentičnosti](../resource-group-authenticate-service-principal.md)

Traje 30 minuta slijedite korake u nastavku.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Korak 1: Stvaranje projekta za Visual Studio i instalirati biblioteke

Paketi NuGet su Najlakši način instaliranja biblioteke koje su vam potrebne za dovršetak ovog praktičnog vodiča. Da biste dobili biblioteke koje su vam potrebne u Visual Studio, učinite sljedeće:

1. Kliknite **datoteka** > **nove** > **projekta**.

2. U **predlošcima** > **Visual C#**, odaberite **Konzole za aplikaciju**, unesite naziv i mjesto projekta, a zatim **u redu**.

3. Desnom tipkom miša kliknite naziv projekta u pregledniku rješenja, a zatim kliknite **Upravljanje NuGet paketa**.

4. Vrste *Servisa Active Directory* u okvir za pretraživanje kliknite **instalaciju** paketa provjera autentičnosti biblioteke imenika Active Directory, a zatim slijedite upute za instalaciju paketa.

5. Pri vrhu stranice odaberite **Uključi predizdanja**. Vrsta *Microsoft.Azure.Management.Compute* u okvir za pretraživanje kliknite **Instaliraj** za .NET biblioteke za izračun, a zatim slijedite upute za instalaciju paketa.

6. Vrsta *Microsoft.Azure.Management.Network* u okvir za pretraživanje kliknite **Instaliraj** za .NET biblioteke mreže, a zatim slijedite upute za instalaciju paketa.

7. Vrsta *Microsoft.Azure.Management.Storage* u okvir za pretraživanje kliknite **Instaliraj** za .NET biblioteke za pohranu, a zatim slijedite upute za instalaciju paketa.

8. U okvir za pretraživanje upišite *Microsoft.Azure.Management.ResourceManager* , kliknite **Instaliraj** za upravljanje biblioteke resursa.

Sada ste spremni za početak rada s bibliotekama da biste stvorili aplikacije.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Korak 2: Stvaranje vjerodajnica koje služe za provjeru autentičnosti zahtjeva

Sada oblikujte informacije o aplikaciji koji ste prethodno stvorili u vjerodajnica koje služe za provjeru autentičnosti zahtjeva za Azure Voditelj resursa.

1. Otvorite datoteku Program.cs projekta koji ste stvorili, a zatim dodajte ih pomoću naredbi na vrh datoteku:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Da biste stvorili tokena koje je potrebno, dodati ta metoda predmete Program:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {Id klijenta} zamijenite identifikator Azure Active Directory aplikacija, a zatim {klijent-tajna} s tipka za pristup aplikacije za AD i {id klijenta} s identifikator klijent za vašu pretplatu. Id klijenta možete pronaći tako da pokrenete Get-AzureRmSubscription. Tipkovni prečac možete pronaći pomoću portala za Azure.

3. Da biste nazvali način na koji ste prethodno dodali, dodati kod metodu glavne u datoteci Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Spremite datoteku Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Korak 3: Registrirati davatelji resursa i stvaranje resursa

### <a name="register-the-providers-and-create-a-resource-group"></a>Registrirajte se za davatelje i stvaranje grupa resursa

Svi resursi moraju se nalaziti u grupu resursa. Prije nego što dodate resursa u grupu, pretplate mora biti registriran s drugim davateljima resursa.

1. Dodati varijable korakom glavne klase Program da biste odredili imena koja želite koristiti za resurse:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Zamijenite sve varijable vrijednosti s nazivima i identifikator koji želite koristiti. Identifikator pretplate možete pronaći tako da pokrenete Get-AzureRmSubscription.

2. Da biste stvorili grupu resursa i Registrirajte se za davatelje, dodajte ovu metodu predmete Program:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Da biste nazvali način na koji ste prethodno dodali, dodati kod glavna načina:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

Za spremanje datoteke virtualne tvrdi disk koji je stvoren za virtualnog računala potreban je [račun za pohranu](../storage/storage-create-storage-account.md) .

1. Da biste stvorili račun za pohranu, dodajte ovu metodu predmete Program:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Da biste nazvali način na koji ste prethodno dodali, dodati kod metodu glavne klase Program:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Stvaranje na javnu IP adresu

Javna IP adresa nije potrebno možete komunicirati s virtualnog računala.

1. Da biste stvorili javnu IP adresu virtualnog računala, dodajte ovu metodu predmete Program:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Da biste nazvali način na koji ste prethodno dodali, dodati kod metodu glavne klase Program:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Stvaranje virtualne mreže

Virtualnog računala koja je stvorena s modelom resursima implementacije mora biti u virtualne mreže.

1. Da biste stvorili podmreži i virtualne mreže, dodajte ovu metodu predmete Program:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Da biste nazvali način na koji ste prethodno dodali, dodati kod metodu glavne klase Program:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Stvaranje mrežnog sučelja

Virtualno računalo mora mrežno sučelje za komunikaciju na virtualne mreže.

1. Da biste stvorili mrežno sučelje, dodajte ovu metodu predmete Program:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Da biste nazvali način na koji ste prethodno dodali, dodati kod metodu glavne klase Program:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Stvaranje skupa dostupnosti

Dostupnost skupove olakšavaju upravljanje održavanja virtualnim strojevima koristi aplikacija.

1. Da biste stvorili postavljanje dostupnosti, dodajte ovu metodu predmete Program:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Da biste nazvali način na koji ste prethodno dodali, dodati kod metodu glavne klase Program:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Stvaranje virtualnog računala

Sad kad ste stvorili podrške resursa, možete stvoriti virtualnog računala.

1. Da biste stvorili virtualnog računala, dodajte ovu metodu predmete Program:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Pomoću ovog praktičnog vodiča stvara virtualnog računala na kojem je instalirana verzija operacijskog sustava Windows Server. Dodatne informacije o odabiru druge slike potražite u članku [Kretanje i slika odaberite Azure virtualnog računala pomoću komponente Windows PowerShell i EŽA Azure](virtual-machines-linux-cli-ps-findimage.md).

2. Da biste nazvali način na koji ste prethodno dodali, dodati kod glavna načina:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Korak 4: Brisanje resursi

Budući da vam se naplatiti za resurse koji se koriste u Azure, uvijek je dobro da biste izbrisali resursa koje više nisu potrebne. Ako želite da biste izbrisali virtualnih računala i pomoćne resursa, sve što trebate učiniti je izbrisati grupu resursa.

1.  Da biste izbrisali grupu resursa, dodajte ovu metodu predmete Program:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Da biste nazvali način na koji ste prethodno dodali, dodati kod glavna načina:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Korak 5: Pokretanje aplikacije konzole

1. Da biste pokrenuli aplikaciju konzole, kliknite **pokretanje** u Visual Studio, a prijavite se u za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

2. Nakon svakog Šifra stanja, vraća se da biste stvorili svakog resursa, pritisnite **Enter** . Nakon stvaranja virtualnog računala prije pritiska na tipku Enter da biste izbrisali sve resurse učiniti na sljedeći korak.

    Traje oko pet minuta za ovu aplikaciju konzole da biste pokrenuli potpuno od početka do kraja. Prije nego što pritisnete tipku Enter da biste pokrenuli brisanje resursa, može potrajati nekoliko minuta da biste provjerili stvaranje resursa na portalu za Azure prije nego ih izbrišete.

3. Da biste vidjeli status resursa, otiđite zapisnike nadzora Azure portalu:

    ![Pregled zapisnika nadzora Azure portalu](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Daljnji koraci

- Iskoristite prednost pomoću predloška za stvaranje virtualnog računala pomoću informacija u [uvođenja programa Azure virtualnog računala pomoću C# i resursima predložak](virtual-machines-windows-csharp-template.md).
- Informirajte se o upravljanju virtualnog računala koju ste stvorili tako da pročitate [Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i PowerShell](virtual-machines-windows-csharp-manage.md).
