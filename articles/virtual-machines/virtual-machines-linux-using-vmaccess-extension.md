<properties
    pageTitle="Ponovno postavljanje pristupa za Azure Linux VMs korištenje nastavka VMAccess | Microsoft Azure"
    description="Ponovno postavljanje pristupa za Azure Linux VMs korištenje nastavka VMAccess."
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
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Upravljanje korisnicima, SSH i potvrdite ili popravak diskova na Azure Linux VMs korištenje nastavka VMAccess

U ovom se članku objašnjava pomoću VMAcesss proširenje Azure provjerite ili popravite na disku, ponovno postavljanje korisničkog pristupa, upravljati korisničkim računima ili ponovno postavljanje konfiguracije SSHD na Linux. U članku zahtijeva:

- Azure račun ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure EŽA](../xplat-cli-install.md) prijavili `azure login`.

- način za Azure Voditelj resursa Azure EŽA _mora biti u_ `azure config mode arm`.

## <a name="quick-commands"></a>Brzi naredbe

Da biste koristili VMAccess na vašem Linux VMs na dva načina:

- Korištenje EŽA Azure i obavezni parametri.
- Korištenje neobrađenog JSON datoteka koje VMAccess procesa, a zatim djelovanje na.

Naredba brzi sekcije smo namjeravate koristiti EŽA Azure `azure vm reset-access` način. U sljedećim primjerima naredba zamijenite vrijednosti koje sadrže "primjer" s vrijednostima iz vlastite okruženja.

## <a name="create-a-resource-group-and-linux-vm"></a>Stvaranje grupa resursa i Linux VM

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Stvaranje Debian VM

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Ponovno postavljanje lozinke korijen

Da biste ponovno postavili lozinku korijen:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>Vrati izvorne postavke SSH ključa

Da biste ponovno postavili SSH ključ koji nisu korijenski korisnika:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Stvaranje korisnika

Da biste stvorili korisnika:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Uklanjanje korisnika

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>Ponovno postavljanje SSHD

Da biste ponovno postavili konfiguracije SSHD:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Detaljni vodič

### <a name="vmaccess-defined"></a>Definirani VMAccess:

Disk na vašem VM Linux prikazuju pogreške. Nekako ponovno postavljanje lozinke korijenski za vaše VM Linux ili izbriše SSH privatni ključ. Ako je koji se dogodilo natrag u danima podatkovnog centra, trebali biste pogon postoji, a zatim otvorite KVM da biste na konzoli poslužitelja. Smatrati proširenje Azure VMAccess taj parametar KVM koji vam omogućuje pristup konzole za vraćanje pristup Linux ili održavanje razine disk.

Detaljni vodič smo namjeravate koristi dugi oblik VMAccess koji koristi neobrađenog JSON datoteke.  Te se datoteke VMAccess JSON naziva možete se i iz Azure predložaka.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Provjerite ili popravite disk Linux VM pomoću VMAccess

Pomoću VMAccess možete učiniti na fsck pokrenuti na disku u odjeljku vaše VM Linux.  Možete učiniti i provjeru diska i disk popravak pomoću programa VMAccess.

Da biste provjerili i zatim popravite disk pomoću sljedeće skripte VMAccess:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Izvršavanje skripti VMAccess pomoću:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Korištenje VMAccess za ponovno postavljanje korisničkog pristupa Linux

Ako ste izgubili pristup korijenski na vašem VM Linux, možete pokrenuti skriptu VMAccess za ponovno postavljanje lozinke korijenski.

Da biste ponovno postavili lozinku korijen, pomoću sljedeće skripte VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Izvršavanje skripti VMAccess pomoću:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Da biste vratili ključ SSH korisnika koji nisu korijen, pomoću sljedeće skripte VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Izvršavanje skripti VMAccess pomoću:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Upravljanje korisničkim računima na Linux pomoću VMAccess

VMAccess je Python skriptu koja se može se koristiti za upravljanje korisnicima na vašem VM Linux bez prijave u i pomoću sudo ili račun korijen.

Da biste stvorili korisnika, pomoću sljedeće skripte VMAccess:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Izvršavanje skripti VMAccess pomoću:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Da biste izbrisali korisnika, pomoću sljedeće skripte VMAccess:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Izvršavanje skripti VMAccess pomoću:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Korištenje VMAccess za vraćanje izvorne konfiguracije SSHD

Ako promjene konfiguracije Linux VMs SSHD i prekinuti vezu SSH prije potvrđivanja promjene, koji mogu biti onemogućeno SSH'ing ponovno prijavi.  VMAccess mogu se vratiti SSHD konfiguraciju u dobru konfiguraciju bez zapisuju u putem SSH.

Da biste ponovno postavili konfiguracije SSHD pomoću sljedeće skripte VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Izvršavanje skripti VMAccess pomoću:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Daljnji koraci

Ažuriranje Linux korištenje Azure VMAccess proširenja je jedan od načina promjene na tekući VM Linux.  Da biste izmijenili vaše VM Linux na pokretanje možete koristiti i alate kao što je oblak pokretanja i Azure predložaka.

[O proširenja virtualnog računala i značajkama](virtual-machines-linux-extensions-features.md)

[Voditelj resursa Azure omogućeno s datotečnim nastavcima Linux VM](virtual-machines-linux-extensions-authoring-templates.md)

[Da biste prilagodili Linux VM tijekom stvaranja pomoću oblaka pokretanja](virtual-machines-linux-using-cloud-init.md)
