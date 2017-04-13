<properties
   pageTitle="Omogućavanje ili onemogućavanje Azure VM nadzora"
   description="U članku se opisuje kako omogućiti i onemogućiti Azure VM nadzora"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Omogućivanje i onemogućivanje Azure VM nadzora

U ovom se odjeljku opisuje kako omogućiti ili onemogućiti nadzor na virtualnim strojevima sustavom Azure. Prema zadanim postavkama nadzor omogućena na Azure virtualnog računala ako implementiran putem [portala za Azure](https://portal.azure.com) i nadzor grafova postoje se prema zadanim postavkama točkom 1-minutni. Možete omogućiti ili onemogućiti nadzor pomoću portala ili Azure sučelje naredbenog retka za Mac i Linux Windows (Azure EŽA). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Omogući / Onemogući nadzor putem portala za Azure
 
Možete omogućiti praćenje od vašeg Azure VM, koji pruža podataka o vašoj instanci u razdobljima 1 minute. (Promjena prostora za pohranu primijeniti). Dijagnostika detaljne podatke pa dostupna je za VM portala grafikona ili putem na API-JA. Prema zadanim postavkama portal za Azure omogućuje praćenje, ali ga možete isključiti prema uputama u nastavku. Možete omogućiti nadzor dok u VM radi ili u prestao stanje.

- Otvorite Azure portala pri ** [https://portal.azure.com](https://portal.azure.com)**

- U lijevom navigacijskom oknu kliknite virtualnih računala.

- Na virtualnim strojevima popisa odaberite pokrenut ili zaustavljen instance. Otvorit će se blad virtualnog računala.

- Kliknite "Sve postavke".

- Kliknite "Dijagnostika".

- Status promijeniti u položaj uključeno ili isključeno. Možete odabrati i u ovom plohu razinu nadzor Detalji o kojima želite omogućiti za virtualnog računala.

[Azure.Note] Parametar Dijagnostika na je zadana postavka prilikom stvaranja novog virtualnog računala

![Omogući / Onemogući nadzor putem portala za Azure.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Omogući / Onemogući nadzor s Azure EŽA
 
Da biste omogućili nadzor za programa Azure VM.

- Stvorite datoteku pod nazivom kao što su PrivateConfig.json sa sljedećim sadržajem.
        {"storageAccountName": "za pohranu računa za primanje podataka", "storageAccountKey": "ključ računa"}
- Pokrenite sljedeću naredbu Azure EŽA.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Možete promijeniti iz verzije 2.0 noviju verziju kada su dostupni. 

Dodatne informacije o konfiguriranju nadzor metriku i uzorke potražite dokument - **[Pomoću Linux dijagnostičkih Proširenje monitora Linux VM performanse i dijagnostičkih podataka](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

