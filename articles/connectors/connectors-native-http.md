<properties
    pageTitle="Dodajte akciju HTTP u aplikacijama logike | Microsoft Azure"
    description="Pregled akciju HTTP sa svojstvima"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Početak rada s akcijom HTTP

S akcijom HTTP možete proširiti tijekova rada za tvrtku ili ustanovu, a komunikacije za sve krajnju točku putem HTTP-a.

možeš:

- Stvorite logiku aplikacije tijekove rada koji se aktiviraj (okidača) kada je web-mjesta kojima upravljate funkcionira.
- Komunikacija sve krajnju točku putem HTTP-a da biste proširili u tijekove rada u drugim servisima.

Da biste počeli koristiti HTTP akcija u aplikaciji logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Koristite okidač HTTP

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada koji je definiran u aplikaciji logike. [Dodatne informacije o okidača](connectors-overview.md).

Evo primjera slijeda upute za postavljanje okidača HTTP u dizajneru logike aplikacije.

1. Pokretanje HTTP dodatka logike aplikacije.
2. Ispunite parametara za krajnju točku HTTP koji želite provjeriti.
3. Izmjena intervala ponavljanja na koliko često treba provjeriti.
4. Aplikaciju logike aktivira se sada s sav sadržaj koji je vratio prilikom svakog provjere.

