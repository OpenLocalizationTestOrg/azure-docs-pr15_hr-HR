<properties 
    pageTitle="Servis Bus asinkronog poruka | Microsoft Azure"
    description="Opis servisa Bus asinkronog razmjene poruka."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asinkrona razmjenu uzoraka i visoke dostupnosti

Asinkrona porukama se mogu primijeniti na različite načine. Redovi, teme i pretplate Bus servisa Azure podržava asynchrony putem trgovine i prosljeđivanje mehanizam. U običnom (sinkronizirano) postupak, slanje poruka redova i teme i primanje poruka iz redova i pretplate. Aplikacija u pišete ovise o tome te entiteti uvijek budu dostupne. Prilikom promjene stanja entitet, zbog nekog od slučajeva, potrebno je način za pružanje entitet smanjene mogućnosti koje možete zadovoljavaju potrebe za većinu.

Aplikacija obično koriste asinkronog razmjenu uzorke da biste omogućili broj scenariji komunikacije. Možete izrađivati aplikacijama u kojima klijenti mogli slati poruke servise, čak i ako nije pokrenut servis. Za aplikacije te sučelje bursts komunikacije, a zatim u redu čekanja mogu pomoći Razina opterećenja unosom mjesto za komunikaciju u međuspremnik. Na kraju, možete dobiti na jednostavni, ali učinkovitih opterećenja za raspodjelu poruka na više računala.

Da biste zadržali dostupnost bilo kojeg od tih entiteti, razmotrite nekoliko različitih načina u kojoj te entiteti može se pojaviti dostupne za durable sustava za razmjenu. Općenito govoreći, Vidimo entitet postaju dostupni aplikacijama pisanju na sljedeće načine:

- Nije moguće poslati poruke.

- Nije moguće primati poruke.

- Nije moguće upravljati entiteti (Stvaranje, dohvatiti, ažuriranje i brisanje entiteti).

- Nije moguće pristupiti servis.

Za svaki od tih pogrešaka Načini rada s različitim neuspjeh postoji koja omogućuju aplikacije da biste nastavili s izvršiti neke razini smanjene mogućnosti. Na primjer, sustav koji možete slati poruke, ali ne i primati možete i dalje primati narudžbe kupaca, ali nije moguće obraditi te narudžbe. U ovoj se temi objašnjava potencijalne probleme koji se mogu pojaviti, a kako su mitigated te probleme. Servis Bus sadrži uvedene broj mitigations koje morate uključivanje u, a u ovoj se temi navode pravila korištenja te uključivanje mitigations.

## <a name="reliability-in-service-bus"></a>Pouzdanost u Bus servisa

Postoji nekoliko načina za rukovanje poruke i entitet problemi, a postoje smjernice koje se tiču odgovarajuće korištenje te mitigations. Da biste shvatili smjernice, najprije morate razumjeti što možete neće uspjeti Bus servisa. Zbog dizajna Azure sustavima, sve te probleme obično se short-lived. Visoke razine različite uzroka nedostupnosti prikazuju se na sljedeći način:

-   Ograničavanje iz s vanjskim sustavom servisa Bus ovisi. Ograničavanje pojavljuje se iz interakcije s resursima za pohranu i računalnim.

-   Problem za sustav servisa Bus ovisi. Dani dijela radnog prostora za pohranu, na primjer, mogu pojaviti problemi.

-   Pogreška servisa Bus na jednom podsustav. U tom slučaju računalnim čvor možete dobiti u kojima nisu dosljedno stanje i morate ponovno pokrenuti sam uzrokuje svi entiteti ga služi za učitavanje saldo na ostale čvorove. To shodno mogu prouzročiti kratko obrade sporo poruke.

-   Pogreška servisa Bus unutar Azure podatkovnog centra. Ovo je na "Katastrofalna pogreška" tijekom kojeg sustav nije dostupno za mnoge minuta ili nekoliko sati.

> [AZURE.NOTE] Razdoblja **za pohranu** može značiti Azure prostora za pohranu i SQL Azure.

