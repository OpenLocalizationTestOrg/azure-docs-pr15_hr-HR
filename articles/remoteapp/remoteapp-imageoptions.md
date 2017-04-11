<properties
    pageTitle="Stvaranje slike Azure RemoteApp | Microsoft Azure"
    description="Dodatne informacije o mogućnostima koje su dostupne za stvaranje slike za Azure RemoteApp"
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



# <a name="create-an-azure-remoteapp-image"></a>Stvaranje slike Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp koristi slike na čuvanje aplikacije koju šaljete drugim korisnicima. (Ne možemo poduzeti sliku te ga koristiti da biste stvorili VMs – to je koje korisnicima programa access, kada se prijavite u Azure RemoteApp.) Da biste stvorili zbirku programa Azure RemoteApp uz odabir aplikacije, hoće li se oblaka ili hibridnog, počnite tako da stvorite sliku s tim instalirane aplikacije. Zatim stvorite zbirku koja koristi tu sliku, dodijeliti korisnicima zbirke i objavljivanje aplikacije tim korisnicima.

Imate nekoliko mogućnosti za stvaranje i korištenje slika. Osnovni [zahtjeva](remoteapp-imagereqs.md) za sliku je da ga pokrenuti Windows Server 2012 R2 i imati instaliran ulogu udaljene radne površine sesiju glavnog računala (RDSH). Kako doći koji je gdje pronaći zanimljive stvari.

Kada je riječ o slike imate sljedeće mogućnosti:

- Možete uvesti i koristiti je [slika na temelju Azure virtualnog računala](remoteapp-image-on-azurevm.md). To je dobro redak tvrtke aplikacije za koje su potrebne prilagođene postavke. Možete prilagoditi slike tako da radi za aplikaciju.
- Možete [stvoriti i prenijeti prilagođenu sliku](remoteapp-create-custom-image.md). To je dobro ako već imate sliku koju koristite za lokalnu implementaciju za udaljene radne površine.
- Koristite neku od [Slika predložaka](remoteapp-images.md) obuhvaćeno pretplatom na RemoteApp. Te slike se stvaraju i održava tima RemoteApp i sadrže neke standardne aplikacije (kao što je paketa Office) koje možete učiniti dostupnom korisnicima. Imajte na umu samo na Office 365 Pro Plus slike mogu se koristiti u radnog postavka.

Bez obzira na to gdje se sliku ili kako stvoriti, ćete htjeti provjerili [preduvjeti za aplikaciju](remoteapp-appreqs.md) da biste bili sigurni da se aplikacijom funkcionira dobro u RemoteApp. Zatim, sljedeći je korak da biste stvorili [oblaka](remoteapp-create-cloud-deployment.md) ili [hibridnog](remoteapp-create-hybrid-deployment.md) zbirke.
