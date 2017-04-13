<properties
    pageTitle="Postavljanje pristupa WinRM za virtualnim strojevima u Azure Voditelj resursa | Microsoft Azure"
    description="Kako postaviti WinRM pristup za korištenje s programa upravitelj resursa Azure virtualnog računala"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Postavljanje pristupa WinRM za virtualnim strojevima u Azure Voditelj resursa

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM u Upravljanje servisom Azure Dodavanje veze za vanjskih Voditelj resursa za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasični implementacije

* Da biste saznali kako od Voditelj resursa Azure, pročitajte članak u ovom se [članku](../azure-resource-manager/resource-group-overview.md)
* Razlike između Upravljanje servisom Azure i upravljanja resursima Azure potražite u ovom [članku](../resource-manager-deployment-model.md)

Ključne razlike u odjeljku Postavljanje konfiguracije WinRM između dva snop je kako dobiti certifikat instalirano na VM. U stogu Voditelj resursa Azure certifikati su katalog modeliran kao resurse upravlja davatelj resursa za zbirke ključeva ključ. Dakle, korisnik mora unijeti vlastiti certifikat i prenesite ga ključ sigurnog prije korištenja u na VM.

Ovdje su navedeni koraci koje treba poduzeti da biste postavili na VM s povezivanjem WinRM

1. Stvaranje ključa zbirke ključeva
2. Stvaranje samopotpisane potvrde
3. Prijenos samopotpisani certifikat sigurnog ključ
4. Dohvaćanje URL-a samopotpisani certifikat u sigurnog ključ
5. Prilikom stvaranja na VM referencu samopotpisane potvrde URL-a

## <a name="step-1-create-a-key-vault"></a>Korak 1: Stvaranje ključa zbirke ključeva

Možete koristiti u ispod naredbu da biste stvorili sigurnog ključ

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Korak 2: Stvaranje samopotpisane potvrde
Možete stvoriti pomoću ovog skriptu PowerShell samopotpisanog certifikata

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Korak 3: Prijenos samopotpisani certifikat sigurnog ključ

Prije prijenosa certifikat sigurnog ključ stvorili u koraku 1 treba pretvoriti u oblik davatelja resursa Microsoft.Compute će razumjeti. U nastavku PowerShell skriptu da bi to učiniti

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Korak 4: Dohvaćanje URL-a samopotpisani certifikat u sigurnog ključ

Davatelja resursa Microsoft.Compute mora URL-a tajna unutar zbirke ključeva ključa prilikom dodjele resursa u VM. Time se omogućuje davatelja resursa Microsoft.Compute da biste preuzeli na tajna i stvaranje ekvivalentan certifikata na na VM.

>[AZURE.NOTE]URL za tajna mora uključiti u verziji. Primjer URL izgleda ispod https://contosovault.vault.azure.net:443/tajne/contososecret/01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Predlošci

Možete dobiti vezu do URL-a pomoću predloška u nastavku kod

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell

Možete pristupiti pomoću URL-a u nastavku naredbe komponente PowerShell

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Korak 5: Referenca samopotpisane potvrde URL-a tijekom stvaranja na VM

#### <a name="azure-resource-manager-templates"></a>Predlošci Azure Voditelj resursa

Prilikom stvaranja VM kroz predloške certifikata poziva može vidjeti u odjeljku tajne i u odjeljku winRM kao ispod:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Predložak uzorka za navedenog Ovdje možete pronaći na [201 – vm – winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Izvorni kod za ovaj predložak možete pronaći na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Korak 6: Povezivanje s VM
Prije nego što se možete povezati s VM morat ćete provjerite je li vaše računalo je konfiguriran za daljinsko upravljanje WinRM. Pokretanje programa PowerShell kao administrator i izvoditi na ispod naredbu da biste provjerili sve postavite.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Bilo bi dobro da biste bili sigurni da se pokreće servis za WinRM ako navedenog ne funkcionira. To možete učiniti s tom pomoću`Get-Service WinRM`

Kada Završi postavljanje, možete se povezati pomoću VM u nastavku naredbe

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate