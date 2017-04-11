<properties
   pageTitle="Razlike između servisa u Oblaku i usluge tkanina | Microsoft Azure"
   description="Konceptualni pregled migracije aplikacije servisa u Oblaku tkanina servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Informirajte se o razlikama između servisa u Oblaku i usluge tkanina prije migracije aplikacije.
Tkanina usluge za Microsoft Azure je u oblak dalje generacije aplikacije za vrlo prilagodljivi, Visoko pouzdanog raspodijeljeno aplikacije. On predstavlja nove značajke za pakiranje, implementaciji, Nadogradnja i upravljanje aplikacijama raspodijeljeno oblaka. 

Ovo je uvodni vodič za migriranje aplikacije servisa u Oblaku tkanina servisa. Usredotočuje prvenstveno na arhitektonski i razlike u dizajnu između servisa u Oblaku i tkanina servisa.
 
## <a name="applications-and-infrastructure"></a>Aplikacije i infrastrukture

Osnovna razlika između servisa u Oblaku i tkanina servis je odnos između VMs, radnih opterećenja i aplikacijama. Radno opterećenje ovdje definirana je kao kod u pišete za obavljanje određenog zadatka ili pružanje usluge.
 
 - **Servisi u oblaku se o implementaciji aplikacije kao VMs.** Kod u pišete čvrsto je povezano s instancom VM, kao što su ulogu suradnika ili Web. Za implementaciju radno opterećenje na servise u Oblaku je za implementaciju jedan ili više instanci VM koji se izvode na radno opterećenje. Postoji bez odvojenosti aplikacije i VMs i tako da nema bez formalno definiciju aplikacije. Aplikaciju možete biti mislili od kao skup weba ili uloga suradnika instance unutar servisi u Oblaku za implementaciju ili cijele implementacije za servise u Oblaku. U ovom primjeru aplikacija prikazuje se kao skup uloga instance.
 
![Oblak servisa aplikacija i topologija][1]

 - **Servis tkanina je o implementaciji aplikacije postojeće VMs ili računala izvode tkanina servisa u sustavu Windows ili Linux.** Servisi u pišete su potpuno samostalne iz temeljnog infrastrukture koji je izvučene Odsutan u platforma aplikacije servisa tkanina tako da se aplikacija koje će biti implementirano u više okruženja. Radno opterećenje u servis tkanina jest "usluge", a jedan ili više usluga grupiraju se u formally definirani računala koja se izvršava na platformi aplikacije servisa tkanina. Više aplikacija može uvesti u jednom klaster tkanina servisa.
 
![Aplikacije servisa tkanina i topologija][2]
 
Servis tkanina sam je aplikacijskom sloju platformu koja se pokreće u sustavu Windows ili Linux, dok je servise u Oblaku sustava za implementaciju Azure upravlja VMs s radnih opterećenja priložene.
Model aplikacije servisa tkanina sadrži brojne prednosti:

 - Vrijeme brzog uvođenja. Stvaranje instance VM može dosta dugo trajati. U tkanina servisa VMs samo uvode se jednom za klaster koji hostira aplikacijske platforme tkanina servisa. Od tog trenutka na paketa aplikacije mogu biti implementirano u klaster vrlo brzo.
 - High-density hostirati. Na servise u Oblaku, VM za ulogu suradnika hostira jedan radno opterećenje. U tkanina servisa aplikacije razlikuju se od VMs koji se izvode, što znači da možete implementirati velikog broja aplikacijama mali broj VMs, možete spustiti ukupni trošak za veće implementacije.
 - Tkanina servisa platforme možete pokrenuti bilo koje mjesto koje sadrži Windows Server ili Linux strojeva li Azure ili na lokalnim poslužiteljima. Platforme nudi programa sloj apstrakcije putem podlozi infrastrukture da aplikacije mogu se izvoditi na različitim okruženjima. 
 - Raspodijeljeno Upravljanje aplikacijama. Servis tkanina je da ne samo domaćini distributed aplikacije, ali i pojednostavnjuje upravljanje životnog neovisno o hostinga VM ili životni ciklus računala.

## <a name="application-architecture"></a>Arhitektura aplikacije

Arhitektura aplikacije za servise u Oblaku obično sadrži brojne zavisnosti vanjskih servisa, kao što su Bus servisa, Azure tablice i spremište blobova platforme, SQL, Redis i drugima da biste upravljali stanje i podaci aplikacije i komunikaciju između Web- a uloge suradnika u implementaciji servisa u Oblaku. Primjer dovršeno aplikacije za servise u Oblaku može izgledati ovako:  

![Arhitektura servise u oblaku][9]

