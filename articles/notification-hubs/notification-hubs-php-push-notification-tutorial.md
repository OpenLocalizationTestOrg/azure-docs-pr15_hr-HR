<properties 
    pageTitle="Kako koristiti koncentratora obavijesti s PHP" 
    description="Saznajte kako koristiti koncentratora Azure obavijesti iz programa i pozadinske." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Kako koristiti koncentratora obavijesti iz PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Sve značajke obavijesti koncentratora možete pristupiti iz programa Java/PHP/fonetski zapis pozadinske pomoću obavijesti koncentrator OSTALE sučelja kao što je opisano u članku MSDN [Obavijesti koncentratora REST API -ji](http://msdn.microsoft.com/library/dn223264.aspx).

U ovoj temi Pokazat ćemo način:

* Sastavljanje OSTALE klijenta za značajke koncentratora obavijesti u PHP;
* Slijedite [Početak rada vodič](notification-hubs-ios-apple-push-notification-apns-get-started.md) za vaš mobilni platformu po izboru, implementacijom dijelu pozadinsku u PHP.

## <a name="client-interface"></a>Sučelje klijenta
Sučelje za glavni klijenta možete unijeti iste metode koje su dostupne u [.NET obavijesti koncentratora SDK](http://msdn.microsoft.com/library/jj933431.aspx), to će vam izravno prijevod vodiče i uzorke koje su trenutno dostupne na web-mjestu omogućiti i pridonio zajednica na Internetu.

Možete pronaći sve dostupne u [uzorka omot i OSTALE]kodove.

Na primjer, da biste stvorili klijent:

    $hub = new NotificationHub("connection string", "hubname"); 

Da biste poslali sustava iOS nativni obavijest:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementacija
Ako već nije niste, slijedite naše [Početak rada vodič] do posljednje sekcije kojoj imate implementirati pozadinske.
Osim toga, ako želite možete pomoću koda iz [i OSTALE omot uzorak] i prijeđite izravno na odjeljak [Dovršeno vodič](#complete-tutorial) .

Sve pojedinosti implementaciju cijelog OSTALE omot možete pronaći na [MSDN-u](http://msdn.microsoft.com/library/dn530746.aspx). U ovom odjeljku ne možemo će opišite implementacije i glavne korake potrebne za pristup krajnje točke OSTALE koncentratora obavijesti:

1. Raščlaniti niz za povezivanje
2. Generiranje token za provjeru autentičnosti
3. Izvođenje HTTP poziv

### <a name="parse-the-connection-string"></a>Raščlaniti niz za povezivanje

Ovo su glavni predmete implementacijom klijentskog programa čije Graditelj koji raščlanjuje niz za povezivanje:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Stvaranje sigurnosnog tokena
Detalje o sigurnosni token stvaranje dostupne su [u nastavku](http://msdn.microsoft.com/library/dn495627.aspx).
Sa sljedećim korakom mora dodati klase **NotificationHub** za stvaranje tokena na temelju URI trenutni zahtjev i vjerodajnice dobivenih iz niza za povezivanje.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Slanje obavijesti
Najprije Javite nam definirati predmete koji predstavlja obavijest.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Klase je spremnik za tijelo izvorne obavijesti, ili skup svojstava na slučaj predložak obavijesti i skup zaglavlja koja sadrži ovisne svojstva (kao što su Apple isteka svojstvo i zaglavlja WNS) i oblika (nativni platformu ili predložak).

Potražite u [dokumentaciji obavijesti koncentratora REST API -ji](http://msdn.microsoft.com/library/dn495827.aspx) i oblici određene obavijesti platforme za sve mogućnosti koje su dostupne.

Armed s klase, smo možete odmah Pošalji obavijest metode zapisivanja unutar klase **NotificationHub** .

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Gore navedenih načina poslati zahtjev HTTP POST /messages krajnjoj točci obavijesti središte s točan tijela i zaglavlja za slanje obavijesti.

##<a name="complete-tutorial"></a>Dovršavanje vodič
Sada možete dovršiti Praktični vodič za početak rada slanjem obavijesti iz programa i pozadinske.

Pokretanje obavijesti koncentratora klijenta (zamjenski niz i koncentrator naziv veze kao instructed u [Početak rada vodič]):

    $hub = new NotificationHub("connection string", "hubname"); 

Zatim dodajte kod Pošalji ovisno o vaše ciljne platforme za mobilne.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Iz Windows trgovine i Windows Phone 8.1 (koji nisu Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 i 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Pokretanje i kod trebale davati sada obavijesti koji se pojavljuje na uređaju cilj.


## <a name="next-steps"></a>Daljnji koraci
U ovoj temi smo prikazivao kako stvoriti jednostavan Java OSTALE klijent za obavijesti koncentratora. Na tom mjestu možete učiniti sljedeće:

* Preuzmite cijeli [i OSTALE omot uzorka], koja sadrži sve gore navedeni kod.
* Daljnje informacije o obavijesti koncentratora označavanje značajku u [najnovije vijesti praktičnom vodiču]
* Dodatne informacije o margina obavijesti o pojedinačnim korisnicima [obavijestite korisnike praktičnog vodiča]

Dodatne informacije potražite u odjeljku [Centar za razvojne inženjere i](/develop/php/).

[Ogledna omot i OSTALE]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Početak rada vodiča]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
