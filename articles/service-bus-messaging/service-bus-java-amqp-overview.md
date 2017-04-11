<properties 
    pageTitle="Pregled servisa Bus AMQP s Java | Microsoft Azure" 
    description="Informirajte se o korištenju Java s na napredne poruke stavljanje Protocol (AMQP) 1.0 u Azure." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>Podrška za AMQP 1.0 u Bus servisa

Servis u oblaku Bus servisa Azure i lokalno [Servisa Bus za Windows Server (servis Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) podržava na napredne poruke reda čekanja Protocol (AMQP) 1.0. AMQP vam omogućuje sastavljanje različite platforme, hibridnog aplikacije pomoću protokola open standard. Možete sastaviti aplikacije pomoću komponente koji su ugrađeni pomoću jezicima i okviri, a koji se pokreću na različitim operacijskim sustavima. Sve te komponente možete povezati Bus servisa i jednostavno razmjenjivati poruke strukturirane tvrtke učinkovito i na potpunoj.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Uvod: Što je AMQP 1.0 i zašto je važno?

Najčešći vezanima uz poruku proizvod proizvodi koriste služe protokoli za komunikaciju između klijentske aplikacije i Brokerske djelatnosti. To znači da kada odaberete razmjenu broker određenog dobavljača, morate koristiti taj dobavljača biblioteke za povezivanje klijentske aplikacije za taj broker. Ovo rezultira stupanj Opširnije o tom dobavljaču jer porting aplikaciju za neki drugi proizvod potreban je kod promjene u svim aplikacijama povezani. 

Osim toga, povezivanja razmjenu Brokerske djelatnosti iz različitih dobavljača je evidencija. To najčešće mora biti premošćivanja razini aplikacije za premještanje poruka iz sustava u drugi i prevođenja njihove oblika vlasničkih poruka. To je potrebna; na primjer, kada morate unijeti u starijim sustavima raznovrsne novo sučelje za Sjedinjeno komuniciranje, ili integrirati IT sustavi praćenja u spajanja tvrtki.

Industrijske softver je brzo premještanje business; Novi programskog jezika i okviri aplikacije uvode se katkad bewildering tempom. Isto tako, preduvjeti za IT sustavi poslovanja s vremenom i razvojni inženjeri želite iskoristiti najnovije značajke platforme. No ponekad odabrani razmjenu dobavljač ne podržava te platforme. Budući da su vlasničkih protokola za razmjenu poruka, nije moguće drugim korisnicima omogućuje biblioteke za te nove platforme. Dakle, morate koristiti pristupa kao što su stvaranje pristupnika ili bridges koji omogućuju vam da biste nastavili koristiti razmjenu proizvoda.

Razvoj programa na napredne poruke stavljanje Protocol (AMQP) 1.0 je motivirani po te probleme. Potječe pri Morgan JP Chase koji, kao što su najčešće financijske rada services, su u podebljanom korisnici vezanima uz poruku proizvod. Cilj je jednostavno: da biste stvorili protokol razmjenu programa Otvori standardna koji omogućuje sastavljanje aplikacije utemeljene na poruku pomoću komponente izgrađene pomoću jezicima, okviri i operacijskih sustava, svi koji koriste najbolje vrsta komponente iz raspona dobavljače.

## <a name="amqp-10-technical-features"></a>Tehnički značajke AMQP 1.0

AMQP 1.0 je programa učinkovitog pouzdan, žičani razinom razmjenu protokol koje možete koristiti da biste sastavili robusne, različite platforme, razmjenu aplikacije. Protokol je jednostavan cilj: da biste definirali mehanika sigurne, pouzdanog i učinkovito prijenos poruka između dvije strane. Poruka same kodirani su prikaz podataka koji omogućuje heterogenih pošiljatelja i primatelja za razmjenu poruka strukturirane tvrtke na potpunoj. Ovo je sažetak najvažnije značajke:

*    **Efficient**: AMQP 1.0 je veza usmjerena protokol te koristi u binarni kodiranja protokol upute i poruke koje su tvrtke prenijeti iznad nje. Ona sadrži sofisticirane kontrolu toka sheme Maksimiziranje korištenjem mreže i povezani komponente. Koje rečeno, protokol je namijenjenu pak postizanje ravnoteže između učinkovitosti, fleksibilnost i interoperabilnost.
*    **Reliable**: U AMQP 1.0 protokol omogućuje poruka razmijenjenih s raspon pouzdanosti jamstva, iz fire – i – zaboraviti pouzdan, točno-jednom označeni isporuke.
*    **Flexible**: AMQP 1.0 je fleksibilne protokol koji se mogu koristiti za podršku različite topologija. Isti protokol može se koristiti za klijenta za klijenta, broker klijenta i broker broker komunikaciju.
*    **Neovisno broker modela**: U AMQP 1.0 specifikacija provjerite neki preduvjeti na razmjenu modelu koji se koriste u broker. To znači da je moguće jednostavno dodati AMQP 1.0 podršku postojeće razmjenu Brokerske djelatnosti.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 je Standard (s u veliko slovo ")

AMQP 1.0 je u razvoju od 2008 skupina osnovni više od 20 tvrtki tehnologije dobavljače i rada krajnjeg korisnika. Tijekom tog razdoblja korisnik rada utječu svoje tvrtke stvarnog života preduvjeti i dobavljače tehnologije su razvile protokol za zadovoljava te preduvjete. Tijekom postupka, dobavljače su sudjelovali u workshops koji su surađivali da biste provjerili valjanost interoperabilnost njihove implementacije.

U listopad 2011 prebačena tehničke committee unutar tvrtke ili ustanove u napredovanje od strukturirane informacije standarde (ORGANIZACIJA) i 1.0 Standard AMQP za ORGANIZACIJA posla razvoj objavljen u listopad 2012. Sljedeće rada sudjelovali u tehničke committee tijekom razvoja standardni:

*    **Proizvođači tehnologija**: Axway softver, Huawei tehnologije, IIT softver, INETCO sustavima, Kaazing, Microsoft, Mitre Corporation, Primeton tehnologije, tijeku softver, crveno razgovor, SITA, softver AG, Solace sustavi, VMware, WSO2, Zenika.
*    **Korisnik rada**: banke Amerika, Švicarska kreditne kartice, Njemačke Boerse, Goldman Sachs, JPMorgan Chase.

Najčešće navedeni prednosti open standarde obuhvaćaju sljedeće:

*    Manje izgledi dobavljača lock u
*    Interoperabilnost
*    Široke dostupnosti biblioteka i tooling
*    Zaštita od obsolescence
*    Dostupnost dobro upućeni osoblja
*    LOWER i moguće upravljati rizika

## <a name="amqp-10-and-service-bus"></a>AMQP Bus 1.0 i usluge

Podrška za AMQP 1.0 u Bus servisa Azure znači možete sada pod utjecajem stavljanje Bus servisa i objavljivanja/pretplate brokered razmjenu značajke iz raspona platforme učinkovitog binarni protokolom. Osim toga, možete sastaviti aplikacije koji se sastoji od komponente izgrađene pomoću kombinacije jezika, okviri i operacijske sustave.

Sljedeća slika prikazuje implementacije sustava primjer u kojem Java klijenti pokrenuti na Linux koji se pišu pomoću standardne Java poruka servisa (JMS) API i .NET klijenti koji se izvodi u sustavu Windows, razmjenu poruka putem servisa Bus pomoću AMQP 1.0.

![][0]

**Slika 1: Primjer implementacije scenarij prikazuje različite platforme porukama pomoću servisa Bus i AMQP 1.0**

Trenutno sljedeće biblioteke klijent poznato je da biste radili s Bus servisa:

| Jezik | Biblioteka                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Klijent Apache Qpid Java poruka servisa (JMS)<br/>Klijent IIT softver SwiftMQ Java |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton-PHP                                                        |
| Python   | Apache Qpid Proton-Python                                                     |


**Slika 2: Tablica biblioteka klijentski AMQP 1.0**

Dodatne informacije o dobiti i pomoću te biblioteke servisa Bus potražite u članku [Vodič za servis Bus AMQP programere][]. Veze na dodatne informacije potražite u odjeljku [sljedeće korake](service-bus-java-amqp-overview.md#next-steps) .

## <a name="summary"></a>Sažetak

*    AMQP 1.0 je u otvorenim, pouzdanog razmjenu protokol koje možete koristiti da biste sastavili različite platforme, hibridnog aplikacije. AMQP 1.0 je ORGANIZACIJA standard.
*    Podrška za AMQP 1.0 sada je dostupna na Bus servisa Azure, kao i usluge Bus za Windows Server (servis Bus 1.1). Cijene, jednako je kao postojeće protokola.

## <a name="next-steps"></a>Daljnji koraci

Posjetite sljedeće veze na dodatne informacije o podršci za AMQP Bus servisa.

*    [Kako koristiti AMQP 1.0 s API za .NET Bus usluge](service-bus-dotnet-advanced-message-queuing.md)
*    [Upute za korištenje na Java poruka servisa (JMS) API servisa Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Vodič za Bus AMQP programiranje za servis][]
*    [ORGANIZACIJA napredne poruke stavljanje Protocol (AMQP) verzije 1.0 specifikacija](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Vodič za Bus AMQP programiranje za servis]: service-bus-amqp-dotnet.md

 
