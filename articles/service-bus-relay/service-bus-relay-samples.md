<properties 
    pageTitle="Bus usluge preusmjeravanja uzoraka pregled | Microsoft Azure"
    description="Kategorizira, a zatim u članku se opisuje Bus usluge preusmjeravanja uzorka s vezama na svakom."
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
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Bus usluge preusmjeravanja uzorka

Uzorci preusmjeravanja servisa Bus demonstrirati ključne značajke u [Bus usluge preusmjeravanja](https://azure.microsoft.com/services/service-bus/). U ovom se članku kategoriziranje i opisuje uzoraka dostupne s vezama na svakom.

>[AZURE.NOTE] Uzorci Bus servisa nisu instalirane s SDK-a. Da biste nabavili uzorcima, posjetite [Azure SDK uzoraka stranice](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Uz to, postoji ažurirani skup Bus usluge preusmjeravanja uzoraka [ovdje](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (od pisanja ovog oni ne opisane u ovom članku).  

Razmjenu uzorka, potražite u članku [servis Bus poruka uzorka](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Bus usluge preusmjeravanja

Sljedeća primjera prikazuju kako napisati aplikacije koje koriste usluge preusmjeravanja Bus servisa.

Imajte na umu uzoraka preusmjeravanja potrebno je niz za povezivanje da biste pristupili prostora za naziv Bus servisa.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Da biste dobili niza za povezivanje za Bus servisa Azure

1. Prijavite se na [portal za Azure](http://portal.azure.com).

1. U stupcu na lijevoj strani kliknite **Bus servisa**.

1. Kliknite naziv svoje prostora za naziv na popisu.

3. U plohu prostor naziva kliknite **pravila za zajedničko korištenje programa access**.

4. U plohu **pravilnike pristup za zajedničko korištenje** kliknite **RootManageSharedAccessKey**.

6. Kopirajte niz za povezivanje u međuspremnik.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Da biste dobili niza za povezivanje za Bus servisa za Windows Server

1. Pokrenite sljedeći cmdlet komponente PowerShell:

    ```
    get-sbClientConfiguration
    ```

2. Zalijepite niz za povezivanje u datoteku App.config uzorka.

## <a name="service-bus-relay"></a>Bus usluge preusmjeravanja

Uzorci kojima se ilustrira Bus usluge preusmjeravanja.

### <a name="getting-started"></a>Početak rada

|Primjer naziva|Opis|Minimalna SDK verzija|Dostupnost|
|---|---|---|---|
|[Povezivati razmjene poruka: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Pokazuje kako pokrenuti servis Bus klijenta i servisa Azure. Ovaj primjer programski konfigurira Bus servisa. Samo podatke o sigurnosti i okruženje pohranjena u konfiguracijskoj datoteci.|1.8|Bus servisa Microsoft Azure|
|[Povezivati poruka provjere autentičnosti: Tajna zajedničko korištenje](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Pokazuje kako koristiti naziv izdavača i izdavatelj tajna za provjeru s Bus servisa.|1.8|Bus servisa Microsoft Azure|
|[Povezivati poruka provjere autentičnosti: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Pokazuje kako da biste otkrili HTTP servisa koja ne zahtijeva provjeru autentičnosti korisnika klijenta.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Pokazuje kako koristiti povezivanje **WebHttpRelayBinding** da biste se vratili binarne podatke putem weba programiranje modela.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: NetTcp povezivati](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Pokazuje kako koristiti **NetTcpRelayBinding** povezivanja.|1.8|Bus servisa Microsoft Azure|

### <a name="exploring-features"></a>Istraživanje značajke

Uzorci kojima se ilustrira različite značajke Bus usluge preusmjeravanja.

|Primjer naziva|Opis|Minimalna SDK verzija|Dostupnost|
|---|---|---|---|
|[Povezivati poruka provjere autentičnosti: Jednostavni WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Prikazano kako koristiti jednostavne web vjerodajnica za tokena za provjeru s Bus servisa. Uzorak je slična uzorka jeka, nekoliko promjenama. Konkretno, ovaj uzorak dodaje ponašanje u aplikacijama ChannelFactory (klijent) i ServiceHost (servis).|1.8|Bus servisa Microsoft Azure|
|[Povezivati se poruka: Učitavanje saldo](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Pokazuje kako koristiti Microsoft Azure servisa Bus za usmjeravanje poruka većem broju primatelja. Prikazuje više instanci jednostavne servisa komunikaciju s klijentskim putem **NetTcpRelayBinding** povezivanja|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: Neto događaj](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Pokazuje korištenje **NetEventRelayBinding** povezivanja na Bus usluge za Microsoft Azure.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: Sesije WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Pokazuje povezivanja **WS2007HttpRelayBinding** pomoću pouzdanog sesije omogućena. Prikazuje i određivanje usluga Bus vjerodajnice u konfiguracijskoj datoteci umjesto programski.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: MsgSecCertificate WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Pokazuje kako koristiti **WS2007HttpRelayBinding** povezivanja s sigurnosnih poruka za sigurnost završetka do kraja poruke dok još potrebno klijenata za provjeru s Bus servisa.|1.8|Bus servisa Microsoft Azure|
|[Povezivati se poruka: Razmjene metapodataka](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Pokazuje kako da biste otkrili metapodataka krajnju točku koja koristi preusmjeravanja povezivanja. **MetadataExchange** je podržano u sljedećim preusmjeravanja povezivanja: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**i **WS2007HttpRelayBinding**.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: Izravna NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Pokazuje kako konfigurirati povezivanja **NetTcpRelayBinding** način povezivanja **hibridnog** najprije uspostavlja relayed vezu, a ako je to moguće, prelazi automatski izravne veze između klijenta i usluge podrške.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: Korisničko ime za MsgSec NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Pokazuje kako koristiti **NetTcpRelayBinding** povezivanja s sigurnosnih poruka.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: Neto Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Pokazuje kako izlaganje i zauzeti krajnja točka servisa pomoću **NetOnewayRelayBinding** povezivanja.|1.8|Bus servisa Microsoft Azure|
|[Povezivati razmjenu povezivanja: Jednostavno WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Pokazuje pomoću **WS2007HttpRelayBinding** povezivanja. Pokazuje jednostavne servis koji koristi bez sigurnosne mogućnosti, a ne zahtijeva klijenata za provjeru autentičnosti.|1.8|Bus servisa Microsoft Azure|

## <a name="next-steps"></a>Daljnji koraci

U sljedećim temama konceptualni pregled Bus servisa.

- [Pregled Bus usluge preusmjeravanja](service-bus-relay-overview.md)
- [Servis Bus arhitekture](../service-bus-messaging/service-bus-architecture.md)
- [Osnove Bus servisa](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)