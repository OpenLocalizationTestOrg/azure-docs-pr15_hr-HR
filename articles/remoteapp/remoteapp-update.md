<properties
   pageTitle="Ažurirajte vaše zbirke Azure RemoteApp | Microsoft Azure"
   description="Saznajte kako ažurirati zbirka Azure RemoteApp"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Ažuriranje zbirke RemoteApp Azure

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Prosljeđivala postoji jedan inevitably, kad trebate ažurirati aplikacije ili slike u sklopu Azure RemoteApp zbirke. Ako koristite neku od slike u sklopu pretplate Azure RemoteApp u oblaku ili hibridnog zbirci sve ažuriranja rješava Azure RemoteApp sam tako da položite jednostavno.

Međutim, ako koristite prilagođenu sliku (ugrađen od nule ili stvorio izmjenom neke od naše slike), koje ste zaduženi za održavanje slike i aplikacije. Ako je potrebno ažurirati sliku ili bilo koju od aplikacija unutar njega morate stvoriti novi, ažuriranu verziju slike, a zatim nova slika ažurirane zamijeniti postojeću sliku u zbirci web.

Dakle, kako vam se o ažuriranju zbirka? Dobro je prilično jednostavne:

1. Ažurirajte sliku koju ste koristili u zbirci web. Primjena zakrpa ni ažuriranja potrebna, a zatim je spremite pod novim nazivom.
2. [Prijenos](remoteapp-uploadimage.md) ili [Uvoz](remoteapp-image-on-azurevm.md) koju sliku da biste RemoteApp.
3. Sada na stranici zbirke kliknite **Ažuriraj**.
4. Odaberite novu sliku s popisa **Slika predložak** .
4. Ovdje je evidencija dio – morate odlučite kako želite da se baviti svi korisnici koji trenutno koristite aplikaciju u zbirci. Imate sljedeće mogućnosti:
    - **Korisnicima 60 minuta nakon ažuriranja**. Čim ažuriranje završi, Azure RemoteApp prikazat će poruke aktivni korisnici nemogućnosti spreme posao i odjavite i prijavite se ponovno prijavi. Nakon 60 minuta aktivni korisnici koji se niste prijavili isključivanje zapisat će se automatski. Korisnici mogu odmah prijavite.
    - **Prijava korisnika izvan odmah**. Čim ažuriranje završi, odjavite se svi korisnici automatski bez sve upozorenja. Ako odaberete tu mogućnost, korisnici mogu izgubiti podatke. Međutim, oni možete povezati aplikaciju odmah.

1. Kliknite kvačicu da biste pokrenuli ažuriranje.
