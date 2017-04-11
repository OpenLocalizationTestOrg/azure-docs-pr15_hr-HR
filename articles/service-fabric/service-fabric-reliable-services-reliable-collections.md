<properties
   pageTitle="Pouzdan zbirke | Microsoft Azure"
   description="Servis tkanina s praćenjem stanja servisa omogućuju pouzdanog zbirke koje omogućuju pisanje iznimno dostupna, skalabilni i niska Latencija oblaka aplikacije."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Uvod u pouzdanog zbirke u tkanina servisa Azure s praćenjem stanja servisa

Pouzdan zbirke omogućuju pisanje iznimno dostupna, skalabilni i niska Latencija oblaka aplikacije kao da su pisanje aplikacije za jedno računalo. Klase u naziva **Microsoft.ServiceFabric.Data.Collections** navedite skup Izlaz u-tvorničke zbirke automatski dostupnim stanje iznimno. Razvojni inženjeri morati program samo za pouzdan API-ji zbirke i omogućuju pouzdanog zbirke upravljanje repliciranu i lokalne stanje.

Ključne razlika između pouzdanog zbirke i druge tehnologije visoke dostupnosti (primjerice Redis, servis za Azure tablice i usluga reda čekanja za Azure) je da stanje u kojem se sinkroniziraju lokalno instanca servisa tijekom izvršavaju iznimno dostupna. To znači da:

- Sve čitanja su lokalni, što rezultira niske latencije i čita visokom propusnošću.
- Sve zapisivanja plaćati najmanji broj IOs mreže, zbog čega niske latencije i piše visokom propusnošću.

