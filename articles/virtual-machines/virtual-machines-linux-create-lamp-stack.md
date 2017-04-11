<properties
    pageTitle="Implementacija ŽARULJE na Linux virtualnog računala | Microsoft Azure"
    description="Saznajte kako instalirati stog ŽARULJE na Linux VM"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>Implementacija ŽARULJE snop na Azure
U ovom se članku će vas voditi kroz kako implementirati Apache web-poslužitelj, MySQL i PHP (stog ŽARULJE) na Azure. Trebat će vam račun Azure ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)) i [Azure EŽA](../xplat-cli-install.md) koji je [povezan s vašim računom Azure](../xplat-cli-connect.md).

Postoje dva načina za instalaciju ŽARULJE u ovom članku:

## <a name="quick-command-summary"></a>Naredba brzi sažetak

1) Implementacija ŽARULJE na novu VM

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Implementacija ŽARULJE na postojeće VM

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Implementacija ŽARULJE na novi vodič VM

Možete pokrenuti tako da stvorite novu [grupu resursa](../azure-resource-manager/resource-group-overview.md) koja će sadržavati na VM:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Da biste stvorili VM sam, možete koristiti predložak Voditelj resursa Azure već napisanih pronaći [ovdje na GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Trebali biste vidjeti odgovora na pitanja neke dodatne unose:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Sada ste stvorili Linux VM s ŽARULJE već instaliran na njemu. Ako želite, možete provjeriti instalaciju tako da skoka prema dolje do [provjerite ŽARULJE uspješno instaliran].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Implementacija ŽARULJE na postojeće VM vodič

Ako vam je potrebna pomoć pri stvaranju Linux VM head [ovdje da biste saznali kako stvoriti Linux VM] (. / virtual-machines-linux-quick-create-cli.md). Nakon toga morat ćete SSH u Linux VM. Ako vam je potrebna pomoć pri stvaranju ključa SSH head [ovdje da biste saznali kako stvoriti ključa SSH na Linux/Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Ako već imate ključa SSH, odaberite unaprijed i SSH u vašem VM Linux s `ssh username@uniqueDNS`.

Sada kada radite unutar vaše Linux VM, ne možemo će voditi kroz instalacije stog ŽARULJE na koji se temelji na Debian distribucije. Exact naredbe može se razlikovati od drugih distros Linux.

#### <a name="installing-on-debianubuntu"></a>Instalacija na Debian/Ubuntu

Morat ćete sljedeće paketa instaliranih: `apache2`, `mysql-server`, `php5`, i `php5-mysql`. Možete instalirati te izravno grabbing te pakete ili pomoću Tasksel. Upute za obje mogućnosti navedena su u nastavku.
Prije instalacije morat ćete preuzeti i ažuriranje popisa paketa.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Pojedinačne paketa
Korištenje Zemaljska get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Korištenje Tasksel
Umjesto toga možete preuzeti Tasksel, alat za Debian/Ubuntu koja se instalira više povezanih pakete usklađenih "zadatak" na sustav.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Nakon izvođenja na bilo koji od navedenih mogućnosti zatražit će se da biste instalirali te pakete i broj druge ovisnosti. Pritisnite "y", a zatim Enter da biste nastavili i slijedite druge upute za postavljanje administratorske lozinke za MySQL. To se može instalirati najmanji potreban i proširenja potrebne za uporabu i MySQL. 

![][1]

Pokrenite sljedeću naredbu da biste vidjeli drugim datotečnim nastavcima i koji su dostupni kao paketa:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Stvaranje dokumenta info.php

Sada trebali da biste provjerili koju verziju sustava Apache, MySQL i i imate putem naredbenog retka upisivanjem `apache2 -v`, `mysql -v`, ili `php -v`.

Ako želite da biste testirali Dodatno, možete stvoriti brzi stranice i informacije za prikaz u pregledniku. Stvaranje nove datoteke s Nano uređivač teksta s sljedeću naredbu:

    user@ubuntu$ sudo nano /var/www/html/info.php

U uređivaču teksta GNU Nano dodajte sljedeće retke:

    <?php
    phpinfo();
    ?>

Zatim spremite i zatvorite uređivač teksta.

Ponovno pokrenite Apache pomoću ove naredbe da bi se sve nove instalacije stupile na snagu.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Provjerite je li ŽARULJE uspješno instaliran

Sada možete provjeriti na stranici s podacima o PHP koji ste upravo stvorili u pregledniku tako da http://youruniqueDNS/info.php, izgledat slična ovoj.

![][2]

Prikaz Apache2 Ubuntu zadane stranice tako da vam http://youruniqueDNS/ možete provjeriti Apache instalaciju. Trebali biste vidjeti otprilike ovako.

![][3]

Čestitamo, imate samo postavljanje ŽARULJE snop na vašem VM Azure!

## <a name="next-steps"></a>Daljnji koraci

Pročitajte dokumentaciju Ubuntu na stogu ŽARULJE:

- [https://help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
