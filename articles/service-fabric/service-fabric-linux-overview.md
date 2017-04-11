<properties
   pageTitle="Tkanina servisa Azure na Linux | Microsoft Azure"
   description="Servis tkanina klastere podržava Linux i Java, što znači da ćete moći uvesti i aplikacije tkanina servis glavnog računala pisane Java i C# na Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Servis tkanina na Linux

Pregled servisa tkanina na Linux omogućuje vam omogućuje stvaranje, uvođenje i upravljanje aplikacijama iznimno dostupan, Visoko skalabilni na Linux isti način kao u sustavu Windows. Okviri tkanina servisa (pouzdanog Services i pouzdan Glumci) dostupne su u Java na Linux uz C# (.NET Core).  Možete sastaviti i [izvršna servise za goste](service-fabric-deploy-existing-app.md) s bilo kojim jezik ili framework. Osim toga, pretpregled podržava orchestrating Docker spremnika. Docker spremnika mogu se izvoditi izvršne datoteke goste ili nativni tkanina servisa services koje koriste okviri tkanina servisa.

Servis tkanina na Linux jednako pojmovno tkanina servisa u sustavu Windows (osim specifičnosti OS i programskog jezika podrška). Dakle, većina naš [postojeće dokumentaciju](http://aka.ms/servicefabricdocs) primjenjuje olakšavaju Upoznavanje s tehnologija.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Podržani operacijski sustavi i programskog jezika

Ograničeno pretpregled ne podržava stvaranje klastere razvoj jedan okvir uz više strojne grupe klastere u Azure pokrenut 16.04 Ubuntu poslužitelja. Pretpregled podržava pouzdanog Glumci i okviri pouzdanog bez praćenja stanja servisa u Java i C# osim gosta izvršne datoteke i orchestrating Docker spremnika.  

>[AZURE.NOTE] Pouzdan zbirke nisu podržane u Linux još. Nisu podržani samostalno klastere za umetanje crtičnog koda ili – samo jednog okvira Azure Linux više strojne grupe klastere podržane su i u pretpregledu.

## <a name="supported-tooling"></a>Podržani tooling

Pretpregled podržava interakciju s klaster kroz Azure EŽA. Za razvojne inženjere Java, Integracija sa Eclipse i Yeoman je dao Eclipse podržane na Linux i OSX. Integracija OSX koristi Linux VM Pokaži napredne postavke putem vagrant. Za C# razvojnim inženjerima, Integracija s Yeoman navedeni su za generiranje predlošci aplikacije.

## <a name="next-steps"></a>Daljnji koraci


1. Upoznavanje s [Pouzdanog Glumci](service-fabric-reliable-actors-introduction.md) i [Pouzdan Services](service-fabric-reliable-services-introduction.md) programiranje okviri.

2. [Priprema razvojno okruženje na Linux](service-fabric-get-started-linux.md)

3. [Priprema razvojno okruženje na OSX](service-fabric-get-started-mac.md)

4. [Stvaranje prve Java tkanina servisa aplikacija na Linux](service-fabric-create-your-first-linux-application-with-java.md)
