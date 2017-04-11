<properties
    pageTitle="Što je Azure preusmjeravanja? | Microsoft Azure"
    description="Pregled Azure preusmjeravanja"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Što je preusmjeravanja Azure?

Azure usluge preusmjeravanja olakšava aplikacija hibridnog omogućujući vam da biste sigurno otkrili servise koje se nalaze u mrežu tvrtke tvrtke javno oblak bez otvaranja veze s vatrozidom ili većih promjena unutar poslovne mrežne infrastrukture su potrebne. Prijenos podržava raznih protokola za različite prijenosa i web-servisa standarde.

Servis preusmjeravanja podržava tradicionalni jednosmjerna, / odgovor na zahtjev i promet ravnopravni članovi. Podržava i distribucija događaj pri internet opseg da biste omogućili scenariji objavljivanja/pretplate i dvosmjerni socket komunikacije s povećanom point-to-point učinkovitosti. 

U tansfer uzorak podataka povezivati servis za lokalni povezuje usluge preusmjeravanja putem izlaznog priključak i stvara dvosmjerni socket za komunikaciju uz određenu rendezvous adresa. Klijent pa mogu komunicirati s lokalnim servisom slanjem promet na servis preusmjeravanja za određivanje adresu rendezvous. Servis preusmjeravanja će zatim "preusmjeravanje" podatke na servis za lokalni kroz dvosmjerni socket trakom za svaki klijent. Klijent ne trebate izravne veze s lokalnim servisa, nije potrebno znati gdje se nalazi servis, a lokalnog servisa nije potrebna nikakve ulaznog priključke otvorena na vatrozida.

Elementi ključa mogućnost nudi preusmjeravanja se piše Dvosmjerno, unbuffered komunikacije preko granica mreže s TCP nalik ograničavanje, otkrivanje krajnje točke, povezivosti i preklapanje sigurnost krajnjoj točki. Mogućnosti preusmjeravanja na razlikuju se od mreže razinom Integracija tehnologije kao što su VPN-a u da ga može biti iz djelokruga krajnja točka jedne aplikacije na jednom računalu, dok je VPN tehnologija puno više većih kao što je ovisi mijenjanja mrežnom okruženju.

Azure preusmjeravanja sastoji se od dvije značajke:

1. [Hibridno veze](#hybrid-connections) – koristi se otvori standardne Web Sockets omogućivanjem scenarija multiplatform

2. [WCF preusmjeravanje](#wcf-relays) - koristi Windows Communication Foundation da biste omogućili daljinski proceduralne pozivi

Hibridno veze i WCF preusmjeravanje omogućuju sigurna veza assests koji postoje u mrežu tvrtke tvrtke. Korištenje jednu iznad druge ovisi o vašim potrebama za određeni detaljne ispod:

|                                    | WCF preusmjeravanja | Hibridno veze |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **Temeljni .NET**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Utemeljenih na standardima protokola Open**  |           |         x          |
| **Više RPC Programing modela** |           |         x          |
*<sub>Prema dostupnosti Općenito</sub>

## <a name="hybrid-connections"></a>Hibridno veze

Azure preusmjeravanja hibridnog veze je mogućnost sigurnog, Otvori protokol proizvod postojeće preusmjeravanja značajke koje se mogu implementirati na bilo kojoj platformi i u bilo koji jezik koji ima osnovni mogućnost WebSocket koji se izričito zajednički obuhvaća WebSocket API web-preglednika. Veza za hibridno temelji se na HTTP- a WebSockets.

## <a name="wcf-relays"></a>WCF preusmjeravanje

Prijenos WCF funkcionira za na punu .NET Framework (NETFX) i WCF. Pokretanje veza između lokalnog usluge i usluge preusmjeravanja pomoću paketa spajanja WCF "preusmjeravanja". U pozadini povezivanja preusmjeravanja mapirajte nove elemente povezivanje prijenosa dizajniran da biste stvorili komponente WCF kanala integrirati s Bus servis u oblaku.

## <a name="service-history"></a>Servis za povijest

Veza za hibridno transplants bivši, jednako pod nazivom "BizTalk Services" značajka koja je ugrađena na prijenos WCF Bus servisa Azure. Nova veza hibridnog mogućnost dopuna postojeće preusmjeravanja WCF i ove mogućnosti dva usluge će postoji jedno po drugo u servisu za prijenos u budućnosti foreseeable; zajedničko korištenje uobičajenih pristupnika, ali su inače različite implementacije.

WCF preusmjeravanja je naslijeđene preusmjeravanja koja nudi da više klijenata možda već koristite s WCF programing modela.

## <a name="next-steps"></a>Sljedeće korake:

- [Prijenos najčešća Pitanja](relay-faq.md)
- [Stvaranje prostor naziva](relay-create-namespace-portal.md)
- [Početak rada s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Početak rada s čvor](relay-hybrid-connections-node-get-started.md)