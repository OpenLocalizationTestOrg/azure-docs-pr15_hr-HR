<properties
 pageTitle="Koncentrator IoT HA i DR | Microsoft Azure"
 description="U članku se opisuje značajke koje vam pomažu da biste sastavili iznimno dostupnih IoT rješenja s Izrada mogućnosti za oporavak."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Koncentrator IoT visoke dostupnosti i Izrada oporavak

Kao Azure service IoT koncentrator nudi visoke dostupnosti (HA) pomoću ponavljanja na razini Azure regija bez dodatnog potrebnih rješenja. Osim toga, Azure nudi brojne značajke koje omogućuju stvaranje rješenja s mogućnostima Izrada oporavak (DR) ili regije-dostupnost po potrebi. Morate dizajnirati i Priprema vašeg rješenja da iskoristite prednost te značajke DR ako želite navesti globalni, regije-visoke dostupnosti korisnika programa uređaja. U članku [Azure tvrtke Continuity Tehnički vodič](../resiliency/resiliency-technical-guidance.md) opisuje ugrađene značajke u Azure za poslovanje i DR. [Izrada oporavak i visoke dostupnosti za Azure aplikacije][] papira sadrži smjernice arhitektura na strategije Azure aplikacije da biste postigli HA i DR.

## <a name="azure-iot-hub-dr"></a>Azure DR IoT koncentratora
Osim HA intra regija IoT koncentrator implementira prebacivanje mehanizme za oporavak Izrada kojih nema intervencije od korisnika. IoT koncentrator DR koja se sama pokreće, a sadrži oporavak vrijeme cilj (RTO) sati 2 26 i sljedeće točke ciljevi oporavak (RPOs).

| Funkcija | RPO |
| ------------- | --- |
| Dostupnost servisa za registar i komunikacije operacije | Mogući gubitak CName |
| Identitet podataka u registru identiteta uređaja | 0 do 5 minuta gubitka podataka |
| Uređaj u oblak poruke | Gube svih nepročitanih poruka |
| Operacije praćenje poruka | Gube svih nepročitanih poruka |
| Oblak uređaj poruke | 0 do 5 minuta gubitka podataka |
| Red čekanja oblaka uređaj povratnih informacija | Gube svih nepročitanih poruka |

## <a name="regional-failover-with-iot-hub"></a>Regionalne prebacivanje s IoT koncentratora

Dovršavanje postupka od topologija implementacije u IoT rješenja je izvan opsega ovog članka, ali radi visoke dostupnosti i oporavak Izrada rado ćemo dodavanje razmotriti model implementacije *Regionalne prebacivanje* .

U modelu regionalnih prebacivanje, rješenje pozadinskih radi prvenstveno na jednom mjestu podatkovnog centra, ali dodatni koncentrator IoT i pozadinskih uvode na drugom mjestu podatkovnog centra za prebacivanje svrhe, u slučaju središtu IoT u primarni podatkovnog centra suffers do prekida ili mrežne veze s uređaja s podatkovnim centrom za primarni je došlo do prekida. Uređaji koriste krajnja točka za sekundarne usluge kad god primarni pristupnik nije moguće dohvatiti. S mogućnošću na više područja prebacivanje dostupnost rješenja mogu poboljšati izvan visoke dostupnosti jedno područje.

Visoke razine, možete implementirati model regionalne prebacivanje s IoT središte ćete sljedeće.

* **Sekundarni koncentrator IoT i uređaja usmjeravanje logika**: slučaju prekidu servisa u vašoj regiji primarni, morate uređaji pokrenuti povezati sekundarne regije. Uz stanje umu prirode Većina servise koji je uključen, uobičajeno je za administratore rješenja da biste pokrenuli proces prebacivanje među regija. Najbolji način za komunikaciju krajnju uređajima, uz zadržavanje kontrolu nad postupak, je ih redovito provjerava *concierge* služba za trenutni aktivni krajnjoj točki. Servis concierge može biti jednostavne web-aplikacije koje je replicirati i zadržati dostupna Upotreba tehnika DNS preusmjeravanje (na primjer, pomoću [Upravitelja promet za Azure][]).
* **Identitet registra replikacije** – da bi se moći koristiti, sekundarne IoT koncentrator mora sadržavati sve identitete uređaja koji je moguće povezati rješenje. Rješenje trebali biste čuvajte zemlj replicirati uređaj identiteta i prenesite ih sekundarni koncentrator IoT prije prebacivanja aktivni krajnja točka za uređaje. Izvoz funkcionalnost identiteta za uređaj IoT koncentrator vrlo je praktična u ovom kontekstu. Dodatne informacije potražite u članku [IoT koncentrator za razvojne inženjere vodič – identiteta registra][].
* **Spajanje logike** – kada primarni regija ponovno postane dostupan, sve stanje i podataka koje su stvorene u sekundarnog web-mjesta potrebno migrirati sigurnosno primarni regija. To najčešće odnosi identiteta uređaja i računala meta-podataka koje morate spojiti s središtu primarni IoT i druge trgovine specifičnim aplikacijama u području primarni. Da biste pojednostavnili ovaj korak, obično preporučuje se da koristite idempotent operacije. Time se smanjuje strani-efekte ne samo usmjerenog dosljedan raspodjele događaja, iz duplikate ili isporuke redoslijed događaja. Uz to, logike aplikacije treba se dizajnirati da tolerate potencijalne nedosljednosti ili "malo" iz datum stanja. To je zbog dodatne vrijeme potrebno za sustava "heal" temelji na oporavak točke ciljevi (RPO).

## <a name="next-steps"></a>Daljnji koraci

Slijedite ove veze da biste saznali više o Azure IoT koncentratora:

- [Početak rada s koncentratorima IoT (vodič)][lnk-get-started]
- [Što je koncentrator IoT Azure?][]

[Izrada oporavak i visoke dostupnosti za Azure aplikacije]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Upravitelj promet]: https://azure.microsoft.com/documentation/services/traffic-manager/
[Vodič za razvojne inženjere koncentrator IoT - identiteta registra]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Što je koncentrator IoT Azure?]: iot-hub-what-is-iot-hub.md
