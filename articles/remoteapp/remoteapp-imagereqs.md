
<properties
    pageTitle="Azure preduvjeti slika RemoteApp | Microsoft Azure"
    description="Dodatne informacije o preduvjetima za stvaranje slike koje će se koristiti s Azure RemoteApp"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Preduvjeti za Azure RemoteApp slike

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp koristi Windows Server 2012 R2 slika za hostiranje sve programe koji želite zajednički koristiti s korisnicima. Da biste stvorili prilagođenu sliku, možete početi s postojeću sliku ili [stvorite novi](remoteapp-create-custom-image.md).

> [AZURE.TIP] Jeste li znali pretplate Azure RemoteApp omogućuje vam pristup sustava Windows Server 2012 R2 sliku u galeriji Azure VM koje možete koristiti da biste stvorili predložak sliku? [Odjavite](remoteapp-image-on-azurevm.md).  


Preduvjeti za sliku koja je moguće prenijeti za Azure RemoteApp su:


- Prilagođene aplikacije ne Pohranjujte podataka lokalno na slici. Te slike su bez praćenja stanja, a mora sadržavati samo aplikacijama.
- Slika sadrže podatke koji se mogu izgubiti.
- Veličina slike mora biti višekratnik broja MB prostora. Ako pokušaju prenijeti sliku koja nije višekratnik prijenos neće uspjeti.
- Veličina slike mora biti 127 GB ili manje.
- Mora se nalaziti na VHD datoteke (datoteke VHDX trenutno nisu podržani).
- Na VHD mora biti generacije 2 virtualnog računala.
- Na VHD može biti-veličine ili dinamički širi. Dinamički širi VHD preporučuje se jer je potrebno manje vremena da biste prenijeli na Azure od-veličina datoteke VHD.
- Na disku morate pokrenuti pomoću matrice pokretanje zapis (MBR) particija stil. Stil particije tablice (GPT) particije GUID nije podržana.
- Na VHD mora sadržavati jednu instalaciju sustava Windows Server 2012 R2. Može sadržavati više jedinica, ali samo jedan koji sadrži instalacija sustava Windows.
- Mora biti instaliran ulogu udaljene radne površine sesiju glavnog računala (RDSH) i značajku doživljaja radne površine.
- Morate ulogu Broker veze udaljene radne površine *nije* moguće instalirati.
- Šifriranje datotečni sustav (EFS) mora biti onemogućena.
- Slika mora biti SYSPREPed pomoću parametre **/OOBE / generalize Shutdown** (koji ne koristi parametar **/mode:vm** ).
- Prijenos na VHD iz snimke lanac nisu podržani.

Potražite u članku [Stvaranje slike Azure RemoteApp](remoteapp-imageoptions.md) dodatne informacije o stvaranju slike za Azure RemoteApp.