Servis Bus sadrži broj mitigations za te probleme. U sljedećim se odjeljcima navode svako pitanje i njihov odgovarajući mitigations.

### <a name="throttling"></a>Ograničavanje

S Bus servisa ograničavanje omogućuje upravljanje stopa suradničkom poruke. Svaki pojedinačne čvor Bus servisa nalaze se brojni entiteti. Svaki od tih entiteti čini zahtjeve u sustavu procesora, memorije, za pohranu i drugih pozornici. Kada neki od ovih pozornici otkrije korištenje koji premašuje definirani pragovi, servis Bus možete ukinuti zahtjeva za dani. Pozivatelj prima [ServerBusyException][] i ponovnih pokušaja nakon 10 sekundi.

Kao ublažiti, kod mora pročitajte pogrešku i zaustavili sve ponovne pokušaje poruke najmanje 10 sekundi. Budući da se pogreška može dogoditi preko dijelove aplikacija klijenta, očekuje svakom dijelu neovisno izvršava logike pokušajte ponovno. Kod možete smanjiti vjerojatnost ograničio vrijeme omogućivanjem particija na redu ili temu.

### <a name="issue-for-an-azure-dependency"></a>Problem za Azure ovisnost

Druge komponente unutar Azure povremeno može imati problemi sa servisom. Ako, na primjer, nakon nadogradnje sustava koji koristi Bus servisa, tom sustavu možete privremeno sučelje smanjene mogućnosti. Da biste zaobišli te vrste problema, servis Bus redovito istražuje stanje i implementira mitigations. Prikazuju se strani efekata te mitigations. Ako, na primjer, učiniti tranzitne probleme s prostora za pohranu, Bus servisa implementira sustav koji omogućuje operacije slanja poruke da biste dosljedno rad. Zbog prirode na ublažiti, poslana poruka može potrajati do 15 minuta da se prikazuju u problematične reda čekanja ili pretplatu, a će spremna za primanje operacija. Općenito govoreći, većina entiteti ne pojave tog problema. Međutim, dobili broj entiteti u servis Bus unutar Azure, ovaj ublažiti ponekad potreban je za mali podskup kupci Bus servisa.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Servis Bus pogrešku na jednom podsustav

S bilo kojom aplikacijom okolnostima mogu prouzročiti Interna komponenta servisa Bus postati koje nisu usklađene. Kada servis Bus otkrije to, prikuplja podatke iz aplikacije da biste olakšali dijagnosticiranje što se dogodilo. Kada prikupljanja podataka u pokušaj da biste se vratili u ujednačeno stanje ponovnog pokretanja aplikacije. Ovaj postupak će se dogoditi prilično brzo, a rezultate u entitet prikažu biti dostupno za najviše nekoliko minuta, iako uobičajeni dolje puta su puno kraće.

U tim slučajevima klijentska aplikacija generira [System.TimeoutException][] ili [MessagingException][] iznimke. Servis Bus sadrži ublažiti za taj problem u obrascu logike pokušaj automatiziranog klijenta. Nakon što razdoblje pokušaj iskorištavanja i poruka je isporučena, možete istražiti pomoću drugih značajki kao što su [upareni prostora naziva][]. Upareni mjesta za naziv imaju druge upozorenja koji se spominju u tom članku.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Pogreška servisa Bus unutar Azure podatkovnim centrom

Većina probable razlog usmjerivanje Azure podatkovnog centra je nije uspio implementaciju nadogradnje servisa Bus ili zavisne sustava. Kao što je matured platforme, smanjiti je vjerojatnost ovu vrstu pogreške. Pogreška podatkovnog centra također može dogoditi razloga koji obuhvaćaju sljedeće:

-   Električni nedostupnosti (napajanja i generiranje power nestaju).
-   Povezivanje (internet prijeloma između klijenata i Azure).

U oba slučaja prirodnim ili man-made Izrada uzrokovale problem. Da biste to može zaobići i provjerite je li i dalje možete slati poruke, možete koristiti [upareni prostore za naziv][] da biste omogućili poruke slati na neko drugo dok primarno mjesto postala je dobar ponovno. Dodatne informacije potražite u članku [najbolje prakse za insulating aplikacije izvoditi na servis Bus kvarove i disasters][].

