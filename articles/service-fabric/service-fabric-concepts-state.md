<properties
   pageTitle="Definiranje i upravljanje stanje | Microsoft Azure"
   description="Definiranje i upravljanje stanje servisa tkanina servisa"
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

# <a name="service-state"></a>Stanje servisa
**Stanje servisa** se odnosi na podatak koju ovaj servis zahtijeva da bi funkcionirati. Obuhvaća strukture podataka i varijabli koje servis čita i piše za obavljanje posla.

Razmislite o jednostavni kalkulator servis, na primjer. Taj servis koristi dvaju brojeva i vraća njihov zbroj. Ovo je isključivo bez praćenja stanja servisa koji nema podataka povezan s njom.

Sada razmislite o isti kalkulator, ali uz računalno zbroj ima način za vraćanje zadnje ga sadrži izračunati zbroj. Taj servis je sada s praćenjem stanja – sadrži neke stanje u kojem je zapisuje (kada je formula izračunava nove zbroj) i čitanja iz (kada je vraća zadnji izračunati zbroj).

U tkanina servisa Azure, prvi servis naziva bez praćenja stanja servisa. Drugi servis je pozvan s praćenjem stanja servisa.

## <a name="storing-service-state"></a>Spremanje stanje servisa
Stanje možete biti externalized ili Suradnja nalazi kod koji je rukovanje stanje. Externalization stanja obično učiniti pomoću vanjske baze podataka ili iz trgovine. U našem primjeru Kalkulator to može biti bazom podataka SQL trenutni rezultat u kojoj je pohranjen u tablici. Svaki zahtjev za izračun zbroja izvodi ažuriranje na redak.

Stanje može biti Suradnja nalazi kod koji upravlja kod. S praćenjem stanja servisa u servis tkanina ugrađenih pomoću ovog modela. Tkanina servis nudi infrastrukturu da biste bili sigurni da je to stanje iznimno dostupan i kvara pogreške u slučaju pogreške.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o servisa tkanina koncepata pogledajte sljedeće:

- [Dostupnost servisa tkanina servisa](service-fabric-availability-services.md)

- [Skalabilnost servisa tkanina servisa](service-fabric-concepts-scalability.md)

- [Stvaranje particija servisa tkanina services](service-fabric-concepts-partitioning.md)
