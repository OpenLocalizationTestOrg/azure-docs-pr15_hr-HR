<properties
   pageTitle="Stvoriti i prenijeti sliku FreeBSD VM | Microsoft Azure"
   description="Naučite kako stvoriti i prenijeti virtualne tvrdog diska (VHD) koji sadrži FreeBSD operacijski sustav da biste stvorili Azure virtualnog računala"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Stvaranje i prijenos FreeBSD VHD Azure

U ovom se članku prikazuje kako stvoriti i prenijeti virtualne tvrdog diska (VHD) koji sadrži FreeBSD operacijski sustav. Kada prenesete, možete je koristiti kao vlastite slike za stvaranje virtualnog računala (VM) u Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da imate sljedeće stavke:

- **Pretplata programa Azure**– ako nemate račun, možete stvoriti jednu u samo nekoliko minuta. Ako imate pretplatu na MSDN, pročitajte članak [mjesečni Azure odobrenja za pretplatnike na Visual Studio.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). U suprotnom, Saznajte kako [stvoriti pomoću računa za probno razdoblje](https://azure.microsoft.com/pricing/free-trial/).  

- **Alati za azure PowerShell**– u Azure PowerShell modul mora biti instaliran i konfiguriran za korištenje Azure pretplatu. Da biste preuzeli modul, potražite u članku [Preuzimanje Azure](https://azure.microsoft.com/downloads/). Praktični vodič koji opisuje kako instalirati i konfigurirati modula dostupna je u nastavku. Pomoću cmdleta [Azure preuzimanje](https://azure.microsoft.com/downloads/) da biste prenijeli na VHD.

- **FreeBSD operacijskih sustava u datoteci .vhd**– je podržano FreeBSD operacijski sustav mora biti instaliran virtualne tvrdi disk. Postoji više alata za stvaranje .vhd datoteka. Ako, na primjer, možete koristiti virtualizacije rješenja kao što su Hyper-V da biste stvorili datoteku .vhd i instalirati operacijski sustav. Upute za instalaciju i korištenje Hyper-V, potražite u članku [Instalacija Hyper-V i stvaranje virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] U noviji oblik VHDX nije podržan u Azure. Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet [Pretvori vhd](https://technet.microsoft.com/library/hh848454.aspx). Uz to, postoji [vodič na MSDN-o tome kako koristiti FreeBSD s Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Ovaj zadatak obuhvaća sljedeće pet koraka.

## <a name="step-1-prepare-the-image-for-upload"></a>Korak 1: Priprema slike za prijenos

Na kojem je instaliran operacijski sustav FreeBSD virtualnog računala dovršite sljedeći postupak:

1. Omogućivanje DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Omogućivanje SSH.

    SSH omogućena je prema zadanim postavkama nakon instalacije s diska. Ako nije omogućena zbog nekog razloga ili ako koristite FreeBSD VHD izravno, upišite sljedeće:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Postavljanje konzole za serijski.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Instalirajte sudo.

    Račun za korijenske je onemogućen u Azure. To znači da morate koristiti sudo unprivileged korisnika za izvođenje naredbe s dodatnim ovlastima.

        # pkg install sudo
;
5. Preduvjeti za Azure Agent.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Instalirajte Agent za Azure.

    Najnovije izdanje Agent za Azure uvijek možete pronaći na [github](https://github.com/Azure/WALinuxAgent/releases). Verzija 2.0.10 + službeno podržava FreeBSD 10 i 10,1 i verzija 2.1.4 službeno podržava 10,2 FreeBSD i noviji izdanja.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Za 2.0, recimo pomoću 2.0.16 kao primjer:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Za 2.1, recimo pomoću 2.1.4 kao primjer:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Nakon što instalirate Agent za Azure, preporučuje se u da biste provjerili je li pokrenut:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision sustav.

    Deprovision sustava očistiti i provjerite prikladan za ponovno dodjeljivanje. Sljedeća naredba briše se i zadnji dodijeljenu korisnički račun i povezani podaci:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Sada možete zatvoriti na VM.

## <a name="step-2-create-a-storage-account-in-azure"></a>Korak 2: Stvaranje računa za pohranu u Azure ##

Potreban račun za pohranu u Azure da biste prenijeli datoteke .vhd tako da se koristi za stvaranje virtualnog računala. Azure klasični portal možete koristiti za stvaranje računa za pohranu.

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).

2. Na naredbenoj traci odaberite **Novo**.

3. Odaberite **podatkovni servisi** > **prostora za pohranu** > **brzo stvaranje**.

    ![Brzo stvorite račun za pohranu](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Popunite polja na sljedeći način:

    - U polje **URL** upišite naziv poddomenu za korištenje u URL-u račun za pohranu. Stavka može sadržavati od brojeva 3 24 i mala slova. Ovaj naziv postaje naziv glavnog računala unutar URL koji se koriste za rješavanje spremište blobova platforme Azure, reda čekanja Azure prostora za pohranu ili tablice Azure resursa za pohranu za pretplatu.

    - Na padajućem izborniku **Lokacije/afinitet grupe** odaberite **mjesto ili afinitet grupe** za račun za pohranu. Grupe sustava afinitet možete staviti servise u oblaku i prostora za pohranu u centru za iste podatke.

    - U polju **replikacije** odlučivanje o korištenju **Zemlj suvišnih** replikacije za račun za pohranu. Zemlj replikacije uključena je prema zadanim postavkama. Tu mogućnost replicira podataka na sekundarnom mjesto, uz bez naknadu, tako da se prostora za pohranu ne uspije pokazivač na to mjesto ako dođe do pogreške glavne na primarnom mjestu. Sekundarni mjesto automatski dodjeljuje i ne može se promijeniti. Ako vam je potrebna veću kontrolu nad mjesto oblaku prostora za pohranu zbog zakonskih ili pravila tvrtke ili ustanove, možete isključiti zemlj replikacije. Međutim, imajte na umu da ako kasnije uključite zemlj replikacije, koje naplatit će naknade za prijenos jednokratni podataka za replikaciju postojećih podataka na sekundarnom mjesto. Servise za pohranu bez zemlj replikacije se nude na popust. Dodatne informacije o upravljanju zemlj replikacije prostora za pohranu računa nalazi se ovdje: [Stvaranje, upravljanje, i brisanje računa za pohranu](../storage-create-storage-account/#replication-options).

    ![Unesite pojedinosti o računu za pohranu](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Odaberite **Stvaranje računa za pohranu**. Račun sada prikazuje u odjeljku **prostora za pohranu**.

    ![Račun za pohranu uspješno stvorili](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Nakon toga stvaranje spremnika datoteke prenesene .vhd. Odaberite naziv računa za pohranu, a zatim odaberite **spremnika**.

    ![Detalji o računu prostora za pohranu](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Odaberite **Stvaranje spremnika**.

    ![Detalji o računu prostora za pohranu](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. U polje **naziv** upišite naziv za svoje kontejner. Zatim na padajućem izborniku **pristup** odaberite željenu vrstu pravila za pristup.

    ![Naziv spremnika](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Prema zadanim postavkama kontejner je privatna i može se pristupiti samo vlasnik računa. Da biste omogućili javno pristup čitanju blob-ova u spremniku, ali ne spremnik svojstva i metapodatke, koristite mogućnost **Javno Blob** . Da biste omogućili potpuni javno pristup za čitanje za spremnik i blob-ova, koristiti **Javno kontejner** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Korak 3: Priprema veza Azure

Prije nego što možete prenijeti datoteke .vhd, morate uspostaviti sigurnu vezu između računala i Azure pretplatu. Da biste to učinili možete koristiti u Azure Active Directory (Azure AD) i metode certifikata.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Upotrijebite metodu Azure AD da biste prenijeli datoteku .vhd

1. Otvorite konzolu za Azure PowerShell.

2. Upišite sljedeću naredbu:  
    `Add-AzureAccount`

    Ta se naredba otvara prozor prijavu gdje možete se prijaviti pomoću računa tvrtke ili obrazovne ustanove.

    ![U prozoru ljuske PowerShell](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure potvrđuje i sprema podatke vjerodajnica. Zatim zatvara prozor.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Upotrijebite metodu potvrdu da biste prenijeli datoteku .vhd

1. Otvorite konzolu za Azure PowerShell.

2. Vrsta:  `Get-AzurePublishSettingsFile`.

3. U prozoru preglednika otvorit će se i traži preuzimanje datoteke .publishsettings. Datoteka sadrži informacije i certifikat za Azure pretplatu.

    ![Stranice za preuzimanje preglednika](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Spremite datoteku .publishsettings.

4. Vrsta:  `Import-AzurePublishSettingsFile <PathToFile>`, pri čemu `<PathToFile>` je cijeli put do datoteke .publishsettings.

   Dodatne informacije potražite u članku [Početak rada s Azure cmdleta](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Dodatne informacije o instaliranju i konfiguriranju komponente PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Korak 4: Prijenos datoteke .vhd

Kada prenesete .vhd datoteku, možete ga postaviti bilo gdje unutar spremištu blobova. Ovo su neke uvjete koji će se koristiti kada Prenesite datoteku:
-  **BlobStorageURL** je URL za pohranu račun koji ste stvorili u koraku 2.
-  **YourImagesFolder** je spremnik unutar blobova mjesto na koje želite pohraniti slike.
- **VHDName** je oznaku koja će se pojaviti na portalu Azure klasični da biste odredili virtualne na tvrdom disku.
- **PathToVHDFile** je cijeli put i naziv datoteke .vhd.


U prozoru Azure PowerShell ste koristili u prethodnom koraku, upišite:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Korak 5: Stvaranje na VM pomoću datoteke prenesene .vhd
Kada prenesete .vhd datoteku, možete ga dodati kao sliku na popis prilagođene slike koje su vezane uz pretplatu te stvaranje virtualnog računala s ovom prilagođenu sliku.

1. U prozoru Azure PowerShell ste koristili u prethodnom koraku, upišite:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Koristite Linux kao vrstu OS. Trenutna verzija Azure PowerShell prihvaća samo "Linux" ili "Windows" kao parametar.

2. Kada dovršite prethodne korake, nova slika nalazi se kada odaberete karticu **slike** na portalu Azure klasični.  

    ![Odaberite sliku](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Iz galerije stvaranje virtualnog računala. U ovom novu sliku sada je dostupan u odjeljku **Moja slika**.
4. Odaberite novu sliku. Zatim prijeđite do upute za postavljanje glavno računalo, lozinka, SSH ključ i tako dalje.

    ![Prilagođenu sliku](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Kada dovršite s dodjelom resursa, vidjet ćete svoje FreeBSD VM izvodi u Azure.

    ![Slika FreeBSD u azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