## <a name="paired-namespaces"></a>Upareni prostora naziva

Značajka [upareni prostora naziva][] podržava scenariji u kojima servis Bus entitet ili uvođenje unutar podatkovnog centra postane nedostupan. Dok je ovaj događaj diskovni, raspodijeljeno sustavi i dalje morate se pripremite za rukovanje najgoreg slučaja scenarija. Ovaj događaj obično događa jer neki element servisa Bus ovisi se pojavili kratkotrajni problem. Da biste zadržali dostupnosti aplikacija tijekom do prekida, Bus servisa korisnici mogu koristiti dvije zasebne prostora naziva, dajući prednost u zasebne podatke, za hostiranje njihove razmjenu entiteti. Ostatak ovog odjeljka koristi terminologija vezana uz sljedeće:

-   Primarni prostora za naziv: prostor naziva s kojim se aplikacije stupi u interakciju, za slanje i primanje operacije.

-   Sekundarni prostora za naziv: prostora za naziv koji funkcionira kao sigurnosnu kopiju za primarni naziva. Logika aplikacije interakciji imenski.

-   Prebacivanje interval: vrijeme da biste prihvatili normalni pogrešaka prije aplikacije prelazi iz primarnog polja naziva sekundarne naziva.

Uparena prostora naziva podrška za *Slanje dostupnosti*. Slanje dostupnosti zadržava mogućnost slanja poruke. Da biste koristili slanje dostupnosti, aplikacija mora zadovoljavati sljedeće uvjete:

1.  Samo primljene poruke od primarni naziva.

2.  Poruke poslane danom redu čekanja ili tema možda stignu izvan redoslijeda.

3.  Ako aplikacija koristi sesije, izvan redoslijeda možda stigne poruka unutar sesije. Ovo je prijelom s normalnom funkcionalnost sesije. To znači da program koristi da biste logično grupne poruke. Stanje sesije samo održavaju na primarni naziva.

4.  Izvan redoslijeda možda stigne poruka unutar sesije. Ovo je prijelom s normalnom funkcionalnost sesije. To znači da program koristi da biste logično grupne poruke.

5.  Stanje sesije samo održavaju na primarni naziva.

6.  Primarni reda čekanja mogu potjecati online i počnite prihvaća poruke prije sekundarne reda čekanja nudi sve poruke u primarni reda čekanja.

U sljedećim se odjeljcima navode API-ji, kako API-ji primjenjuju i prikazuje uzorak koda koji koristi značajku. Imajte na umu da su prisutne naplate posljedice povezane s tom značajkom.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>MessagingFactory.PairNamespaceAsync API-JA

Značajku upareni prostora naziva obuhvaća način [PairNamespaceAsync][] na [Microsoft.ServiceBus.Messaging.MessagingFactory][] klasa:

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Kada se Završi zadatak, uparivanje polja naziva se i potpune i spremni djelovanje u skladu s za sve [MessageReceiver][], [QueueClient][]ili [TopicClient][] stvorene pomoću [MessagingFactory][] instance. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] je osnovni klasa za različite vrste uparivanje koje su dostupne u [MessagingFactory][] objekt. Trenutno samo izvedena klasa je jedan imenovani [SendAvailabilityPairedNamespaceOptions][], koji implementira preduvjeti za slanje dostupnosti. [SendAvailabilityPairedNamespaceOptions][] ima skup constructors koje se temelje na drugome. Pogledate u Graditelj s većinom parametra, mogu razumjeti ponašanje drugih constructors.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Parametara imati sljedeće značenja:

-   *secondaryNamespaceManager*: inicijalizirana instance [NamespaceManager][] za sekundarne [PairNamespaceAsync][] način možete koristiti da biste postavili sekundarni naziva polja naziva. Upravitelj prostor naziva se koristi da biste dobili popis redova u prostoru naziva i da biste provjerili postoji redova potrebna zaostale. Ako ne postoji te redovi, oni se stvaraju. [NamespaceManager][] potrebna mogućnost da biste stvorili token zahtjeva za **Upravljanje** .

