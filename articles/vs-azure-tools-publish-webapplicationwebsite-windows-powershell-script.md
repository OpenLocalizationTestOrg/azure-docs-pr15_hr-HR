<properties
   pageTitle="Objavljivanje-WebApplicationWebSite (skripte komponente Windows PowerShell) | Microsoft Azure"
   description="Saznajte kako objavljivanje projekta web Azure web-mjesto. Ova skripta stvara potrebni resursi za Azure pretplatu ne postoje li."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Objavljivanje-WebApplicationWebSite (skripte komponente Windows PowerShell)

##<a name="syntax"></a>Sintaksa

Objavljuje projekta web Azure web-mjesto. Skripta stvara obavezni resursi za Azure pretplatu ne postoje li.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguracija

Put do JSON konfiguracijska datoteka koje opisuju detalje implementacije.

|Parametar|Zadana vrijednost|
|---|---|
|Pseudonima|Ništa|
|Obavezan?|TRUE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

## <a name="subscriptionname"></a>SubscriptionName

Naziv Azure pretplatu u koju želite stvoriti web-mjesta u.

|Parametar|Zadana vrijednost|
|---|---|
|Pseudonima|Ništa|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

## <a name="webdeploypackage"></a>WebDeployPackage

Put do paketa za implementaciju web za objavljivanje na web-mjesto. Paket možete stvoriti i pomoću čarobnjaka za objavljivanje Web u Visual Studio. Dodatne informacije potražite u članku [Početak rada s Azure servise u Oblaku i ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parametar|Zadana vrijednost|
|---|---|
|Pseudonima|Ništa|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje kanalima unosa?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Korisničko ime i lozinku za bazom podataka SQL Azure.

|Parametar|Zadana vrijednost|
|---|---|
|Pseudonima|Ništa|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|Ništa|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Ako je true, ispis poruke skriptu strujanje Izlaz.

|Parametar|Zadana vrijednost|
|---|---|
|Pseudonima|Ništa|
|Obavezan?|FALSE|
|Položaj|pod nazivom|
|Zadana vrijednost|FALSE|
|Prihvaćanje unosa kanal?|FALSE|
|Prihvaćanje zamjenskih znakova?|FALSE|

## <a name="remarks"></a>Napomene

Potpuno objašnjenje kako koristiti skriptu da biste stvorili razvojni i testiranje okruženja, potražite u članku [Pomoću programa Windows PowerShell skripte za objavljivanje na web-mjesto razvojni i u okvir za testiranje okruženju](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfiguracijska datoteka JSON određuje detalje o što je uvesti. Obuhvaća podatke koji ste naveli kada ste stvorili u projekt, primjerice naziv i korisničko ime za web-mjesta. Uključuje i baze podataka za dodjelu resursa, ako postoje. Sljedeći kod prikazuje primjera JSON konfiguracije datoteke:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Konfiguracijska datoteka JSON da biste promijenili što je implementiran možete uređivati. Potreban je odjeljku web-mjesto, ali sekciji baza podataka nije obavezno.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u članku [Objavljivanje WebApplicationVM (skripte komponente Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)
