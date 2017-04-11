<properties
    pageTitle="Preuzmite Azure SDK za PHP"
    description="Saznajte kako preuzeti i instalirati Azure SDK za i."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Preuzmite Azure SDK za PHP

## <a name="overview"></a>Pregled

Azure SDK i sadrži komponente koje vam omogućuju da razviti, uvođenje i upravljanje aplikacijama i za Azure. Konkretno, SDK Azure i obuhvaća sljedeće:

* **I u biblioteke klijenta za Azure**. Ove biblioteke klasa navedite sučelje za pristup Azure značajke, kao što su servisa za upravljanje podacima i servise u oblaku.  
* **Azure sučelja naredbenog retka za Mac i Linux, Windows (Azure EŽA)**. To je skup naredbi za implementaciju i upravljanje Azure servisa, kao što su Azure web-mjesta i virtualnim računalima sustava Azure. Azure EŽA rad na bilo kojoj platformi, uključujući Mac i Linux sustava Windows.
* **Azure PowerShell (samo za Windows)**. To je skup PowerShell cmdleti za uvođenje i upravljanje servisa Azure, kao što su servisi u Oblaku i virtualnih računala.
* **Azure Emulatora (samo za Windows)**. Emulatora računalnim i pohranu su lokalni Emulatora servisi u oblaku i upravljanje podacima koji omogućuju lokalno aplikacija za testiranje. Azure Emulatora izvoditi samo na Windows.

U odjeljcima opisuju kako preuzeti i instalirati komponente prethodno opisan.

Upute u ovoj temi pretpostavlja da imate [PHP] [ install-php] instaliran.

> [AZURE.NOTE] Morate imati PHP 5,5 ili noviji da biste koristili biblioteka klijentski PHP Azure.

##<a name="php-client-libraries-for-azure"></a>Biblioteka klijentski PHP za Azure

Biblioteke i klijenta za Azure omogućuje sučelje za pristup Azure značajke, kao što su servisa za upravljanje podacima i oblaku servisi, operacijske sustave. Te biblioteke mogu instalirati putem skladatelja.

Informacije o korištenju biblioteka klijenta i za Azure potražite [u]članku korištenje usluge Blob[blob-service], [upute za korištenje servisa tablice] [ table-service] i [način korištenja usluga reda čekanja][queue-service].

###<a name="install-via-composer"></a>Instalacija putem Skladatelj

1. [Instalacija brojka][install-git].


    > [AZURE.NOTE] U sustavu Windows, i morat ćete dodati brojka izvršna varijablu okruženja put.

2. Stvorite datoteku pod nazivom **composer.json** u korijenskoj mapi projekta i dodati sljedeći kod:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Preuzmite ** [composer.phar] [ composer-phar] ** u korijensko projekta.

4. Otvorite naredbeni redak, i to izvršavanje u korijensko projekta

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell i Azure Emulatora

Azure PowerShell je skup PowerShell cmdleti za implementaciju i upravljanje servisa Azure (kao što su servisi u Oblaku i virtualnih računala). Azure Emulatora su Emulatora servise u oblaku i upravljanje podacima koji omogućuju lokalno aplikacija za testiranje. Podržane su sljedeće komponente samo u sustavu Windows.

Preporučeni način za instalaciju Azure PowerShell i Emulatora Azure je koristiti [Microsoft Web platformu Installer][download-wpi]. Imajte na umu da možete instalirati drugih komponenti razvoj, kao što su PHP, SQL Server, a zatim Drivers Microsoft SQL Server za PHP i WebMatrix.

Informacije o korištenju Azure PowerShell potražite [u]članku korištenje ljuske PowerShell Azure[powershell-tools].

##<a name="azure-cli"></a>Azure EŽA

Azure EŽA je skup naredbi za implementaciju i upravljanje Azure servisa, kao što su Azure web-mjesta i virtualnim računalima sustava Azure. Informacije o instaliranju Azure EŽA potražite u članku [Instalacija EŽA Azure](xplat-cli-install.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere i](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
