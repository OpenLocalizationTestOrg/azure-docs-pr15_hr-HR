<properties
    pageTitle="Usporedba radnje Azure Mobile s ostalim servisima slične Azure"
    description="Usporedba radnje Azure Mobile s ostalim slične Azure servisima - HockeyApp, AppInsights, a zatim obavijesti koncentratora"
    services="mobile-engagement"
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Usporedba radnje Azure Mobile s ostalim servisima slične Azure

Na popis servisa Microsoft Azure koje nudi ikad proširivanja, a ponekad možda se pitate po čemu se razlikuje od taj servis koji ste upravo čitati ili primanje obavijesti o Azure Mobile radnje. U ovom se članku pokušava poništite zbrku i usmjerava odabir radnje Mobile Azure kada je najprikladniji za vaše korištenje. 
 
Azure Mobile radnje je servis namijenjeno isključivo **za digitalni često/CMOs** , ali mogu koristiti sve **aplikacije za mobilne uređaje vlasnik ili publisher** tko želi da biste povećali Upotreba, zadržavanja i monetization svoje mobilne aplikacije. 

*Je korisnik radnje platformu (SaaS) – kao-na-servis softvera koji omogućuje utemeljenih na podacima uvida u korištenje aplikacije, segmente korisnika u stvarnom vremenu i omogućuje contextually umu automatske obavijesti i poruka u aplikaciji.* 

Raščlaniti ove definicije, ćemo sa sljedećim karakteristikama ključa koje se ističu i njegov broj jedinstvenih vrijednosti:

1.  **Contextually umu automatske obavijesti i poruka u aplikaciji**
        
    Ovo je žarište core proizvoda – izvršavanje ciljnim i personalizirane automatske obavijesti. I to da se dogodi, ne možemo prikupljanje obogaćenog behavioral analize podataka. 

2.  **Utemeljenih na podacima uvida u korištenje aplikacije**

    Dajemo unakrsni platform SDK-ovi za prikupljanje behavioral analitičkih podataka o korisnicima aplikacije. Jer smo usredotočite se na način na koji korisnici aplikacije koristite aplikaciju Imajte na umu termina behavioral analize (as against analize performanse). Ne možemo prikupljanje podataka analize osnovni performanse pogreške, ruši itd, ali to znači ne fokusa core proizvoda. 

3.  **Segmente korisnika u stvarnom vremenu**

    Kada ste prikupili korisnici aplikacije behavioral analize podataka, ne možemo omogućuju segment publici utemeljene na razne parametrima i prikupiti podatke koji omogućuje pokretanje ciljano automatske kampanja. 

4.  **Softver-kao-na-service (SaaS):**

    Imamo neovisan portal za upravljanje Azure koji je optimiziran za interakciju i prikaz obogaćenih behavioral analitičke podatke o korisnicima aplikacije i pokretanje marketinških kampanja automatske portal. Proizvod je usmjerena za početak rada čas!   
 
Pomoću ovog skupa razlikovanja u skladištu Evo kako ćemo usporedbu druge prividno slične ponuda za Azure – bilješke u članku nema ugrađenu razine usporedbu značajki, ali potrebno više scenarij koji se temelje pristup da biste odredili koji proizvod najbolje funkcionira:
 
1.  [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) je tvrtke Microsoft mobilne DevOps rješenja. Pomoću njega možete distribucija izgradi za mogućnost beta, prikupljanje podataka pad sustava i prikupljanje povratnih informacija korisnika. Također se integrira s Visual Studio tim servisima omogućivanja jednostavno Sastavi implementacije i integracija stavke rad. 
    
    Ovdje fokus je na DevOps i prikupljanje podataka performanse analitičke podatke o mobilne aplikacije. Možda završe s integriranje oba HockeyApps i korištenje mobilne aplikacije i koje neće biti neobično jer čak i ako postoji neki preklapanja u prikupljene podatke, fokus temeljni proizvoda razlikuje se, a oni omogućuju da u postizanje različite ciljevi za vas.  

2.  [Aplikacija uvida](../application-insights/app-insights-overview.md) Ako je mobilne aplikacije na strani poslužitelja, pa će koristiti aplikaciju uvida praćenje strani poslužitelja web aplikacije, ali za mobilne aplikacije strani klijenta koristite HockeyApp. 

3.  [Obavijest o koncentratora](https://azure.microsoft.com/services/notification-hubs/) omogućuje laganih uslugu u smislu da ne morate SDK-a integrira u mobilnoj aplikaciji možete imaju potpunu kontrolu nad mobilne aplikacije i omogućuje slanje automatske obavijesti ima osnovni označavanje mogućnosti. Ovo je sjajno vlasnika bilo koju aplikaciju tko vodi brigu manje o ciljanja i više slanja/komunikacije informacije trenutačno da korisnici svoje aplikacije (emitiranje velike populaciji). 

    Imajte na umu je fokus na slanje blazing brzo obavijesti s mogućnošću pretraživanja kroz osnovne segmente. 

Pogledajmo o nekim scenarijima:

1.  Tim je dio digitalni marketinški tim u trgovini Adventure Works koje objavljuje mobilne aplikacije za korisnike. Tim-cilj je da biste bili sigurni da korisnike kako preuzeti svoje mobilne aplikacije nastaviti koristiti i često. Ovo se ne samo povećava na marke priložiti s na korisnici aplikacije, ali se povećava monetization kada korisnici aplikacije provjerite Nabava pomoću mobilne aplikacije. Za ovaj Tim morat ćete poslati ciljano obavijesti korisnici aplikacije koje su korisni, da biste potaknuli ih da biste otvorili aplikaciju i vratite se na njega često. To je mjesto Tim će korisni Mobile radnje. 

2.  Joann je dio od tima za razvoj mobilnih aplikacija na Adventure Works i traži proizvoda za pojedinosti o ruši, iznimke u zapisniku i performanse telemetrijskih iz aplikacije. To je mjesto Joann će korisni HockeyApp. Joann nije moguće integrirati i HockeyApp za svoj scenarije odnosila za razvojne inženjere i Azure Mobile radnje za Tim u aplikaciji isti da u potpunosti obje. 

3.  Robin je dio tim za razvoj mobilnih aplikacija na mrežu tvrtke Contoso novosti i sve Ana želi je pošaljite prekidanje novostima svim korisnicima bez koliko segmente čim aktivira upozorenje novosti. To je mjesto Robin će korisni koncentratora obavijesti. U ovom scenariju moguće je, no kao mobilne aplikacije stari, ima li zahtjev za učinite još mnogo bogatiji segmente i dohvaćanje detalja o ponašanju korisnika aplikacije. Trenutačno Robin morat će rezultirati Azure Mobile radnje. 
 
Recap, svrha radnje Mobile nije samo za prikupljanje analytics – nije "još jedan analize proizvoda iz Microsoft". Već je riječ o slanju ciljano automatske obavijesti i za ovaj ciljanja, ne možemo prikupljanje behavioral analize podataka, ali žarište ostaje na slanje automatskih obavijesti koji najviše odgovara korisnicima aplikacije tako da ga ne dolaze kao neželjena pošta. 

Više pojedinosti – pogledajte u ovom [kratkom videozapisu](mobile-engagement-overview.md) o mogućnostima Mobile ne duljimo. 

