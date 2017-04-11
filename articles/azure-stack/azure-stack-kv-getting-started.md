<properties
    pageTitle="Uvod u Azure stogu ključa sigurnog | Microsoft Azure"
    description="Prvi koraci pri korištenju Azure stogu ključ sigurnog"
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
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Uvod u sigurnog ključ

U ovom se odjeljku opisuju koraci koje morate stvarati u sigurnog, upravljanje tipke i tajne kao i autorizirali korisnika ili aplikacije za pozivanje postupke u sigurnog u stogu Azure. Sljedeći koraci pretpostavlja postoji pretplatu klijenta i KeyVault servis je registrirana unutar tu pretplatu. Sve naredbe primjer temelje se na cmdleta KeyVault dostupno u sklopu Azure SDK PowerShell.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Omogućivanje pretplate klijenta za sigurnog operacije 

Prije nego što možete izdati operacija na temelju bilo koje sigurnog, morate provjerite je li vaša pretplata omogućen za sigurnog operacije. Provjerite koji slanjem sljedeću naredbu komponente PowerShell:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Izlaz iz gornju naredbu treba izvješće "Registrirani" za stanje "Registracija" za svaki redak.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Ako to nije slučaj, trebali biste pozivanje sljedeću naredbu da biste registrirali servis KeyVault unutar pretplate:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

I u sljedeće rezultatu naredbe:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Ako nailazite na pogrešku: "*pretplate nije registriran Azure ključ sigurnog*" pozivanje cmdleta KeyVault potvrdite koje ste omogućili davatelja resursa KeyVault po gore navedene upute.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Stvaranje kontejnera hardened (sigurnog) u stogu Azure za pohranu i upravljanje šifriranja tipke i tajne

Da biste stvorili na sigurnog, klijentom morate stvoriti grupu resursa. Sljedeće naredbe ljuske PowerShell Stvorite grupu resursa, a zatim na sigurnog u toj grupi resursa. Primjer uključuje i standardne Izlaz iz tog cmdleta.

### <a name="creating-a-resource-group"></a>Stvaranje grupe resursa:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Izlaz:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Stvaranje na sigurnog:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Izlaz:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Izlaz iz ovaj cmdlet prikazuje svojstva zbirke ključeva ključa koji ste upravo stvorili. Dva najvažnija svojstva su:

-   **Naziv zbirke ključeva**: U ovom primjeru to je **vault010**. Koristit ćete naziv cmdleti za drugi ključ sigurnog.

-   **URI sigurnog**: U ovom primjeru to je https://vault010.vault.azurestack.local. Aplikacije koje koriste vaš sigurnog do njegova REST API-JA morate koristiti ovaj URI.

Račun za Azure sada ovlašteni za izvođenje operacije na ovom ključa sigurnog. Što još nitko drugi je.


## <a name="operating-on-keys-and-secrets"></a>Na ključeva i tajne

Kada stvorite na zbirke ključeva, slijedite na ispod korake da biste stvorili upravljanje tipke i tajne:

### <a name="creating-a-key"></a>Stvaranje ključa

Da biste stvorili ključ, koristite **Dodaj AzureKeyVaultKey** po primjeru u nastavku. Nakon uspješne stvaranje ključa cmdlet će izlaz novostvorenu ključne detalje.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Izlaz iz cmdlet za *Dodavanje AzureKeyVaultKey* je:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Sada možete referencirati ovaj ključ koji ste stvorili ili prenijeli sigurnog ključ Azure, pomoću njegov URI. Pomoću **tipke/https://vault010.vault.azurestack.local:443/keyVaultKeyName001** uvijek preuzeli trenutnu verziju; i **https://vault010.vault.azurestack.local:443/tipke/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** da biste pristupili slijedite ovaj određenu verziju.

### <a name="retrieving-a-key"></a>Dohvaćanje ključa

**Get-AzureKeyVaultKey** možete koristiti za dohvat ključem i detalja u sljedećem primjeru:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Slijedi izlaz Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Postavljanje programa tajna

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Izlaz

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Dohvaćanje na tajna

    Get-AzureKeyVaultSecret -VaultName vault010

Izlaz

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Sada ključa sigurnog i ključ ili tajna spremna je za korištenje aplikacijama.
Morate Autorizirajte aplikacije da biste ih koristiti.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorizirajte aplikaciju koju želite koristiti ključ ili tajna 

Da biste autorizirali aplikacije da biste pristupili ključ ili tajna u na sigurnog, koristite cmdlet skup -**AzureRmKeyVaultAccessPolicy** .

Na primjer, ako se zovete sigurnog *ContosoKeyVault* i aplikaciju koju želite autorizirali sadrži *ID klijenta* *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*, a želite da biste autorizirali aplikacija kako bi dešifrirali i prijavite se pomoću tipki u vašem sigurnog, pokrenite sljedeću naredbu:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Ako želite da biste autorizirali tu istu aplikaciju da biste pročitali tajne u vaše zbirke ključeva pokrenite sljedeću naredbu:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Daljnji koraci
[Implementacija VM lozinkom sigurnog ključ](azure-stack-kv-deploy-vm-with-secret.md)

[Implementacija VM certifikatom sigurnog ključ](azure-stack-kv-push-secret-into-vm.md)