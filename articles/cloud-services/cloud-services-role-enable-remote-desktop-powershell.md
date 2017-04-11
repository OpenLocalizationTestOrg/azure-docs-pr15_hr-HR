<properties
pageTitle="Omogućivanje udaljene radne površine za uloga u servise u Oblaku Azure pomoću komponente PowerShell"
description="Upute za konfiguriranje aplikacije servisa azure oblaka pomoću komponente PowerShell da dopušta veze za udaljene radne površine"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Omogućivanje udaljene radne površine za uloga u servise u Oblaku Azure pomoću komponente PowerShell

>[AZURE.SELECTOR]
- [Azure klasični portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Udaljena radna površina omogućuje vam pristup radnoj površini uloge izvodi u Azure. Pomoću veze udaljene radne površine da biste i otklanjanja poteškoća s aplikacijom dok se izvodi.

U ovom se članku opisuje kako omogućiti udaljene radne površine na svoje uloge servisa oblak pomoću komponente PowerShell. Preduvjeti koja su potrebna za u ovom se članku potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) . PowerShell koristi datotečni nastavak udaljene radne površine tako da možete omogućiti udaljene radne površine kada je implementiran aplikacije.


## <a name="configure-remote-desktop-from-powershell"></a>Konfiguriranje udaljene radne površine iz komponente PowerShell

[Postavljanje AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet omogućuje vam da biste omogućili udaljene radne površine na navedeni uloge ili sve uloge servisa uvođenja oblaka. Cmdlet omogućuje određivanje korisničkog imena i lozinke za udaljene radne površine korisnika putem parametar *vjerodajnica* koje prihvaća PSCredential objekt.

Ako koristite PowerShell interaktivno, objekt PSCredential jednostavno možete postaviti tako da nazovete cmdlet [Get-vjerodajnice](https://technet.microsoft.com/library/hh849815.aspx) .

```
$remoteusercredentials = Get-Credential
```

Ta naredba prikazuje dijaloški okvir koji vam omogućuje unesite korisničko ime i lozinku za daljinski korisnik siguran način.

Budući da PowerShell pomaže u scenarijima Automatizacija, postavite i **PSCredential** objekt na način koji nije potreban interakcije s korisnikom. Najprije morate postaviti sigurne lozinke. Započnite s određivanje lozinke običnog teksta pretvoriti sigurne niza pomoću [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx). Potom morati sigurne niz pretvoriti šifrirane standardne niz koji je pomoću [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx). Sada možete spremiti šifrirane standardne niz datoteku pomoću [Skupa sadržaja](https://technet.microsoft.com/library/ee176959.aspx).

Možete stvoriti i sigurne lozinke datoteke tako da ne morate svaki put upišite u okvir lozinka. Osim toga, sigurne lozinke datoteka je bolje nego datoteka običnog teksta. Da biste stvorili datoteku sigurne lozinke, koristite sljedeće komponente PowerShell:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Prilikom postavljanja lozinku, provjerite zadovoljavate li [složenosti](https://technet.microsoft.com/library/cc786468.aspx).

Da biste stvorili objekt vjerodajnica iz datoteke sigurne lozinke, čitati sadržaj datoteke te ih ponovno pretvoriti sigurne niza pomoću [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx).

[Postavljanje AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet prihvaća i parametar *isteka* koji određuje **Datum i vrijeme** isteka korisnički račun. Na primjer, nije moguće postaviti račun istječe nekoliko dana od trenutnog datuma i vremena.

U ovom se primjeru PowerShell prikazuje kako postaviti proširenje udaljene radne površine na neki servis u oblaku:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Također možete navesti vremensko razdoblje uvođenja i uloge koje želite omogućiti udaljene radne površine na. Ako nisu navedeni parametara, cmdlet omogućuje udaljene radne površine na svih uloga u vremensko razdoblje za implementaciju **proizvodnje** .

Proširenje udaljene radne površine povezan je s implementacije. Ako stvorite novu implementaciju za servis, morate omogućiti udaljene radne površine u toj implementaciji. Ako uvijek želite imati omogućeno udaljene radne površine, zatim razmislite o integraciji skripti komponente PowerShell u tijeka rada za implementaciju.


## <a name="remote-desktop-into-a-role-instance"></a>Udaljena radna površina u ulozi instanci
Cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) koristi se za udaljene radne površine u određenim ulogama pojavljivanja servis u oblaku. Preuzimanje datoteke RDP lokalno možete koristiti parametar *LocalPath* . Ili možete koristiti parametar *pokretanje* izravno pokretanje dijaloški okvir udaljene radne površine da biste pristupili uloga instanca servisa oblaka.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Provjera udaljene radne površine proširenje omogućena na usluzi
Cmdlet [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) prikazuje koji omogućuje ili onemogućuje na servis za implementaciju udaljene radne površine. Cmdlet vraća korisničko ime za udaljene radne površine korisnika i uloge kojima je omogućen udaljene radne površine nastavak. Po zadanom se to dogodi na vremensko razdoblje za implementaciju, pa možete odabrati namijenjenu pripremna vremensko razdoblje.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Uklonite proširenje udaljene radne površine na servisu
Ako ste već omogućili udaljene radne površine nastavak na implementacije, a morate ažurirati postavke udaljene radne površine, najprije uklonite datotečni nastavak. I ponovno Omogućivanje s novim postavkama. Na primjer, ako želite da biste postavili novu lozinku za udaljeni korisnički račun ili račun istekao. Na taj način potreban je na postojeće implementacijama udaljene radne površine nastavkom omogućena. Za novi implementacije, možete jednostavno primijeniti proširenje izravno.

Da biste uklonili implementacijskih udaljene radne površine kućni broj, koristite cmdlet [Ukloni AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) . Također možete odrediti ulogu iz kojeg želite ukloniti proširenje udaljene radne površine i implementaciju vremensko razdoblje.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Da biste potpuno uklonili konfiguracije kućni broj, nazovite cmdlet *ukloniti* s parametrom **UninstallConfiguration** .
>
>Parametar **UninstallConfiguration** deinstalira sve konfiguracije kućni broj koji se primjenjuje na servis. Svaki konfiguracije kućni broj povezan je s konfiguraciju servisa. Pozivanje cmdlet *Uklanjanje* bez **UninstallConfiguration** disassociates <mark>implementaciju</mark> iz konfiguracije kućni broj, stoga učinkovito ukida datotečni nastavak. Međutim, konfiguracije proširenje ostaje povezan sa servisom.



## <a name="additional-resources"></a>Dodatni resursi

[Konfiguriranje servisa u Oblaku](cloud-services-how-to-configure.md)
