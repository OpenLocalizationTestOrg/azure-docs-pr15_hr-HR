<properties
    pageTitle="Slanje automatskih obavijesti s koncentratorima Azure obavijesti i Node.js"
    description="Saznajte kako koristiti koncentratora obavijesti da biste poslali automatske obavijesti iz aplikacije Node.js."
    keywords="automatske obavijesti, slanje automatskih notifications,node.js, automatske ios"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Slanje automatskih obavijesti s koncentratorima Azure obavijesti i Node.js
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Pregled

> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Ovaj vodič će vam pokazati kako Pošalji automatske obavijesti uz pomoć Azure obavijesti koncentratora izravno iz aplikacije Node.js. 

Scenariji u kojima je moguće Uključi automatske obavijesti da pošaljete aplikacije na platformama sljedeće:

* Android
* iOS
* Windows Phone
* Univerzalni Windows platforma 

Dodatne informacije o obavijesti koncentratora potražite u odjeljku [Sljedeće korake](#next) .

##<a name="what-are-notification-hubs"></a>Što su obavijesti koncentratora?

Azure koncentratora obavijesti pružaju se jednostavno koristi više platforme, skalabilni infrastrukture za slanje slanje obavijesti za mobilne uređaje. Pojedinosti o infrastruktura za servis, posjetite stranicu [Koncentratora Azure obavijesti](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Stvaranje aplikacije Node.js

Prvi korak u ovom ćete praktičnom vodiču stvara novu praznu Node.js aplikaciju. Upute o stvaranju Node.js aplikacije potražite u članku [Stvaranje i implementaciju aplikacija za Node.js Web-mjesta za Azure][nodejswebsite], [Servis u Oblaku Node.js] [ Node.js Cloud Service] pomoću komponente Windows PowerShell ili [Web-mjesto s WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Konfiguriranje aplikacija za korištenje koncentratora obavijesti

Da biste koristili koncentratora Azure obavijesti, morate preuzeti i pomoću [azure paket](https://www.npmjs.com/package/azure)Node.js koji sadrži ugrađeni skup pomoćnih biblioteka koje komunikaciju sa servisima za OSTALE automatske obavijesti.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Korištenje upravitelja čvor paket (NPM) da biste dobili pakiranje

1.  Korištenje naredbenog retka sučelja kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Linux), a zatim dođite do mape u koju ste stvorili prazna aplikacija.

2.  U prozoru naredba vrstu **npm instalacija azure sb** .

3.  Možete ručno pokrenuti naredbu **ls** ili **dir** da biste potvrdili da na **čvor\_modula** mapu stvorena. U toj mapi pronađite **azure** paket koji sadrži biblioteke morate pristupiti središtu obavijesti.

>[AZURE.NOTE] Možete saznati više o instaliranju NPM na službeni [NPM bloga](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Modul za uvoz

Korištenje uređivaču teksta, dodajte sljedeće na vrh **server.js** datoteka aplikacije:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Postavite vezu Azure obavijesti koncentratora

Objekt **NotificationHubService** omogućuje rad s koncentratorima obavijesti. Sljedeći kod stvara objekt **NotificationHubService** za središtu nofication pod nazivom **hubname**. Dodajte ga pri vrhu datoteke **server.js** nakon naredbe da biste uvezli modul azure:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Vrijednost **connectionstring** veze možete preuzeti s [Portala za Azure] pomoću sljedećih koraka:

1. U lijevom navigacijskom oknu kliknite **Pregledaj**.

2. Odaberite **Koncentratora obavijesti**, a zatim pronađite koncentratora koje želite koristiti za uzorak. Može se odnositi na [Windows Store Uvod vodič](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) ako vam je potrebna pomoć pri stvaranju nove koncentrator obavijesti.

3. Odaberite **Postavke**.

4. Kliknite **pravila za pristup**. Prikazat će se oba niza veze zajedničke i puni pristup.

![Azure portala - koncentratora obavijesti](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Također možete dohvatiti niz za povezivanje pomoću cmdleta **Get-AzureSbNamespace** pruža [Azure PowerShell](../powershell-install-configure.md) ili naredbe **Prikaži azure sb prostor naziva** s [Azure sučelje naredbenog retka (Azure EŽA)](../xplat-cli-install.md).

##<a name="general-architecture"></a>Općenito arhitekture

Objekt **NotificationHubService** izlaže sljedeće slučajeve objekt za slanje automatskih obavijesti određenim uređajima i aplikacijama:

* **Sa sustavom android** – pomoću objekta **GcmService** koja je dostupna na **notificationHubService.gcm**
* **iOS** – pomoću objekta **ApnsService** koja je dostupna na **notificationHubService.apns**
* **Windows Phone** – pomoću objekta **MpnsService** koja je dostupna na **notificationHubService.mpns**
* **Univerzalni platformu sustava Windows** – pomoću objekta **WnsService** koja je dostupna na **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Kako: Pošalji automatske obavijesti da biste aplikacija sa sustavom Android

Objekt **GcmService** omogućuje **Slanje** metode koje je moguće koristiti za slanje automatskih obavijesti aplikacija sa sustavom Android. Način za **Slanje** prihvaća sljedećih parametara:

* **Oznake** - identifikator oznake. Ako je navedena bez oznaka obavijest će biti poslana al klijente.
* **Tereta** - JSON na poruku ili tereta neobrađenog niz.
* **Povratnog** - funkciju povratnog.

Dodatne informacije o obliku tereta potražite u odjeljku **tereta** [Implementaciju poslužitelja GCM](http://developer.android.com/google/gcm/server.html#payload) dokumenta.

Sljedeći kod koristi instancu **GcmService** koji prikazuje **NotificationHubService** da biste poslali automatske obavijesti da biste sve registrirani klijente.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Kako: Pošalji automatske obavijesti aplikacijama za iOS

Isti kao i kod Android aplikacije na prethodno opisan objekt **ApnsService** omogućuje **Slanje** metode koje je moguće koristiti za slanje automatskih obavijesti aplikacija sa sustavom iOS. Način za **Slanje** prihvaća sljedećih parametara:

* **Oznake** - identifikator oznake. Ako je navedena bez oznaka obavijesti poslat će se za sve klijente.
* **Tereta** - JSON poruke ili tereta niz.
* **Povratnog** - funkciju povratnog.

Oblikovanje tereta dodatne informacije potražite u odjeljku **Tereta obavijesti** [lokalne i automatske obavijesti vodiču za programiranje](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumenta.

Sljedeći kod koristi instancu **ApnsService** koji prikazuje **NotificationHubService** da biste poslali poruku upozorenja za sve klijente:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Kako: slanje slanje obavijesti za Windows Phone aplikacije

Objekt **MpnsService** omogućuje **Slanje** metode koje je moguće koristiti za slanje automatskih obavijesti aplikacija za Windows Phone. Način za **Slanje** prihvaća sljedećih parametara:

* **Oznake** - identifikator oznake. Ako je navedena bez oznaka obavijesti poslat će se za sve klijente.
* **Tereta** - tereta XML poruke.
* **TargetName**  -  `toast` skočnoj obavijesti. `token`za obavijesti pločica.
* **NotificationClass** - prioritet obavijesti. U odjeljku **Elementi zaglavlja HTTP** dokumenta [automatske obavijesti s poslužitelja](http://msdn.microsoft.com/library/hh221551.aspx) za valjane vrijednosti.
* **Mogućnosti** – zaglavlja zahtjeva nije obavezno.
* **Povratnog** - funkciju povratnog.

Popis valjane mogućnosti **TargetName**, **NotificationClass** i zaglavlja, pogledajte stranicu [automatske obavijesti s poslužitelja](http://msdn.microsoft.com/library/hh221551.aspx) .

Sljedeći primjer kod koristi instancu **MpnsService** koji prikazuje **NotificationHubService** da biste poslali skočnoj automatske obavijesti:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Kako: Pošalji automatske obavijesti aplikacijama univerzalni platforme Windows (UWP)

Objekt **WnsService** omogućuje **Slanje** metode koje je moguće koristiti za slanje automatske obavijesti aplikacija univerzalni platformu sustava Windows.  Način za **Slanje** prihvaća sljedećih parametara:

* **Oznake** - identifikator oznake. Ako je navedena bez oznaka obavijesti poslat će se za sve klijente registrirane.
* **Tereta** - opseg XML poruke.
* **Vrsta** - vrstu obavijesti.
* **Mogućnosti** – zaglavlja zahtjeva nije obavezno.
* **Povratnog** - funkciju povratnog.

Popis valjane vrste i zaglavlja zahtjeva, potražite u članku [automatske obavijesti servisnog zahtjeva i odgovora zaglavlja](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Sljedeći kod koristi instancu **WnsService** koji prikazuje **NotificationHubService** da biste poslali skočnoj automatske obavijesti UWP aplikacije:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Daljnji koraci

Isječci uzorka iznad omogućuju jednostavno stvaranje infrastruktura za izlaganje automatske obavijesti da biste raznih uređaja. Sad kad ste naučili osnove korištenja koncentratora obavijesti s node.js, slijedite ove veze da biste saznali više o kako možete proširiti ove mogućnosti.

-   [Azure obavijesti koncentratora](https://msdn.microsoft.com/library/azure/jj927170.aspx)potražite u članku referenca MSDN.
-   Posjetite [Azure SDK za čvor] spremište na GitHub dodatne primjere i Detalji o implementaciji.

  [Azure SDK za čvor]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Web-mjesto s WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Portal za Azure]: https://portal.azure.com
