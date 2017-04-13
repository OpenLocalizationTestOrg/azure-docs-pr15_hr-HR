<properties
    pageTitle="Upravljanje VMs pomoću upravitelja resursa Azure i C# | Microsoft Azure"
    description="Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i C#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Upravljanje virtualnim računalima sustava Azure pomoću upravitelja resursa Azure i C#  

Zadaci u ovom se članku objašnjava upravljanje virtualnim računalima sustava, kao što je pokretanje, zaustavljanje i ažuriranja. Virtualnog računala moraju se nalaziti u grupu resursa za dovršenje zadatka u ovom članku.

Dovršavanje zadataka u ovom članku, morate:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Token za provjeru autentičnosti](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Stvaranje projekta za Visual Studio i instalirajte paketa

NuGet paketa su najjednostavniji načina da biste instalirali biblioteke koje su vam potrebne da biste dovršili zadataka u ovom članku. Biblioteke koje instalirate za ovaj članak su Azure provjera autentičnosti biblioteke za Active Directory i biblioteka za izračunavanje resursa davatelja usluga. Poduzmite sljedeće korake da biste dobili biblioteke u Visual Studio:

1. Kliknite **datoteka** > **nove** > **projekta**.

2. U **predlošcima** > **Visual C#**, odaberite **Konzole za aplikaciju**, unesite naziv i mjesto projekta, a zatim **u redu**.

3. Desnom tipkom miša kliknite naziv projekta u pregledniku rješenja, a zatim kliknite **Upravljanje NuGet paketa**.

4. Vrste *Servisa Active Directory* u okvir za pretraživanje kliknite **instalaciju** paketa provjera autentičnosti biblioteke imenika Active Directory, a zatim slijedite upute za instalaciju paketa.

5. Pri vrhu stranice odaberite **Uključi predizdanja**. Vrsta *Microsoft.Azure.Management.Compute* u okvir za pretraživanje kliknite **Instaliraj** za .NET biblioteke za izračun, a zatim slijedite upute za instalaciju paketa.

Sada ste spremni za početak rada s bibliotekama da biste upravljali virtualnim računalima.

## <a name="set-up-the-project"></a>Postavljanje projekta

Sad kad je stvorena aplikacija i instaliraju biblioteke, stvorite token pomoću informacije o aplikaciji. Ovaj token se koristi za zahtjeve za Azure Voditelj resursa za provjeru autentičnosti.

1. Otvorite datoteku Program.cs projekta koji ste stvorili, a zatim dodajte ih pomoću naredbi na vrh datoteku:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Dodati varijable korakom glavne klase Program da biste odredili naziv grupe resursa i naziv virtualnog računala i identifikatora svoje pretplate:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Identifikator pretplate možete pronaći tako da pokrenete Get-AzureRmSubscription.
    
3. Da biste dobili tokena koje je potrebno za stvaranje vjerodajnice, dodajte ovu metodu predmete Program:

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
    
4. Da biste stvorili vjerodajnice, dodavanje koda za ovu metodu glavne u Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Spremite datoteku Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Prikaz informacija o virtualnog računala

1. Dodajte ovu metodu Program za predmete u programu project koju ste prethodno stvorili:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Spremite datoteku Program.cs.

4. Kliknite **pokretanje** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

    Kada pokrenete ovu metodu, trebali biste vidjeti nešto kao u ovom primjeru:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Zaustavljanje virtualnog računala

Možete isključiti virtualnog računala na dva načina. Možete zaustaviti virtualnog računala i zadržavanje njegove postavke, ali i dalje se naplatiti ga ili možete i zaustaviti virtualnog računala te je deallocate. Kada je deallocated virtualnog računala, svi resursi povezani su i deallocated i naplatu završava za njega.

1. Komentar izvan kod koji ste prethodno dodali na način glavne osim kod radi dobivanja vjerodajnica.

2. Ta metoda dodati predmete Program:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Ako želite deallocate virtualnog računala, promijenite poziv ugasi kod:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Spremite datoteku Program.cs.

5. Kliknite **pokretanje** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

    Prikazat će se status promijeni virtualnog računala da biste zaustavljanja. Ako ste pokrenuli način pozivanja Deallocate, status je zaustavljen (deallocated).

## <a name="start-a-virtual-machine"></a>Pokretanje virtualnog računala

1. Komentar izvan kod koji ste prethodno dodali na način glavne osim kod radi dobivanja vjerodajnica.

2. Dodajte ovu metodu predmete Program:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Spremite datoteku Program.cs.

5. Kliknite **pokretanje** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

    Trebali biste vidjeti status virtualnog računala promijeniti pokrenut.

## <a name="restart-a-running-virtual-machine"></a>Ponovno pokrenite izvodi virtualnog računala

1. Komentar izvan kod koji ste prethodno dodali na način glavne osim kod radi dobivanja vjerodajnica.

2. Ta metoda dodati predmete Program:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Spremite datoteku Program.cs.

5. Kliknite **pokretanje** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

## <a name="resize-a-virtual-machine"></a>Promjena veličine virtualnog računala

U ovom se primjeru pokazuje kako promijeniti veličinu izvodi virtualnog računala.

1. Komentar izvan kod koji ste prethodno dodali na način glavne osim kod radi dobivanja vjerodajnica.

2. Dodajte ovu metodu predmete Program:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Spremite datoteku Program.cs.

5. Kliknite **Start** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite s pretplatom na.

    Trebali biste vidjeti veličina Standard_A1 se promjene virtualnog računala.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Dodavanje podatkovni disk virtualnog računala

U ovom se primjeru prikazuje kako dodati podatkovni disk izvodi virtualnog računala.

1. Komentar izvan kod koji ste prethodno dodali na način glavne osim kod radi dobivanja vjerodajnica.

2. Dodajte ovu metodu predmete Program:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Spremite datoteku Program.cs.

5. Kliknite **pokretanje** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

## <a name="delete-a-virtual-machine"></a>Brisanje virtualnog računala

1. Komentar izvan kod koji ste prethodno dodali na način glavne osim kod radi dobivanja vjerodajnica.

2. Dodajte ovu metodu predmete Program:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Spremite datoteku Program.cs.

5. Kliknite **pokretanje** u Visual Studio, a zatim se prijavite za Azure AD pomoću korisničko ime i lozinku koje koristite za vašu pretplatu.

## <a name="next-steps"></a>Daljnji koraci

Ako postoje problemi s implementacije, možda pogledajte [Otklanjanje poteškoća resursa grupe implementacijama pomoću portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)
