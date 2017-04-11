<properties
    pageTitle="Linux Gosti na Azure stogu | Microsoft Azure"
    description="Saznajte kako stvoriti sustavom Linux virtualnim strojevima na hrpu Azure."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Implementacija Linux virtualnim strojevima na hrpu Azure

Možete implementirati Linux virtualnim strojevima na hrpu PNA Azure dodavanjem slika sa sustavom Linux u stogu trgovine Windows Azure. Nekoliko dobavljača Linux ste naveli slike koje se mogu se dodati na PNA snop Azure ili možete stvoriti i vlastite.

## <a name="download-an-image"></a>Preuzimanje slika

 1. Preuzimanje i izdvojiti Azure stogu kompatibilan s preglednikom sliku iz sljedećih veza ili Priprema vlastiti:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Izdvajanje slika VHD ako je potrebno i [Dodavanje slike u trgovinu](azure-stack-add-vm-image.md). Provjerite je li u `OSType` parametar postavljen na `Linux`.
 
 3. Nakon dodavanja slike u trgovinu, stvaranja trgovine stavke, a možete implementirati Linux virtualnog računala.
  
## <a name="prepare-your-own-image"></a>Priprema vlastite slike

1. Priprema vlastite slike Linux koristite neku od sljedeće upute:
 - [Distribucija utemeljen na centOS](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Crvena je vaša Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Preuzmite i instalirajte [Azure Linux Agent](https://github.com/Azure/WALinuxAgent/)

    Agent za Linux Azure verziju 2.1.3 ili noviji potreban je Dodjela vaše VM Linux na hrpu Azure. Mnoge distribucija već naveden obuhvatiti ovu verziju agenta ili noviji kao paket njihove spremištima (obično naziva `WALinuxAgent` ili `walinuxagent`). Međutim, ako je verziju paketa Azure agent manje od 2.1.3 (odnosno 2.0.18 ili podređeniji), zatim agenta morate instalirati ručno. Instaliranoj verziji mogu se odrediti iz polja Naziv paketa ili tako da pokrenete `/usr/sbin/waagent -version` na na VM.

    Slijedite upute u nastavku da biste instalirali agent za Azure Linux ručno-

 - Najprije preuzmite najnovije agent za Azure Linux iz [Github](https://github.com/Azure/WALinuxAgent/releases)primjer:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Otpakiravanje agent za Azure:

            # tar -vzxf v2.2.0.tar.gz

 - Instalacija python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Instalirajte agent za Azure:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Sustavi s Python 2.x i Python 3.x instaliran po usporednu morati pokrenite sljedeću naredbu:

        # sudo python3 setup.py install --register-service

    Dodatne informacije potražite u članku Azure Linux Agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Dodavanje slike na tržištu](azure-stack-add-vm-image.md). Provjerite je li u `OSType` parametar postavljen na `Linux`.

4. Nakon dodavanja slike u trgovinu, stvaranja trgovine stavke, a možete implementirati Linux virtualnog računala.

## <a name="next-steps"></a>Daljnji koraci

[Najčešća pitanja o snopu Azure](azure-stack-faq.md)