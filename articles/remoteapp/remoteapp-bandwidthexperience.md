<properties 
    pageTitle="Azure RemoteApp – kako učinite propusnost mreže i kvalitete iskustvo rada zajedno? | Microsoft Azure"
    description="Saznajte kako propusnost mreže Azure RemoteApp može utjecati na korisničkom kvalitetu sučelja."
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

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – kako učinite propusnost mreže i kvalitete iskustvo rada zajedno?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Kada gledate [ukupne propusnosti mreže](remoteapp-bandwidth.md) potrebne za Azure RemoteApp, imajte na umu sljedeće čimbenike: to su sve dio dinamički sustav koji utječe Sveukupna korisničkog sučelja. 

- **Dostupna propusnost mreže i trenutni uvjeti na mreži** – postavljanje parametara (Latencija, gubitka jitter) na istoj mreži u određeno vrijeme može utjecati aplikacije strujanje doživljaj, što znači da se spušteni cjelokupan korisničkog sučelja. Dostupna u vašoj mreži propusnost je funkcija zagušenja, slučajni gubitka, Latencija jer ti parametri utječe na kontrolu mehanizam zagušenja koji pak kontrole brzinu prijenosa da biste izbjegli sudara.  Ako, na primjer, s gubicima mrežu ili mreže s visokom latencijom će postati korisničko sučelje loša čak i na mreži s propusnost 1000 MB. Gubitak i Latencija razlikuju se ovisno o broju korisnika koji se nalaze na istoj mreži i što ti korisnici rade (na primjer, gledanje videozapisa, preuzimanje i prijenos velikih datoteka, ispis).
- **Korištenje scenarij** – sučelje ovisi o tome što korisnici rade kao osobe i grupe na istoj mreži. Na primjer, jedan slajd za čitanje zahtijeva samo jedan okvir da biste se ažurirati Ako korisnik skims i pomiče preko sadržaja tekst dokumenta, koje su im potrebne veći broj okvira ažurirati sekundi. Komunikacija i obrnuto u poslužitelja u ovom scenariju zauzeti će naposljetku više propusnost mreže. Preporučuje se primjer ekstremne: više korisnika su gledanje videozapisa visoke razlučivosti (kao što je razlučivost 4K), držite HD konferencijske pozive, reprodukcije 3D video igre ili rad na mogućnost umetanja sustavima. Sve te možete raditi čak i zaista visoke propusnosti mreže gotovo nestabilan.
- Za potpuno ažuriranje zaslonima veće od manjim zaslonima potreban je **razlučivost zaslona i broj zaslona** – dodatne propusnost mreže. Pozadinsku tehnologiju ne vrlo dobar posao kodiranja i prijenosom područja zaslone ažuriranih, ali jednom u je while cijelog zaslona potrebno ažurirati. Ako korisnik ima višu razlučivost zaslona (na primjer 4K razlučivost), se ažuriranje zahtijeva više propusnost mreže od zaslon s nižu razlučivost (kao što je 1024x768px). Ovaj isti logiku primjenjuje ako koristite više zaslona za preusmjeravanje. Da biste povećali s brojem zaslonima mora se propusnosti.
- **Preusmjeravanje međuspremnik i uređaja** – to je problem ne vrlo očite, ali u većini slučajeva ako korisnik pohranjuje velike bloka podataka u međuspremnik traje malo vremena za te podatke za prijenos s Udaljena radna površina na poslužitelj. Sučelje za do možete utječe pisanju upstream slanje sadržaja međuspremnika. Isto vrijedi za preusmjeravanje – ako skenera ili web-kamera daje velike količine podataka koji se šalje upstream s poslužiteljem ili pisač mora primiti velikih dokumenata ili lokalno spremište mora biti dostupne aplikacije izvodi u oblaku da biste kopirali velike datoteke, korisnici mogu primijetiti izostavljanje okvira ili privremeno "zamrznut" videozapis jer podatke koji su potrebni za preusmjeravanje uređaja povećava propusnost mreže mora. 

Kada procijenili vašim potrebama propusnosti mreže, provjerite je li svi od sljedećih čimbenika funkcionira u skladu s sustava.

Vratite se u [članku propusnosti glavni mreže](remoteapp-bandwidth.md)ili prijeđite na testiranje [propusnost mreže](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>uči više
- [Procjenu korištenja propusnosti mreže Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - testiranje vašeg korištenja propusnosti mreže s nekim uobičajeni scenariji](remoteapp-bandwidthtests.md)

- [Azure RemoteApp propusnost mreže - opće smjernice (Ako ne možete testirati vlastite)](remoteapp-bandwidthguidelines.md)