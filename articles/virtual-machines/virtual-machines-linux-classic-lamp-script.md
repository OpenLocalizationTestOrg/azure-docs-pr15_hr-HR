<properties
    pageTitle="Korištenje proširenje CustomScript na Linux VM | Microsoft Azure"
    description="Saznajte kako koristiti CustomScript proširenje za implementaciju aplikacije na Linux virtualnim strojevima u Azure stvoren pomoću model klasični implementacije."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Implementacija aplikacije LAMPICE pomoću Azure CustomScript proširenje za Linux#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


U programu Microsoft Azure CustomScript proširenja za Linux omogućuje da biste prilagodili virtualnim strojevima (VMs) tako da pokrenete proizvoljne kod napisan skriptnog jezika podržava VM (na primjer, Python i tulumu). To omogućuje prilagodljivih da biste automatizirali implementaciju aplikacije na više računala.

Možete implementirati proširenje CustomScript pomoću portala za Azure klasični, komponente Windows PowerShell ili Azure sučelja naredbenog retka (Azure EŽA).

U ovom se članku ćemo pomoću EŽA Azure za implementaciju jednostavne aplikacije ŽARULJE da biste je Ubuntu VM stvoren pomoću model klasični implementacije.

## <a name="prerequisites"></a>Preduvjeti

Primjerice, stvoriti dvije Azure VMs sa Ubuntu 14.04 ili novijim. U VMs se nazivaju *skripte vm* i *žarulje vm*. Koristite jedinstvene nazive prilikom stvaranja na VMs. Se koristi za izvođenje naredbe EŽA i se koristi za implementaciju aplikacije ŽARULJE.

Morate račun za Azure prostora za pohranu i ključ za pristup (možete dobiti to s portala za Azure klasični).

Ako vam je potrebna pomoć pri stvaranju Linux VMs na Azure odnose se na [Stvori virtualnog računala radi Linux](virtual-machines-linux-classic-createportal.md).

Naredbe za instalaciju pretpostavlja Ubuntu, ali možete prilagoditi instalaciju za sve podržane distro Linux.

Skripta vm VM mora imati Azure EŽA instaliran s vezom radu Azure. Pomoć za to potražite [instalirate i konfigurirate Azure sučelja naredbenog retka](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Prijenos skripte

Proširenje CustomScript ćemo koristiti da biste pokrenuli skriptu na udaljenom VM da biste instalirali stog ŽARULJE i stvorite stranicu i. Da biste pristupili skriptu s bilo kojeg mjesta smo ćete prenijeti ga kao blobova platforme Azure.

### <a name="script-overview"></a>Pregled skripte

Primjer skripte instalira ŽARULJE stogu Ubuntu (uključujući postavljanje tihu instalaciju sustava MySQL), piše i jednostavnog i pokrenuti Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Prijenos skripte

Spremanje skriptu u obliku tekstne datoteke, primjerice *install_lamp.sh*, i prijenos Azure prostora za pohranu. To možete jednostavno s EŽA Azure. U sljedećem primjeru prenosi datoteku pod nazivom "skripte" spremnik za pohranu. Ako ne postoji spremnik morat ćete prvo stvorite.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Stvoriti datoteku JSON koji opisuje kako preuzeti skriptu iz spremišta Azure. Spremite to kao *public_config.json* (zamjene "mystorage" s nazivom vašeg računa za pohranu):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Implementacija datotečni nastavak

Sada možete koristiti sljedeće naredbe za implementaciju CustomScript proširenje Linux na udaljenom VM pomoću EŽA Azure.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Naredba prethodne preuzima i pokreće skripte *install_lamp.sh* VM naziva *žarulje vm*.

Budući da aplikaciju obuhvaća web-poslužitelj, imajte na umu da biste otvorili HTTP listening priključak na udaljenom VM s sljedeću naredbu.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Nadzor i otklanjanje poteškoća

Možete provjeriti na koliko će se dobro prilagođene skripte izvodi zatrebaju datoteku zapisnika na udaljenom VM. SSH *žarulje vm* i krakom datoteku zapisnika pomoću sljedeće naredbe.

    cd /var/log/azure/customscript
    tail -f handler.log

Kada pokrenete proširenje CustomScript, možete pregledavati i stranicu koju ste stvorili informacije. Stranice i na primjer u ovom članku je *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Dodatni resursi

Koristite iste osnovne korake za implementaciju složenije aplikacije. U ovom primjeru skripta za instalaciju je spremljen kao javno blobova platforme Azure pohrane u. Sigurnije mogućnosti bi da biste spremili skripta za instalaciju kao sigurnu blob [Siguran pristup potpis](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Dodatni resursi za Azure EŽA, Linux i nastavak CustomScript navedene su sljedeće.

[Automatiziranje zadataka za prilagodbu Linux VM korištenje nastavka CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux Extensions (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux i Otvori izvor računalstvo na Azure](virtual-machines-linux-opensource-links.md)
