<properties
   pageTitle="Implementacija višestruki NIC VMs pomoću predloška u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako uvesti više NIC VMs pomoću predloška u upravitelju resursa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Implementacija višestruki NIC VMs pomoću predloška

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Budući da u ovoj fazi vremena ne mogu imati VMs s jednom NIC i VMs s više NIC-ovi u istoj grupi resursa, provodite pozadinskih poslužitelja u grupu resursa i druge komponente drugi sigurnosne grupe. Koraci u nastavku koristiti grupu resursa pod nazivom *IaaSStory* za grupu glavnom resursa i *IaaSStory pozadinskog* za pozadinskih poslužitelja.

## <a name="prerequisites"></a>Preduvjeti

Prije nego što možete implementirati pozadinskih poslužitelja, morate uvesti grupe glavnom resursa s sve potrebne resurse za taj scenarij. Da biste implementirali tih resursa, slijedite korake u nastavku.

1. Dođite do [stranice za predložak](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Na stranici predložak s desne strane **nadređenu grupu resursa**, zatim **Implementiraj za Azure**.
3. Ako je potrebno, promijenite vrijednosti parametra, a zatim slijedite korake na portalu Azure pretpregled za implementaciju u grupu resursa.

> [AZURE.IMPORTANT] Provjerite jesu li imena pohranu računa jedinstveni. Ne možete imati nazive računa dupliciranih prostora za pohranu Azure.

## <a name="understand-the-deployment-template"></a>Razumijevanje uvođenje predloška

Prije implementacije predložak koji ste dobili uz ove dokumentacije, provjerite je li razumijevanje funkcija. Koraci u nastavku omogućuju dobar pregled dotičnog predložak.

1. Dođite do [stranice za predložak](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kliknite **azuredeploy.json** da biste otvorili datoteku predloška.
3. Obratite pozornost na to parametar *osType* koja je navedena u nastavku. Ovaj parametar služi za odabir slike koje VM za poslužitelj baze podataka, zajedno s više operacijski sustav povezane postavke.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Pomaknite se na popisu varijable pa provjerite definiciju za varijable **dbVMSetting** navedena u nastavku. Prima neke elemente polja koje se nalaze u **dbVMSettings** varijabli. Ako ste upoznati s software development terminologija, možete pogledati varijabla **dbVMSettings** kao tablicu raspršivanja ili na dictionay.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Ako se odlučite za implementaciju sustava Windows VMs radi SQL u pozadinska. Zatim vrijednost za **osType** bio *sustava Windows*, a varijabla **dbVMSetting** sadržavat će navedena u nastavku, element koji predstavlja prvu vrijednost u varijablu **dbVMSettings** .

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Obratite pozornost na to **vmSize** sadrži vrijednost *Standard_DS3*. Samo određene veličine VM omogućuju korištenje više NIC-ovi. Možete provjeriti koji veličina VM su višestruki NIC omogućeno [višestruki NIC pregled](virtual-networks-multiple-nics.md)web-mjestu.
7. Pomaknite se do odjeljka **resurse** i obratite pozornost na prvi element. Opisuje s računom za pohranu. Taj račun za pohranu će se koristiti za održavanje diskova podataka koji se koriste VM za svaku bazu podataka. U ovom scenariju VM za svaki baza podataka ima OS na disku pohranjene u pravilnim prostora za pohranu i dva podataka diskova pohranjene u spremište SSD (premium).

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Pomaknite se do sljedećeg resursa navedene u nastavku. Ovaj resurs predstavlja NIC koji se koristi za pristup bazi podataka u VM za svaku bazu podataka. Obratite pozornost na korištenje funkcije **Kopiraj** u ovaj resurs. Predložak omogućuje implementacija proizvoljan broj VMs kako želite, na temelju parametara **dbCount** . Stoga ćete morati stvoriti istu količinu NIC-ovi za pristup bazi podataka, jedan za svaki VM.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Pomaknite se do sljedećeg resursa navedene u nastavku. Ovaj resurs predstavlja NIC koji se koriste za upravljanje u VM za svaku bazu podataka. Još jednom, morate imati jednu od ovih NIC-ovi za VM za svaku bazu podataka. Obratite pozornost na element **networkSecurityGroup** povezivanja programa NSG koja omogućuje pristup RDP/SSH da biste NIC samo.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Pomaknite se do sljedećeg resursa navedene u nastavku. Ovaj resurs predstavlja raspoloživost postaviti za zajedničko korištenje VMs sve baze podataka. Na taj način koji jamči da će uvijek se jedan VM u skupu pokrenut tijekom održavanja.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Pomaknite se do sljedećeg resursa. Ovaj resurs predstavlja baze podataka VMs, kao što je vidjeti u prvih nekoliko redaka navedena u nastavku. Obratite pozornost na korištenje funkcije **Kopiraj** ponovno jamči da više VMs stvaraju na parametar **dbCount** . Primijetit ćete zbirke **dependsOn** . Popisuje dva NIC-ovi koji se potrebno stvoriti prije nego je na VM implementiran, zajedno s postavljanje dostupnosti i račun za pohranu.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Pomaknite se prema dolje u resursa VM element **networkProfile** navedene u nastavku. Imajte na umu da postoje dva NIC-ovi koji se referenca za svaki VM. Kada stvorite više NIC-ovi za na VM, morate postaviti **primarni** svojstvo nešto na NIC-ovi *true*i postavite na *false*.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Uvođenje predloška ARM pomoću klik za implementaciju

> [AZURE.IMPORTANT] Provjerite je li koraka [stara requisites](#Pre-requisites) prije slijedeći upute u nastavku.

Predložak uzorak u spremištu javno koristi parametar datoteku koja sadrži zadane vrijednosti za generiranje scenarij prethodno opisan. Da biste implementirali ovaj predložak pomoću kliknite implementacija, slijedite [ove veze](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), s desne strane **pozadinskog resursa grupe (pogledajte dokumentaciju)** kliknite **Implementacija Azure**, zamjena zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu.

Na slici u nastavku prikazuje sadržaj novu grupu resursa, nakon implementacije.

![Grupa resursa u pozadini](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Uvođenje predloška pomoću komponente PowerShell

Da biste implementirali predložak koji ste preuzeli pomoću komponente PowerShell, slijedite korake u nastavku.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Pokretanje u **`New-AzureRmResourceGroup`** cmdlet da biste stvorili grupu resursa pomoću ovog predloška.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Očekivani izlaz:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Uvođenje predloška pomoću EŽA Azure

Da biste pomoću EŽA Azure uvođenje predloška, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
2. Pokretanje u **`azure config mode`** naredbu da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Evo očekivanog izlaza iznad naredbe:

        info:    New mode is arm

3. Otvorite [datoteku parametar](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json)odaberite njegov sadržaj, a spremite datoteku na računalu. Primjerice, ne možemo spremili datoteku parametara *parameters.json*.

4. Pokretanje u **`azure group deployment create`** cmdlet za implementaciju novi VNet pomoću predloška i parametar datoteke možete preuzeti i izmijeniti iznad. Popis nakon izlaz objašnjava koristi parametre.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Očekivani izlaz:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
