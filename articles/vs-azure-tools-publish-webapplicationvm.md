<properties
   pageTitle="Objavljivanje WebApplicationVM | Microsoft Azure"
   description="Saznajte kako implementirati web-aplikaciju za virtualnog računala. Ova skripta stvara potrebni resursi za Azure pretplatu ne postoje li."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Objavljivanje-WebApplicationVM (skripte komponente Windows PowerShell)

Uvodi web-aplikaciju za virtualnog računala. Skripta stvara obavezni resursi za Azure pretplatu ne postoje li.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguracija

Put do JSON konfiguracijska datoteka koje opisuju detalje implementacije.

|Pseudonima|Ništa|
|---|---|
|Obavezan?|TRUE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

### <a name="subscriptionname"></a>SubscriptionName

Naziv Azure pretplatu u koju želite stvoriti virtualnog računala.

|Pseudonima|Ništa|
|---|---|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Koristi prvi pretplate u datoteci pretplate|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

### <a name="webdeploypackage"></a>WebDeployPackage

Put do paketa za implementaciju web objavljivanje virtualnog računala. Paket možete stvoriti i pomoću čarobnjaka za objavljivanje Web u Visual Studio. U odjeljku [Kako: Stvaranje paketa za implementaciju Web u Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Pseudonima|Ništa|
|---|---|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

### <a name="allowuntrusted"></a>AllowUntrusted

Ako je true, dopušta korištenje certifikata koje nisu potpisao pouzdani korijenski za izdavanje certifikata.

|Pseudonima|Ništa|
|---|---|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|FALSE|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

### <a name="vmpassword"></a>VMPassword

Vjerodajnice za račun virtualnog računala. Primjer: - VMPassword @{Name = "administrator"; Lozinka = "lozinku"}

|Pseudonima|Ništa|
|---|---|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Vjerodajnice za bazom podataka SQL Azure. Primjer: - DatabaseServerPassword @{Name = "administrator"; Lozinka = "lozinku"}

|Pseudonima|Ništa|
|---|---|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Ako je true, ispis poruke skriptu strujanje Izlaz.

|Pseudonima|Ništa|
|---|---|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|FALSE|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

## <a name="remarks"></a>Napomene

Potpuno objašnjenje kako koristiti skriptu da biste stvorili razvojni i testiranje okruženja, potražite u članku [Pomoću programa Windows PowerShell skripte za objavljivanje na web-mjesto razvojni i u okvir za testiranje okruženju](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfiguracijska datoteka JSON određuje detalje o što je uvesti. Obuhvaća podatke koje ste naveli prilikom stvaranja projekt, primjerice naziv, afinitet grupe, VHD slike i veličinu virtualnog računala. I sadrži krajnje točke na virtualnog računala i baze podataka za dodjelu resursa, ako postoje, a web-implementacije parametara. Sljedeći kod prikazuje primjera JSON konfiguracije datoteke:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Možete urediti JSON konfiguracijska datoteka da biste promijenili što je dodijeljena. Potrebni su virtualnog računala i servise u oblaku, ali sekciji baza podataka nije obavezno.