![Slika proizvod zbirki.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Pouzdan zbirke možete smatrati prirodnim proizvod sustava klase **System.Collections** : novi skup zbirke namijenjene aplikacije oblaka i više računala bez povećava složenost za programer. Kao takve, pouzdanog zbirke su:

- Replicirati: Promjena stanja su replicirati za visoke dostupnosti.
- Ista i: Podataka je ista i na disk za rok trajanja protiv veliki kvarove (na primjer, bez napajanja podatkovnog centra).
- Asinkrona: API-ji su asinkronog da biste bili sigurni da niti se blokiraju kada povećavanja IO.
- Transakcijskih: API-ji koristi funkciju apstrakcije transakcije tako da se jednostavno upravljajte više zbirki pouzdanog unutar servisa.

Pouzdan zbirke omogućuju istaknuti dosljednost jamčiti iz okvir da biste olakšali zaključivanja o stanju aplikacije.
Osiguravanje transakcije pamti Završi tek nakon što se cijeli transakcije prijavljena na Većina kvorum od replike, uključujući primarni postiže se istaknuti dosljednost.
Da biste postigli weaker dosljednost aplikacije možete prihvatite vratite se u klijent/tražitelju prije vraća asinkronog potvrdi.

Pouzdan API-ji zbirke su je proizvod Istodobni zbirki API-ji (pronađeni u prostor za naziv **System.Collections.Concurrent** ):

- Asinkrona: Vraća zadatak jer, za razliku od Istodobni zbirke replicirati i dosljedan operacije.
- Više izgleda parametara: koristi `ConditionalValue<T>` da biste se vratili na booleovom i vrijednost umjesto out parametara. `ConditionalValue<T>`poput `Nullable<T>` , ali ne zahtijeva T da biste se s struct.
- Transakcije: Koristi transakcije objekt da biste omogućili korisnika u grupi Akcije u više zbirki pouzdanog u transakcije.

Danas, **Microsoft.ServiceFabric.Data.Collections** sadrži dvije zbirke:

- [Pouzdan rječnik](https://msdn.microsoft.com/library/azure/dn971511.aspx): predstavlja repliciranu transakcijskih i asinkronog skup parove ključa vrijednosti. Slično **ConcurrentDictionary**, ključ i vrijednost može biti bilo koje vrste.
- [Pouzdan reda čekanja](https://msdn.microsoft.com/library/azure/dn971527.aspx): predstavlja repliciranu transakcijskih i asinkronog izričite prvog-u, first-out (FIFO) reda. Slično **ConcurrentQueue**, vrijednost može biti bilo koje vrste.

## <a name="isolation-levels"></a>Razine odvajanja
Razinu odvajanja definira stupanj kojoj transakcije odvajanja iz promjene druge transakcije.
Postoje dvije razine odvajanja koje su podržane u pouzdanog zbirke:

- **Čitanje kontrolirani**: određuje izvješća nije moguće čitanje podataka koji sadrži izmijenili, ali još nije provedena tako da druge transakcije i bez transakcije možete mijenjati podatke koje je pročitao trenutne transakcije dok se ne dovrši trenutne transakcije. Dodatne informacije potražite u članku [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Snimka**: određuje podataka čitati sve naredbe u transakcije će biti putem transakcije dosljedan verziji podataka programa na početku transakcije.
Transakcije može prepoznati samo izmjene podataka koje su izvršene prije početka transakcije.
Podaci promjene tako da druge transakcije nakon početka trenutne transakcije nisu vidljive naredbe izvršavanja u trenutne transakcije.
Rezultat je kao ako izvješća u transakcije se snimka izvršenja podataka kao što je postojao na početku transakcije.
Brze snimke dosljedni u pouzdanog zbirkama.
Dodatne informacije potražite u članku [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Pouzdan zbirke automatski odaberite željenu razinu odvajanja za dani operacija čitanja ovisno o postupak i ulogu replike prilikom stvaranja transakcije.
Slijedi tablice koje prikazuje zadane razine odvajanja za pouzdan rječnik i reda čekanja operacija.

| Operacija \ uloga      | Primarni          | Sekundarni        |
| --------------------- | :--------------- | :--------------- |
| Čitanje jedan entitet    | Čitanje kontrolirani  | Brze snimke         |
| Numeriranje \ Count   | Brze snimke         | Brze snimke         |

>[AZURE.NOTE] Uobičajenih primjera za jedan entitet operacije su `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Pouzdan rječnik i pouzdan reda čekanja podržava čitanje vaše zapisivanja.
Drugim riječima, sve pisanje unutar transakcije će biti vidljiv sljedeće čitanje koji pripada istom transakcijom.

## <a name="locking"></a>Zaključavanje
U odjeljku pouzdanog zbirke sve transakcije su dvije osigurati: transakcije pustite zaključavanja sadrži dobivene dok transakcije završava na prekid ili na potvrdi.

Pouzdan rječnik koristi retka razinu zaključavanje za sve operacije jedan entitet.
Pouzdan reda čekanja trades isključivanje istodobnosti izričite transakcijskih FIFO svojstva.
Pouzdan reda čekanja koristi operacija razine zaključavanja dopuštanja jedan transakcija s `TryPeekAsync` i/ili `TryDequeueAsync` i jedan transakcija s `EnqueueAsync` odjednom.
Imajte na umu da da biste sačuvali FIFO, ako je `TryPeekAsync` ili `TryDequeueAsync` ikad uočiti da pouzdanog reda čekanja je prazan, oni će zaključati `EnqueueAsync`.

Pisanje operacije uvijek uzimaju zaključavanja isključujući te dvije vrijednosti.
Za dodatne operacije zaključavanje ovisi o nekoliko čimbenika.
Bilo koji operacija čitanja gotovo pomoću brze snimke odvajanja je zaključavanje besplatne.
Sve operacije kontrolirani čitanje prema zadanim postavkama otvara zaključavanja zajednički se koristi.
Međutim, za sve dodatne operacije koje podržava kontrolirani čitanje korisnika mogu zatražiti programa lock ažuriranje umjesto lock zajednički se koristi.
Zaključavanje je ažuriranje je asimetričnim lock koji se koristi da biste spriječili uobičajenih oblika zastoja zbog koji se pojavljuje kada više transakcije zaključavanje resursi za potencijalne ažuriranja kasnije.

Zaključavanje kompatibilnosti matrice nalazi se ispod:

| Zahtjev za \ odobreno | Ništa         | Zajedničko korištenje       | Ažuriranje      | Isključujući te dvije vrijednosti    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Zajedničko korištenje            | Nema sukoba  | Nema sukoba  | Sukob    | Sukob     |
| Ažuriranje            | Nema sukoba  | Nema sukoba  | Sukob    | Sukob     |
| Isključujući te dvije vrijednosti         | Nema sukoba  | Sukob     | Sukob    | Sukob     |

Imajte na umu isteka argument u pouzdanog API-ji zbirke služi za otkrivanje zastoja zbog.
Na primjer, dvije transakcije (T1 i T2) pokušavate čitati i ažurirati K1.
Moguće je za njih zastoja zbog, jer su oba završe ako lock zajednički se koristi.
U ovom slučaju jedne ili obaju operacije će istek vremena.

Imajte na umu da iznad scenarij zastoja zbog sjajno primjera kako je ažuriranje lock možete spriječiti deadlocks.

## <a name="persistence-model"></a>Postojanost modela
Pouzdan stanje i upravljanja pouzdanog zbirke slijedite postojanost modelu koji se naziva zapisnika i kontrolne točke.
To je model gdje se prijavili na disku i primijeniti samo u memoriji svake promjene stanja.
Dovršavanje stanje sam je ista i samo povremeno (poznatog Kontrolna točka).
Prednost je deltas pretvaraju u nizu samo dodaju zapisivanja na disku za bolje performanse.

Da biste bolje razumjeli modela zapisnika i kontrolne točke, najprije Pogledajmo scenarij beskonačno disk.
Pouzdan Upravitelj stanje zapisnike svaki postupak prije nego ga je replicirati.
Time se omogućuje pouzdanog zbirke da biste primijenili samo na operaciju u memoriji.
Budući da se ista su i zapisnika čak i kad je replike ne uspije, a potrebno je ponovno pokrenuti, pouzdanog stanje je dovoljno informacija u njegov zapisnicima na ponavljanje sve operacije replike se gube.
Disk je beskonačno, zapisnika zapise nikad ne morate ukloniti i pouzdan zbirke mora upravljanje samo stanje u memoriji.

Sada Pogledajmo scenarij konačne disk.
Kao zapise zapisnika skupiti, pouzdanog Upravitelj stanje će se pokrenuti nema dovoljno prostora.
Prije nego što se to dogodi, pouzdanog Upravitelj stanje treba skratiti njegov zapisnika da biste oslobodili prostor zapisa koji se noviji.
To će zatražiti pouzdanog zbirki prema Kontrolna točka njihovo stanje u memoriji disk.
To je pouzdan zbirke odgovornosti održati stanje do tog trenutka.
Kada pouzdanog zbirke dovršiti njihove checkpoints, pouzdanog Upravitelj stanja možete skratiti zapisnik da biste oslobodili prostor na disku.
Na taj način, kad replike potrebno je ponovno pokrenuti pouzdanog zbirke će vratiti njihovo checkpointed stanje, a pouzdanog Upravitelj stanje će oporavak i reprodukcija promjena stanja nastalih od Kontrolna točka.

>[AZURE.NOTE] Dodavanje druge vrijednosti od checkpointing je ga poboljšavaju performanse oporavak uobičajena slučaja.
To je zato checkpoints sadrže najnovije verzije.

## <a name="recommendations"></a>Preporuke

- Izmjena objekta prilagođene vrste vratio operacije čitanja (npr., `TryPeekAsync` ili `TryGetValueAsync`). Pouzdan zbirke, baš kao i Istodobni zbirke vraćanje reference na objekte i ne Kopiraj.
- Učinite kopiji niže vraćenog objekta u prilagođenu vrstu prije no što izmijenite ga. Budući da structs i ugrađene vrste su računanja prema vrijednosti, ne morate precizno Kopiraj na njima.
- Nemojte koristiti `TimeSpan.MaxValue` za istek. Istek treba koristiti da biste otkrili deadlocks.
- Nemojte koristiti transakcije kada je izvršenom, prekinut, ili rashodovana.
- Izvan opsega transakcije stvorena je pomoću enumeracija.
- Stvaranje transakcije unutar drugog transakcije `using` izjava jer se može uzrokovati deadlocks.
- Pobrinite se da vaše `IComparable<TKey>` implementaciju točni. Sustav vodi ovisnost o tome za spajanje checkpoints.
- Pomoću lock ažuriranje prilikom čitanja stavke s programa namjera ažurirajte ga da biste spriječili na deadlocks predmete.
- Razmislite o korištenju sigurnosno kopiranje i vraćanje funkcijama koje ste Izrada oporavak.
- Izbjegavajte miješanje više entitet postupke i jedan entitet operacije (npr. `GetCountAsync`, `CreateEnumerableAsync`) u istom transakcijom zbog odvajanja različite razine.

Evo nekih Napomena koje valja imati na umu:

- Prekoračenje vremena zadani je 4 sekunde za sve pouzdane zbirke API-ji. Većina korisnika treba zamijeniti to.
- Otkazivanje token zadani je `CancellationToken.None` u sve pouzdane zbirke API-ji.
- Parametar vrsta ključa (*TKey*) pouzdanog rječnika pravilno morate provesti `GetHashCode()` i `Equals()`. Strelicama mora biti immutable.
- Da biste postigli visoke dostupnosti za pouzdan zbirke, svaki servis moraju imati barem cilj i minimalne replike Postavljanje veličine 3.
- Čitanje operacije na sekundarnom može pročitati verzije koje nisu kvorum izvršenom.
To znači da se verzije podataka koje je za čitanje iz jedne sekundarni može biti false progressed.
Naravno, čitanja od primarne su uvijek stabilan: možete nikad se false progressed.

## <a name="next-steps"></a>Daljnji koraci

- [Pouzdani servisi brzi početak](service-fabric-reliable-services-quick-start.md)
- [Rad s pouzdanog zbirki](service-fabric-work-with-reliable-collections.md)
- [Pouzdani servisi obavijesti](service-fabric-reliable-services-notifications.md)
- [Pouzdani servisi sigurnosnog kopiranja i vraćanja (Izrada oporavak)](service-fabric-reliable-services-backup-restore.md)
- [Konfiguracija upravitelja pouzdanog stanja](service-fabric-reliable-services-configuration.md)
- [Početak rada s API-JA servisa tkanina Web services](service-fabric-reliable-services-communication-webapi.md)
- [Napredno korištenje pouzdanog servisa programiranje modela](service-fabric-reliable-services-advanced-usage.md)
- [Referenca za razvojne inženjere za zbirke pouzdanog mjesta](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
