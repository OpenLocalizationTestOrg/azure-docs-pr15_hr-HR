<properties 
    pageTitle="Insulating Bus servisa aplikacija izvoditi na kvarove i disasters | Microsoft Azure"
    description="U članku se opisuje tehnike koje možete koristiti da biste zaštitili aplikacije protiv potencijalne nedostupnosti servisa Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Najbolje prakse za insulating aplikacije izvoditi na servis Bus kvarove i disasters

Zaštita njihove privatnosti ovise ključnih aplikacije mora djelovati pokretom, čak i Neplanirana kvarove ili disasters. U ovoj se temi opisuju tehnike koje možete koristiti da biste zaštitili aplikacije servisa Bus protiv potencijalne nedostupnosti servisa ili Izrada.

Do prekida se definira kao privremene nedostupnosti Bus servisa Azure. Na nedostupnosti može utjecati na neke komponente Bus servisa, kao što su razmjenu spremište ili čak i cijeli podatkovnog centra. Kada je problem riješen, servis Bus ponovo postane dostupna. Obično je nedostupnosti uzrokovati gubitak poruke ili druge podatke. Primjer neuspješne komponente je nedostupnosti određeni razmjenu trgovine. Primjer podatkovnog centra razini nedostupnosti je struje s podatkovnim centrom ili mreže skretnice neispravan podatkovnog centra. Do prekida može trajati od nekoliko minuta do nekoliko dana.

Na Izrada se definira kao trajni gubitak servisa Bus skaliranje jedinica ili podatkovnog centra. S podatkovnim centrom možda ili možda neće biti dostupna ponovno. Obično je Izrada uzrokuje pad neke ili sve poruke ili druge podatke. Primjeri disasters su fire, flooding ili earthquake.

## <a name="current-architecture"></a>Trenutni arhitekture

Servis Bus koristi više poruka trgovine za spremanje poruka koji se šalju redova ili teme. Koje nisu particije reda čekanja ili temu dodijeliti jedan razmjenu trgovine. Ako je ovo razmjenu spremište nije dostupan, sve operacije na tom reda čekanja ili tema neće uspjeti.

Svi Bus usluge SMS entiteti (redovi, teme, preusmjeravanje) nalaze u servis naziva koji je povezan s na podatkovnog centra. Servis Bus omogućiti automatsko zemlj. – replikaciju podataka niti je dopušteno prostora za naziv da biste se protežu na više podatkovnim centrima.

## <a name="protecting-against-acs-outages"></a>Zaštita protiv kvarove ACS

Ako koristite ACS vjerodajnice i ACS postane nedostupan, klijenti više ne možete dobiti tokena. Klijenti koji imaju token trenutku ACS funkcionira možete nastaviti koristiti servis Bus dok tokena istječe. Vijek tokena zadano je 3 sata.

