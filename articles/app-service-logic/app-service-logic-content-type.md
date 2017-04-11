<properties
   pageTitle="Logika aplikacije sadržaja upišite rukovanje | Microsoft Azure"
   description="Objašnjenje kako logike aplikacije bavi vrste sadržaja na web-mjesto dizajna i u okvir za vrijeme izvođenja"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Vrsta sadržaja za aplikacije logike rukovanje

Postoje različite vrste sadržaja koje možete tijeka kroz aplikaciju logike – uključujući JSON, XML, paušalni datoteke i binarne podatke.  Tijekom podržane su sve vrste sadržaja, neke nativno razumjele modul logike aplikacije i drugim korisnicima biti potrebna casting ili pretvorbe prema potrebi.  U sljedećem članku opisane su načina na koji modul obrađuje različite vrste sadržaja i kako ih može ispravno riješiti prema potrebi.

## <a name="content-type-header"></a>Zaglavlje vrsta sadržaja

Da biste započeli jednostavne, pogledajmo dva `Content-Types` koja ne zahtijevaju pretvorbe ni casting za korištenje unutar logike aplikaciju – `application/json` i `text/plain`.

### <a name="applicationjson"></a>Aplikacija/json

Ovisi modul tijeka rada na `Content-Type` zaglavlja s HTTP poziva za određivanje obrade odgovarajuće.  Svaki zahtjev s vrstom sadržaja `application/json` bit će spremljeni i obrađuju kao objekt JSON.  Osim toga, JSON sadržaja možete mogu raščlaniti po zadanom bez potrebe za sve casting.  Stoga zahtjev koji sadrži zaglavlja vrsta sadržaja `application/json ` ovako:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

nije moguće raščlaniti u tijeku rada s sličan izraz `@body('myAction')['foo'][0]` da biste dobili vrijednost (u tom slučaju `bar`).  Bez dodatnih casting je potreban.  Ako radite s podacima koji se JSON, ali nisu imali zaglavlja naveden, koju možete ručno ga pretvoriti u korištenje JSON na `@json()` funkciju (na primjer: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Tekst/običnog

Slično `application/json`, HTTP poruke koje su stigle s na `Content-Type` zaglavlju `text/plain` bit će spremljeni u njegova neobrađenog obrasca.  Osim toga, ako uključen u sljedeće akcije bez sve casting zahtjev će izabrati na `Content-Type`: `text/plain` zaglavlje.  Na primjer, ako radite s nehijerarhijskom datotekom možete dobiti sljedeće HTTP sadržaja:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Ako u sljedeću akciju poslana kao tijelu zahtjeva za drugi (`@body('flatfile')`), zahtjev promijenile na `text/plain` zaglavlje vrsta sadržaja.  Ako radite s podacima koji se običnim tekstom, ali nisu imali zaglavlja naveden, koje možete ručno ga pretvoriti u tekst pomoću na `@string()` funkcija (na primjer: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Xml-a/aplikacije i aplikacije/octet-strujanje i funkcije pretvornik

Modul aplikacije logike uvijek zadržava u `Content-Type` koji je primio HTTP zahtjev ili odgovor.  Što to znači da je sadržaja primitku s `Content-Type` od `application/octet-stream`, uključujući koji u sljedeću akciju s bez casting rezultirat će odlazne zahtjev s `Content-Type`: `application/octet-stream`.  Na taj način modul možete guaruntee podaci neće biti izgubljene kao ona prelazi u tijeku rada.  Međutim, stanje akcije (unosa i izlaze) spremaju se u JSON objekt kao teče u tijeku rada.  To znači da bi se očuvao neke podatke, modul pretvorit će sadržaj u binarni base64 kodirani niz s odgovarajućim metapodacima zadržali obje `$content` i `$content-type` -koji će se automatski pretvoriti.  Možete i ručno pretvoriti između vrste sadržaja pomoću ugrađeni pretvornik funkcije:

* `@json()`-oblikuje podataka`application/json`
* `@xml()`-oblikuje podataka`application/xml`
* `@binary()`-oblikuje podataka`application/octet-stream`
* `@string()`-oblikuje podataka`text/plain`
* `@base64()`-Pretvara sadržaj base64 niz
* `@base64toString()`-Pretvara nizu base64 kodirani u`text/plain`
* `@base64toBinary()`-Pretvara nizu base64 kodirani u`application/octet-stream`
* `@encodeDataUri()`-kodira niz kao polje dataUri bajt
* `@decodeDataUri()`-dekodira na dataUri u raspon bajtova

Na primjer, ako ste primili HTTP zahtjev s `Content-Type`: `application/xml` od:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Nije moguće pretvoriti i kasnije koristili brojem kao što su `@xml(triggerBody())`, ili unutar funkcije kao što su `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Vrste sadržaja s drugih

Druge vrste sadržaja podržava i funkcioniraju s logike aplikacije, ali možda ćete morati ručno tako da dekodiranje dohvaćanje tijelo poruke u `$content`.  Na primjer, ako se su pokretanje isključivanje od na `application/x-www-url-formencoded` zahtjev koji bile dok je na sljedeći način:

```
CustomerName=Frank&Address=123+Avenue
```

jer to nije običan tekst ili JSON bit će spremljeni u akciju kao:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Gdje `$content` je opseg kodira kao niz base64 da biste sačuvali svi podaci.  Jer trenutno nema nativni funkciju podataka obrasca, te podatke unutar tijeka rada ne može koristiti i dalje tako da ručno pristup podacima pomoću funkcije kao što su `@string(body('formdataAction'))`.  Ako se istaknuti Moj zahtjev za izlazne i na `application/x-www-url-formencoded` zaglavlje vrsta sadržaja nije samo dodam ga u tijelo akcija bez sve casting kao što su `@body('formdataAction')`.  Međutim, to će raditi samo ako je parametar samo u tijelo na `body` unosa.  Ako pokušate `@body('formdataAction')` unutar programa sustava `application/json` zahtjev prikazat će vam se pogreška pri izvođenju kao poslat će kodirani tijelo.
