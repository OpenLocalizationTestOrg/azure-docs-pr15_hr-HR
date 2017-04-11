<properties
   pageTitle="Automatizacija implementaciju aplikacije s datotečnim nastavcima virtualnog računala | Microsoft Azure"
   description="Praktični vodič DotNet Core Azure virtualnog računala"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Implementacija aplikacije s predlošcima Voditelj resursa za Azure

Kada Azure infrastructural preduvjeti imate prepoznati i prevede uvođenje predloška, stvarni aplikacije implementacije mora biti adresirane. Implementacija aplikacije ovdje je upućuju na instalacije binarne datoteke iz stvarnog aplikacije na Azure resursi. Primjer spremište glazbu, .net Core, NGINX i nadzorne morate instalirati i konfigurirati na svakom virtualnog računala. Binarne datoteke glazbu trgovine moraju biti instalirani na virtualnog računala i baze podataka za glazbu spremišta prethodno stvorili.

Ovaj dokument detalje o kako proširenja virtualnog računala automatiziraju implementaciju aplikacije i konfiguracije Azure virtualnih računala. Istaknute su sve zavisnosti i jedinstvene konfiguracije. Za najbolje mogućnosti rada prije implementacije instance komponente rješenja Azure pretplate i rad uz predloška Azure Voditelj resursa. Dovršavanje predložak Ovdje možete pronaći – [Implementacije spremište glazbu na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Skripte za konfiguraciju

Proširenja virtualnog računala su specijalizirane programi koji se izvoditi na temelju virtualnim strojevima omogućuje automatizaciju konfiguracije. Proširenja dostupni su za posebne namjene kao što je antivirusni, konfiguracije zapisivanja i Docker konfiguracije. Proširenje za prilagođene skripte mogu se pokrenuti kod skripte virtualnog računala. S uzorak glazbu spremište je do prilagođene skripte proširenja za konfiguriranje virtualnim strojevima Ubuntu i instalirati aplikaciju za spremište glazbu.

Prije nego što dovršenog kako su deklarirane proširenja virtualnog računala u predlošku Azure Voditelj resursa, pregledajte skriptu koja se pokreće. Ova skripta konfigurira virtualnog računala Ubuntu za hostiranje glazbu trgovine aplikacija. Prilikom pokretanja, skripta instalira sve potrebne softver, instalirajte aplikaciju iz trgovine glazbu iz izvora kontrole i priprema baze podataka. 

Dodatne informacije o nalaze u .net Jezgra aplikaciju na Linux, pročitajte članak [Objavljivanje na radnom okruženju Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> Ovaj primjer je u svrhu pokazni.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Proširenje VM skripte

VM proširenja mogu se izvoditi na temelju virtualnog računala trenutku Sastavi uključivanjem proširenje resursa u predlošku Voditelj resursa Azure. Proširenje možete dodati pomoću čarobnjaka za Visual Studio dodavanje resursa ili tako da umetnete valjani JSON u predlošku. Proširenje skripte resursa ugniježđen je u resursa virtualnog računala; To se može vidjeti u sljedećem primjeru.

Slijedite vezu da biste vidjeli JSON uzorak u predlošku Voditelj resursa – [VM skripte proširenje](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Obratite pozornost na to u u nastavku JSON skriptu pohranjen u GitHub. Ova skripta također biti pohranjen u spremište blobova platforme Azure. Predlošci Voditelj resursa Azure omogućuju niz izvođenja skripte za konstruirana tako da se vrijednosti parametara predložak moguće je koristiti kao parametar za skripte izvršavanja. U ovom slučaju podaci navedeni kada implementacija predložaka, a te vrijednosti se zatim može koristiti prilikom izvršavanja skriptu.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Dodatne informacije o korištenju prilagođene skripte proširenje potražite u članku [prilagođene skripte proširenja s predlošcima Voditelj resursa](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Sljedeći korak

<hr>

[Istražite više Azure resursima predložaka](https://github.com/Azure/azure-quickstart-templates)
