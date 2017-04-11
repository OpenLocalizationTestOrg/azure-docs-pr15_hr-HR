<properties
    pageTitle="Zajedničko korištenje potpisa pristup pregled | Microsoft Azure"
    description="Što su zajednički pristup potpisa, njihov način rada, te kako ih koristiti u čvor, PHP i C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Zajednički pristup potpisa

*Zajedničko korištenje potpisa u programu Access* (SAS) su primarni sigurnosni mehanizam za servis Bus, uključujući događaj koncentratora brokered poruka (redovima i teme), a povezivati razmjene poruka. U ovom se članku govori o zajednički pristup potpisa, kako funkcioniraju i kako ih koristiti na način platformu agnostic.

## <a name="overview-of-sas"></a>Pregled SAS

Zajednički pristup potpisi su mehanizam provjere autentičnosti na temelju SHA 256 sigurne raspršivanja ili ji. SAS je iznimno Napredna mehanizam koji se koristi za sve servise Bus servisa. Stvarni koristi SAS sadrži dvije komponente: *Zajedničko korištenje pravilnik o pristupu* i *Zajednički pristup potpis* (često se nazivaju u *tokena*).

Detaljnije informacije o potpisima pristup zajednički se koristi sa servisa Bus možete pronaći u [Provjera autentičnosti zajednički pristup potpis s Bus servisa](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Zajednički pristup pravila

Važno da biste se informirali o SAS je sve započinje pravila. Za svaku pravila odlučite na tri dijelovi informacija: **naziv**, **opseg**i **dozvole**. **Naziv** je samo to. Jedinstveni naziv unutar taj doseg. Jednostavno dovoljno je opseg: je URI dotičnog resursa. Za servis Bus prostor naziva je djelokrug na potpuno kvalificirani naziv domene (FQDN), kao što su `https://<yournamespace>.servicebus.windows.net/`.

Dostupne dozvole za pravila su uglavnom self-explanatory:

  + Pošalji
  + Slušanje
  + Upravljanje

Kada stvorite pravilo, što vam je dodijeljen *Primarni ključ* i *Sekundarne ključ*. To su jake tipke. Ne izgubite ih ili ih osipanje – one će vam uvijek biti dostupan [portal za Azure][]. Možete koristiti neku od tipki generirani i Obnovi ih u bilo kojem trenutku. Međutim, ako Obnovi ili promjena primarnog ključa u pravilnik, bilo koji zajednički pristup potpise koji su stvoreni iz nje će se nevažeći.

Prilikom stvaranja servisa Bus prostora za naziv pravila automatski stvara za cijelu naziva pod nazivom **RootManageSharedAccessKey**, a to pravilo ima sve dozvole. Nemojte se prijaviti kao **korijen**, tako da ne koristite ovo pravilo osim ako postoji zaista dobar razlog. Na kartici **Konfiguriraj** za prostor za naziv na portalu možete stvoriti dodatna pravila. Važno je bilješke stabla da jednu razinu u servis Bus (prostor naziva, reda čekanja, središte događaj itd.) možete imati samo do 12 pravila pridružen.

## <a name="shared-access-signature-token"></a>Zajednički pristup potpis (token)

Pravila sam nije token za pristup za Bus servisa. Je objekt s kojeg token za pristup generira - bilo primarni ili sekundarni ključ. Token generira pažljivo obratite niza u sljedećem obliku:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Gdje `signature-string` raspršivanje SHA 256 opsega tokena (**opseg** opisan u prethodnom odjeljku) je riječ o CRLF dodan i neko vrijeme isteka (u sekundama nakon na epoch: `00:00:00 UTC` na 1 1970 siječanj).

Raspršivanje izgleda slično sljedeći kod pomoćni i vraća 32 bajtova.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Su vrijednosti koje nisu hashed u nizu **SharedAccessSignature** tako da je primatelj možete izračunati raspršivanje s istim parametrima, da biste bili sigurni da vraća isti rezultat. URI određuje opseg, a naziv ključa označava pravila koja će se koristiti za izračun raspršivanje. To je važno iz sigurnosnih aspekta. Ako potpis ne odgovaraju onome koji koji automatski izračunava primatelja (servis Bus), zatim je odbijen. Sada možete biti sigurni da pošiljatelju imali pristup do ključa i mora biti odobren prava navedena u pravilo.

## <a name="generating-a-signature-from-a-policy"></a>Stvaranje potpisa iz pravila

Kako učiniti zapravo to učiniti u kodu? Pogledajmo na nekoliko od njih.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C i #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Korištenje potpisa za zajedničko korištenje programa Access (na razini HTTP-a)
 
Sad kad znate kako stvoriti zajednički pristup potpisa za svi entiteti u Bus servisa, spremni ste za izvođenje HTTP POST:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Imajte na umu to funkcionira za sve. Možete stvoriti SAS za red, teme, pretplatu, koncentrator događaj ili preusmjeravanja. Ako koristite po publisher identiteta za događaj koncentratora, možete jednostavno dodati `/publishers/< publisherid>`.

Ako date pošiljatelja ili klijenta SAS tokena, izravno nemaju tipku pa ih nije moguće poništiti raspršivanje da biste ga nabavili. Kao takve, imate kontrolu time što se može pristupiti i koliko dugo. Važno što biste trebali upamtiti jest da ako promijenite primarni ključ u pravilnik, bilo koji zajednički pristup potpise koji su stvoreni iz nje će se nevažeći.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Korištenje potpisa za zajedničko korištenje programa Access (na razini AMQP)

U prethodnom odjeljku vidjeli kako koristiti SAS token sa zahtjevom za HTTP POST slanja podataka Bus servisa. Kao što znate, možete pristupiti Bus servis pomoću na napredne poruke stavljanje Protocol (AMQP) koji se nalazi željeni protokol za boljih performansi u mnogim situacijama. Korištenje tokena SAS s AMQP je opisano u dokumentu [AMQP Claim-Based sigurnost verzije 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) koji se nalazi u radu skica od 2013, ali dobro podržava Azure danas.

Prije početka tržišta servisa za slanje podataka u programu publisher morate poslati token SAS unutar poruke AMQP dobro definiranom AMQP čvor naziva **$cbs** (vidjet ćete ga kao "Posebno" red usluga koristi za dohvaćanje i provjeru svih tokena SAS). U programu publisher morate navesti **OutboundSMTPServer** polja unutar poruke AMQP; Ovo je čvor u kojoj se servis odgovori publisher rezultatom tokena provjere valjanosti (jednostavni zahtjev/odgovaranje uzorak između programa publisher i usluge). Stvara se odgovori čvor "u hodu," govoriti o "dinamički stvaranja udaljene čvor" kao što je opisano po specifikacija AMQP 1.0. Nakon provjere SAS token vrijedi, u programu publisher možete naprijed i počnite slanje podataka na servis.

Na sljedeći način prikažite slanju SAS token s protokolom AMQP pomoću [AMQP.Net ograničeno](https://github.com/Azure/amqpnetlite) biblioteke. To je korisno ako ne možete koristiti službene SDK Bus usluge (primjerice na WinRT, .net Compact Framework, .net Micro Framework i Mono) razvoj u C\#. Naravno, biblioteke korisno je pomogao shvatiti način utemeljene na zahtjevima sigurnost funkcionira na razini AMQP vidjeli kako funkcionira na razini HTTP (s zahtjev HTTP POST i token SAS poslane unutar zaglavlje "Autorizacije"). Ako ne trebate takve precizno znanja o AMQP, možete koristiti službene SDK Bus servisa s .net Framework aplikacije koja će se čini umjesto vas.

### <a name="c35"></a>C i #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Na `PutCbsToken()` način prima *veze* (AMQP veze predmete instance kao što je navedeno [AMQP .NET ograničeno biblioteke](https://github.com/Azure/amqpnetlite)) koji predstavlja TCP veza na servis, a parametar *sasToken* koji je token SAS da biste poslali. 

> [AZURE.NOTE] Nije važno da se veza stvorena pomoću **mehanizam provjere autentičnosti SASL postavljena na VANJSKI** (i ne OBIČNOG zadani pomoću korisničkog imena i lozinke koje koristite kada želite poslati SAS token).

Nakon toga u programu publisher stvara dvije AMQP veze za slanje SAS token i primanje odgovori (rezultat tokena provjere valjanosti) iz servisa.

Poruka AMQP sadrži skup svojstava i dodatne informacije od jednostavne poruke. SAS token je tijela poruke (korištenje njegov Graditelj). Svojstvo **"OutboundSMTPServer"** postavljen na čvor naziva za primanje rezultata provjere valjanosti na vezu tekstnog okvira (možete promijeniti njegov naziv ako želite i će biti stvorena dinamički servis). Zadnja tri aplikacije/prilagođenih svojstava koriste servis koji određuju Kakvu vrstu operacija ima izvršiti. Kao što je opisano po skice specifikacija CBS moraju biti **Naziv operacije** ("stavi-token"), **Upišite tokena** (u ovom slučaju "servicebus.windows.net:sastoken") i **"naziv" ciljne skupine** na koju token odnosi (cijeli entitet).

Nakon što pošaljete SAS token na vezu za pošiljatelja, izdavača mora biti postavljeno odgovor na vezu tekstnog okvira. Odgovor je jednostavne AMQP poruke s svojstvo aplikacije programa pod nazivom **"Šifra stanja"** može sadržavati iste vrijednosti kao HTTP kod stanja. 

## <a name="next-steps"></a>Daljnji koraci

Potražite u članku [Referenca za servis Bus REST API -JA](https://msdn.microsoft.com/library/azure/hh780717.aspx) za dodatne informacije o tome što možete raditi s tokena SAS.

Dodatne informacije o provjeri autentičnosti servisa Bus potražite u članku [Bus usluge provjere autentičnosti i autorizacije](service-bus-authentication-and-authorization.md). 

Više primjera SAS u C# i Java Script su u [ovom blogu](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Portal za Azure]: https://portal.azure.com