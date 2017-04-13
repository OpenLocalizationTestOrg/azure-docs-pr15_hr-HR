<properties
    pageTitle="Ažuriranje Azure Agent Linux iz GitHub | Microsoft Azure"
    description="Saznajte kako ažuriranje Azure Linux Agent za vaše VM Linux u Azure verziju lateset iz Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Kako ažurirati Linux Agent Azure na na VM na najnoviju verziju s GitHub

Da biste ažurirali svoje [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) na Linux VM u Azure, morate već imati:

1. Izvodi Linux VM u Azure.
2. Veza na tom VM Linux pomoću SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Ako će izvođenju tog zadatka na računalu sa sustavom Windows, možete koristiti PuTTY za SSH u računalu Linux. Dodatne informacije potražite u članku [upute za prijavu radi Linux virtualnog računala](virtual-machines-linux-mac-create-ssh-keys.md).

Azure licencira Linux distros ste postavili paketa Azure Linux Agent njihove spremišta, pa Imajte potvrdite i instalirali najnoviju verziju te Distro spremištu najprije ako je to moguće.  

Za Ubuntu, samo upišite:

    #sudo apt-get install walinuxagent

I na CentOS, upišite:

    #sudo yum install waagent


Za Oracle Linux, provjerite je li u `Addons` spremište omogućena. Odaberite da biste uredili datoteku `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) ili `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) i taj redak promijenite `enabled=0` da biste `enabled=1` u odjeljku **[ol6_addons]** i **[ol7_addons]** u ovoj datoteci.

Zatim, da biste instalirali najnoviju verziju Azure Linux Agent, upišite:


    #sudo yum install WALinuxAgent

Ako ne pronađete spremište Dodatak jednostavno možete dodati retke na kraju .repo datoteku prema vašeg Oracle Linux izdanje:

Za Oracle Linux 6 virtualnim računalima sustava:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Za Oracle Linux 7 virtualnim računalima sustava:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Upišite:

    #sudo yum update WALinuxAgent

Obično je to sve potrebne, ali ako iz nekog razloga morate instalirati iz https://github.com izravno, poduzmite sljedeće korake.


## <a name="install-wget"></a>Instalacija wget

Prijavite se na vaše VM pomoću SSH.

Instalacija wget (su neke distros koji ne instaliraju po zadanom kao što su Redhat, CentOS i Oracle Linux verzije 6,4 i 6.5) tako da upišete `#sudo yum install wget` u naredbenom retku.


## <a name="download-the-latest-version"></a>Preuzmite najnoviju verziju

Otvorite [izdanje Azure Linux Agent u GitHub](https://github.com/Azure/WALinuxAgent/releases) na web-stranici, a saznati broj najnoviju verziju. (Trenutna verzija možete pronaći tako da upišete `#waagent --version`.)

### <a name="for-version-20x-type"></a>Za verziju 2.0.x, upišite:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Sljedeći redak kao primjer koristi verziju 2.0.14:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Za verziju 2.1.x ili noviju verziju, upišite:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Sljedeći redak kao primjer koristi verziju 2.1.0:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Instalacija Azure Linux Agent

### <a name="for-version-20x-use"></a>Za verziju 2.0.x, koristite:

 Provjerite waagent izvršne datoteke:

    #chmod +x waagent

 Kopirajte novi izvršnu datoteku /usr/sbin /.

  Za većinu Linux, koristite:

    #sudo cp waagent /usr/sbin

  Da biste postigli CoreOS, koristite:

    #sudo cp waagent /usr/share/oem/bin/

  Ako je to Nova instalacija programa Azure Linux Agent, pokrenite:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Za verziju 2.1.x, koristite:

Možda ćete morati instalirati paket `setuptools` potražite najprije – [ovdje](https://pypi.python.org/pypi/setuptools). Pokrenite:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Ponovno pokrenite servis za waagent

Za većinu linux Distros:

    #sudo service waagent restart

Da biste postigli Ubuntu, koristite:

    #sudo service walinuxagent restart

Da biste postigli CoreOS, koristite:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Provjerite verziju Azure Linux Agent

    #waagent -version

Za CoreOS, iznad naredba možda neće funkcionirati.

Prikazat će se ažurirati da verzija Azure Linux Agent novu verziju.

Dodatne informacije o Azure Linux Agent potražite u članku [README Agent za Azure Linux](https://github.com/Azure/WALinuxAgent).
