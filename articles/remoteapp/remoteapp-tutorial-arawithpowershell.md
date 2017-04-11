<properties
   pageTitle="Pomoću cmdleta ljuske PowerShell s Azure RemoteApp | Microsoft Azure"
   description="Saznajte kako pomoću cmdleta ljuske Windows Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Pomoću cmdleta ljuske Windows Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

 Koristite Azure RemoteApp PowerShell cmdleti za administriranje i održavanja zbirke. Poslužite se sljedećim informacijama za početak rada.

## <a name="get-the-cmdlets"></a>Početak u cmdleta 
-------------
Najprije preuzmite Azure Powershell cmdleti [ovdje](http://go.microsoft.com/?linkid=9811175)RemoteApp cmdleta nalaze se u njoj. 

Potražite u članku [Pomoć za Azure RemoteApp cmdlet](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfiguriranje Azure Cmdlete da biste koristili pretplatu
------------------
Da biste mogli koristiti s cmdletima protiv Azure pretplatu, slijedite [ovaj vodič](../powershell-install-configure.md) .

Pomoću ovih koraka možete brzo počnite s radom:

1.  Preuzmite i instalirajte [cmdleta ljuske PowerShell Azure](http://go.microsoft.com/?linkid=9811175).
2.  Pokrenite Microsoft Azure PowerShell.
3.  Pokrenite **Dodaj AzureAccount** za provjeru autentičnosti u pretplatu za Azure. Kada se to od vas zatraži, unesite isto korisničko ime i lozinku koje koristite za prijavu na portal za Azure.  
4.  Pokrenite **Get-AzureSubscription** popis pretplata povezana s vašim korisničkim računom. 
5.  Pokrenite **Odaberite AzureSubscription** i navedite naziv pretplate ili ID za korištenje na konzoli PowerShell.

Čestitamo, konzole za Azure PowerShell je konfiguriran i možete ga koristiti. Imajte na umu da ćete morati repeate korake 2 do 5 svaki put kada pokrenete na konzoli za Azure PowerShell.  

## <a name="create-a-cloud-collection"></a>Stvaranje zbirke oblaka
--------------------
Jednostavno je, pokrenite sljedeću naredbu:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Naredbe iznad automatski objavljuje aplikacije Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, Visio i Word).

Stvaranje zbirke može potrajati 30 minuta ili dulje. Stoga ta naredba Vrati ID praćenja koje možete koristiti na sljedeći način:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Po završetku zbirke dodate korisnike u zbirku pomoću sljedeće naredbe:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

I gotovo! Korisnik mora biti može se povezati s računala pomoću klijentskog programa Azure RemoteApp pronaći [ovdje](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Dostupne cmdleta
Postoji mnogo drugih naredbi koje imamo, potražite u dokumentaciji ih uskoro će biti uskoro:

Osnovni RemoteApp zbirke Cmdlete: 

- Novi AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Postavljanje AzureRemoteAppCollection
- Ažuriranje AzureRemoteAppCollection
- Uklanjanje AzureRemoteAppCollection
- Dodavanje AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Uklanjanje AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Prekid veze AzureRemoteAppSession
- Pozivanje AzureRemoteAppSessionLogoff
- Slanje AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Objavljivanje AzureRemoteAppProgram
- Poništavanje objave AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Cmdleti za RemoteApp virtualne mreže:

- Novi AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Postavljanje AzureRemoteAppVNet
- Uklanjanje AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Dohvaćanje – AzureRemoteAppVpnDeviceConfigScript
- Ponovno postavljanje AzureRemoteAppVpnSharedKey

RemoteApp predložak slika Cmdlete:

- Novi AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Preimenovanje AzureRemoteAppTemplateImage
- Uklanjanje AzureRemoteAppTemplateImage

Cmdleti za druge RemoteApp:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Postavljanje AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
