
<properties
    pageTitle="Migriranje korisničkih podataka iz Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako migrirati korisničkih podataka i Azure RemoteApp."
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



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Upute za migraciju podataka u i Odjava iz njega Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Mnogo različitih alata i načine možete koristiti za prijenos [korisničkih podataka](remoteapp-upd.md) u i Odjava iz njega Azure RemoteApp. Evo nekoliko načina:

- Kopiranje i lijepljenje putem zajedničkog korištenja međuspremnik
- Kopiranje datoteke i podatke na poslužitelj datoteka
- Kopiranje datoteka na OneDrive za tvrtke putem preglednika
- Kopiranje datoteka pomoću alata za preusmjeravanje

>[AZURE.NOTE] 
> Ne možete omogućiti OneDrive za tvrtke ili potrošača agenti sinkronizaciju - one [nisu podržane](remoteapp-onedrive.md) u Azure RemoteApp.

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopiranje i lijepljenje u Eksploreru za datoteke

Kopiranje i lijepljenje koristi međuspremnik omogućena RemoteApp implementacijama [prema zadanim postavkama](remoteapp-redirection.md). Omogućuje korisnicima kopirali datoteke između svojih lokalne PC i RemoteApp aplikacija. Često kroz normalni tečaja korištenja aplikacija RemoteApp korisnici napravili datoteke da biste UPDs - premještanje da podatke iz RemoteApp jednostavno je:

1. [Objavljivanje Eksplorer za datoteke kao aplikacije](remoteapp-publish.md) u zbirci RemoteApp. (Imajte na umu da je to administrativnih zadataka.)
2. Izravni korisnike da biste pokrenuli aplikaciju Eksplorer za datoteke koji ste objavili i koristite kopirajte i zalijepite datoteke u njihove UPD i iz njega.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Prijenos datoteke i podatke na poslužitelj datoteka pomoću standardnih mrežnom kopijom datoteke

Često tvrtkama ili ustanovama koristiti poslužitelje datoteku da biste pohranili podatke Općenito. Ako znate naziv poslužitelja ili mjesta, vaši korisnici možete pregledavati lokalne mreže za poslužitelj, a zatim kopirajte svoje datoteke na njemu, slično kao što su jeste li iznad. Ponovno ćete htjeti objavite Eksplorer za datoteke RemoteApp i zatim ga zajednički koristiti s korisnicima.

>[AZURE.NOTE] 
> Poslužitelj datoteka mora biti na usmjeravati mrežu s kojom je RemoteApp uveden u.

## <a name="copy-files-to-onedrive-for-business"></a>Kopiranje datoteka na OneDrive za tvrtke
Iako ne možete omogućiti OneDrive za tvrtke sinkronizaciju agent RemoteApp, možete i dalje kopirati datoteke iz vaše UPD na OneDrive za tvrtke putem preglednika. 

1. Objavite Eksplorer za datoteke RemoteApp i zatim uputite korisnike da biste im pristupili putem te aplikacije. 
2. Najlakše ćete prenijeti datoteke ako spojene, da bi korisnici trebali biste stvarati .zip datoteku koja sadrži sve datoteke da biste premjestili na OneDrive za tvrtke.
3. Zatražite od korisnika da otvorite portal sustava Office 365 i idite na OneDrive i prijenos datoteke .zip.

## <a name="copy-files-by-using-drive-redirection"></a>Kopiranje datoteke pomoću pogon preusmjeravanje

Ako ste omogućili [preusmjeravanje pogon](remoteapp-redirection.md), već ste stvorili mapiranih pogon za korisnike. U tom slučaju možete zip svoje datoteke na preusmjereni pogon i ih spremiti na njihove lokalnom Računalu.