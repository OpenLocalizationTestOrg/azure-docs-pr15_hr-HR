<properties
    pageTitle="Usmjeravanje i oznaka izraza"
    description="U ovoj se temi objašnjava usmjeravanje i oznaka izraze za Azure obavijesti koncentratora."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Servis za usmjeravanje i oznaka izraza

##<a name="overview"></a>Pregled

Izrazi oznake omogućuju cilj određenim skupovima uređaja ili preciznije registracije prilikom slanja automatske obavijesti putem koncentratora obavijesti.


## <a name="targeting-specific-registrations"></a>Određivanje određene registracije

Jedini način cilj određene obavijesti registracije se želite pridružiti, oznake pa ciljne te oznake. Kako je opisano u [Odjeljku Upravljanje Registracija](notification-hubs-push-notification-registration-management.md), da bi se primajte automatske obavijesti aplikacije mora registrirati bolji uređaj na koncentratora za obavijesti. Nakon što je Registracija je na obavijesti koncentrator, pozadinska aplikacija možete poslati automatske obavijesti da biste ga.
Pozadinska aplikacija možete odabrati registracije ciljno s određenim obavijesti na sljedeće načine:

1. **Emitiranje**: sve registracije u središtu obavijesti obavijest.
2. **Oznaka**: sve registracije koje sadrže oznaku navedeni obavijest.
3. **Izraz oznake**: sve registracije čiji skup oznake odgovaraju navedeni izraz obavijest.

## <a name="tags"></a>Oznaka

Oznake može biti bilo koji niz znakova, a najviše 120 znakova, koji sadrži alfanumeričke znakove koji nisu alfanumerički: '_', ‘@’, "#", ". ',':", "-". Sljedeći primjer prikazuje aplikaciju iz koje primite skočnu pločicu obavijesti o određenim glazbu grupe. U ovom scenariju jednostavan način usmjeravanje obavijesti je registracije naljepnice s oznakama koje predstavljaju različite traka, kao na sljedećoj slici.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Na ovoj slici poruka označenih **Beatles** dosegne samo registriran s oznakom **Beatles**tablet.

Dodatne informacije o stvaranju registracije za oznake potražite u članku [Upravljanje registracije](notification-hubs-push-notification-registration-management.md).

Obavijesti možete poslati oznake pomoću slanje obavijesti metode u `Microsoft.Azure.NotificationHubs.NotificationHubClient` predmet u [Obavijesti koncentratora za Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Možete koristiti i Node.js ili na automatske obavijesti REST API-ji.  Slijedi primjer pomoću SDK-a.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Oznaka ne mora biti unaprijed dodijeljenu i može se odnositi na više koncepata specifične za aplikacije. Korisnici u ovom primjeru aplikacije, na primjer, mogu komentirati traka i želite primati toasts, ne samo za komentare na svoje omiljene traka, nego i za sve komentare iz svoje prijateljima, bez obzira na to traka na kojoj su komentiranje. Na sljedećoj je slici prikazan primjer scenarij:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Na ovoj slici Alice zanimati ažuriranja za u Beatles, a Teo zanimati ažuriranja za na Wailers. Teo i zanimati Hrvoje, komentare, a Hrvoje u zainteresirani u Wailers. Kada se šalje obavijest Hrvoje, komentara na na Beatles, Alice i Teo dobiti.

Dok možete kodiranje više opasnosti oznake (na primjer, "band_Beatles" ili "follows_Charlie"), oznake su jednostavni nizove i svojstva s vrijednostima. Na Registracija se uparuje samo na prisutnost ili odsutnost određenih oznaka.

Potpuni detaljne vodič o tome kako koristiti oznake za slanje kamata grupe potražite u članku [Najnovije vijesti](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Korištenje oznaka ciljne korisnicima

Drugi način za korištenje oznaka je prepoznavanje uređaja određenom korisniku. Registracija možete označiti oznakom koja sadrži korisnički id, kao na sljedećoj slici:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Na ovoj slici za poruke označene uid:Alice je dostigne sve uid:Alice označeni registracije; Dakle, sve Alice na uređaje.


##<a name="tag-expressions"></a>Oznaka izraza

Postoje slučajevi u kojima se obavijest ima ciljni skup registracije koja je označena ne po oznaku, ali po Booleov izraz na oznake.

Razmislite o Sportovi aplikacije koje šalje podsjetnik svima u Meso bostonske o utakmica između crveno Sox i Cardinals. Ako aplikaciju klijent dnevnike oznaka o interes timovima i lokaciju, obavijesti treba ciljani svima u Meso bostonske koji želi Sox crvene boje ili na Cardinals. Ovaj uvjet može biti izražena pomoću sljedećih Booleov izraz:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Oznaka izrazi mogu sadržavati sve logički operatori, kao što su i (& &), ili (|), a ne (!). Također mogu sadržavati zagrade. Oznaka izraza ograničeni su na 20 oznake koje sadrže samo ORs; u suprotnom su ograničeni na 6 oznake.

Slijedi primjer za slanje obavijesti s izrazima oznaku pomoću SDK-a.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
