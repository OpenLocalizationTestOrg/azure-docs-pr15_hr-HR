<properties
    pageTitle="ID klijenta ključa sigurnog promijeniti nakon pretplatu premještanje | Microsoft Azure"
    description="Saznajte kako se prebaciti ID klijenta za ključne sigurnog nakon premještanja pretplatu na drugi klijent"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Promjena ključa sigurnog klijentu ID-a nakon premještanje pretplatu
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>P: pretplate premješten iz klijenta odgovora klijent B. Kako promijeniti ID klijenta za moj postojeće ključne sigurnog i postavljanje točan ACL-a za upravitelji u klijentu B?

Prilikom stvaranja novog ključa sigurnog u pretplatu, automatski se uz zadani ID klijenta Azure Active Directory za tu pretplatu. Sve stavke pravila za pristup i uz ovaj ID klijenta Kada premještate pretplate Azure iz klijenta odgovora klijent B, vaše postojeće ključne sefovi se ne može pristupiti tako da upravitelje (korisnike i aplikacije) u klijentu B. Da biste riješili taj problem, morate:

- Promjena ID klijenta pridružena sve postojeće ključne sefovi u ovu pretplatu klijent B.
- Uklonite sve postojeće stavke pristup pravila.
- Dodavanje nove stavke pristup pravila koja se povezuju s klijentom B.

Ako, na primjer, ako imate ključ sigurnog 'myvault' u pretplatu za koju je premještena iz klijenta odgovora za klijenta B, Evo kako promijeniti ID klijenta za ovog ključa sigurnog i uklonili stare pravilnike za pristup.

<pre>
$vaultResourceId = (get-AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() skup AzureRmResource - ResourceId $vaultResourceId-svojstva $vault. Svojstva
</pre>

Jer ovo sigurnog u klijentu A prije prijenosa, izvornu vrijednost **$vault. Properties.TenantId** je A klijent, tijekom **(Get-AzureRmContext). Tenant.TenantId** je smjernice B.

Sad kad vaše zbirke ključeva vezan uz ID ispravni klijenta i uklanjaju se starih stavki pravila programa access, postavite novi pristup pravila stavke s [Skup AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Daljnji koraci

Ako imate pitanja o sigurnog ključ Azure, posjetite [Forume sigurnog ključ za Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
