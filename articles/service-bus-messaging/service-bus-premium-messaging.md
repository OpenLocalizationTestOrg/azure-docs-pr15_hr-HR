<properties
    pageTitle="Servis Bus Premium i cijene razine pregled standardne poruka | Microsoft Azure"
    description="Servis Bus Premium i standardne poruka"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Servis Bus Premium i standardne razmjenu razine 

Bus servis za razmjenu poruka, koja sadrži poruka entiteti kao što su redovima i teme, a zatim na enterprise kombinira poruka mogućnosti s obogaćenim objavljivanja-pretplate semantiku na razini oblaka. Razmjenu poruka servisa knjižna se koristi kao backbone komunikacije za brojne složene oblaka rješenja.

Razina *Premium* Bus usluge razmjene poruka adrese uobičajenih zahtjeva za korisničku oko mjerilo, performanse i dostupnost zaštita njihove privatnosti ovise ključnih aplikacije. Iako gotovo isti su u skupove, ove dvije razine Bus usluge razmjene poruka su osmišljeni za različite korištenje slučajeva.

U tablici u nastavku istaknute su neke razlike više razine.

| Premium                               | Standardna                       |
|---------------------------------------|--------------------------------|
| Visoke propusnost                       | Varijable propusnost            |
| Predvidljivi performansi               | Varijable Latencija               |
| Predvidljivi cijene                   | Plaćanje kao vam Idi varijable cijene |
| Mogućnost skaliranja gore i dolje radno opterećenje | N/D                            |
| Poruka veličine > 256KB                  | Ako je poruka 256KB          |

**Servis Bus Premium poruka** sadrži resursa odvajanja sloju opterećenje procesora i memorije tako da se pokreće svaki radno opterećenje kupca u odvajanja. Ovaj spremnik resursa naziva u *porukama jedinicu*. Prostor naziva svaki premium dodijeljeno najmanje jednu razmjenu jedinicu. Možete kupiti 1, 2 ili 4 razmjenu jedinica za svaki servis Bus Premium prostor naziva. Jedan radno opterećenje ili entitet mogu obuhvaćati više jedinica za razmjenu poruka i broj jedinica za razmjenu se mogu mijenjati po želji, iako naplata naknade 24-satnom ili dnevni stopa. Rezultat je predvidljivi te kontrolirani performanse za servis Bus rješenje.

Ne samo je ovaj performanse predvidljivi i dostupan, ali preporučuje se i brže. Razmjenu poruka servisa Bus Premium sastavlja modul prostora za pohranu u [Koncentratora Azure događaj](https://azure.microsoft.com/services/event-hubs/). Premium poruke, performanse Vršna je mnogo brže od sa standardnom sloju.

## <a name="premium-messaging-technical-differences"></a>Tehnički razlike u porukama Premium

Evo nekoliko razlike između Premium i standardne razmjenu razine.

### <a name="partitioned-queues-and-topics"></a>Particioniranom redovima i teme

Particioniranom redovima i temama koje su podržane u Premium poruka, ali ne funkcioniraju na isti način kao razine standardnu i osnovni Bus usluge razmjene poruka. Premium poruka ne koristi SQL kao izvor podataka i više nije moguće resursa konkurencije povezane sa zajedničke platforme. Zbog toga particija nije potreban. Uz to, broj particije promijenjen od 16 particije u standardni porukama 2 particije u Premium. Imate dvije particije provjerava dostupnost i prikladnije broj okruženju izvođenja Premium. Dodatne informacije o stvaranju particija potražite u članku [Partitioned redovima i teme](service-bus-partitioning.md).

### <a name="express-entities"></a>Express entiteti

Jer Premium poruka pokreće se u okruženju potpuno Izolirani izvođenja, eksplicitnih entiteti nisu podržane u prostorima naziva Premium. Dodatne informacije o značajci express potražite u članku svojstvo [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) .

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o razmjeni poruka Bus servisa, potražite u sljedećim temama.

- [Uvod u Premium Bus servisa Azure na razmjenu poruka (članak na blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Uvod u Premium Bus servisa Azure na razmjenu poruka (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Bus usluge SMS pregled](service-bus-messaging-overview.md)
- [Upute za korištenje servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)
