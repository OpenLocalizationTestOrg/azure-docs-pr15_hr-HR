<properties
    pageTitle="Rješavanje problema s povezivanjem u SSH da biste na VM | Microsoft Azure"
    description="Upute za otklanjanje poteškoća kao što su 'SSH veze nije uspjela"ili" SSH odbio ' za programa Azure VM izvodi Linux."
    keywords="ssh veze odbio, ssh pogreška, azure ssh, SSH nije uspjelo povezivanje"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Otklanjanje poteškoća s SSH vezama na VM Linux za Azure koji ne uspije, pogreške, ili je odbio
Postoje različite uzroke koje ćete naići na pogreške sigurne ljuske (SSH), SSH pogrešaka veze, ili pak SSH je odbio kada pokušate povezati Linux virtualnog računala (VM). U ovom se članku olakšava traženje i ispravite probleme. Azure portalu Azure EŽA ili VM pristup proširenja za Linux omogućuju pronalaženje i otklanjanje problema s povezivanjem.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i stoga prelijevanje forume](http://azure.microsoft.com/support/forums/). Osim toga, možete arhivirati incident Azure podršci. Idite na [Azure podržava web-mjesta](http://azure.microsoft.com/support/options/) i odaberite **Pomoć**. Informacije o korištenju Azure podržava, pročitajte [Microsoft Azure podržava najčešća Pitanja](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Brzi koraci za otklanjanje poteškoća
Nakon svakog koraka za otklanjanje poteškoća, pokušajte ponovno s VM.

1. Ponovno postavljanje SSH konfiguracije.
2. Ponovno postavljanje vjerodajnica za korisnika.
3. Provjerite je li pravila [Mrežom sigurnosnoj grupi](../virtual-network/virtual-networks-nsg.md) dopušta promet SSH.
    - Provjerite postoji li pravilo mreže sigurnosnoj grupi dopušta promet SSH (po zadanom, TCP priključak 22).
    - Ne možete koristiti priključak preusmjeravanje / mapiranja bez korištenja Azure opterećenja.
4. Provjera [stanja VM resursa](../resource-health/resource-health-overview.md). 
    - Provjerite je li da se VM izvješća kao dobar.
    - Ako imate omogućen Dijagnostika za pokretanje, provjerite je li u VM je izvješćivanje o pogreškama pokretanje u zapisnicima.
5. Ponovno pokrenite na VM.
6. Ponovno implementirate u VM.

Čitanje za detaljnije upute za otklanjanje poteškoća i objašnjenja i dalje.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Dostupni načini za rješavanje problema s povezivanjem SSH

Možete ponovno postaviti vjerodajnice ili SSH konfiguraciju na jedan od sljedećih načina:

- [Portal za azure](#using-the-azure-portal) - odlične Ako morate brzo ponovno postavljanje konfiguracije SSH ili SSH ključ i nemate instalirane alate za Azure.
- [Naredbe azure EŽA](#using-the-azure-cli) – ako ste već u naredbenom retku brzo ponovno postaviti SSH konfiguraciju ili vjerodajnice.
- [Proširenje azure VMAccessForLinux](#using-the-vmaccess-extension) – stvaranje i ponovno korištenje json definicija datoteke da biste ponovno postavili SSH konfiguraciju ili korisnik vjerodajnice.

Nakon svakog koraka za otklanjanje poteškoća, pokušajte ponovno povezati svoje VM. Ako se i dalje ne možete povezati, pokušajte sa sljedećim korakom.


## <a name="using-the-azure-portal"></a>Pomoću portala za Azure
Portal za Azure omogućuje brz način da biste ponovno postavili SSH konfiguraciju ili korisnik vjerodajnice bez instaliranja bilo koji alat na lokalnom računalu.

Odaberite vaše VM na portalu za Azure. Pomaknite se prema dolje do odjeljka **podršku + otklanjanje poteškoća** i odaberite **ponovno postavljanje lozinke** kao u sljedećem primjeru:

![Ponovno postavljanje konfiguracije SSH ili vjerodajnice na portalu za Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Ponovno postavljanje konfiguracije SSH
U prvom koraku odaberite `Reset SSH configuration only` s padajućeg izbornika **način** kao u prethodnom snimku zaslona, zatim kliknite gumb **Vrati izvorno** . Kada ovu akciju završi, pokušajte ponovno pristupili vaše VM.

### <a name="reset-ssh-credentials-for-a-user"></a>Ponovno postavljanje vjerodajnica SSH za korisnika
Da biste vratili vjerodajnice za postojećeg korisnika, odaberite nešto od sljedećeg `Reset SSH public key` ili `Reset password` s padajućeg izbornika **način** kao u prethodnom snimku zaslona. Navedite korisničko ime i SSH ključ ili novu lozinku, a zatim kliknite gumb **Vrati izvorno** .

Možete stvoriti i korisnik s ovlastima sudo na VM s tog izbornika. Unesite novi korisničko ime i lozinka povezani ili SSH ključ, a zatim kliknite gumb **Vrati izvorno** .


## <a name="using-the-azure-cli"></a>Korištenje Azure EŽA
Ako to već niste učinili, [instalirajte EŽA Azure i povezati Azure pretplatu](../xplat-cli-install.md). Provjerite koristite li način upravljanja resursima na sljedeći način:

```
azure config mode arm
```

Ako ste stvorili i prenijeli prilagođenu sliku Linux disk, provjerite je li [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) verziju 2.0.5 ili je noviju verziju. Za VMs stvoren pomoću galerije slika, ovaj pristup kućni broj već instaliran i konfiguriran za vas.

### <a name="reset-ssh-configuration"></a>Ponovno postavljanje SSH konfiguracija
Konfiguracija SSHD sam je možda pogrešno ili servis naišao na pogrešku. Možete ponovno postaviti SSHD da biste bili sigurni konfiguracija SSH sam nije valjan. Vraćanje SSHD mora biti snimljenih prvog koraka otklanjanja poteškoća.

Sljedeći primjer vraća SSHD na VM pod nazivom `myVM` u grupu resursa pod nazivom `myResourceGroup`. Korištenje vlastite nazive grupa VM i resursa na sljedeći način:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Ponovno postavljanje vjerodajnica SSH za korisnika
Ako SSHD se čini da pravilno funkcionirati, možete ponovno postaviti lozinke korisnika giver. Sljedeći primjer vraća vjerodajnice za `myUsername` vrijednosti navedene u `myPassword`, na VM pod nazivom `myVM` u `myResourceGroup`. Pomoću vlastitih vrijednosti na sljedeći način:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Ako koristite ključ za provjeru autentičnosti SSH, možete ponovno postaviti SSH ključ za dani korisnika. U sljedećem primjeru ažurira tipku SSH pohranjene u `~/.ssh/azure_id_rsa.pub` za korisnika pod nazivom `myUsername`, na VM pod nazivom `myVM` u `myResourceGroup`. Pomoću vlastitih vrijednosti na sljedeći način:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>Korištenje nastavka VMAccess
Nastavak VM programa Access za Linux čita u datoteci json koji definira akcije da biste izveli. Ove se radnje obuhvaćaju ponovnom SSHD, ponovno postavljanje ključa SSH ili dodavanje korisnika. I dalje koristiti EŽA Azure da biste nazvali proširenje VMAccess, ali možete ponovno koristiti datoteke json preko više VMs po želji. Taj se način omogućuje stvaranje spremišta json datoteka koje se može zatim pozvati za navedeni scenarija.

### <a name="reset-sshd"></a>Ponovno postavljanje SSHD
Stvaranje datoteku pod nazivom `PrivateConf.json` pomoću sljedećeg sadržaja:

```bash
{  
    "reset_ssh":"True"
}
```

Korištenje EŽA Azure, zatim pozovite na `VMAccessForLinux` nastavak da biste ponovno postavili vezu SSHD navođenjem json datoteku. Sljedeći primjer vraća SSHD na VM pod nazivom `myVM` u `myResourceGroup`. Pomoću vlastitih vrijednosti na sljedeći način:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Ponovno postavljanje vjerodajnica SSH za korisnika
Ako se pojavi SSHD ispravni, možete ponovno postaviti vjerodajnice za giver korisnika. Da biste ponovno postavili lozinku za korisnika, stvorite datoteku pod nazivom `PrivateConf.json`. Sljedeći primjer vraća vjerodajnice za `myUsername` vrijednosti navedene u `myPassword`. Unesite sljedeće retke u vašem `PrivateConf.json` datoteku pomoću vlastitih vrijednosti:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Ili da biste vratili SSH ključ za korisnika, najprije stvorite datoteku pod nazivom `PrivateConf.json`. Sljedeći primjer vraća vjerodajnice za `myUsername` vrijednosti navedene u `myPassword`, na VM pod nazivom `myVM` u `myResourceGroup`. Unesite sljedeće retke u vašem `PrivateConf.json` datoteku pomoću vlastitih vrijednosti:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Nakon stvaranja datoteke json, koristite EŽA Azure da biste pozvali na `VMAccessForLinux` proširenje za ponovno postavljanje korisničke vjerodajnice za SSH navođenjem json datoteku. Sljedeći primjer vraća vjerodajnice na VM pod nazivom `myVM` u `myResourceGroup`. Pomoću vlastitih vrijednosti na sljedeći način:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Ponovno pokrenite na VM
Ako imate SSH konfiguraciju i korisničke vjerodajnice za ponovno postavljanje ili je naišao na pogrešku tako, mogu pokušajte ponovno VM adresu podlozi računalnim problema.

### <a name="azure-portal"></a>Portal za Azure
Da biste ponovno pokrenuli VM pomoću portala za Azure, odaberite svoje VM i kliknite u ***ponovno pokrenite** gumb kao u sljedećem primjeru:

![Ponovno pokrenite na VM na portalu za Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure EŽA
U sljedećem primjeru pokreće VM pod nazivom `myVM` u grupu resursa pod nazivom `myResourceGroup`. Pomoću vlastitih vrijednosti na sljedeći način:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Ponovno implementirate u VM
Možete implementirati VM na drugi čvor unutar Azure, riješiti probleme podlozi umrežavanje. Informacije o redeploying na VM potražite u članku [ponovno implementirate virtualnog računala da biste novi Azure čvor](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Po završetku ovaj postupak efemerni disk podataka bit će izgubljeni i ažurirat će se dinamički IP adrese koje su vezane uz virtualnog računala.

### <a name="azure-portal"></a>Portal za Azure
Da biste ponovno implementirate VM pomoću portala za Azure, odaberite VM i pomaknite se prema dolje do odjeljka **podršku + otklanjanje poteškoća** . Kliknite gumb **ponovno implementirate** kao u sljedećem primjeru:

![Ponovno implementirate VM na portalu za Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure EŽA
U sljedećem primjeru redeploys VM pod nazivom `myVM` u grupu resursa pod nazivom `myResourceGroup`. Pomoću vlastitih vrijednosti na sljedeći način:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>VMs stvoren pomoću model implementacije Classic

Isprobajte sljedeće korake da biste riješili najčešćih pogrešaka veze SSH za VMs koje su stvorene pomoću klasične implementacije modela. Nakon svakog koraka, pokušajte ponovno s VM.

- Ponovno postavljanje daljinski pristup s [portala za Azure](https://portal.azure.com). Na portalu Azure odaberite vaše VM, a zatim kliknite gumb **Vrati udaljene...** .

- Ponovno pokrenite na VM. [Portal za Azure](https://portal.azure.com)odaberite vaše VM, a zatim kliknite gumb **ponovno pokrenite** .

    – ILI –

    [Azure klasični portala](https://manage.windowsazure.com)odaberite **virtualnim strojevima** > **instance** > **ponovno pokrenite**.

- Ponovno implementirate VM novi Azure čvor. Informacije o kako implementirati na VM potražite u članku [ponovno implementirate virtualnog računala da biste novi Azure čvor](virtual-machines-windows-redeploy-to-new-node.md).

    Po završetku ovaj postupak efemerni disk podataka bit će izgubljeni i ažurirat će se dinamički IP adrese koje su vezane uz virtualnog računala.

- Slijedite upute iz članka [vraćanje lozinke ili SSH za sustavom Linux virtualnim strojevima](virtual-machines-linux-classic-reset-access.md) :
    - Ponovno postavljanje lozinke ili SSH ključ.
    - Stvorite _sudo_ korisnički račun.
    - Ponovno postavljanje SSH konfiguracije.

- Provjera stanja resursa u VM probleme platforme.<br>
  Odaberite VM i pomaknite se prema dolje **Postavke** > **Provjera stanja**.


## <a name="additional-resources"></a>Dodatni resursi

- Ako i dalje ne možete SSH za vaše VM nakon nakon korake u nastavku, potražite u članku [detaljnije korake za otklanjanje poteškoća](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) da biste pregledali dodatne korake da biste riješili problem.

- Dodatne informacije o otklanjanju poteškoća s aplikaciju programa access potražite u članku [Otklanjanje poteškoća s pristupom aplikacije koje se izvode na Azure virtualnog računala](virtual-machines-linux-troubleshoot-app-connection.md)

- Dodatne informacije o otklanjanju poteškoća s virtualnim strojevima koje su stvorene pomoću klasične implementacije modela potražite [u](virtual-machines-linux-classic-reset-access.md)članku ponovno postavljanje lozinke ili SSH za virtualnim strojevima sustavom Linux.
