<properties
   pageTitle="Na bilo kojem uređaju s Azure RemoteApp dobiti isti sučelja za Office 365 | Microsoft Azure"
   description="Saznajte kako zajednički koristiti bilo koju aplikaciju za Office 365 s korisnicima Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Na bilo kojem uređaju s Azure RemoteApp dobiti isti sučelja za Office 365

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

U ovom se članku obrađuje upute za implementaciju sustava Office 365 na bilo kojem uređaju u svojoj tvrtki. Korisnici mogu dobiti isti mogućnosti i sučelje za korisničko Sučelje na Android, Apple i Windows.

Ne možemo će to postigli pomoću hostiranje sustava Office 365 na skaliranje mogućnost virtualnim strojevima Azure koji se mogu povezati korisnici Azure RemoteApp. Ovaj skup virtualnih računala ne možemo poziva "oblaka zbirke".

## <a name="create-a-cloud-collection"></a>Stvaranje zbirke oblaka

Prvi put kada stvorite račun za Azure, pomaknite se do **RemoteApp** klikom na vezu na lijevoj strani.
![Prikazuje Azure RemoteApp portala za Azure](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Nastavite tako da kliknete **Novi** na dnu i "brzo stvaranje" zbirku. Navedite naziv, područje, pretplatu, plana i slike "Office Proffesional 2013" Nudimo.
![Dijaloški okvir stvaranje](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Kada završite s obrasca postupak stvaranja zbirke trebala. To može potrajati prema gore na jedan sat ili da.

![Čekanje](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Nakon dovršetka postupka izgleda otprilike ovako. Ako ne možemo kliknite **Objavljivanje** možemo vidjeti da većina aplikacije sustava Office objavljeni bismo već.
![Zbirka stvorili](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Objavljeni aplikacije](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Sada možete i dodati više korisnika koji imaju pristup za tu zbirku tako da kliknete **Korisničkog pristupa**.
![Konfiguriranje korisničkog pristupa](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Sada ćemo isprobavanje povezivanje sustava Office 365!

## <a name="connect-to-office-365"></a>Povezivanje sa sustavom Office 365

Ne možemo ćete glavni iznad da biste [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), pomaknite se dolje i kliknite **Preuzimanje klijente** instalirati klijent za Azure RemoteApp na uređaju koji koristite. Snimke zaslona koja se nalazi ispod su za Windows.

Nakon pokretanja aplikacije koje će se zatražiti da se prijavite pomoću Microsoftova računa (prije se zvao "Live ID"), koristi iste jedan kao račun za Azure Zasad. Kada ste prijavljeni u trebali biste vidjeti obavijest o novi pozivnice, kliknite nema i popisa kao što su nešto trebali biste vidjeti ispod. Prihvatite poziv koji odgovara račun za Azure vlasnik e-pošte.

![Novi poziv](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Kako izgleda kada postoje nove pozivnice.

![Prihvaćanje aplikacije](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Nakon što prihvatite poziv trebali biste vidjeti sve aplikacije sustava Office u klijent za Azure RemoteApp.

![Popis aplikacija](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Kada kliknete na bilo koju od tih aplikacija bi trebao počinjati na Azure virtualnog računala, a mora biti postavljena! Uživajte!

![pokretanje](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
