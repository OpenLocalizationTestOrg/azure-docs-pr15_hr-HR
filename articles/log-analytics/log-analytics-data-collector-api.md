<properties
    pageTitle="Prijava analize podataka HTTP prikupljanje API | Microsoft Azure"
    description="API za prikupljanje zapisnika analize HTTP podataka možete koristiti da biste dodali podatke objavu JSON spremište prijava analitiku iz bilo kojeg klijenta koji možete nazvati REST API-JA. U ovom se članku opisuje kako koristiti u API-JA, a sadrži primjere objavljivanje podataka pomoću raznih programskog jezika."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Sakupljač web-aplikacija zapisnika analize podataka HTTP API-JA

Kada koristite Azure zapisnika analize HTTP podataka prikupljanje API, možete dodati objavu JavaScript objekt notaciju (JSON) podataka u spremište prijava analitiku iz bilo kojeg klijenta koji možete nazvati REST API-JA. Ako koristite taj način, možete slati podatke iz aplikacije drugih proizvođača ili skripte, kao što su iz runbook u automatizaciji Azure.  

## <a name="create-a-request"></a>Stvaranje zahtjeva za

Sljedeća dva tablicama navedeni atributi koji su potrebni za svaki zahtjev za API za prikupljanje zapisnika analize HTTP podataka. Ne možemo opišite svaki atribut detaljno u nastavku članka.

### <a name="request-uri"></a>URI zahtjeva

| Atribut | Svojstvo |
|:--|:--|
| Način | OBJAVA |
| URI-JA | https://\<IdKlijenta\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Vrste sadržaja | aplikacija/json |

### <a name="request-uri-parameters"></a>Zahtjev za URI parametara
| Parametar | Opis |
|:--|:--|
| ID klijenta  | Jedinstveni identifikator za radni prostor Microsoft operacije upravljanja paket. |
| Resurs    | Naziv resursa API-JA: / api/zapisnika. |
| API verzija | Verzija API će se koristiti za ovaj zahtjev. Trenutno je 2016-04-01. |

### <a name="request-headers"></a>Zahtjev za zaglavlja
| Zaglavlje | Opis |
|:--|:--|
| Autorizacija | Autorizacija potpis. U nastavku članka, Saznajte kako stvoriti HMAC SHA256 zaglavlje. |
| Vrste zapisa | Navedite vrstu podataka koja se šalje. Trenutno vrste zapisa podržava samo alfa znakova. Ne podržava numeričke vrijednosti ili posebne znakove. |
| x-ms-date | Datum koji je zahtjev obrađen, u obliku RFC 1123. |
| vrijeme koje generira-polja | Naziv polja u podataka koji sadrže vremensku oznaku podatkovne stavke. Ako navedete polje njegov sadržaj se koriste za **TimeGenerated**. Ako to polje nije naveden, zadana vrijednost za **TimeGenerated** je vrijeme koje je ingested poruku. Sadržaj polja poruke morate slijediti oblik ISO 8601 gggg-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Autorizacija

Bilo koji zahtjev za API za prikupljanje zapisnika analize HTTP podataka mora sadržavati autorizacije zaglavlje. Za provjeru autentičnosti zahtjeva, morate se prijaviti zahtjev s primarnih i sekundarnih ključ za radni prostor koji je upućivanje zahtjeva. Nakon toga prenesite potpis kao dio zahtjev.   