-   *messagingFactory*: instancu [MessagingFactory][] za sekundarne naziva. Objekt [MessagingFactory][] služi za slanje i, ako je svojstvo [EnableSyphon][] postavljeno na **true**, poruka iz redova zaostale.

-   *backlogQueueCount*: broj redova zaostale da biste stvorili. Ta vrijednost mora biti najmanje 1. Prilikom slanja poruka u zaostale jednu od ovih redova nasumično odabran. Ako je vrijednost postavite na 1, zatim samo jednog reda čekanja mogu ikad se. Kada se to dogodi, a jedan zaostale reda generira pogreške, klijent je nećete moći Isprobajte različite zaostale red i možda se neće poslati poruku. Preporučujemo da postavka tu vrijednost za neke veću vrijednost i Zadana vrijednost 10. To možete promijeniti više ili niže vrijednosti ovisno o tome koliko je podataka aplikacije šalje prema danu. Svakom zaostale redu čekanja mogu sadržavati do 5 GB poruke.

-   *failoverInterval*: količinu vremena tijekom kojeg ćete prihvatiti pogreške na primarni naziva prije prebacivanja sve jedan entitet sekundarne naziva. Failovers će se pojaviti na osnovi entitet prema entitet. Osobe u jednom naziva često uživo u različitim čvorove unutar servisa Bus. Pogreška u jedan entitet ukazuju na pogreške u drugu. Možete postaviti tu vrijednost da [System.TimeSpan.Zero][] prebacivanje sekundarnom odmah nakon pogreške u prilikom prvog, koji nisu prolaznim. Pokretanje timer prebacivanje problemi su sve [MessagingException][] u kojem je svojstvo [IsTransient][] false ili [System.TimeoutException][]. Ostale iznimke, kao što su [UnauthorizedAccessException][] prouzročiti prebacivanje, jer pokazuju nepravilno konfiguriran klijent. [ServerBusyException][] uzrokovati prebacivanje jer je točan uzorak čekati 10 sekundi pa ponovno poslati poruku.

-   *enableSyphon*: označava da ova određeni uparivanje treba i syphon poruke iz sekundarne naziva natrag u primarni naziva. Općenito govoreći, aplikacije koje se poruke slati trebali biste tu vrijednost postavite na **false**; aplikacije koje poruka mora tu vrijednost postavite na **true**. Razlog za to je često, ima li manje primatelja poruke od pošiljatelja. Ovisno o broju primatelja, možete odabrati da bi aplikacija za jedinstvenu instancu rukovati syphon obavezama koje imate. Korištenje mnogo primatelja sadrži naplate posljedice svaki zaostale red.

Da biste pomoću koda stvorite primarni [MessagingFactory][] instancu, sekundarne [MessagingFactory][] instancu, sekundarne instancu [NamespaceManager][] i [SendAvailabilityPairedNamespaceOptions][] instance. Poziv može biti lako i svodi se sljedeće:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Po dovršetku zadatka vratio metodu [PairNamespaceAsync][] sve postavljen i spreman za korištenje. Prije nego što se vraća zadatak, možda ne dovršite sav rad pozadine potrebne za na uparivanje rad desno. Kao rezultat koji nije trebala slanja poruka dok vraća zadatak. Ako je došlo do sve pogreške, kao što su neispravne vjerodajnice ili Nemogućnost stvaranja redova zaostale te iznimke će izbačena nakon dovršetka zadatka. Nakon što zadatak vraća, provjerite redova su pronaći ili stvorio Provjera svojstvo [BacklogQueueCount][] na vašoj [SendAvailabilityPairedNamespaceOptions][] instanci. Za šifru prethodni postupak pojavit će se na sljedeći način:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove asinkronog poruka u Bus servis, pročitajte dodatne detalje o [upareni prostora naziva][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Najbolje prakse za insulating aplikacije izvoditi na servis Bus kvarove i disasters]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [upareni prostora naziva]: service-bus-paired-namespaces.md