![Pokretanje HTTP](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>Kako funkcionira pokretanje HTTP

Pokretanje HTTP čini poziv krajnje HTTP na intervala ponavljanja. Po zadanom sve HTTP odgovor kod manji od 300 rezultate u aplikaciji logike pokrenuti. Možete dodati uvjet u prikazu koda koji će rezultirati nakon HTTP poziva da biste utvrdili je li pokrenuti aplikaciju logike. Evo primjera okidača HTTP koji se aktivira kada Šifra stanja vraćena je veće od ili jednako `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Sve pojedinosti o parametrima pokretanje HTTP dostupnih na [MSDN-u](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Akcijom HTTP

Akciju je postupak koji se provodi tako da se tijek rada koji je definiran u aplikaciji logike. [Dodatne informacije o akcije](connectors-overview.md).

1. Odaberite gumb **Novi korak** .
2. Odaberite **Dodaj akciju**.
3. U okvir za pretraživanje akciju upišite **http** popis HTTP akciju.

    ![Odaberite akciju HTTP](./media/connectors-native-http/using-action-1.png)

4. Dodajte parametre koji su potrebni za HTTP poziv.

    ![Dovršite akciju HTTP](./media/connectors-native-http/using-action-2.png)

5. Kliknite u gornjem lijevom kutu na alatnoj traci da biste spremili. Logika aplikacije će i spremite i objavite (aktivirati).

## <a name="http-trigger"></a>Pokretanje HTTP

Evo detalja za pokretanje s podrškom za ovaj poveznik. Poveznik za HTTP sadrži jedan okidača.

|Pokretanje|Opis|
|---|---|
|HTTP|Čini poziv HTTP-a i vraća sadržaj odgovor.|

## <a name="http-action"></a>HTTP akcija

Evo detalja za akcije koje podržava ovaj poveznik. Poveznik za HTTP sadrži jedan moguće radnje.

|Akcija|Opis|
|---|---|
|HTTP|Čini poziv HTTP-a i vraća sadržaj odgovor.|

## <a name="http-details"></a>Detalji o HTTP

Tablice u nastavku opisuju obaveznih i neobaveznih unosa polja za akcije i odgovarajuće izlaz pojedinosti koje su vezane uz akcija.


#### <a name="http-request"></a>HTTP zahtjev
U nastavku su Ulazna polja za akciju čega HTTP Izlazni zahtjev.
A * znači da je obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Način *|način|Glagolski HTTP-a za korištenje|
|URI *|URI-ja|URI za HTTP zahtjev|
|Zaglavlja|zaglavlja|Objekt programa JSON HTTP zaglavlja da biste uključili|
|Tijelo|tijelo|Tijelo HTTP zahtjev|
|Provjera autentičnosti|Provjera autentičnosti|Detalji o u odjeljku [Provjera autentičnosti](#authentication)|
<br>

#### <a name="output-details"></a>Detalji o Izlaz

Slijede izlaz detalje o HTTP odgovor.

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Zaglavlja|objekt|Odgovor zaglavlja|
|Tijelo|objekt|Objekt odgovora|
|Šifra stanja|INT|Šifra stanja HTTP|

## <a name="authentication"></a>Provjera autentičnosti

Značajka aplikacije logike aplikacije servisa za Azure ne omogućuje korištenje različitih vrsta provjere autentičnosti na temelju HTTP krajnje točke. Koristite ovu provjeru autentičnosti s **HTTP** **[HTTP + Swagger](./connectors-native-http-swagger.md)**i **[HTTP Webhook](./connectors-native-webhook.md)** poveznika. Prilagodljivo su sljedeće vrste provjere autentičnosti:

* [Osnovna provjera autentičnosti](#basic-authentication)
* [Provjera autentičnosti klijentskih potvrda](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth provjere autentičnosti](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Osnovna provjera autentičnosti

Sljedeći objekt za provjeru autentičnosti nije potrebno za osnovnu provjeru autentičnosti.
A * znači da je obavezno polje.

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Vrsta *|Vrsta|Vrsta provjere autentičnosti (mora biti `Basic` za osnovnu provjeru autentičnosti)|
|Korisničko ime *|korisničko ime|Korisničko ime za provjeru autentičnosti|
|Lozinka *|lozinke|Lozinka za provjeru autentičnosti|

>[AZURE.TIP] Ako želite koristiti lozinku koju nije moguće dohvatiti iz definicije, koristite na `securestring` parametar i `@parameters()` [funkcija definiciju tijeka rada](http://aka.ms/logicappdocs).

Tako da biste stvorili objekt kao što je ovaj u polju provjere autentičnosti:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Provjera autentičnosti klijentskih potvrda

Sljedeći objekt za provjeru autentičnosti je potreban za provjeru autentičnosti klijenta certifikata. A * znači da je obavezno polje.

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Vrsta *|Vrsta|Vrsta provjere autentičnosti (mora biti `ClientCertificate` za SSL certifikate klijenta)|
|PFX *|pfx|Base64 kodirani sadržaj datoteke osobne podatke sustava Exchange (PFX)|
|Lozinka *|lozinke|Lozinku za pristup datoteci PFX|

>[AZURE.TIP] Možete koristiti na `securestring` parametar i `@parameters()` [funkcija definiciju tijeka rada](http://aka.ms/logicappdocs) koristiti parametar koji se neće biti čitati u definiciji nakon spremanja logike aplikacije.

Ako, na primjer:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth provjere autentičnosti

Sljedeći objekt za provjeru autentičnosti je potreban za provjeru autentičnosti Azure AD OAuth. A * znači da je obavezno polje.

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Vrsta *|Vrsta|Vrsta provjere autentičnosti (mora biti `ActiveDirectoryOAuth` za Azure AD OAuth)|
|Klijent *|klijent|Identifikator klijenta za Azure AD klijenta|
|Ciljne skupine *|ciljne skupine|Postavite`https://management.core.windows.net/`|
|Klijent ID *|clientId|Identifikator klijenta za aplikaciju Azure AD|
|Tajna *|tajna|Tajna klijenta koji zahtijeva tokena|

>[AZURE.TIP] Možete koristiti na `securestring` parametar i `@parameters()` [funkcija definiciju tijeka rada](http://aka.ms/logicappdocs) za korištenje parametar koji se neće biti čitati u definiciji nakon spremanja.

Ako, na primjer:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Daljnji koraci

Sada, isprobajte platforme i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Druge dostupne poveznika u aplikacijama logike možete istražiti tako da pogledate našem [popisu API-ji](apis-list.md).
