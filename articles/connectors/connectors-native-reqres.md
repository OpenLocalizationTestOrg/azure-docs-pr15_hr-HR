<properties
    pageTitle="Korištenje akcija zahtjeva i odgovora | Microsoft Azure"
    description="Pregled zahtjeva i odgovora okidača i akcija u aplikaciji programa Azure logike"
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
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-request-and-response-components"></a>Početak rada s komponente zahtjeva i odgovora

Komponentama zahtjeva i odgovora u aplikaciji logike, možete odgovoriti u stvarnom vremenu na događaje.

Na primjer, možete:

- Odgovoriti na zahtjev HTTP s podacima iz lokalne baze podataka putem logike aplikacije.
- Pokretanje logike aplikacije iz vanjske webhook događaja.
- Nazovite logike aplikaciju pomoću programa zahtjeva i odgovora akcija u nekoj drugoj aplikaciji logike.

Da biste počeli koristiti akcije zahtjeva i odgovora u aplikaciji logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>Koristite okidač HTTP zahtjev

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada koji je definiran u aplikaciji logike. [Dodatne informacije o okidača](connectors-overview.md).

Slijedi primjer slijeda kako postaviti HTTP zahtjev u dizajneru logike aplikacije.

1. Dodajte okidača **zahtjev - kada an HTTP zahtjev je primljen** aplikaciji logike. Po izboru možete unijeti JSON shema (pomoću alata kao što je [JSONSchema.net](http://jsonschema.net)) za tijelo zahtjev. Time se omogućuje dizajner za generiranje tokena svojstva u HTTP zahtjev.
2. Dodavanje drugih akcija tako da možete spremiti aplikaciju logike.
3. Kada spremite aplikaciju logike, možete dobiti HTTP zahtjev za URL s kartice zahtjev.
4. U HTTP POST (poslužite se alatom kao što je [Postman](https://www.getpostman.com/)) na URL pokreće aplikaciju logike.

>[AZURE.NOTE] Ako ne definirate neku akciju odgovor na `202 ACCEPTED` odgovor odmah vraća pozivatelju. Akcijom odgovor da biste prilagodili odgovor.

![Odgovor okidača](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Akcijom HTTP odgovor

Akcija HTTP odgovor vrijedi samo kada koristite u tijek rada koji se aktivira HTTP zahtjev. Ako ne definirate neku akciju odgovor na `202 ACCEPTED` odgovor odmah vraća pozivatelju.  Akcija za odgovor možete dodati na bilo kojem koraku unutar tijeka rada. Aplikaciju logike samo zadržava dolazni zahtjev Otvori jedne minute za odgovor.  Nakon jedne minute, ako nema odgovora je poslana s tijeka rada (i odgovor akcija postoji u definiciji), u `504 GATEWAY TIMEOUT` vraća pozivatelju.

Evo kako dodati akciju HTTP odgovor:

1. Odaberite gumb **Novi korak** .
2. Odaberite **Dodaj akciju**.
3. U okvir za pretraživanje akciju upišite **odgovor** na popisu akcija za odgovor.

    ![Odaberite akciju odgovor](./media/connectors-native-reqres/using-action-1.png)

4. Dodajte parametre koji su potrebni za poruka s odgovorom HTTP-a.

    ![Dovršite odgovor akciju](./media/connectors-native-reqres/using-action-2.png)

5. Kliknite gornji lijevi kut alatne trake da biste spremili i aplikacije logike će i spremite i objavite (aktivirati).

## <a name="request-trigger"></a>Zahtjev za pokretanje

Evo detalja za pokretanje s podrškom za ovaj poveznik. Postoji jedan zahtjev za pokretanje.

|Pokretanje|Opis|
|---|---|
|Zahtjev|Pojavljuje se primljena HTTP zahtjev|

## <a name="response-action"></a>Odgovor akcija

Evo detalja za akcije koje podržava ovaj poveznik. Postoji jedan odgovor akcije koje se mogu koristiti samo kad ga je praćeni okidač zahtjev.

|Akcija|Opis|
|---|---|
|Odgovor|Vraća odgovor povezanog HTTP zahtjev|

### <a name="trigger-and-action-details"></a>Detalji o okidača i akcija

Tablice u nastavku opisuju polja za unos za pokretanje i Akcije, a odgovarajući izlaz pojedinosti.

#### <a name="request-trigger"></a>Zahtjev za pokretanje
Polje unosa za pokretanje iz dolazne HTTP zahtjev je.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|JSON sheme|shema|Shema JSON tijela HTTP zahtjev|
<br>

**Detalji o Izlaz**

Slijede izlaz detalje o zahtjev.

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Zaglavlja|objekt|Zahtjev za zaglavlja|
|Tijelo|objekt|Zahtjev za objekt|

#### <a name="response-action"></a>Odgovor akcija

U nastavku su Ulazna polja za akciju HTTP odgovor. A * znači da je obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Šifra stanja *|kôd statusa|Šifra stanja HTTP|
|Zaglavlja|zaglavlja|Objekt programa JSON zaglavlja bilo kojeg odgovora da biste dodali|
|Tijelo|tijelo|Tijelo odgovora|

## <a name="next-steps"></a>Daljnji koraci

Sada, isprobajte platforme i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Druge dostupne poveznika u aplikacijama logike možete istražiti tako da pogledate našem [popisu API-ji](apis-list.md).
