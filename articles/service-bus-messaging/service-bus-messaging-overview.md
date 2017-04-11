<properties
    pageTitle="Bus usluge SMS pregled | Microsoft Azure"
    description="Servis Bus Messaging: isporuku fleksibilne podataka u oblaku"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Razmjenu poruka servisa Bus: isporuku fleksibilne podataka u oblaku

Microsoft Azure servisa Bus je servis za isporuku pouzdane informacije. Svrha taj servis je da biste olakšali komunikacije. Kada želite dva ili više stranama razmjenu informacija, koje su im potrebne mehanizam komunikacije. Servis Bus je mehanizam komunikacije brokered ili drugih proizvođača. To je slično poštanski servisa u svijetu. Poštanski servisi provjerite vrlo lako slati različite vrste pisama i paketa s raznim isporuke jamstva, bilo kojeg mjesta na svijetu.

Slično servis poštanski izlaganja slova, Bus servis je isporuke fleksibilne informacije iz pošiljatelja i primatelja. Servis za razmjenu osigurava se isporučuje podatke čak i ako su dvije strane nikad ne oba online istovremeno ili ako nisu dostupne istovremeno točan. Na taj način poruka slično je šalje pismo, dok komunikacije koje nisu brokered slično je stavljanju nazvati (ili kako nazvati se prije nalazila na – prije poziv na čekanju i pozivatelja ID koji su mnogo sličniji brokered porukama).

Pošiljatelj poruke možete zatražiti i brojne karakteristike isporuke, uključujući transakcije, duplikata, a zatim utemeljen na vrijeme isteka i grupnog slanja promjena. Ove uzorci imati kao i obične analogies: ponavljanje isporuke, potreban potpis, promijenite adresu ili opoziv.

Servis Bus podržava dva različita razmjenu uzorke: *prijenosa* i *brokered poruka*.

## <a name="service-bus-relay"></a>Prijenos Bus servisa

Komponenta [preusmjeravanja](../service-bus-relay/service-bus-relay-overview.md) servisa Bus je servis središnje (ali vrlo raspoređivačem) koji podržava raznih protokola za različite prijenosa i web-servisa standarde. To obuhvaća SOAP WS-, *, te čak i OSTALE. [Usluge preusmjeravanja](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) pruža mnoštvo mogućnosti povezivanja različite preusmjeravanja i omogućuju sklopite izravne veze ravnopravni članovi kad je to moguće. Servis Bus je optimiziran za razvojne inženjere .NET koje koriste Windows Communication Foundation (WCF), oba s obzirom na performanse i upotrebljivosti, i njihovi puni pristup njegove usluge preusmjeravanja putem sučelja SOAP i POSTAVITE. To omogućuje SOAP ni OSTALE programiranje okruženje za integraciju s Bus servisa.

Servis preusmjeravanja podržava tradicionalni jednosmjerna poruke, / odgovor na zahtjev razmjene poruka i-ravnopravnih članova razmjene poruka. Podržava i distribucija događaj pri Internet opseg da biste omogućili objavljivanje-pretplata scenariji i dvosmjerni socket komunikacije s povećanom point-to-point učinkovitosti. U relayed razmjenu uzorak servis za lokalni povezuje usluge preusmjeravanja putem izlaznog priključak i stvara dvosmjerni socket za komunikaciju uz određenu rendezvous adresa. Klijent pa mogu komunicirati s lokalnim servisom slanjem poruke na servis preusmjeravanja za određivanje adresu rendezvous. Servis preusmjeravanja će zatim "preusmjeravanje" poruke na lokalni socket dvosmjerni već na mjestu. Klijent nije potrebno izravne veze s lokalnim servisa niti je potreban je znati gdje se nalazi servis, a lokalnog servisa nije potrebna nikakve ulaznog priključke otvorena na vatrozida.

Uputite veza između lokalnog usluge i usluge preusmjeravanja pomoću paketa spajanja WCF "preusmjeravanja". U pozadini povezivanja preusmjeravanja mapirajte prijenosa povezivanje elementi stvoreni da biste stvorili komponente WCF kanala integrirati s Bus servis u oblaku.

Prijenos Bus servis nudi brojne prednosti, ali potrebno poslužitelja i klijenta i biti u mreži u isto vrijeme za slanje i primanje poruka. Nije optimalnih za komunikaciju HTTP stil u kojem zahtjeve možda neće biti obično long-lived ni za klijente povezati samo povremeno, kao što su preglednici, mobilne aplikacije i tako dalje. Brokered porukama podržava samostalne komunikacije, a ima vlastitu prednosti; Klijenti i poslužitelji možete povezati kada potrebne i izvođenje njihove operacija asinkronog način.

## <a name="brokered-messaging"></a>Brokered poruka

Za razliku od shemu preusmjeravanja [brokered porukama](service-bus-queues-topics-subscriptions.md) se mislili od kao asinkronog ili "dostavljanju samostalne." Proizvođača (pošiljatelja) i korisnici (primatelje) ne moraju biti u mreži u isto vrijeme. Infrastruktura za razmjenu pouzdano sprema poruke u "broker" (kao što su reda) dok dosta strana spreman primati. Time se omogućuje komponente distribuirane aplikacije prekidanjem, ili po želji; Ako, na primjer, za održavanje ili zbog neočekivanog komponente, bez utjecaja na cijelog sustava. Osim toga, primanju aplikacija može sadržavati samo tijekom određenog vremena dana, kao što je sustav upravljanja zalihama potrebnog samo za pokretanje na kraju dan tvrtke dolazi online.

Središnje komponente Bus servisa za razmjenu infrastrukture brokered su redova, teme i pretplate.  Primarno je da teme podržavaju mogućnosti objavljivanja/pretplate koje je moguće koristiti za složene utemeljen na sadržaj usmjeravanje i isporuke logike, uključujući slanje većem broju primatelja. Te komponente Omogućivanje novih asinkronog razmjenu scenarija, kao što su vremenski decoupling objavljivanja/pretplate te uravnoteženje opterećenja. Dodatne informacije o ove razmjenu entiteti potražite u članku [servis Bus redova, teme i pretplate](service-bus-queues-topics-subscriptions.md).

Kao i kod infrastruktura za prijenos brokered mogućnost razmjene navedeni su za programere WCF i .NET Framework i putem ostalih.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o razmjeni poruka Bus servisa, potražite u sljedećim temama.

- [Osnove Bus servisa](service-bus-fundamentals-hybrid-solutions.md)
- [Servis Bus redovi, tema i pretplate](service-bus-queues-topics-subscriptions.md)
- [Upute za korištenje servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)
- [Upute za korištenje servisa Bus teme i pretplate](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
