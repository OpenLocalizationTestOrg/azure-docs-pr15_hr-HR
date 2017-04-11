
<properties
    pageTitle="Nikad ne Pohranjujte povjerljive podatke na prilagođenu slike za Azure RemoteApp | Microsoft Azure"
    description="Dodatne informacije o smjernice za spremanje podataka u prilagođene slike u Azure RemoteApp"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Nikad ne Pohranjujte povjerljive podatke na prilagođenu slike

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Ako pak hostirate vlastite aplikacije RemoteApp Azure, na prvi je korak da biste stvorili prilagođenu sliku. Da biste stvorili VM instance koje služe aplikacija korisnicima koristimo tu prilagođenu sliku. Prilagođenu sliku smiju sadržavati samo aplikacije i nikad povjerljive podatke koji se mogu izgubiti, kao što su baze podataka SQL, osoblje datoteke ili posebne podatkovne datoteke kao što je QuickBooks datoteke za tvrtke. Sve povjerljive podatke trebali biste nalaze izvan Azure RemoteApp na datotečnom poslužitelju, drugi VM Azure, ili u SQL Azure. Slika treba samo hostira aplikaciju koja se povezuje s izvorom podataka i prikazuje podatke. Dodatne informacije pogledajte [preduvjete za Azure RemoteApp slike](remoteapp-imagereqs.md) . 

Da biste shvatili Zašto nećete pohranjivati povjerljive podatke, morate shvatiti način funkcioniranja Azure RemoteApp. Kada zbirka Stvori ili ažurira, u pozadini više clones ili kopije slike se stvaraju. Sve ove instance VM su točno replike prilagođenu sliku; Kada korisnici pokretanje aplikacije su povezani na neki od ovih VM instance. No u istoj instanci se zajamčeno i treba bitan jer su koje nisu stalni. Su instanci VM hosting aplikacije koje nisu stalni i mogu uništava ili izbrisati temelji, na primjer, tijekom zbirke ažuriranja. 

Nakon što je zbirka dodjeli i korisnici počnu povezati s VMs, je stalni korisničkih podataka, a jer on se sprema na zasebnom prostora za pohranu unutar na VHD ćemo poziva na [disku profila korisnika (UPD)](remoteapp-upd.md), što je korisnički profil u c:\users\<korisničkog profila >. Prilikom pokretanja aplikacije na UPD je postavljen i tretira se kao lokalne korisničkog profila za operacijski sustav. Saznajte više o korištenju [Azure RemoteApp sprema korisničke podatke i postavke](remoteapp-upd.md).

Oglednim podacima moraju se nalaziti na slici:

- Zajednički se koristi podatke za korisnike za pristup
- SQL DB ili QuickBooks DB
- Podatke u D:\

Oglednim podacima koji se mogu nalaziti na zadani profil za kopiranje u UPD za svakog korisnika:

- Datoteka konfiguracije po korisniku
- Skripte koje bi korisnici moraju sačuvati u svoje UPD

Važne stavke:

- Nikad ne spremište osjetljive podatke koji mogu se izgubiti na slici prilikom stvaranja prilagođenu sliku.
- Povjerljive podatke trebali biste uvijek nalaze na poslužitelju zasebne datoteke, odvojene Azure VM na u oblak i uvijek izvan instance VM hosting aplikacija unutar Azure RemoteApp. 
- Korisnički podaci se sprema i nastavi u korisničkog profila na disku (UPD)