Da biste zaštitili kvarove ACS, koristite tokeni zajednički pristup potpis (SAS). U ovom slučaju klijent potvrđuje izravno s servisa Bus tako da se prijavite koja se sama minted token s ključem tajnu. Pozivi ACS više nisu potrebni. Dodatne informacije o SAS tokeni potražite u članku [Bus servis za provjeru autentičnosti][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Zaštita redovima i teme protiv poruka pogreške trgovine

Koje nisu particije reda čekanja ili temu dodijeliti jedan razmjenu trgovine. Ako je ovo razmjenu spremište nije dostupan, sve operacije na tom reda čekanja ili tema neće uspjeti. Red čekanja particioniranom s druge strane, sastoji se od više fragmentirane. Svaki dio se pohranjuju u različite razmjenu trgovine. Prilikom slanja poruke da bi se particioniranom reda čekanja ili temu servisa Bus nešto na fragmentirane dodjeljuje poruku. Ako odgovarajuće razmjenu spremište nije dostupan, servis Bus zapisuje poruku različite fragment, ako je to moguće. Dodatne informacije o particioniranom entiteti potražite u članku [particije razmjenu entiteti][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Zaštita protiv kvarove podatkovnog centra ili disasters

Da biste omogućili prebacivanje između dva podatkovnim centrima, možete stvoriti polje naziva za servisa Bus servisa u svakom podatkovnog centra. Na primjer, servis Bus servisa prostor naziva **contosoPrimary.servicebus.windows.net** koje se možda nalaze u području Sjeverna središnje Sjedinjenih Američkih Država i **contosoSecondary.servicebus.windows.net** koje se možda nalaze u području NAM Jug/Central. Ako servis Bus poruka entitet mora biti dostupno x. podatkovnog centra prekida, možete stvoriti taj entitet u obje se prostori naziva.

Dodatne informacije potražite u odjeljku "Neuspjeh Bus servisa unutar Azure podatkovnog centra" u [asinkronog razmjenu uzoraka i visoke dostupnosti][].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Zaštita krajnje točke preusmjeravanja protiv kvarove podatkovnog centra ili disasters

Zemlj replikacije od krajnje točke preusmjeravanja omogućuje servis koji se izlaže preusmjeravanja krajnje bude dostupno x. kvarove Bus servisa. Da biste postigli zemlj replikacije servis morate stvoriti dvije preusmjeravanja krajnje točke u različitim prostorima naziva. Prostori moraju se nalaziti u različitim podatkovnim centrima i dva krajnje točke moraju imati različite nazive. Ako, na primjer, primarni krajnjoj točki proslijediti u odjeljku **contosoPrimary.servicebus.windows.net/myPrimaryService**dok postoji njegov sekundarne zamjena u obliku proslijediti u odjeljku **contosoSecondary.servicebus.windows.net/mySecondaryService**.

Servis zatim očekuje podatke i krajnje točke, a klijenta možete pozvati servis putem ili krajnjoj točki. Klijentska aplikacija slučajno izdvajanja nešto na preusmjeravanje kao primarni krajnjoj točki i šalje zahtjev za njegov aktivni krajnjoj. Postupak ne uspije kod pogreške, to označava krajnju točku prijenos nije dostupna. Aplikacija otvara kanala za sigurnosne kopije krajnjoj točki i reissues zahtjev. U tom trenutku aktivnog i sigurnosne kopije krajnje točke prebacivanje uloge: klijentska aplikacija smatra stare aktivni krajnju točku krajnju sigurnosne kopije i stare sigurnosne kopije krajnju točku biti aktivna krajnju. Ako i poslali operacije prekida, mijenjaju uloge dva entiteti i, vraća se pogreška.

Ogledna [zemlj replikacije sa servisa Bus povezivati poruka][] pokazuje kako replicirati preusmjeravanje.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Zaštita redovima i teme protiv kvarove podatkovnog centra ili disasters

Da biste postigli resilience protiv podatkovnog centra kvarove prilikom korištenja brokered poruka, servis Bus podržava dva pristupa: *aktivna* i *pasivni* replikacije. Za svaki pristup ako danom redu čekanja ili temu mora biti dostupno x. prekida u podatkovnim centrom možete stvoriti ga u obje se prostori naziva. Oba entiteti mogu imati isti naziv. Ako, na primjer, primarni reda čekanja proslijediti u odjeljku **contosoPrimary.servicebus.windows.net/myQueue**dok postoji njegov sekundarne zamjena u obliku proslijediti u odjeljku **contosoSecondary.servicebus.windows.net/myQueue**.

Ako aplikacija ne zahtijeva trajna pošiljatelja primatelj komunikacije, aplikaciju mogli implementirati durable klijentsko reda čekanja da biste spriječili gubitak poruke i shield pošiljatelja iz tranzitne servisa Bus pogreške.

## <a name="active-replication"></a>Aktivni replikacije

Aktivni replikacije koristi entiteti u obje se prostori naziva za svaki postupak. Bilo koji klijent koja šalje poruke šalje dvije kopije ista poruka. Prvi Kopiraj šalju se u primarni entitet (na primjer, **contosoPrimary.servicebus.windows.net/sales**), a drugi kopiju poruke šalju se u sekundarni entitet (na primjer, **contosoSecondary.servicebus.windows.net/sales**).

Klijent prima poruke iz obje redova. Primatelja obrađuje prvi kopiju poruke, a zatim je izostavljena drugu kopiju. Izostavlja duplicirane poruke pošiljatelja morate oznaku svaku poruku s Jedinstveni identifikator. Obje kopije poruke mora biti označene s istim identifikatorom. Svojstva [BrokeredMessage.MessageId][] ili [BrokeredMessage.Label][] ili prilagođeno svojstvo možete koristiti da biste označili poruku. Primatelja morate zadržati popis poruka koja je već dobila.

Ogledna [zemlj replikacije sa servisa Bus Brokered poruka][] pokazuje aktivni replikacije razmjene poruka entiteti.

> [AZURE.NOTE] Pristup aktivni replikacije doubles broj operacija, stoga se odlučile može dovesti do veći trošak.

## <a name="passive-replication"></a>Pasivne replikacije

U slučaju kvara slobodno pasivni replikacije koristi samo jedan od dva razmjenu entiteti. Klijent šalje poruku u aktivni entitet. Ne uspijete postupka na aktivni entitet kod pogreške koja pokazuje s podatkovnim centrom na kojem je smještena aktivni entitet možda neće biti dostupne, klijent šalje kopiju poruke sigurnosne kopije entitet. U tom trenutku aktivnog i sigurnosne kopije entiteti prebacivanje uloge: slanje klijent smatra stare aktivni entitet se novi sigurnosne kopije entitet, a stare sigurnosne kopije entitet je novi aktivni entitet. Ako i poslali operacije prekida, mijenjaju uloge dva entiteti i, vraća se pogreška.

Klijent prima poruke iz obje redova. Jer postoji mogućnost da prima da primatelja dvije kopije ista poruka, primatelja morate izostavi duplicirane poruke. Duplikate možete prekinuti na isti način kao što je opisano za replikaciju active.

Općenito govoreći, pasivni replikacije je više najekonomičniji od aktivnog replikacije jer u većini slučajeva se provodi operacija samo jedan. Latencija, propusnost i novčane trošak jednaki scenarij koji nisu replicirati.

Prilikom korištenja pasivni replikacije u sljedećim scenarijima poruke možete biti izgubljene ili Primljeno dvaput:

-   **Odgađanje poruke ili gubitka**: pretpostavlja da pošiljatelj uspješno poslana poruka m1 primarni red, a zatim postane nedostupan reda čekanja prije primatelja prima m1. Pošiljatelj šalje poruku koja slijedi m2 sekundarne red. Ako je primarni reda čekanja privremeno nedostupan primatelja prima m1 nakon redu čekanja postane dostupan ponovno. U slučaju Izrada, primatelja nikad primiti m1.

-   **Dupliciranje prijam**: pretpostavlja pošiljatelj šalje poruku m primarni red. Servis Bus uspješno obrađuje m, ali ne uspijeva slati odgovor. Nakon što postupak slanja ističe, pošiljatelj šalje identične kopiju m sekundarne red. Ako je primatelja primati prvi kopiju m prije primarni reda čekanja postane dostupan, primatelja prima i kopija m približno u isto vrijeme. Ako je primatelja primati prvi kopiju m prije primarni redu čekanja postane nedostupan, primatelja prethodno prima samo drugu kopiju m, ali prima drugi kopiju m, kada primarni redu čekanja postane dostupna.

Ogledna [zemlj replikacije sa servisa Bus Brokered poruka][] pokazuje pasivni replikacije razmjene poruka entiteti.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o oporavku Izrada potražite u sljedećim člancima:

- [Neprekidno poslovanje Azure SQL baze podataka][]
- [Tehnički vodič Azure otpornost][]

  [Provjera autentičnosti Bus servisa]: service-bus-authentication-and-authorization.md
  [Particioniranom entiteti za razmjenu poruka]: service-bus-partitioning.md
  [Asinkrona razmjenu uzoraka i visoke dostupnosti]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Zemlj replikacije sa servisa Bus povezivati poruke]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Zemlj replikacije sa servisa Bus Brokered poruke]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Neprekidno poslovanje Azure SQL baze podataka]: ../sql-database/sql-database-business-continuity.md
  [Tehnički vodič Azure otpornost]: ../resiliency/resiliency-technical-guidance.md
