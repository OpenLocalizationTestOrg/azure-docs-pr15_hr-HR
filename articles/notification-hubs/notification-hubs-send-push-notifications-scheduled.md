<properties
    pageTitle="Kako poslati zakazani obavijesti | Microsoft Azure"
    description="U ovoj se temi opisuju zakazano obavijesti pomoću koncentratora Azure obavijesti."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="automatske obavijesti, automatske obavijesti, Planiranje automatske obavijesti"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Upute: Zakazano slanje obavijesti


##<a name="overview"></a>Pregled

Ako imate scenarij u kojem želite poslati obavijest u budućnosti trenutku, no nećete imati jednostavnu aktivira kopije pozadinske kod za slanje obavijesti. Standardni sloju obavijesti koncentratora podržava značajku koja omogućuje zakazivanje obavijesti gore 7 dana u budućnosti.

Prilikom slanja obavijesti, jednostavno [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klasu koristiti u SDK koncentratora obavijesti kao što je prikazano u sljedećem primjeru:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Osim toga, možete otkazati prethodno planirano obavijest pomoću njegov notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Postoje bez ograničenja broja zakazani obavijesti možete poslati.