<properties
    pageTitle="Vraćanje ključ sigurnog ključ i tajna za šifriranu VMs korištenju sigurnosnih kopija Azure | Microsoft Azure"
    description="Saznajte kako vratiti ključ sigurnog ključ i tajna u sigurnosne kopije Azure pomoću komponente PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Vraćanje ključ sigurnog ključ i tajna za šifriranu VMs korištenju sigurnosnih kopija Azure
U ovom se članku govori o korištenju programa Azure VM Backup da biste izvršili vraćanje šifrirane VMs Azure, ako ključ i tajna ne postoji u ključa zbirke ključeva. Ove korake i može se koristiti ako želite zadržati svoj primjerak ključ (ključ ključa za šifriranje) i tajna (BitLocker ključa za šifriranje) za vraćene VM.

## <a name="pre-requisites"></a>Prije requisites

1. **Sigurnosno kopiranje šifrirane VMs** - šifrirane Azure VMs sigurnosno kopirane pomoću Azure sigurnosnu kopiju. Potražite u članku [Upravljanje sigurnosnog kopiranja i vraćanja VMs Azure pomoću komponente PowerShell](backup-azure-vms-automation.md) detalje o sigurnosno kopiranje šifrirane Azure VMs.

2. **Konfiguriranje sigurnog Azure ključ** – Provjerite ključ sigurnog kojoj tipke i tajne moraju biti vraćena je već postoji. Potražite u članku [Početak rada s Azure ključ sigurnog](../key-vault/key-vault-get-started.md) detalje o upravljanju ključa zbirke ključeva.

## <a name="setup-recovery-services-vault"></a>Postavljanje oporavak servisa sigurnog 
Poduzmite sljedeće korake da se prijavi PowerShell i postavljanje oporavak servisa sigurnog kontekst

### <a name="log-in-to-azure-powershell"></a>Prijavite se u sustav Azure PowerShell 

Prijavite se na račun za Azure pomoću sljedeći cmdlet

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Postavljanje oporavak servisa sigurnog kontekst

Kada prijavljeni, koristite sljedeći cmdlet da biste dobili popis dostupnih pretplate

```
PS C:\> Get-AzureRmSubscription
```

Odaberite pretplatu u kojem su resursi dostupni

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Postavljanje konteksta sigurnog korištenja oporavak servisa sigurnog kojima je omogućeno sigurnosne kopije za šifriranu VMs

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Početak točka vraćanja 

Odaberite kontejner u zbirke ključeva koji predstavlja šifrirane Azure virtualnog računala

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Koristite ovaj spremnik, vratiti stavku za odgovarajuće virtualnog računala prema gore

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Početak polja točaka oporavak sigurnosne kopije odabrane stavke u varijable to

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Vraćanje šifrirane virtualnog računala
Poduzmite sljedeće korake da biste vratili šifrirane VM njegove ključ i tajna.

### <a name="restore-key"></a>Vraćanje ključ

Polja $rp gore navedena su sortirani obrnutim redoslijedom vremena s najnovijim oporavak točku u indeks 0. Na primjer: $rp [0] odabire najnovije točka vraćanja.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Kada ovaj cmdlet uspješno pokrene, blob datoteka dobiva generira u određenu mapu na računalu u kojem se pokreće. Datoteka blob predstavlja ključ šifrirane šifrirane obrascu.

Vraćanje ključa natrag ključa sigurnog korištenja sljedeći cmdlet. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Vraćanje tajna

Vraćanje tajnu podataka iz točka vraćanja nabavili iznad

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Tekst ispred vault.azure.net predstavlja izvorni naziv ključa zbirke ključeva. Tekst iza tajne / predstavlja tajnu naziv. 

Pronađite tajnu naziv i vrijednost iz izlaza cmdlet pokrenuti iznad, u slučaju da želite koristiti isti naziv tajnu. U drugim slučajevima $secretname ispod mora se ažurirati koriste novi naziv tajnu. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Postavljanje oznake za tajna, u slučaju VM treba kao i vratiti. Za oznaku DiskEncryptionKeyFileName vrijednost mora sadržavati naziv tajna namjeravate koristiti. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Vrijednost za DiskEncryptionKeyFileName je isto kao tajnu ime nabavili iznad. Vrijednost za DiskEncryptionKeyEncryptionKeyURL biti dobivenog od ključne sigurnog nakon Vraćanje natrag ključeve i pomoću cmdleta [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx)   

Postavljanje tajnu natrag da biste ključa zbirke ključeva

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Vraćanje virtualnog računala
Iznad cmdleta ljuske PowerShell vam vraćanje ključ i tajnu natrag ključa sigurnog ako ste sigurnosnu kopiju šifrirane VM korištenju sigurnosnih kopija VM Azure. Nakon vraćanju, potražite u članku [Upravljanje sigurnosnog kopiranja i vraćanja VMs Azure pomoću komponente PowerShell](backup-azure-vms-automation.md) da biste vratili šifrirane VMs.
