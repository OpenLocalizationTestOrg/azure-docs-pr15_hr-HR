<properties
    pageTitle="Implementacija VM pomoću C# i resursima predložak | Microsoft Azure"
    description="Naučite kako koristiti C# i resursima predložak za implementaciju sustava VM Azure."
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
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Implementacija programa Azure virtualnog računala pomoću C# i resursima predloška

Pomoću grupa resursa i predloške uspijevate upravljanje svih resursa zajedno s podrškom za aplikacije. U ovom se članku objašnjava koristite Visual Studio i C# da biste postavili provjeru autentičnosti, stvorite predložak, a Implementirajte Azure resurse pomoću predložak koji ste stvorili.

Najprije morate da biste bili sigurni da ste ih dodali korake za postavljanje:

- Instalirajte [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Provjerite je li instalaciju paketa [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ili [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Početak [token za provjeru autentičnosti](../resource-group-authenticate-service-principal.md)
- Stvorite grupu resursa pomoću [Komponente PowerShell Azure](../resource-group-template-deploy.md), [Azure EŽA](../resource-group-template-deploy-cli.md)ili [Azure portal](../resource-group-template-deploy-portal.md).

Traje 30 minuta slijedite korake u nastavku.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Korak 1: Stvaranje projekta za Visual Studio, datoteka predloška i datoteke parametara

### <a name="create-the-template-file"></a>Stvaranje predloška datoteke

Predložak programa Azure Voditelj resursa omogućuje uvođenje i upravljanje Azure resursi zajedno. Predložak je opis JSON resurse i pridruženi implementacije parametara.

U Visual Studio, učinite sljedeće:

1. Kliknite **datoteka** > **nove** > **projekta**.

2. U **predlošcima** > **Visual C#**, odaberite **Konzole za aplikaciju**, unesite naziv i mjesto projekta, a zatim **u redu**.

3. Desnom tipkom miša kliknite naziv projekta u programu Explorer rješenje, kliknite **Dodaj** > **Nova stavka**.

4. Kliknite Web, odaberite JSON datoteku, unesite *VirtualMachineTemplate.json* naziv, a zatim **Dodaj**.

5. Otvaranje i zagrade zatvaranja VirtualMachineTemplate.json datoteke, dodajte potrebne elementa i element potrebna contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parametri](../resource-group-authoring-templates.md#parameters) uvijek nisu potrebni, no oni omogućuju unos vrijednosti kad je implementiran u predložak. Dodajte parametre element i njegovih podređenih elemenata nakon contentVersion element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Varijable](../resource-group-authoring-templates.md#variables) može se koristiti u predlošku da biste odredili vrijednosti koje se često mijenjaju ili vrijednosti koje je potrebno stvoriti od kombinacije vrijednosti parametara. Dodati varijable element nakon odjeljka parametara:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Resursi](../resource-group-authoring-templates.md#resources) kao što su virtualnog računala, virtualne mreže i račun za pohranu sljedeće su definirani u predlošku. Dodavanje sekcije resursi nakon odjeljka varijable:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Spremite datoteku predloška koji ste stvorili.

### <a name="create-the-parameters-file"></a>Stvaranje datoteke parametara

Da biste odredili vrijednosti za parametre resursa koji su definirani u predložak, stvarate datoteku parametri koji sadrži vrijednosti koje se koriste kada je implementiran u predložak. U Visual Studio, učinite sljedeće:

1. Desnom tipkom miša kliknite naziv projekta u programu Explorer rješenje, kliknite **Dodaj** > **Nova stavka**.

2. Kliknite Web, odaberite JSON datoteku, unesite *Parameters.json* naziv, a zatim **Dodaj**.

3. Otvorite datoteku parameters.json, a zatim dodajte JSON sadržaja:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] U ovom se članku stvara virtualnog računala na kojem je instalirana verzija operacijskog sustava Windows Server. Dodatne informacije o odabiru druge slike potražite u članku [Kretanje i slika odaberite Azure virtualnog računala pomoću komponente Windows PowerShell i EŽA Azure](virtual-machines-linux-cli-ps-findimage.md).

4. Spremite datoteku parametre koje ste stvorili.

## <a name="step-2-install-the-libraries"></a>Korak 2: Instalacija biblioteke

Paketi NuGet su Najlakši način instaliranja biblioteke koje su vam potrebne za dovršetak ovog praktičnog vodiča. Morate biblioteke za upravljanje Azure resursa i provjera autentičnosti biblioteke za Active Directory Azure stvaranje resursa. Da biste dobili te biblioteke u Visual Studio, učinite sljedeće:

1. Desnom tipkom miša kliknite naziv projekta u pregledniku rješenja, kliknite **Upravljanje NuGet paketa**, a zatim Pregledaj.

2. Vrste *Servisa Active Directory* u okvir za pretraživanje kliknite **instalaciju** paketa provjera autentičnosti biblioteke imenika Active Directory, a zatim slijedite upute za instalaciju paketa.

4. Pri vrhu stranice odaberite **Uključi predizdanja**. Vrsta *Microsoft.Azure.Management.ResourceManager* u okvir za pretraživanje kliknite **Instaliraj** upravljanje bibliotekama Microsoft Azure resursa, a zatim slijedite upute za instalaciju paketa.

Sada ste spremni za početak rada s bibliotekama da biste stvorili aplikacije.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Korak 3: Stvaranje vjerodajnica koje služe za provjeru autentičnosti zahtjeva

Stvaranja aplikacije Azure Active Directory te se instalira provjera autentičnosti biblioteke. Sada oblikujte informacije o aplikaciji u vjerodajnica koje služe za provjeru autentičnosti zahtjeva za Voditelj resursa Azure.

1. Otvorite datoteku Program.cs projekta koji ste stvorili, a zatim dodajte ih pomoću naredbi na vrh datoteku:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Dodajte ovu metodu predmete Program da biste dobili tokena koje je potrebno da biste stvorili vjerodajnice:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {Id klijenta} zamijenite identifikator Azure Active Directory aplikacija, a zatim {klijent-tajna} s tipka za pristup aplikacije za AD i {id klijenta} s identifikator klijent za vašu pretplatu. Id klijenta možete pronaći tako da pokrenete Get-AzureRmSubscription. Tipkovni prečac možete pronaći pomoću portala za Azure.

3. Da biste stvorili vjerodajnice, dodavanje koda za ovu metodu glavne u datoteci Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Spremite datoteku Program.cs.

## <a name="step-4-deploy-the-template"></a>Korak 4: Uvođenje predloška

U ovom ćete koraku koristite grupu resursa koji ste prethodno stvorili, ali ste mogli stvoriti grupu resursa pomoću [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) i klase [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) .

1. Dodati varijable korakom glavne klase Program da biste odredili nazive resursa koji ste prethodno stvorili, naziv implementacije i identifikatora svoje pretplate:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Zamijenite vrijednost naziv grupe naziv grupe resursa. Zamijenite vrijednost deploymentName naziv koji želite koristiti za implementaciju. Identifikator pretplate možete pronaći tako da pokrenete Get-AzureRmSubscription.

2. Dodajte ovu metodu predmete Program za implementaciju resursa u grupu resursa pomoću predloška koji ste definirali:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Ako ste željeli uvođenje predloška s računa za pohranu, svojstvo predloška možete zamijeniti svojstvo TemplateLink.

3. Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Korak 5: Brisanje resursi

Budući da vam se naplatiti za resurse koji se koriste u Azure, uvijek je dobro da biste izbrisali resursa koje više nisu potrebne. Ne morate zasebno brisanje svakog resursa iz grupe resursa. Brisanje grupe resursa i njegovih resursa se automatski brišu.

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

2.  Da biste nazvali način na koji ste upravo dodali, dodati kod glavna načina:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Korak 6: Pokretanje aplikacije konzole

1.  Pokrenite konzolu, kliknite **pokretanje** u Visual Studio i prijavite se u za Azure AD pomoću istih vjerodajnica koje koristite za vašu pretplatu.

2.  Kada se pojavi status prihvaćena, pritisnite **Enter** .

    Traje oko pet minuta za ovu aplikaciju konzole da biste pokrenuli potpuno od početka do kraja. Prije nego što pritisnete tipku Enter da biste pokrenuli brisanje resursa, može potrajati nekoliko minuta da biste provjerili stvaranje resursa na portalu za Azure prije nego ih izbrišete.

3. Da biste vidjeli status resursa, otiđite zapisnike nadzora Azure portalu:

    ![Pregled zapisnika nadzora Azure portalu](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Daljnji koraci

- Ako postoje problemi s implementaciju, možete pogledati [implementacijama za grupe resursa otklanjanje poteškoća s portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)bio bi sljedeći korak.
- Informirajte se o upravljanju virtualnog računala koju ste stvorili tako da pročitate [Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i PowerShell](virtual-machines-windows-csharp-manage.md).
