<properties
    pageTitle="Pregled Bus usluge preusmjeravanja | Microsoft Azure"
    description="Pregled Bus usluge preusmjeravanja."
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
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Pregled Bus usluge preusmjeravanja

Glavna komponenta Bus servisa je središnje (ali vrlo raspoređivačem) *Prijenos* servis koji vam omogućuje sastavljanje hibridnog programa koji se pokreću u Azure podatkovnog centra i vlastitu lokalnog okruženja enterprise.  Bus usluge preusmjeravanja podržava raznih protokola za različite prijenosa i web-servisa standarde. To obuhvaća SOAP WS-, *, te čak i OSTALE. Servis preusmjeravanja olakšava aplikacija hibridnog omogućujući vam da biste sigurno otkrili servisa Windows Communication Foundation (WCF) koji se nalaze u mrežu tvrtke tvrtke javno oblak bez otvaranja veze s vatrozidom ili većih promjena unutar poslovne mrežne infrastrukture su potrebne. 

![Prijenos koncepti](./media/service-bus-relay-overview/sb-relay-01.png)

Servis preusmjeravanja podržava tradicionalni jednosmjerna poruke, / odgovor na zahtjev razmjene poruka i-ravnopravnih članova razmjene poruka. Podržava i distribucija događaj pri internet opseg da biste omogućili scenariji objavljivanja/pretplate i dvosmjerni socket komunikacije s povećanom point-to-point učinkovitosti. 

U relayed razmjenu uzorak servis za lokalni povezuje usluge preusmjeravanja putem izlaznog priključak i stvara dvosmjerni socket za komunikaciju uz određenu rendezvous adresa. Klijent pa mogu komunicirati s lokalnim servisom slanjem poruke na servis preusmjeravanja za određivanje adresu rendezvous. Servis preusmjeravanja će zatim "preusmjeravanje" poruke na lokalni socket dvosmjerni već na mjestu. Klijent ne trebate izravne veze s lokalnim servisa, nije potrebno znati gdje se nalazi servis, a lokalnog servisa nije potrebna nikakve ulaznog priključke otvorena na vatrozida.

Pokretanje veza između lokalnog usluge i usluge preusmjeravanja pomoću paketa spajanja WCF "preusmjeravanja". U pozadini povezivanja preusmjeravanja mapirajte nove elemente povezivanje prijenosa dizajniran da biste stvorili komponente WCF kanala integrirati s Bus servis u oblaku. 

## <a name="next-steps"></a>Daljnji koraci

Detalje o Bus usluge preusmjeravanja, potražite u sljedećim temama.

- [Pregled za Azure Service Bus arhitekture](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Upute za korištenje usluge preusmjeravanja Bus servisa](service-bus-dotnet-how-to-use-relay.md)

 