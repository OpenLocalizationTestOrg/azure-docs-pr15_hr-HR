
<properties
    pageTitle="Informacije o VNET Azure RemoteApp za promjenu veličine | Microsoft Azure"
    description="Informirajte se o preduvjetima za IP adresa za Azure RemoteApp raditi s na VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informacije o VNET Azure RemoteApp za promjenu veličine

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Kada koristite Azure RemoteApp s virtualne mreže (VNET), RemoteApp koristi IP adrese unutar podmreži. Na temelju skale na servisu RemoteApp, morate provjerite imaju li vašoj podmreži dovoljno IP adresa za RemoteApp virtualnih računala. Dok ovo upute za promjenu veličine nije savršen dali kako dinamično Vrtnja gore RemoteApp i Vrtnja dolje virtualnim strojevima unutar zbirke, to će vam procjenu raspon podmreže pomoći. To je osobito važno kao kada servisa RemoteApp nalazi se u na VNET, ne možete povećati veličinu podmreže bez uklanjanja RemoteApp.

Za svaku zbirku RemoteApp koji želite pokrenuti na maksimalnog kapaciteta, imat ćete 100 IP adrese dostupna. Na primjer, ako imate jednu zbirku RemoteApp u standardni plan, a želite imati maksimalni broj 500 korisnika, imat ćete 100 IP adresa za tu zbirku. Isto tako, morate imati 100 IP adrese za zbirku RemoteApp u osnovni plan koji ima 800 korisnika. Ako planirate imaju manje korisnika (manje od najveće dopuštene), možete smanjiti IP adrese potrebno po zbirci. Veličina potrebna minimalne podmreže je 30 IP adrese (/ 27).

Pogledajte sljedeće informacije da biste provjerili vaše VNET je konfiguriran i radi propertly:

- [Migrirati iz osobne VNET u VNET programa Azure](remoteapp-migratevnet.md)
- [Provjerite valjanost Azure VNET će se koristiti za Azure RemoteApp](remoteapp-vnet.md)
