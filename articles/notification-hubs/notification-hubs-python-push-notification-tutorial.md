<properties 
    pageTitle="Kako koristiti koncentratora obavijesti s Python" 
    description="Saznajte kako koristiti koncentratora Azure obavijesti iz programa Python pozadinsku." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Kako koristiti koncentratora obavijesti iz Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Sve značajke obavijesti koncentratora možete pristupiti iz programa Java/PHP/Python/fonetski zapis pozadinske pomoću obavijesti koncentrator OSTALE sučelja kao što je opisano u članku MSDN [Obavijesti koncentratora REST API -ji](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] Ovo je implementacije u uzorka vodič za implementaciju šalje obavijest u Python i nije službeno podržani SDK Python koncentrator obavijesti.
>
> Ovaj primjer zapisuje pomoću Python 3.4.

U ovoj temi Pokazat ćemo način:

* Sastavljanje OSTALE klijenta za značajke koncentratora obavijesti u Python.
* Slanje obavijesti putem sučelja Python da biste REST API-ji koncentrator obavijesti. 
* Pronađite na ispis na OSTALE HTTP/odgovor na zahtjev za ispravljanje pogrešaka/obrazovne svrhe. 

[Početak rada vodič](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) možete slijediti za vaš mobilni platformu po izboru, implementacijom dijelu pozadinske u Python.

> [AZURE.NOTE] Opseg uzorka samo ograničenu za slanje obavijesti, a ne li sve upravljanje Registracija.

