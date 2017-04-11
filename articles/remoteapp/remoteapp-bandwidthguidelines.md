<properties 
    pageTitle="Azure RemoteApp propusnost mreže - opće smjernice | Microsoft Azure"
    description="Objašnjenje nekoliko smjernica propusnost osnovne mreže za Azure RemoteApp zbirke i aplikacije."
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
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp propusnost mreže - opće smjernice (Ako ne možete testirati vlastite)

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Ako nemate vremena ili mogućnost da biste pokrenuli [testira propusnosti mreže](remoteapp-bandwidthtests.md) za Azure RemoteApp, Evo nekoliko smjernica prilično generički koje omogućuju procjene propusnosti mreže po korisniku.

Ako imate kombinacije tih scenarija, ne preporučujemo nešto manje od (ili jednako) 10 MB/s kao propusnost mreže MINIMUM Moderna internetskom vezom aplikacije u okruženju udaljene. (Iako, kao što je navedeno, to će jamči je bolja od prosjeka korisnički doživljaj.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Složeni programa PowerPoint, jednostavno programa PowerPoint

Azure RemoteApp najbolje ne na LAN 100 MB. Na 10 MB/s mrežni profil kada je razrješava zahtjeve iznad 120 ms više od 5%, korisnik će vidjeti average iskustvo. U 1 MB/s različitim je glaring – na primjer, u dijaprojekciju, korisnik možda nećete vidjeti animirane prijelaze uopće jer su preskočeno slika.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer miješana PDF-a, PDF-a, teksta

U većini aspekte 10 MB/s mrežni profil je blizu LAN-a. 1 MB/s će ponuditi u redu, iako možda postoje neka razrješava zahtjeve kada korisnik pomiče dok su slike na zaslonu.

## <a name="flash-video-youtube"></a>Flash video (YouTube)

100 MB/s LAN-a nudi najbolje mogućnosti rada dok je 10 MB/s prihvatljiva (značenje smo održavanje stopa okvira, ali jitter povećava). 1 MB/s, razrješava zahtjeve biti vrlo Visoka i uočljivijih.

## <a name="word-typing-word-remote-input"></a>Word unosa (input udaljene Word)
Ovo je korištenje niske propusnosti scenarij. Pri 256 KB/s nudimo vam dobro, od iskustvo kao LAN-a.

## <a name="learn-more"></a>uči više
- [Procjenu korištenja propusnosti mreže Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp – kako učinite propusnost mreže i kvalitete iskustvo rada zajedno?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting vašeg korištenja propusnosti mreže s nekim uobičajeni scenariji](remoteapp-bandwidthtests.md)