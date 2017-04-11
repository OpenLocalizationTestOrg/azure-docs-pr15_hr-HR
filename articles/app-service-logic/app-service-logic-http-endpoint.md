<properties
   pageTitle="Logika aplikacije kao callable krajnje točke"
   description="Kako stvoriti i konfigurirati pokretanje krajnje točke te ih koristiti u aplikaciji logike u aplikacije servisa za Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Logika aplikacije kao callable krajnje točke

Logika aplikacije nativno možete izložiti sinkrono krajnjoj točki HTTP kao okidač.  Uzorak callable krajnje točke možete koristiti i za pozivanje logika aplikacije kao ugniježđene tijek rada kroz "tijek rada" akcija u aplikaciji logika.

Postoje 3 vrste okidača koji možete primati zahtjeva za:

* Zahtjev
* ApiConnectionWebhook
* HttpWebhook

Ostatak u članku koristit ćemo **zahtjev** kao primjer, ali sve način jednak način primijeniti na druge 2 vrste okidača.

## <a name="adding-a-trigger-to-your-definition"></a>Dodavanje okidač definicije
U prvi je korak da biste dodali okidač definicije aplikacije logike koji možete primati zahtjevi za razgovore.  Možete pretraživati u dizajneru za "HTTP zahtjev" da biste dodali okidača kartice. Možete definirati zahtjev tijelo JSON sheme i dizajnera generirat će tokeni koje omogućuju analizirati i prenesite podatke iz ručno pokretanje kroz tijek rada.  Li preporučujemo korištenje alata kao što je [jsonschema.net](http://jsonschema.net) za generiranje JSON shemu iz teret za tijelo uzorka.

![Zahtjev za pokretanje kartice][2]

Nakon što spremite definicije logika aplikacije, povratni URL-a bit će načinjena slično ovome:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

U ovom URL sadrži ključ SAS u parametara upita koji je korišten za provjeru autentičnosti.

U ovom krajnje točke možete dobiti i na portalu za Azure:

![][1]

Ili tako da nazovete:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Sigurnost URL okidača

Logika aplikacije povratnog URL-ovi generiraju sigurnog korištenja pristup potpisa zajedničko korištenje.  Potpis je prošla kao parametra upita, a morate provjeriti prije nego što će pokrenuti aplikaciju logika.  Generira se kroz jedinstvenu kombinaciju tajnu ključ po logika aplikacije, naziv okidača i provodi operacija.  Osim ako netko ima pristup tipku tajnu logika aplikacije, oni će nećete moći da biste generirali valjani potpis.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Pozivanje okidača aplikacije logika krajnje točke

Kada ste stvorili krajnju točku za vaše okidača, možete ga putem pokrenuti na `POST` cijeli URL. Možete uključiti dodatnih zaglavlja i bilo koji sadržaj u tijelo.

Ako je vrsta sadržaja `application/json` , a zatim moći ćete svojstva referenca unutar zahtjev. U suprotnom će se tretirati kao jedna jedinica binarni koji se proslijediti druge API-ji, ali ne može se pozivati unutar tijeka rada bez pretvaranja sadržaja.  Na primjer, ako `application/xml` sadržaja možete koristiti `@xpath()` da biste učinili izdvajanjem programa xpath ili `@json()` da biste pretvorili JSON iz XML-a.  Dodatne informacije o radu sa sadržajem vrste [može biti ovdje](app-service-logic-content-type.md)

Osim toga, možete odrediti shemu JSON u definiciji. Zbog toga je dizajner za stvaranje tokena koje možete proslijediti u korake.  Na primjer će učiniti sljedeće u `title` i `name` token dostupni u dizajneru:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Pozivate sadržaj dolazni zahtjev

Na `@triggerOutputs()` funkcija će izlaz sadržaj dolazni zahtjev. Na primjer, izgledat će kao:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Možete koristiti u `@triggerBody()` prečac da biste pristupili na `body` svojstvo posebno. 

## <a name="responding-to-the-request"></a>Odgovaranje na zahtjev

Za neke zahtjeve pokretanje aplikacije logiku želite odgovor na poruke uz neki sadržaj pozivatelju. Postoji Nova vrsta akcije naziva **odgovor** koji se mogu koristiti za sastavljanje Šifra stanja, tijelo i zaglavlja za odgovor. Napomena da ako postoji nema **odgovora** oblika krajnju točku logike aplikacija će *odmah* odgovor na poruke uz **202 prihvaćena**.

![HTTP odgovor akcija][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Odgovori imaju sljedeće:

| Svojstvo | Opis |
| -------- | ----------- |
| kôd statusa | Šifra stanja HTTP odgovoriti na zahtjev za dolazne. To može biti bilo koji valjani status kod koji započinje 2xx, 4xx ili 5xx. kodovi stanja 3xx nije dopušteno. | 
| tijelo | Tijelo objekt koji se može biti niz, JSON objekt ili čak i binarni sadržaj koji se pozivati iz prethodnom koraku. | 
| zaglavlja | Možete definirati bilo koji broj zaglavlja da bi bio uvršten u odgovoru | 

Sve korake u aplikaciji logike koji su potrebni za odgovor morate dovršiti *60 sekundi* za izvorni zahtjev za primanje odgovor **osim ako se kao ugniježđene logike aplikacije koja se zove tijeka rada**. Ako se ne poduzmete odgovora dosegne unutar 60 sekundi, a zatim dolazni zahtjev će istek vremena i dobivate odgovor **408 klijentskih vremensko ograničenje** HTTP-a.  Ugniježđene logika aplikacijama nadređenog logika aplikacija će se i dalje čekati na odgovor dok završi, bez obzira na to koliko dugo traje.

## <a name="advanced-endpoint-configuration"></a>Konfiguriranje napredne krajnje točke

Aplikacija za logika ste Ugrađena podrška za krajnju točku izravan pristup i uvijek koristi u `POST` način da biste započeli Pokreni logika aplikacije. Aplikaciju **Ga Slušatelj HTTP** API prethodno podržano i promjena segmenata URL-a i HTTP metoda. Možete čak i postavili dodatne sigurnosti ili prilagođenu domenu dodavanjem glavno računalo za aplikaciju API-JA (web-aplikacije koje hostira aplikaciju API-JA). 

Ta je funkcija je dostupan putem **upravljanja API-JA**:
* [Promijenite način zahtjev](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Promjena URL segmenata zahtjeva](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Postavljanje domene za upravljanje API-JA na kartici **Konfiguriraj** na portalu klasični Azure
* Postavljanje pravila da biste provjerili Osnovna provjera autentičnosti (**potrebna je veza**)

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Sažetak migracije iz 2014. – 12-01 – pregled

|  2014. – 12-01 – pregled | 2016-06-01 |
|---------------------|--------------------|
| Kliknite u aplikaciju **Ga Slušatelj HTTP** API-JA | Kliknite **ručno pokretanje** (bez API aplikacija obavezno) |
| Ga Slušatelj HTTP postavka "*šalje automatski odgovor*" | Ili uključite **odgovor** akcija ili se ne nalazi definiciju tijeka rada |
| Konfiguriranje basic ili OAuth provjere autentičnosti | putem upravljanja API-JA |
| Konfiguriranje načina HTTP | putem upravljanja API-JA |
| Konfiguriranje relativni put | putem upravljanja API-JA |
| Referencu ulazne tijelo putem`@triggerOutputs().body.Content` | Referenca putem`@triggerOutputs().body` |
| Akcija **Slanje HTTP odgovor** na ga Slušatelj HTTP | Kliknite na **odgovoriti na zahtjev HTTP** (bez API aplikacija obavezno)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