## <a name="client-interface"></a>Sučelje klijenta
Sučelje glavni klijenta možete unijeti iste metode koje su dostupne u [SDK koncentratora za .NET obavijesti](http://msdn.microsoft.com/library/jj933431.aspx). Jednostavan način možete izravno prijevod vodiče i uzorke koje su trenutno dostupne na web-mjestu i pridonio zajednica na Internetu.

Možete pronaći sve kod koje su dostupne u [OSTALE Python omot uzorka].

Na primjer, da biste stvorili klijent:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Da biste poslali skočnoj obavijesti za Windows:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementacija
Ako već nije niste, slijedite naše [Početak rada vodič] do posljednje sekcije kojoj imate implementirati pozadinske.

Sve pojedinosti implementaciju puni OSTALE omot možete pronaći na [MSDN-u](http://msdn.microsoft.com/library/dn530746.aspx). U ovom odjeljku ne možemo opisane su implementacije Python glavni koraci potrebni za pristup krajnje točke OSTALE koncentratora obavijesti i slanje obavijesti

1. Raščlaniti niz za povezivanje
2. Generiranje token za provjeru autentičnosti
3. Slanje obavijesti pomoću HTTP REST API-JA

### <a name="parse-the-connection-string"></a>Raščlaniti niz za povezivanje

Ovo su glavni predmete implementacijom klijentskog programa čije Graditelj raščlanjuje niz za povezivanje:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Stvaranje sigurnosnog tokena
Detalje o sigurnosni token stvaranje dostupne su [u nastavku](http://msdn.microsoft.com/library/dn495627.aspx).
Na sljedeće načine, morate dodati klase **NotificationHub** za stvaranje tokena na temelju URI trenutni zahtjev i vjerodajnice dobivenih iz niza za povezivanje.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Slanje obavijesti pomoću HTTP REST API-JA
Korištenje prvog, omogućite definirati predmete koji predstavlja obavijest.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Klase je spremnik za tijelo izvorne obavijesti ili skup svojstava u slučaju obavijest predložak, skup zaglavlja koja sadrži ovisne svojstva (kao što su Apple isteka svojstvo i zaglavlja WNS) i oblika (nativni platformu ili predložak).

Potražite u [dokumentaciji obavijesti koncentratora REST API -ji](http://msdn.microsoft.com/library/dn495827.aspx) i oblici određene obavijesti platforme za sve mogućnosti koje su dostupne.

Sada pomoću ove klase smo možete napisati slanje obavijesti metode unutar **NotificationHub** predmete.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Gore navedenih načina poslati zahtjev HTTP POST /messages krajnjoj točci obavijesti središte s točan tijela i zaglavlja za slanje obavijesti.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Pomoću svojstva za ispravljanje pogrešaka da biste omogućili detaljne zapisivanje
Omogućivanje ispravljanje pogrešaka svojstvo vrijeme pokretanje središtu obavijesti će pisanje detaljne zapisivanje informacija o HTTP zahtjev i ispis odgovor, kao i detaljne obavijest slanje rezultat. Dodali smo je nedavno ovo svojstvo naziva [svojstvo TestSend koncentratora obavijesti](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) koja vraća detaljne informacije o rezultat slanja obavijesti. Da biste ga koristiti – pokretanje pomoću sljedeće:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Obavijest o koncentrator Pošalji zahtjev HTTP URL dobiva dodaje s "test" querystring kao rezultat. 

##<a name="complete-tutorial"></a>Dovršavanje vodič
Sada možete dovršiti Praktični vodič za početak rada slanjem obavijesti iz programa Python pozadinske.

Pokretanje obavijesti koncentratora klijenta (zamjenski niz i koncentrator naziv veze kao instructed u [Početak rada vodič]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Zatim dodajte kod Pošalji ovisno o vaše ciljne platforme za mobilne. Ovaj primjer dodaje i višu razinu metode da biste omogućili slanje obavijesti koji se temelji na platformi npr send_windows_notification windows; send_apple_notification (za apple) itd. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Iz Windows trgovine i Windows Phone 8.1 (koji nisu Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 i 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Pokretanje koda Python trebale davati obavijesti koji se pojavljuje na uređaju cilj.

## <a name="examples"></a>Primjeri:

### <a name="enabling-debug-property"></a>Omogućivanje svojstva za ispravljanje pogrešaka
Kada omogućite zastavicu za ispravljanje pogrešaka vrijeme pokretanje u NotificationHub, a zatim vidjet ćete detaljno HTTP zahtjev i ispis odgovor kao i NotificationOutcome ovako gdje mogu razumjeti koje HTTP zaglavlja prosljeđuju u zahtjevu za i koje HTTP odgovor primljena u središtu obavijesti:    ![][1]

Prikazat će se detaljne obavijesti koncentrator rezultat npr. 

- Kada poruka uspješno poslana na servis za slanje obavijesti. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Ako nema nema ciljnih web-mjesta za sve automatske obavijesti pa vjerojatno ćete pročitajte u odgovoru (koji označava da nema bez registracije pronaći vjerojatno isporučiti obavijesti jer registracije imali neke neuparene oznake)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Emitiranje skočnoj obavijesti za Windows 

Obratite pozornost na zaglavlja koja se šalje kada šaljete emitiranje skočnoj obavijesti klijenta sustava Windows. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Slanje obavijesti o navodeći oznaka (ili izraz oznake)

Obratite pozornost na to zaglavlja HTTP oznaka koje će dodani na zahtjev HTTP (u primjeru u nastavku ćemo šaljete obavijesti samo registracije s tereta "Sportovi")

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Slanje obavijesti o navodeći više oznaka

Obratite pozornost na to kako mijenja u zaglavlju oznake HTTP šalju više oznaka. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>S predloškom obavijesti

Obratite pozornost na to je poslati kao dio tijela zahtjev HTTP promjene oblika HTTP zaglavlja i tijela tereta:

**Na strani klijenta - registrirani predloška**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Strani poslužitelja – slanje opseg**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Daljnji koraci
U ovoj temi smo prikazivao kako stvoriti jednostavan Python OSTALE klijent za obavijesti koncentratora. Na tom mjestu možete učiniti sljedeće:

* Preuzmite cijeli [Python OSTALE omot uzorka], koja sadrži sve gore navedeni kod.
* Daljnje informacije o obavijesti koncentratora označavanje značajku u [vodič najnovije vijesti]
* Daljnje informacije o značajka obavijesti koncentratora predložaka u [vodič Localizing novosti]

<!-- URLs -->
[OSTALE Python omot uzorka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Početak rada vodiča]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Najnovije vijesti vodiča]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Praktični vodič za localizing novosti]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
