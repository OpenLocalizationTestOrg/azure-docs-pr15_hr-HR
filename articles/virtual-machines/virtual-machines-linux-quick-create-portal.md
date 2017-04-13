<properties
    pageTitle="Stvaranje Linux VM pomoću portala za Azure | Microsoft Azure"
    description="Stvaranje Linux VM pomoću portala za Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Stvaranje Linux VM na Azure pomoću portala


U ovom se članku objašnjava korištenje [Azure portal](https://portal.azure.com/) za stvaranje Linux virtualnog računala.

Preduvjeti su:

- [račun za Azure](https://azure.microsoft.com/pricing/free-trial/)

- [SSH javnim i privatnim ključ datoteke](virtual-machines-linux-mac-create-ssh-keys.md)


1. Prijavljeni na portal sustava Azure pomoću vaš račun za Azure identitet, kliknite **+ Novo** u gornjem lijevom kutu:

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Kliknite **virtualnim strojevima** **Marketplace** , a zatim **Ubuntu Server 14.04 LTS** sa **Značajkama aplikacije** slika popisa.  Provjerite je li pri dnu model implementacije `Resource Manager` , a zatim kliknite **Stvori**.

    ![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Na stranici **Osnove** unesite:
    - Naziv na VM
    - korisničko ime korisnika za administratore
    - Vrsta provjere autentičnosti na **SSH javni ključ**
    - javni ključ SSH kao niz znakova (iz vaše `~/.ssh/` directory)
    - resurs naziv grupe ili odaberite postojeću grupu

    Kliknite **u redu** da biste nastavili i odaberite veličina VM; trebao bi izgledati otprilike ovako:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Odaberite veličinu **DS1** koji se instalira Ubuntu na Premium SSD, a zatim kliknite **Odaberite** da biste konfigurirali postavke.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. U odjeljku **Postavke**ostavite zadanih vrijednosti za pohranu i mrežne pa kliknite **u redu** da biste pogledali sažetak.  Obavijest o vrsti diska postavljen da Premium SSD tako da odaberete DS1, **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Potvrda postavki za svoje nove VM Ubuntu pa kliknite **u redu**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Otvorite Portal nadzornu ploču, a zatim u **sučelje mreže** odaberite vaša SLIKA

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Otvaranje izbornika javnu IP adrese u odjeljku postavke NIC

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH u javnu IP pomoću SSH javni ključ

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Daljnji koraci

Sada ste stvorili Linux VM brzo će se koristiti za testiranje ili pokazni svrhe. Da biste stvorili Linux VM prilagođene za vaše infrastrukture, bilo koji od ovih članaka možete pratiti.

- [Stvaranje Linux VM na Azure pomoću predložaka](virtual-machines-linux-cli-deploy-templates.md)
- [Stvaranje programa SSH osigurani Linux VM na Azure pomoću predložaka](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Stvaranje Linux VM pomoću EŽA Azure](virtual-machines-linux-create-cli-complete.md)
