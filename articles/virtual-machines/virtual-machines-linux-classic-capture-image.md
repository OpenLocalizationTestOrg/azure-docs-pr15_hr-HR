<properties
    pageTitle="Snimite sliku sadržaja Linux VM | Microsoft Azure"
    description="Saznajte kako snimiti sustavom Linux Azure virtualni stroj (VM) stvorene pomoću model klasični implementacije."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Kako snimiti klasični virtualnog računala Linux kao sliku

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](virtual-machines-linux-capture-image.md).

U ovom se članku objašnjava da biste snimili na klasični Azure virtualne računalo sa sustavom Linux kao sliku da biste stvorili druge virtualnih računala. Ova slika sadrži OS disku i podataka diskova priložiti virtualnog računala. Mrežna konfiguracija ne sadrži tako da morate konfigurirati koje stvorite na virtualnim strojevima iz slike.

Azure pohranjuje na slici u odjeljku **slike**, uz sve slike koje ste prenijeli. Dodatne informacije o slikama potražite u članku [O virtualnog računala slike u Azure] [].

## <a name="before-you-begin"></a>Prije početka

Ove se pretpostavlja da ste već stvorili Azure virtualnog računala pomoću modela implementacije klasični i konfigurirali operacijskom sustavu, uključujući prilaganja diskova sve podatke. Ako vam je potrebna za stvaranje na VM, Saznajte [kako stvoriti Linux virtualnog računala] [].


## <a name="capture-the-virtual-machine"></a>Snimite virtualnog računala

1. [Povezivanje s virtualnog računala](virtual-machines-linux-mac-create-ssh-keys.md) pomoću klijentskog programa SSH po izboru.

2. U prozoru SSH upišite sljedeću naredbu. Izlaz iz `waagent` mogu malo razlikovati ovisno o verziji uslužni:

    `sudo waagent -deprovision+user`

    Ta se naredba pokušava očistiti sustav i provjerite prikladna za reprovisioning. Ovaj postupak izvršava sljedeće zadatke:

    - Uklanja tipke SSH glavnog računala (Ako je Provisioning.RegenerateSshHostKeyPair y u konfiguracijskoj datoteci)
    - Čisti konfiguraciju poslužitelja naziva u /etc/resolv.conf
    - Uklanja na `root` korisničku lozinku iz/itd/sjena (Ako Provisioning.DeleteRootPassword "y" u konfiguracijskoj datoteci)
    - Uklanja predmemoriranog zakupa DHCP klijent poslužitelja
    - Naziv glavnog računala na izvorne vrijednosti primjenjuje na localhost.localdomain
    - Brisanje zadnje dodijeljenu korisnički račun (dobivenog od /var/lib/waagent) **i pridruženih podataka**.

    >[AZURE.NOTE] Deprovisioning briše datoteke i podatke za "generalize" slike. Samo se izvoditi tu naredbu na virtualnog računala koje namjeravate snimiti kao novi predložak slike. Ne jamči da slike isključen sve povjerljivih podataka ili je prikladna radi ponovne distribucije trećim stranama.


3. Upišite **y** da biste nastavili. Možete dodati u `-force` parametar da biste izbjegli taj korak za potvrdu.

4. Upišite **Izlaz** da biste zatvorili SSH klijenta.

    >[AZURE.NOTE] Preostale korake pretpostavlja da već imate [instaliran EŽA Azure](../xplat-cli-install.md) na klijentskom računalu. Sljedeći koraci mogu se izvršiti [Azure klasični portal] [].

5. Na klijentskom računalu otvorite EŽA Azure i prijavite se u pretplatu za Azure. Dodatne informacije pročitajte [za povezivanje s Azure pretplate s EŽA Azure](../xplat-cli-connect.md).

6. Provjerite jeste li u načinu rada za upravljanje servisom:

    `azure config mode asm`

7. Isključivanje virtualnog računala koja je već pretplati su uklonjeni resursi u prethodnim koracima sa:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Pronaći sve virtualnim strojevima stvoriti u vašoj pretplati pomoću`azure vm list`

8. Kada se Zaustavi virtualnog računala, snimiti pomoću naredbe:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Upišite naziv slike koje želite umjesto _nove slike-ime_. Ta naredba stvara generalizirano OS slike. Na `-t` podnaredba briše izvornu virtualnog računala.

9.  Sada je dostupan na popisu slike koje se mogu koristiti za konfiguriranje nove virtualnim strojevima novu sliku. Prikaz ga pomoću naredbe:

    `azure vm image list`

    [Azure klasični portal] []pojavljuje se na popisu **SLIKE** .

    ![Slika snimanje uspješno](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Daljnji koraci
Sliku je moguće koristiti za stvaranje virtualnim računalima sustava. Možete koristiti naredbu Azure EŽA `azure vm create` Navedite naziv slike koju ste stvorili. Pogledajte odjeljak [Korištenje EŽA Azure s modelom implementacije klasični](../virtual-machines-command-line-tools.md) detalje o naredbu. Umjesto toga koristite [Azure klasični portal] [] za stvaranje prilagođene virtualnog računala metodom **Iz galerije** i odabirom slike koju ste stvorili. Dodatne informacije potražite u članku [kako stvoriti prilagođeni virtualnog računala] [] .

**Vidi također:** [Agent za Azure Linux korisničkom priručniku](virtual-machines-linux-agent-user-guide.md)

[Azure klasični portal]: http://manage.windowsazure.com
[O slikama virtualnog računala u Azure]: virtual-machines-linux-classic-about-images.md
[Kako stvoriti prilagođene virtualnog računala]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Kako stvoriti Linux virtualnog računala]: virtual-machines-linux-classic-create-custom.md
