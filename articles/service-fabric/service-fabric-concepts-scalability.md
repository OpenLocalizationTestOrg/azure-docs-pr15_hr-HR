<properties
   pageTitle="Skalabilnost servisa tkanina servisa | Microsoft Azure"
   description="Opisuje se kako skaliranje servisa tkanina services"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="scaling-service-fabric-applications"></a>Promjena veličine tkanina servisa aplikacija
Azure servisa tkanina olakšava izraditi skalabilni aplikacije ujednačavanje opterećenja servisa, particije i replike na sve čvorove klasteru. Time se omogućuje Upotreba Maksimalna resursa.

Visoke mjerilo za servis tkanina aplikacije mogu se postići na dva načina:

1. Promjena veličine na razini partition

2. Promjena veličine na razini naziv servisa

## <a name="scaling-at-the-partition-level"></a>Promjena veličine na razini partition
Servis tkanina podržava particija pojedinačnih servisa u više particija manje. [Particija pregled](service-fabric-concepts-partitioning.md) pruža informacije o vrstama stvaranje particija sheme koje su podržane. Replike svaki particija širi preko čvorove klasteru. Razmislite o servis koji koristi ranged shema particioniranja s malo tipke 0, Visoko ključ 99 i četiri particije. Tri čvor klasteru, servis možda biti podsjeća s četiri replike koji Omogućivanje zajedničkog korištenja resursa na svakom čvor kao što je prikazano ovdje:

![Particija raspored s tri čvorove](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Povećanje broja čvorove omogućuje tkanina servisa za iskorištavanje resursa na novi čvorovi pomicanjem neke na replike prazan čvorove. Povećavanjem broja čvorove bude četiri servis sada ima tri replike koji se izvode na svakom čvora (različite particije), čime se omogućuje bolju Upotreba resursa i performanse.

![Particija raspored s četiri čvorove](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Promjena veličine na razini naziv servisa
Instanca servisa je instancu naziv aplikacije, a naziv vrste usluge (pogledajte [tkanina servisa aplikacija životnog ciklusa](service-fabric-application-lifecycle.md)). Tijekom stvaranja servis navedete particija shema (pogledajte [particija tkanina servisa services](service-fabric-concepts-partitioning.md)) koja će se koristiti.

Prva razina skaliranje je naziv servisa. Možete stvoriti nove instance servisa s različitim razinama particija, kao na starije instanci servisa postaju zauzet. Time se omogućuje novi korisnici servisa da biste koristili instanci manje zauzet servisa, a ne one busier.

Jedna od mogućnosti za povećanje kapaciteta, kao i particija broji povećavati ili padajući jest da biste stvorili novu instancu servisa novu shemu particije. Time se dodaje složenosti, no kao sve dosta klijenti moraju znati kada i kako koristiti servis drugačije imenovani.

### <a name="example-scenario-embedded-dates"></a>Primjer scenarij: ugrađene datuma
Jedan scenarij moguće bi da biste koristili datumske informacije kao dio naziva servisa. Na primjer, koristite instanca servisa s određenim nazivom za sve korisnike koji se spajaju u 2013, a drugi naziv za korisnike koji je pridružen 2014.. Ova imenovanja shema omogućuje programski povećanje nazive ovisno o datum (kao pristupa 2014 instanca servisa za 2014 moguće stvoriti na zahtjev).

Međutim, ovaj pristup temelji se na klijente pomoću imenovanja informacije specifične za aplikacije koji se nalazi izvan opsega znanja tkanina servisa.

- *Korištenje programa konvencija imenovanja*: U 2013, kada aplikacija ide uživo, stvorite servisa naziva tkanina: / aplikacije/service2013. Pri drugi kvartal 2013, stvorite drugi servis naziva tkanina: / aplikacije/service2014. Obje od tih servisa su iste vrste servisa. Na taj se način klijent moraju uključivanja logike za sastavljanje naziv odgovarajuće usluge na temelju godine.

- *Putem servisa za pretraživanje*: je drugi uzorak sekundarni usluga koje možete unijeti naziv servisa za željenu tipku. Servis za pretraživanje pa stvaraju nove instance servisa. Sam servis za pretraživanje ne zadržava sve aplikacije podatke, samo podatke o nazivi servisa koji stvara. Dakle, za utemeljen na godine gornjem primjeru klijent bi prvi kontakt servis za pretraživanje da biste saznali naziv servisa obrada podataka za godine, a zatim pomoću taj naziv servisa za izvođenje operacije stvarni. Rezultat prvi pretraživanja mogu biti predmemorirane.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o servisa tkanina koncepata pogledajte sljedeće:

- [Dostupnost servisa tkanina servisa](service-fabric-availability-services.md)

- [Stvaranje particija servisa tkanina services](service-fabric-concepts-partitioning.md)

- [Definiranje i upravljanje stanja](service-fabric-concepts-state.md)
