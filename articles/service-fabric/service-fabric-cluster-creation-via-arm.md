
<properties
   pageTitle="Stvaranje sigurnog klaster tkanina servis pomoću upravitelja resursa Azure | Microsoft Azure"
   description="U ovom se članku opisuje kako postaviti sigurne klaster tkanina servisa u Azure pomoću upravitelja resursa Azure, sigurnog ključ Azure i Azure Active Directory (AAD) za provjeru autentičnosti klijenta."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Stvaranje servisa tkanina klaster u Azure pomoću upravitelja resursa za Azure

> [AZURE.SELECTOR]
- [Azure Voditelj resursa](service-fabric-cluster-creation-via-arm.md)
- [Portal za Azure](service-fabric-cluster-creation-via-portal.md)

Ovo je Postupni vodič koji će vas kroz korake postavljanja sigurne klaster tkanina servisa Azure u Azure pomoću upravitelja resursa Azure. Ovaj vodič vodit će vas kroz sljedeće korake:

 - Postavljanje sigurnog ključ za upravljanje tipke za sigurnost klaster te aplikacije.
 - Stvaranje sigurnog klaster u Azure s Azure Voditelj resursa.
 - Provjeru autentičnosti korisnika s Azure Active Directory (AAD) za upravljanje klaster.

Sigurna klaster je klaster koji sprječava neovlaštenog pristupa operacija upravljanja, koji obuhvaća implementacija, Nadogradnja i brisanje aplikacije, servise i podataka koje sadrže. Nesigurnom klaster je klaster da svi možete povezati u bilo kojem trenutku i izvođenje operacija upravljanja. Iako je moguće da biste stvorili nesigurnom klaster, je **se preporučuje da biste stvorili sigurne klaster**. Na nesigurnom klaster **nije moguće osigurati kasnije** – novi klaster moraju se stvoriti.

