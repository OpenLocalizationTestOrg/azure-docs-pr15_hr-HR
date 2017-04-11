<properties
   pageTitle="Pristup VM ID-a"
   description="U članku se opisuje pristupanje i korištenje Azure VM jedinstveni ID"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Pristupanje i korištenje Azure VM jedinstveni ID

Azure VM jedinstveni ID je kodirana identifikatora 128bits pohranjene u SMBIOS sve Azure IaaS VM, i trenutno je moguće čitati pomoću naredbi BIOS platforme.

Azure VM jedinstveni ID je svojstvo samo za čitanje. Azure jedinstveni ID VM neće se promijeniti nakon zatvaranja nakon ponovnog pokretanja (ili u planu za neplanirano), početak i kraj Poništi dodjelu, servisa automatskog popravka ili vraćanje na mjestu. Međutim, ako u VM snimke stanja i kopirali da biste stvorili novu instancu, novi ID VM Azure konfiguriran.

> [AZURE.NOTE] Ako imate starije VMs stvorili i pokrenut jer je nova značajka imate poslednjeg out (rujan 18 2014.), provjerite ponovno pokretanje sustava VM za automatsko preuzimanje jedinstveni programa Azure ID-a.


Da biste pristupili Azure jedinstveni VM ID iz unutar na VM:


## <a name="create-a-vm"></a>Stvaranje na VM
 

Dodatne informacije potražite u članku [Stvaranje virtualnog računala](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Povezivanje s VM
 

Dodatne informacije potražite u članku [SSH iz Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Upit VM jedinstveni ID

Naredba (primjer koristi **Ubuntu**):

    sudo dmidecode | grep UUID
    
Primjer očekivani rezultati:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Zbog velike Endian bita redoslijed, stvarni jedinstveni ID VM u tom slučaju neće biti:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure VM jedinstveni ID može se koristiti u različitim scenarijima bez obzira na VM radi Azure ili lokalni i omogućuju licenciranja, izvješćivanje i Općenito praćenje potrebama možda je na vašem Azure IaaS implementacijama. Broj nezavisnih proizvođači stvaranje aplikacija i certificiranje ih na Azure biti potrebna za prepoznavanje programa Azure VM tijekom njegova životnog ciklusa i utvrditi je li u VM pokrenut na Azure, lokalnog ili drugih davatelja oblaka. Ovaj identifikator platformu na primjer može pridonijeti otkrije ako pravilno licencirani softver ili omogućuje povezivanje VM podatke za izvor kao što su kao pomoć o postavljanju desnom metriku odgovarajuću platformu i za praćenje i povezivanje te metriku navedenih druge korisnike.
