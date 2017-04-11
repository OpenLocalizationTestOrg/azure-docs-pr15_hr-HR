<properties
   pageTitle="Arhitektura pouzdanog servisa | Microsoft Azure"
   description="Pregled pouzdanog servisa arhitektura za s praćenjem stanja i bez praćenja stanja servisa"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Arhitektura za s praćenjem stanja i bez praćenja stanja pouzdanog usluge

Na Azure servisa tkanina pouzdanog servis možda s praćenjem stanja ili bez praćenja stanja. Svaku vrstu servisa pokreće unutar određenog arhitektura. Ove arhitekturi su opisane u ovom članku.
Potražite u članku [Pregled pouzdanog servisa](service-fabric-reliable-services-introduction.md) dodatne informacije o razlikama između s praćenjem stanja i bez praćenja stanja servisa.

## <a name="stateful-reliable-services"></a>S praćenjem stanja servisa pouzdan

### <a name="architecture-of-a-stateful-service"></a>Arhitektura s praćenjem stanja servisa
![Arhitektura dijagrama s praćenjem stanja servisa](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>S praćenjem stanja servisa pouzdan

Možete s praćenjem stanja servisa pouzdanog proizlaziti iz StatefulService ili StatefulServiceBase predmete. Oba ove osnovne klase nudi tkanina servisa. Oni imaju različite razine podrške i apstrakcije s praćenjem stanja servisa da biste sučelja tkaninom servisa –, a da biste sudjelovali kao servis unutar servisa tkanina klaster.

StatefulService izvedena iz StatefulServiceBase. StatefulServiceBase nudi servise veću fleksibilnost, ali potrebno više razumijevanja internals tkanina servisa.
Potražite u članku [Pregled pouzdanog servisa](service-fabric-reliable-services-introduction.md) i [pouzdan servisa Napredno korištenje](service-fabric-reliable-services-advanced-usage.md) dodatne informacije o specifičnosti od servisa za pisanje pomoću klase StatefulService i StatefulServiceBase.

Oba osnovni Tečajevi upravljanje vijek i uloge o implementaciji servisa. Implementacija servisa zaobići virtualne metode ili klase li implementaciji servisa zadataka na tih točaka u životni ciklus implementaciji servisa – ili ga želi stvorite ga slušatelj objekt komunikacije. Imajte na umu iako servisa implementaciju mogu implementirati vlastitu objekt komunikacije ga slušatelj će ICommunicationListener, u dijagramu iznad, ga slušatelj komunikacije je implementirana putem servisa tkanina – kao što je servis implementaciji koristi ga slušatelj komunikacije koje je primijenjeno po tkanina servisa.

S praćenjem stanja pouzdanog servis koristi upravitelju pouzdanog stanja da biste iskoristili pouzdanog zbirke. Pouzdan zbirke su strukture lokalnih podataka dostupnih iznimno uslugu – koja je, dostupnost, bez obzira na to failovers servisa. Svaku vrstu pouzdanog zbirke je implementirana davatelj pouzdanog stanje.
Dodatne informacije o pouzdanog zbirke potražite u članku [Pregled pouzdanog zbirke](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Upravitelj pouzdanog stanje i stanje davatelja usluga

Upravitelj pouzdanog stanje je objekt koji se upravlja davatelja pouzdanog stanje. Ima funkciju za stvaranje, brisanje, Nabrajanje i davatelja pouzdanog stanje provjerite jesu li postojanog i vrlo dostupan. Instanca za davatelja usluge pouzdanog stanje predstavlja instance komponente strukturu postojanog i vrlo dostupnih podataka, kao što su rječnik ili red.

Svaki davatelj pouzdanog stanje izlaže sučelja koji koriste s praćenjem stanja servisa za interakciju s davateljem pouzdanog stanje. Ako, na primjer, koristi IReliableDictionary sučelje s pouzdanog rječnikom, dok se koristi IReliableQueue sučelje s pouzdanog reda čekanja. Svi davatelji pouzdanog stanje implementirati sučelje IReliableState.

Upravitelj pouzdanog stanje sadrži sučelje pod nazivom IReliableStateManager, koji omogućuje pristup je s s praćenjem stanja servisa. Sučelja pouzdanog stanje usluga vraćaju se kroz IReliableStateManager.

Upravitelj pouzdanog stanje koristi dodatak arhitektura tako da se nove vrste pouzdanog zbirki može biti kabel dinamički.

Pouzdan rječnik i pouzdan reda čekanja ugrađenih nakon implementacije visokih performansi, određuju razlikovno trgovine.

### <a name="transactional-replicator"></a>Transakcijskih umnažanje

Komponenta transakcijskih umnažanje je zadužen za osiguravanje stanja servisa (to jest, stanje unutar upravitelju pouzdanog stanje i pouzdan zbirke) je dosljedan preko svih replike pokrenut servis. Također osigurava da stanje u kojem je ista i prijava. Upravitelj sučelja pouzdanog stanje s transakcijskih umnažanje putem privatne mehanizam.

Transakcijskih umnažanje koristi mrežnih protokola za komunikaciju stanje s drugim replike instanca servisa tako da sva replike imaju informacije o stanju ažuran.

Transakcijskih umnažanje koristi zapisnik održati informacije o stanju tako da se informacije o stanju LIJEKA procesa ili čvor ruši. Sučelje za zapisnik je putem privatne mehanizam.

### <a name="log"></a>Zapisnik

Komponenta zapisnika pruža visokih performansi stalni spremište koje se mogu se optimizirati za pisanje na koji se vrti ili solid-state diskova.  Dizajn zapisnika je za trajni prostora za pohranu (odnosno tvrdi disk) lokalni čvorove koji rade s praćenjem stanja servisa. Time se omogućuje za manje latencies i visoke propusnost, to udaljene stalnih prostor za pohranu nije lokalan za čvor.

Komponenta zapisnika koristi više datoteka zapisnika. Postoji čvor razini zajedničku datoteku zapisnika koja sve replike koristite kao može osigurati Latencija najnižih i najviših propusnost za pohranu podataka o stanju. Po zadanom zajedničke zapisnika nalazi se u imeniku tvrtke čvor tkanina servisa, ali može se konfigurirati i smjestiti na drugo mjesto, najbolje na disk koji će biti rezervirana za zajedničke zapisnika. Svaki replike za servis je i namjenski zapisnik i zapisnik namjenski nalazi se u direktoriju rad na servis. Postoji bez mehanizam da biste konfigurirali zapisnik namjenski smjestiti na neko drugo mjesto.

Zapisnik zajedničke je prijelazne područje za informacije o stanju replike, dok je datoteka zapisnika namjenski konačni odredišni gdje ga je ista i. U ovom dizajn, informacije o stanju prvih zapisan zajedničku datoteku zapisnika i lazily premješten datoteku namjenski zapisnika u pozadini. Na taj način pisanja zajedničke zapisnika promijenile najniže Latencija i najveći propusnost koji omogućuje uslugu da bi brže tijeku.

Čita i zapisivanja u zapisnik zajedničke gotovi putem izravne IO preallocated prostor na disku zajedničke zapisnika. Da biste omogućili optimalnog korištenja prostora na disku sa zapisnicima namjenski, namjenski zapisničke datoteke stvara se kao datoteku kratke NTFS. Imajte na umu da to će omogućiti overprovisioning prostora na tvrdom disku i OS-a prikazat će namjenski zapisničke datoteke pomoću mnogo više prostora na disku nego što je zapravo koristiti.

Osim sučelje zapisnik s minimalnim korisničkom načinu rada zapisnik zapisuje kao upravljački program za otklanjanje način. Tako da koristi kao upravljački program za otklanjanje način zapisnik unijeti najveće performanse svim servisima ga koristiti.

Dodatne informacije o konfiguriranju zapisnik potražite u članku [Konfiguriranje s praćenjem stanja pouzdanog Services](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Bez praćenja stanja servisa pouzdan

### <a name="architecture-of-a-stateless-service"></a>Arhitektura bez praćenja stanja servisa
![Dijagram arhitektura bez praćenja stanja servisa](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Bez praćenja stanja servisa pouzdan

Bez praćenja stanja servisa implementacije proizlaziti iz klase StatelessService ili StatelessServiceBase. Klase StatelessServiceBase omogućuje veću fleksibilnost od StatelessService predmete.
Oba osnovni Tečajevi upravljanje vijek i uloge servisa.

Implementacija servisa zaobići virtualne metode ili klase li servis zadataka na tih točaka u životnom ciklusu servisa – ili ga želi stvorite ga slušatelj objekt komunikacije. Imajte na umu iako servis mogu implementirati vlastitu objekt komunikacije ga slušatelj će ICommunicationListener, u dijagramu iznad, ga slušatelj komunikacije je implementirana po tkanina servisa, kao što je taj servis implementaciji koristi ga slušatelj komunikacije koje je primijenjeno po tkanina servisa.

Potražite u članku [Pregled pouzdanog servisa](service-fabric-reliable-services-introduction.md) i [pouzdan servisa Napredno korištenje](service-fabric-reliable-services-advanced-usage.md) dodatne informacije o specifičnosti od servisa pomoću klase StatelessService i StatelessServiceBase za pisanje.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o tkanina servisa potražite u članku:

[Pregled pouzdanog servisa](service-fabric-reliable-services-introduction.md)

[Brzi početak rada](service-fabric-reliable-services-quick-start.md)

[Pregled pouzdanog zbirki](service-fabric-reliable-services-reliable-collections.md)

[Pouzdan servisa Napredno korištenje](service-fabric-reliable-services-advanced-usage.md)

[Konfiguracija servisa pouzdan](service-fabric-reliable-services-configuration.md)  