Aplikacije servisa tkanina možete koristiti isti vanjske servise u aplikaciji za dovršavanje. Koristite ovaj primjer arhitektura servise u Oblaku, najjednostavniji put migracije servisa u Oblaku za servis tkanina je zamjena samo implementacije servise u Oblaku aplikacijom servisa tkanina čuvanja cjelokupan arhitektura isti. Web- a tempiranja uloge možete biti podrazumijeva tkanina servisa bez praćenja stanja servisa s minimalnim kod promjene.

![Servis tkanina arhitektura nakon jednostavne migracije][10]

U ovoj fazi sustav treba dalje funkcionira na isti način kao prije. Snimanja prednosti servisa tkanina s praćenjem stanja značajki, vanjski stanje trgovine može biti internalized kao što je s praćenjem stanja servisa gdje je to primjenjivo. To je složenije od jednostavne migracije Web i uloge suradnika na servis tkanina bez praćenja stanja servisa, kao što je potrebna je pisanju prilagođene servise koje funkcionalnosti ekvivalentan u aplikaciji kao vanjskih servisa prije. Prednosti time obuhvaćaju sljedeće: 

 - Uklanjanje vanjske ovisnosti 
 - Unifying implementaciju, upravljanje i nadogradnje modela. 
 
Primjer dobivene arhitektura od tih servisa internalizing može izgledati ovako:

![Servis tkanina arhitektura nakon cijelog migracije][11]

## <a name="communication-and-workflow"></a>Komunikacija i tijeka rada

Većina aplikacija u Oblaku se sastoje od više razina. Isto tako, u aplikaciju servisa tkanina sastoji se od više servisa (obično mnoge servise). Dva uobičajena komunikacije modeli su direktnu komunikaciju i putem vanjskih durable pohranu.

### <a name="direct-communication"></a>Izravni komunikacije

S direktnu komunikaciju razine mogli komunicirati izravno putem krajnjoj točki koji prikazuje svaki sloju. U bez praćenja stanja okruženja kao što su servisi u Oblaku, taj znači da odaberete instance komponente ulogu VM ili slučajno ili kružnog da biste učitali saldo i izravno povezivanje njezin krajnju točku.

![Komunikacija s izravnim servise u oblaku][5]

 Izravni komunikacije je uobičajenih model komunikacije u tkanina servisa. Ključna razlika između web-mjesto servisa tkanina i u okvir za servise u Oblaku je te na servise u Oblaku povezati VM, dok je putem servisa tkanina možete se povezati s uslugom. To je važno razliku iz nekoliko razloga:

 - Servisi u servis tkanina su povezane s VMs koji hostira ih; servisi možda kretanje u klasteru i zapravo se očekuje da biste premještali različitih razloga: resursa za ujednačavanje, prebacivanje, aplikacije i infrastrukture nadogradnji i položaj ili učitavanje ograničenja. To znači da instanca servisa adresa možete promijeniti u bilo kojem trenutku. 
 - VM u tkanina servis možete hostirati većem broju servisa, svaka s jedinstvenim krajnje točke.

Servis tkanina nudi mehanizam servis za otkrivanje, pod nazivom servis dodjele naziva, koji se može koristiti da biste riješili adrese krajnju točku usluga. 

![Servis tkanina Izravni komunikacije][6]

### <a name="queues"></a>Reda čekanja

Uobičajeni mehanizam za komunikaciju između razine u okruženju bez praćenja stanja kao što su servisi u Oblaku jest korištenje red čekanja za vanjske prostora za pohranu za durably spremanje radnih zadataka iz jednog sloju u drugu. Uobičajeni scenarij je web razina koje šalje poslove Azure red ili servis Bus gdje ulogu suradnika instance možete dequeue i obrada poslove.

![Komunikacije reda čekanja servise u oblaku][7]

Isti model komunikacije može se koristiti u tkanina servisa. To može biti korisna prilikom migracije postojeću aplikaciju servisa u Oblaku za tkanina servisa. 

![Servis tkanina Izravni komunikacije][8]
 
## <a name="next-steps"></a>Daljnji koraci

Najjednostavniji put migracije servisa u Oblaku za servis tkanina je da biste zamijenili samo implementacije servise u Oblaku aplikacijom servisa tkanina čuvanja cjelokupan arhitektura aplikacije otprilike iste. Sljedeći članak sadrži vodič da biste lakše weba ili uloga suradnika pretvorili tkanina servisa bez praćenja stanja servisa.

 - [Jednostavan migracije: pretvorba Web ili uloga suradnika u servis tkanina bez praćenja stanja servisa](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