Konceptima jednaki su za stvaranje sigurnog klastere li skupina su Linux klastere ili klastere sustava Windows. Dodatne informacije i Pomoćnik za skripte za stvaranje sigurnog Linux klastere, potražite u članku [Stvaranje sigurnog klastere na Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Prijavite se na Azure
Ovaj vodič koristi [Azure PowerShell][azure-powershell]. Prilikom pokretanja novu sesiju ljuske PowerShell, prijavite se na račun za Azure i odaberite pretplate prije izvršavanja Azure naredbi.

Prijavite se na račun za azure:

```powershell
Login-AzureRmAccount
```

Odaberite pretplatu:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Postavljanje sigurnog ključ

U ovom se odjeljku vodi kroz stvaranje zbirke ključeva ključ za servis tkanina klaster u Azure i tkanina servisa aplikacija. Vodič na ključ sigurnog potražite [ključ sigurnog uvodni vodič][key-vault-get-started].

Servis tkanina koristi X.509 certifikate sigurne klaster i pružanja sigurnosnim značajkama aplikacije. Azure sigurnog ključ služi za upravljanje certifikati za klastere tkanina servisa u Azure. Kada je klaster uveden u Azure, odgovorna za stvaranje klastere tkanina servisa Azure resursa davatelj povlači potvrde iz zbirke ključeva ključ i na klaster VMs instalira.

Sljedeći dijagram prikazuje odnos između sigurnog ključ, servis tkanina klaster i davatelja Azure resursa koja koristi certifikate spremljene u ključ sigurnog kada se stvara klaster:

![Instaliranje certifikata][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Stvaranje grupa resursa

U prvi je korak da biste stvorili grupu resursa namijenjene sigurnog ključ. Preporučuje se stavljanje ključ sigurnog u vlastitu grupu resursa. To vam omogućuje da uklonite računalnim i pohranu grupe resursa, uključujući u grupi resursa koji ima svoj klaster tkanina servisa bez gubitka ključeva i tajne. Grupa resursa koja sadrži vaše sigurnog ključ mora biti u području isti kao klaster koji se koristi.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Stvaranje sigurnog ključa 

Stvaranje ključ sigurnog u novu grupu resursa. Na sigurnog ključ **mora biti omogućen za implementaciju** da biste omogućili davatelja usluge tkanina resursa radi potvrde i ga i instalacija na klaster čvorove:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Ako imate postojeće zbirke ključeva ključ, možete je omogućiti za implementaciju pomoću Azure EŽA:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Dodavanje potvrde sigurnog ključ

Potvrde se koriste u tkanina servis za provjeru autentičnosti i šifriranje razne aspekte klaster i njegov aplikacije za sigurnu. Dodatne informacije o korištenja potvrde u servis tkanina, potražite u članku [servis tkanina klaster sigurnost scenariji][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Poslužitelj i klaster certifikat (obavezno) 

Taj certifikat potrebna je sigurna klaster i spriječili neovlašteni pristup. Pruža klaster sigurnosti na nekoliko načina:
 
 - **Klaster provjere autentičnosti:** Potvrđuje čvor čvor komunikacije za vanjski pristup klaster. Samo čvorove koji mogu biti njihov identitet pomoću ovog certifikata možete se uključiti klaster.
 - **Provjera autentičnosti poslužitelja:** Potvrđuje klaster upravljanje krajnje točke za klijent za upravljanje tako da se klijent za upravljanje zna razgovor realni klaster. Taj certifikat nudi SSL za upravljanje HTTPS API-JA i Explorer tkanina servisa putem HTTP.

Da bi služio te namjene certifikat mora zadovoljiti sljedeće preduvjete:

 - Certifikat mora sadržavati privatni ključ.
 - Certifikat mora se stvara za ključne exchange, moguće je izvesti u datoteku osobne razmjena podataka (.pfx).
 - Potvrde predmetni naziv mora podudarati domenu koja se koristi za pristup klaster tkanina servisa. U ovom matchng je obavezan nudi SSL za krajnje točke za upravljanje HTTPS i usluge tkanina Explorer na klaster. Nije moguće nabavite SSL certifikat od ustanove za izdavanje certifikata (CA) za na `.cloudapp.azure.com` domene. Morate pribaviti prilagođenog naziva domene za svoj klaster. Kada certifikat zatražite od izdavanje certifikata, potvrde predmetni naziv mora odgovarati naziv prilagođene domene koji se koristi za svoj klaster.

### <a name="application-certificates-optional"></a>Certifikati aplikacije (neobavezno)

Bilo koji broj Dodatni certifikati moguće je instalirati na klaster aplikacije iz sigurnosnih razloga. Prije stvaranja svoj klaster, razmislite o scenariji sigurnost računala koje zahtijevaju certifikat za instalaciju na čvorove, kao što su:

 - Šifriranje i dešifriranje aplikacije konfiguracijskih vrijednosti
 - Šifriranje podataka preko čvorove tijekom replikacije 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Oblikovanje certifikata za korištenje usluga Azure resursa

Privatni ključ datoteke (.pfx) možete dodati i koristiti izravno putem sigurnog ključ. Davatelja Azure resursa zauzima ključeva za pohranu u posebno JSON OSNOVNI oblik koji sadrži .pfx kao base-64 kodirani niz i privatni ključ lozinku. Kako bi odgovarao te preduvjete, tipke mora biti smješten u niz JSON i zatim pohraniti kao *tajne* u sigurnog ključ.

Da biste olakšali taj proces, modul ljuske PowerShell [dostupna]je na GitHub[service-fabric-rp-helpers]. Slijedite ove korake da biste koristili modula:

  1. Preuzmite cijeli sadržaj u repo u lokalni direktorij. 
  2. Uvoz modul u prozor programa PowerShell:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
Na `Invoke-AddCertToKeyVault` naredbe u ovom modul ljuske PowerShell automatski oblikuje privatni ključ certifikata u niz JSON i prenese sigurnog ključ. Pomoću njega možete dodati klaster certifikat i sve potvrde dodatne aplikacije sigurnog ključ. Ponovite ovaj korak za sve dodatne certifikate koji želite instalirati u svoj klaster.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Prethodni su sve preduvjeti sigurnog ključ za konfiguriranje servisa tkanina resursima predložak klaster koja se instalira certifikata za provjeru autentičnosti čvor, upravljanje krajnjoj točki sigurnost i provjeru autentičnosti i sve značajke sigurnosti dodatne aplikacije koje koriste X.509 certifikate. Sada trebala bi se pojaviti sljedeća postavljanje u Azure:

 - Grupa resursa sigurnog ključ
   - Ključni zbirke ključeva
     - Certifikat za provjeru autentičnosti poslužitelja klaster
     - Certifikati za aplikaciju

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Postavljanje Azure Active Directory za provjeru autentičnosti klijenta

AAD omogućuje upravljanje korisničkim pristupom za aplikacije koje su podijeljeni aplikacije s utemeljen na webu prijava korisničkog Sučelja i aplikacije doživljaj nativni klijent tvrtke ili ustanove (poznatom kao klijenata). U ovom dokumentu, pretpostavimo da ste već stvorili klijentom. Ako nije, najprije [kako doći do klijent za Azure Active Directory]za čitanje[active-directory-howto-tenant].

Servis tkanina klaster nudi nekoliko točke unosa u njegovoj funkcionalnosti upravljanja, uključujući koji se temelji na web [Servisa tkanina Explorer] [ service-fabric-visualizing-your-cluster] i [Visual Studio][service-fabric-manage-application-in-visual-studio]. Zbog toga stvoriti dvije AAD aplikacije za kontrolu pristupa klaster, jednoj web-aplikaciji i jedan matičnoj aplikaciji.

Da biste pojednostavnili neke od koraka konfiguriranje AAD sa servisa tkanina klaster, stvorili smo skup skripte komponente Windows PowerShell.

>[AZURE.NOTE] Morate izvršiti ove korake *prije* stvaranja klaster tako da u slučajevima u kojima se skripte očekivati klaster imena i krajnje točke, te mora biti planiranim vrijednostima ne one koje ste već stvorili.

1. [Preuzimanje skripte] [ sf-aad-ps-script-download] s računalom.

2. Desnom tipkom miša kliknite zip datoteku, odaberite **Svojstva**, a zatim potvrdite okvir **Unblock** i primijenite.

3. Izdvajanje zip datoteke.

4. Pokrenite `SetupApplications.ps1`, koja omogućuje TenantId, ClusterName i WebApplicationReplyUrl kao parametri. Ako, na primjer:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Možete pronaći **TenantId** izvršavanjem naredbe ljuske PowerShell "Get-AzureSubscription" ". Time će se prikazati **TenantId** za svaku pretplatu.

    **ClusterName** koristi se za prefiks AAD aplikacija stvorila skriptu. Ne morate da odgovara nazivu stvarni klaster točno onako kako je namijenjen samo olakšati mapiranje artefakte AAD klaster tkanina usluge koje su se koristi s.

    **WebApplicationReplyUrl** je AAD vraća korisnicima nakon dovršetka postupka prijave zadana krajnja točka. Postavite to krajnjoj Explorer tkanina servisa za svoj klaster, koja se po zadanom je:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Se od vas zatraži prijavite se u računa koji ima administratorske ovlasti za klijent AAD. Kada to učinite, nastavlja se skriptu da biste stvorili web- a izvornim aplikacijama za predstavljanje svoj klaster tkanina servisa. Ako pogledate aplikacije na klijentu [Azure klasični portal][azure-classic-portal], trebali biste vidjeti dva nove stavke:

    - *ClusterName*\_klaster
    - *ClusterName*\_klijenta

    Ispisuje skripte Json potrebnih Voditelj resursa Azure predloška prilikom stvaranja klaster u sljedećem odjeljku tako da bi PowerShell otvara prozor.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Stvaranje predloška resursima servisa tkanina klaster

U ovom se odjeljku Izlaz iz prethodne naredbe ljuske PowerShell se koriste u predlošku resursima servisa tkanina klaster.

Voditelj resursa Ogledni predlošci su dostupni u [galeriji Azure predložaka za brzi početak rada na GitHub][azure-quickstart-templates]. Te predloške možete koristiti kao početnu točku za predložak klaster. 

### <a name="create-the-resource-manager-template"></a>Stvaranje predloška Voditelj resursa

Ovaj vodič koristi [sigurnu klaster 5 čvor] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] primjer predloška i parametri predložaka. Preuzmite `azuredeploy.json` i `azuredeploy.parameters.json` na računalo i otvorite obje datoteke u omiljene uređivač.

### <a name="add-certificates"></a>Dodavanje certifikata

Certifikati dodaju se predložak resursima klaster Upućivanjem sigurnog ključ koji sadrži tipke za potvrdu. Preporučuje se ove vrijednosti ključ sigurnog smještaju u datoteci parametara predloška Voditelj resursa da biste zadržali Voditelj resursa ponovno iskoristiv datoteku predloška i besplatno vrijednosti koje se odnose na implementacije.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Dodavanje Svi certifikati VMSS osProfile

Svaki certifikat koji je potrebno je instalirati u klasteru mora biti konfigurirano u odjeljku osProfile VMSS resursa (Microsoft.Compute/virtualMachineScaleSets). To upućuje davatelja resursa da biste instalirali certifikata na na VMs. To obuhvaća klaster certifikata, kao i bilo koju aplikaciju sigurnosne potvrde namjeravate koristiti za aplikacije:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Konfiguriranje usluge tkanina klaster certifikata

Certifikat za provjeru autentičnosti klaster morate konfigurirati u servis tkanina klaster resursa (Microsoft.ServiceFabric/clusters) i nastavak tkanina servisa za VMSS u VMSS resursa. Time se omogućuje davatelja usluge tkanina resursa da biste konfigurirali za korištenje za web-mjesto klaster provjeru autentičnosti i u okvir za provjeru autentičnosti poslužitelja za upravljanje krajnje točke.

##### <a name="vmss-resource"></a>VMSS resursa:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Servis tkanina resursa:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>Umetanje AAD config

Konfiguracija AAD ste ranije stvorili mogu umetnuti izravno u predlošku Voditelj resursa, no preporučuje se da biste izdvojili vrijednosti u parametara najprije u datoteku parametara da biste zadržali predložak resursima ponovno iskoristiv i besplatno vrijednosti određene za implementacije.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < na "Konfiguriranje-arm" ></a>parametri predložaka za konfiguriranje Voditelj resursa

Na kraju, koristite izlaznih vrijednosti sigurnog ključ i AAD PowerShell naredbe za popunjavanje datoteke parametara:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Sada trebala bi se sljedeće:

 - Grupa resursa sigurnog ključ
    - Ključni zbirke ključeva
    - Certifikat za provjeru autentičnosti poslužitelja klaster
    - Certifikat za šifriranje podataka
 - Azure Active Directory klijenta 
    - AAD aplikacija za upravljanje utemeljen na webu i Explorer tkanina servisa
    - AAD aplikacija za upravljanje nativni klijent
    - Korisnici koji imaju uloge dodijeljeno 
 - Predložak za resursima servisa tkanina klaster
    - Certifikati konfiguriran putem sigurnog ključ
    - Azure Active Directory konfigurirano 

Sljedeći dijagram prikazuje gdje će se ključ sigurnog i AAD konfiguracije uklapaju u predlošku Voditelj resursa.

![Voditelj resursa ovisnost karte][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Stvaranje klaster

Sada ste spremni stvoriti klaster pomoću [OKVIRA implementaciju][resource-group-template-deploy].

#### <a name="test-it"></a>Testiranje je

Koristite sljedeću naredbu komponente PowerShell za testiranje predloška resursima pomoću parametara datoteke:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Njegove implementacije

Ako predložak test resursima prolazi, koristite sljedeću naredbu komponente PowerShell uvođenje predloška resursima pomoću parametara datoteke:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Korisnicima dodijeliti uloge

Nakon stvaranja aplikacije za predstavljanje svoj klaster, morate korisnicima dodijelite uloge podržava tkanina servisa: samo za čitanje i administrator. To možete učiniti pomoću [portala za Azure klasični][azure-classic-portal].

1. Idite na klijentu, a zatim odaberite aplikacije.
2. Odaberite web-aplikaciju koja ima naziv kao što su `myTestCluster_Cluster`.
3. Kliknite karticu korisnika.
4. Odaberite korisnika da biste dodijelili te kliknite gumb **Dodjela** pri dnu zaslona.

    ![Korisnicima dodijeliti uloge gumb][assign-users-to-roles-button]

5. Odaberite ulogu za dodjelu korisnika.

    ![Korisnicima dodijeliti uloge][assign-users-to-roles-dialog]

>[AZURE.NOTE] Dodatne informacije o ulogama u servis tkanina potražite u članku [Upravljanje pristupom utemeljeno na ulogama za klijente tkanina servisa](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Stvaranje sigurnog klastere na Linux

Da biste olakšali postupak je dodijeljen Pomoćnik za skripte [ovdje](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Za korištenje ovaj Pomoćnik za skripte, pretpostavlja se da već imate instaliran EŽA za Azure i on se otvara u vašem putu. Provjerite ima li skriptu dozvole za izvršavanje tako da pokrenete `chmod +x cert_helper.py` nakon preuzimanja. Prvi je korak da biste se prijavili na račun za Azure pomoću EŽA s na `azure login` naredbe. Nakon prijave na račun za Azure pomoću u Pomoćnik za traje certifikata CA, kao što je prikazano sljedeće naredbe:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Ta naredba Vrati sljedeća tri niza kao rezultat: 

1. Na SourceVaultID, što je ID za nove KeyVault ResourceGroup stvara za vas. 

2. CertificateUrl za pristup certifikata.

3. Na CertificateThumbprint, koji se koristi za provjeru autentičnosti.


Sljedeći primjer pokazuje kako koristiti naredbu:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Izvršavanje naredbe prethodni pruža tri niza na sljedeći način:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Potvrde predmetni naziv mora podudarati domenu koja se koristi za pristup klaster tkanina servisa. Ovo je obavezan nudi SSL za krajnje točke za upravljanje HTTPS i usluge tkanina Explorer na klaster. Nije moguće nabavite SSL certifikat od ustanove za izdavanje certifikata (CA) za na `.cloudapp.azure.com` domene. Morate pribaviti prilagođenog naziva domene za svoj klaster. Kada se zatraži potvrdu iz od CA potvrde predmetni naziv mora odgovarati naziv prilagođene domene koji se koristi za svoj klaster.

To su stavke potrebne za stvaranje na servis za sigurnu tkanina klaster (bez AAD) kao što je opisano na [Parametri predložaka za konfiguriranje Voditelj resursa](#configure-arm). Možete se povezati sigurne klaster putem upute u [provjere autentičnosti za klijentski pristup za klaster](service-fabric-connect-to-secure-cluster.md). Linux pretpregled klastere ne podržavaju AAD provjeru autentičnosti. Možete dodijeliti uloge administrator i klijent kao što je opisano u odjeljku [Dodjela uloga korisnicima](#assign-roles). Pri određivanju klijent i administratore ulogama za pretpregled klaster Linux morate unijeti thumbprints certifikat za provjeru autentičnosti (umjesto predmetni naziv, jer provjere valjanosti lanac ni opoziva se provodi u ovom izdanju pretpregled).


Ako želite koristiti samopotpisanog certifikata za testiranje možete koristiti isti skripte za stvaranje samopotpisane potvrde i prenesite ga KeyVault, unosom zastavicu `ss` umjesto pruža certifikat put i naziv certifikata. Ako, na primjer, pogledajte sljedeće naredbe za stvaranje i prijenos samopotpisanog certifikata:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Ta naredba Vrati iste tri niza, SourceVault, CertificateUrl i CertificateThumbprint koji se koristi za stvaranje sigurnog Linux klaster, uz mjesto na mjesto na kojem je nalazi samopotpisani certifikat. Trebat će vam samopotpisani certifikat za povezivanje s klaster.  Možete se povezati sigurne klaster putem upute u [provjere autentičnosti za klijentski pristup za klaster](service-fabric-connect-to-secure-cluster.md). Potvrde predmetni naziv mora podudarati domenu koja se koristi za pristup klaster tkanina servisa. Ovo je obavezan nudi SSL za krajnje točke za upravljanje HTTPS i usluge tkanina Explorer na klaster. Nije moguće nabavite SSL certifikat od ustanove za izdavanje certifikata (CA) za na `.cloudapp.azure.com` domene. Morate pribaviti prilagođenog naziva domene za svoj klaster. Kada se zatraži potvrdu iz od CA potvrde predmetni naziv mora odgovarati naziv prilagođene domene koji se koristi za svoj klaster.

Kao što je opisano u odjeljku [Stvaranje klaster na portalu za Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-portal)parametre koje ste dobili od Pomoćnik za skripte možete ispuniti portalu.

## <a name="next-steps"></a>Daljnji koraci

Sada imate sigurne klaster s Azure Active Directory koja omogućuje upravljanje provjere autentičnosti. Sljedeći, [Povezivanje s svoj klaster](service-fabric-connect-to-secure-cluster.md) te Saznajte kako [upravljati tajne aplikacije](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Otklanjanje poteškoća prilikom postavljanja Azure Active Directory za provjeru autentičnosti klijenta

Ako naiđete na problem prilikom postavljanja Azure Active Directory za provjeru autentičnosti klijenta, pregledajte sljedeće suggestings za moguća rješenja.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Servis tkanina Explorer pita za odabir certifikata

#### <a name="problem"></a>Problem

Nakon prijave uspješno na stranicu za prijavu AAD u programu Explorer tkanina servisa web-pregledniku vraća početnu stranicu, no pita dijaloški okvir za odabir certifikata.

![Dijaloški okvir Odabir certifikat SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Razlog

Korisnik nije dodijeljena uloga u AAD klaster aplikacije. Stoga AAD ne uspije provjera autentičnosti na servis tkanina klaster. Servis tkanina Explorer pada natrag za provjeru autentičnosti certifikata.

#### <a name="solution"></a>Rješenja

Slijedite upute za postavljanje AAD i dodijelite uloge korisnika. Osim toga, "Korisnik DODJELE POTREBNE za Aplikaciju programa ACCESS" preporučuje se biti uključen kao `SetupApplications.ps1` ne.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Povezivanje s PowerShell ne uspijeva uz poruku o pogrešci: navedene vjerodajnice nisu valjane

#### <a name="problem"></a>Problem

Kada pomoću komponente PowerShell za povezivanje s klaster pomoću način sigurnosti "AzureActiveDirectory", nakon prijave uspješno na stranicu za prijavu AAD veze ne uspijeva uz poruku o pogrešci: navedene vjerodajnice nisu valjane prikazano.

#### <a name="solution"></a>Rješenja

Isto kao prethodna.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Servis tkanina Explorer prijava u povrat pogreške: AADSTS50011

#### <a name="problem"></a>Problem

Nakon prijave na AAD stranica za prijavu u programu Explorer tkanina servisa stranice vraća predznak u pogreška – AADSTS50011: adrese za odgovor &lt;url&gt; ne odgovara adrese za odgovor konfiguriran za aplikaciju: &lt;guid&gt;. 

![Adrese za odgovor SFX ne podudaraju][sfx-reply-address-not-match]

#### <a name="reason"></a>Razlog

Cluster(web) aplikacije koji predstavlja pokušaja Explorer tkanina servis za provjeru autentičnosti AAD, kao dio zahtjev nudi preusmjeravanje URL za povrat. No nije naveden na popisu 'Odgovor URL' AAD aplikacija.

#### <a name="solution"></a>Rješenja

Dodajte URL-a servisa tkanina Explorer "Odgovor URL" na kartici Konfiguriraj cluster(web) aplikacije ili zamijenite jednu od stavki na popisu. Zatim Spremi.

![Url za odgovor web-aplikacije][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Možete li koristiti isti AAD klijenta za više klastere?

#### <a name="answer"></a>Odgovor

Da. No imajte na umu da biste dodali na tkanina Explorer URL servis cluster(web) aplikacija ne funkcioniraju drukčije Explorer tkanina servisa.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Zašto li i dalje potreban certifikat poslužitelja dok je omogućena AAD?

#### <a name="answer"></a>Odgovor

FabricClient FabricGateway putem međusobna provjere autentičnosti. U slučaju AAD provjeru autentičnosti, Integracija AAD nudi identiteta klijenta poslužitelj i certifikat poslužitelja se koristi da biste provjerili identitet poslužitelja. Dodatne informacije o funkcioniranje certifikata na servis tkanina, potvrdite okvir [X.509 certifikate i tkanina servisa][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png