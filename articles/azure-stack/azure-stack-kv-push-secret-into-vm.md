<properties
    pageTitle="Implementacija u VM s potvrdom pomoću Azure stogu ključ sigurnog | Microsoft Azure"
    description="Saznajte kako implementirati na VM i ubaciti certifikat iz zbirke ključeva stogu ključ za Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Stvaranje VMs i uključivanje certifikati dohvaćeni iz zbirke ključeva ključ

U stogu Azure VMs su implementiran putem upravitelja za Azure resursa, a sada možete spremiti potvrde u Azure stogu ključ sigurnog. Zatim Azure snop (davatelja Microsoft.Compute resursa biti određene) ih gura ih u vašem VMs kada su implementiran na VMs. Certifikati može se koristiti u mnogim situacijama, uključujući SSL, šifriranje i provjera autentičnosti potvrda temelje.

Ako koristite taj način, možete zadržati certifikat sigurni. Sada je ne VM slike ili aplikacije konfiguracijske datoteke ili nekom drugom nesigurnom mjestu. Postavljanjem odgovarajući pristup pravila za ključne sigurnog možete kontrolirati tko može vidjeti pristup certifikata. Druga pogodnost je da možete upravljati Svi certifikati na jednom mjestu u Azure stogu ključ sigurnog.

Slijedi kratak pregled postupka:

-   Potreban vam je certifikat u obliku .pfx.

-   Stvaranje ključa sigurnog (pomoću predloška ili sljedeće ogledne skripte).

-   Provjerite je li uključile parametar EnabledForDeployment.

-   Prenesite certifikat kao na tajna.

## <a name="deploying-vms"></a>Implementacija VMs

Ovaj primjer skripte stvara ključa sigurnog i pohranjuje certifikat koji je spremljen u .pfx datoteka na lokalnom direktoriju za ključne sigurnog kao na tajna.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Prvi dio skripte čita .pfx datoteka, a zatim sprema kao na JSON objekt s base64 sadržaja datoteke kodiran. Zatim objekt JSON je base64 kodiran.

Nakon toga on stvara novu grupu resursa i stvara ključa zbirke ključeva. Imajte na umu parametar zadnje naredbe nova AzureKeyVault "-EnabledForDeployment", koje dopušta pristup za Azure (posebno za davatelja resursa Microsoft.Compute) da biste pročitali sigurnog tajne iz ključa za implementacije.

Posljednje naredbe jednostavno pohranjuje JSON objekt base64 kodirati ključa sigurnog kao na tajna.

Evo oglednih izlaza iz prethodnog skripte:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Sada ćemo spremni za uvođenje predloška VM. Imajte na umu URI tajna iz izlaza (kao što je istaknuta je u prethodnom izlaz zelenom bojom).

Potreban vam je predložak nalazi se ovdje. Parametri posebno zanimaju (osim uobičajeni parametre VM) nisu naziv zbirke ključeva, grupa resursa sigurnog i tajnu URI. Naravno, preuzmite ga iz trgovine GitHub i izmjena prema potrebi.

Kada je implementiran u ovom VM, Azure umetati ga certifikat u na VM.
U sustavu Windows, potvrde u .pfx datoteka dodane u privatni ključ nije moguće izvesti. Certifikat dodaje se mjesto LocalMachine certifikata, s trgovinom certifikat u kojemu se korisniku. Datoteka certifikata na Linux pod se smješta u imenik /var/lib/waagent, pod nazivom &lt;UppercaseThumbprint&gt;.crt za na X509 datoteka certifikata i &lt;UppercaseThumbprint&gt;.prv za privatni ključ.
Obje se datoteke su .pem oblikovani.

Aplikacija obično pronalazi certifikata pomoću na otisak prsta i ne morate izmjene.

## <a name="retiring-certificates"></a>Povlačenje certifikata


U prethodnom odjeljku smo prikazivao kako automatske novi certifikat za svoje postojeće VMs. No stare certifikata još uvijek u VM i nije moguće ukloniti. Radi dodatne sigurnosti možete promijeniti atribut za stari tajna "Onemogućeno", pa čak i ako se pokuša stari predložak da biste stvorili na VM staru verziju certifikata, ona će. Evo kako postaviti određenu verziju tajnu do onemogućivanja:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Zaključak


S nov način certifikat može biti zadržane neovisan VM slike ili opseg aplikacije. Tako ćemo ste uklonili jednu točku izlaganje.

Certifikat možete obnoviti i prenijeli ključ sigurnog bez potrebe za ponovno stvaranje VM slike ili paketa za implementaciju aplikacije. Aplikacija i dalje se mora da biste dobili uz nove URI za ovu novu verziju certifikat kroz.

Tako što certifikata na VM ili opseg aplikacije, ne možemo imati sada smanjene broj osoblja koji će imati izravan pristup certifikata.

Kao dodatnu pogodnost, sada imate jednom mjestu u sigurnog ključ za upravljanje Svi certifikati, uključujući za sve verzije koje su uvedene tijekom vremena.

## <a name="next-steps"></a>Daljnji koraci

[Implementacija VM lozinkom sigurnog ključ](azure-stack-kv-deploy-vm-with-secret.md)

[Dopustite programu da biste pristupili sigurnog ključ](azure-stack-kv-sample-app.md)