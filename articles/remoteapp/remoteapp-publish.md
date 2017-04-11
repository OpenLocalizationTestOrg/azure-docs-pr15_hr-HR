<properties
    pageTitle="Objavljivanje aplikacije Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako objaviti aplikacija i resursa u Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Kako objaviti aplikacije RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Kada stvorite zbirka RemoteApp, morate objaviti aplikacije ili resursa koje želite učiniti dostupnima za korisnike. Slika predložak koji ste dobili uz pretplatu tek nekoliko aplikacije objavljene po zadanom – da biste zajednički koristili u ostale aplikacije, morate ih objaviti.

> [AZURE.NOTE] Morate li ažurirati aplikaciju? Morat ćete [ažurirate sliku](remoteapp-update.md) prvi put.

Na kartici **Objavljivanje** na portalu kliknite **Objavi**. Možete dodavanje aplikacije s izbornika **Start** predložak slike ili navedite put do gdje je aplikacija instalirana na slici predložak. Ako odaberete da biste dodali s izbornika **Start** , odaberite aplikaciju za objavljivanje s popisa. Ako se odlučite za davanje put do aplikaciju, unesite naziv aplikacije i put aplikaciju. Pomoću varijabli u put – na primjer, "sistemski pogon %" umjesto "c:\".

> [AZURE.NOTE] Ako želite da biste dodali aplikaciju na izborniku **Start** , morate koristiti *dodaje tu aplikaciju da biste na * *pokretanje* * izbornik na sliku predložak.* U suprotnom RemoteApp vidjet će samo koje *je* na izborniku **Start** , a vi ćete biti zbuniti. 

>Da biste bili sigurni da je aplikacija na izborniku **Start** , postavite datoteka prečaca - **.lnk** - u mapi %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs.

> Ako ste zaboravili da biste dodali aplikaciju na izbornik **Start** prilikom stvaranja predloška, odaberite da biste dodali put aplikaciju. (Ili ponovno stvorite predložak sliku, ali koja je prilično veću rada).


 