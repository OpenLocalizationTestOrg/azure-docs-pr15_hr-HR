<properties
    pageTitle="Najčešća pitanja o snopu Azure | Microsoft Azure"
    description="Azure stogu najčešća pitanja."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Najčešća pitanja o snopu Azure

## <a name="deployment"></a>Uvođenje

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Potrebne da biste oblikovali moje podatke diskova prije pokretanja ili pokrenite instalaciju?

Diskova mora biti u obliku neobrađenog. Ako instalirate operacijski sustav, možda ćete morati potvrdite ako je i dalje postoji stari skup prostora za pohranu i izbrisati na sljedeći način:

1. Otvorite upravitelj poslužitelja.
2. Odaberite grupe za pohranu.
3. Potražite u članku nalazi li se grupe aplikacija za pohranu.
4. Desnom tipkom miša kliknite **grupu za pohranu** ako na popisu i Omogući čitanje / pisanje.
5. Desnom tipkom miša kliknite **virtualne na tvrdom disku** (donjem lijevom kutu) i odaberite Izbriši.
6. Desnom tipkom miša kliknite **Grupu za pohranu** , a zatim kliknite Izbriši.
7. Ponovno pokrenite Azure stogu skripte i provjerite je li prosljeđuje potvrdu disk.

Po želji sljedeću skriptu mogu koristiti:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Mogu li koristiti sve SSD diskova za skup prostora za pohranu u instalaciji PNA?

Tu konfiguraciju nije podržan u ovom izdanju.  Dodatne informacije potražite u članku [vodič preduvjeti](azure-stack-deploy.md) za dodatne informacije.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Mogu li koristiti diskova NVMe podataka za PNA snop za Microsoft Azure?

Dok je prostora za pohranu razmake Izravni podržava NVMe diskova, stoga Azure podržava samo podskup vrste mogućih pogon i moguće kombinacije za pohranu razmake izravno. 

### <a name="how-can-i-reinstall-azure-stack"></a>Kako instalirajte stogu Azure?
Slijedite korake u [Vodič za ponovno uvođenje](azure-stack-redeploy.md).  

## <a name="tenant"></a>Klijent

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Možete implementirati vlastitu slike kao klijenta?

Da, baš kao i u Azure, klijenta možete prenijeti slike u stogu Azure uz korištenje slike s administrator servisa. Pregled, potražite u članku [Dodavanje VM slike](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testiranje

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Mogu li koristiti ugniježđene funkcije Virtualizacija za testiranje sustava Microsoft Azure stogu PNA?

Ugniježđene Virtualizacija nije podržana ili s Azure stogu Tehnički pretpregled 2.

## <a name="virtual-machines"></a>Virtualnim strojevima

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Podržava li Azure stogu dinamičkih diskova za virtualnih računala?

Azure stoga ne podržava dinamičkih diskova.

## <a name="next-steps"></a>Daljnji koraci

[Otklanjanje poteškoća](azure-stack-troubleshooting.md)