Evo oblik za autorizaciju zaglavlja:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* je jedinstveni identifikator za radni prostor paket za upravljanje operacije. *Potpis* je [Utemeljen na raspršivanje kod preglednika poruka provjere autentičnosti (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) koji se konstruirana iz zahtjeva, a zatim izračunati pomoću [SHA256 algoritam](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Nakon toga linijski ga pomoću Base64 kodiranja.

Koristite ovaj oblik za kodiranje **SharedKey** niz za potpis:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Evo primjera niza za potpis:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Kada imate niz potpis, kodiranje pomoću algoritam HMAC SHA256 na niz UTF-8-om, a kodiranje rezultat kao Base64. Koristite ovaj oblik:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Uzorci u sljedećim odjeljcima imati uzorak koda za stvaranje autorizacije zaglavlje.

## <a name="request-body"></a>Zahtjev za tijelo

Tijelo poruke mora biti u JSON. U ovom obliku ga morate uključiti jednog ili više zapisa s svojstvo naziv te vrijednost parova:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Pomoću sljedećih oblik možete skupna većeg broja zapisa zajedno u jednom zahtjev. Svi zapisi moraju biti iste vrste zapisa.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Vrsta zapisa i svojstava

Definiranje prilagođenu vrstu zapisa kada slanje podataka putem API za prikupljanje zapisnika analize HTTP podataka. Trenutno nije moguće zapisivanje podataka na postojeće vrste zapisa koje su stvorene drugim vrstama podataka i rješenja. Prijava analitiku čita ulazne podatke i stvara svojstva koja se podudaraju s vrstom podataka s vrijednostima koje ste unijeli.

Svaki zahtjev za API analize zapisnika mora sadržavati zaglavlja **Vrste zapisa** s nazivom za tu vrstu zapisa. Nastavak **_CL** se automatski dodaje na naziv unesete razlikovao od drugih vrsta zapisnika kao prilagođeni zapisnika. Ako, na primjer, ako unesete naziv **MyNewRecordType**, prijava analitiku stvara zapis s vrstom **MyNewRecordType_CL**. To jamči da nema sukoba između stvorili kao korisnik upišite imena i one otpremljene u trenutnom ili buduće rješenja za Microsoft.

Da biste odredili vrstu podataka za neko svojstvo, prijava analitiku dodaje sufiks naziv svojstva. Ako je svojstvo sadrži vrijednost nula, svojstvo nije uključen u tom zapisu. U ovoj su tablici navedeni svojstvo vrsta podataka i odgovarajuće sufiks:

| Svojstvo vrsta podataka | Nastavak |
|:--|:--|
| Niz    | _s |
| Booleove vrijednosti   | _b |
| Dvaput    | _d |
| Datuma/vremena | _t |
| GUID      | _g |


Prijava analitiku koristi za svako svojstvo vrsta podataka ovisi o tome hoće li se vrsta zapisa za novi zapis već postoji.

- Ako je vrsta zapisa ne postoji, prijava analitiku stvara novi. Prijava analitiku koristi vrstu Primjena JSON da biste odredili vrstu podataka za svako svojstvo za novi zapis.
- Ako postoji vrstu zapisa, prijava analitiku pokušava stvoriti novi zapis koji se temelji na postojeća svojstva. Ako vrstu podataka za svojstvo u novom zapisu ne odgovara i ne mogu pretvoriti u postojeće vrste ili ako zapis obuhvaća svojstvo koje se ne postoji, prijava analitiku stvara novo svojstvo koja ima odgovarajuće sufiks.

Na primjer, stavka slanje bi stvorite zapis s tri svojstva, **number_d**, **boolean_b**i **string_s**:

![Ogledna zapis 1](media/log-analytics-data-collector-api/record-01.png)

Ako zatim poslati sljedeću stavku s svih vrijednosti koji su oblikovani kao niz, svojstva bi neće se promijeniti. Postojeće vrste podataka može pretvoriti ove vrijednosti:

![Ogledna zapis 2](media/log-analytics-data-collector-api/record-02.png)

No ako ste pa dalje slanja, prijava analitiku želite stvoriti novi svojstva **boolean_d** i **string_d**. Nije moguće pretvoriti ove vrijednosti:

![Ogledna zapis 3](media/log-analytics-data-collector-api/record-03.png)

Ako sljedeću stavku, zatim poslali prije stvaranja vrstu zapisa, prijava analitiku bi stvorite zapis s tri svojstva, **broj_u**, **boolean_s**i **string_s**. U ovoj se stavci svaki početne vrijednosti je oblikovan kao niz:

![Ogledna zapis 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Ograničenja podataka
Postoje neka ograničenja oko podataka objavljena na prikupljanje zapisnika analize podataka API-JA.

- Najviše 30 MB po objava da biste zapisnika analize podataka prikupljanje API-JA. To je ograničenje veličine za jedan unos. Ako se podaci iz jedan objave koji premašuje 30 MB, trebali biste Podjela podataka do manje veličine blokova i poslati ih istovremeno. 
- Maksimalno ograničenje od 32 KB za vrijednosti polja. Ako je vrijednost veća od 32 KB, podaci će biti odrezana. 
- Preporučena maksimalan broj polja navedene vrste iznosi 50. Ovo je praktičan ograničenje od upotrebljivosti i perspektive sučelje za pretraživanje.  


## <a name="return-codes"></a>Vraćanje kodovi

Šifra stanja HTTP 202 znači da prihvatio zahtjev za obradu, ali obrada još nije završio. To znači da se operacija uspješno dovršena.

U ovoj su tablici navedeni potpunog skupa kodovi stanja koji će se servis prikazati:

| Kod | Status | Kod pogreške | Opis |
|:--|:--|:--|:--|
| 202 | Prihvatili |  | Uspješno je prihvatio zahtjev. |
| 400 | Neispravan zahtjev | InactiveCustomer | Radni prostor je zatvoren. |
| 400 | Neispravan zahtjev | InvalidApiVersion | Servis ne prepoznaje API verziju koju ste naveli. |
| 400 | Neispravan zahtjev | InvalidCustomerId | Radni prostor ID naveden nije valjan. |
| 400 | Neispravan zahtjev | InvalidDataFormat | Nije valjan JSON je poslana. Tijelo odgovora može sadržavati dodatne informacije o rješavanju pogreške. |
| 400 | Neispravan zahtjev | InvalidLogType | Vrste zapisa navedene sadrži posebne znakove i numeričke vrijednosti. |
| 400 | Neispravan zahtjev | MissingApiVersion | API verzija nije naveden. |
| 400 | Neispravan zahtjev | MissingContentType | Vrsta sadržaja nije naveden. |
| 400 | Neispravan zahtjev | MissingLogType | Vrste zapisa potrebna vrijednost nije naveden. |
| 400 | Neispravan zahtjev | UnsupportedContentType | Vrsta sadržaja nije postavljen je na **aplikaciju/json**. |
| 403 | Zabranjen | InvalidAuthorization | Servis nije uspio zahtjev za provjeru autentičnosti. Provjerite jesu li tipku radni prostor ID-a i povezivanje valjane. |
| 500 | Interna pogreška poslužitelja | UnspecifiedError | Servis je naišao Interna pogreška. Pokušajte ponovno zahtjev. |
| 503 | Servis nije dostupan | ServiceUnavailable | Servis trenutno nije dostupan prima zahtjeve. Pokušajte ponovno vaš zahtjev. |

## <a name="query-data"></a>Upit za podatke

Da biste upit podatke poslao API za prikupljanje zapisnika analize HTTP podataka, tražiti zapise pomoću **Vrsta** koja je jednaka vrijednosti **LogType** koju ste navedeni, dodane s **_CL**. Ako, na primjer, ako ste koristili **MyCustomLog**, zatim koje bi vraćanje svih zapisa s **Vrsta = MyCustomLog_CL**.


## <a name="sample-requests"></a>Zahtjevi za uzorak

U sljedećim odjeljcima nalaze se uzoraka upute za slanje podataka API za prikupljanje zapisnika analize HTTP podataka pomoću programiranje jezicima.

Za svaki uzorka, učinite sljedeće korake za postavljanje varijable za autorizaciju zaglavlja:

1. Na portalu paket za upravljanje operacija odaberite pločicu **Postavke** , a zatim karticu **Izvori povezani** .
2. S desne strane **Radni prostor ID**odaberite ikonu kopiju, a zatim zalijepite ID kao vrijednost varijable **ID klijenta** .
3. S desne strane **Primarni ključ**, odaberite ikonu Kopiraj pa zalijepite ID kao vrijednost varijable **Zajednički ključ** .

Osim toga, možete promijeniti varijable vrste zapisa i JSON podataka.

### <a name="powershell-sample"></a>Ogledna PowerShell

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C# uzorka

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Ogledna Python

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Daljnji koraci

- Koristiti [Dizajner prikaza](log-analytics-view-designer.md) za izradu prilagođenih prikaza podataka koje pošaljete.
