<properties
    pageTitle="Dodajte akciju upita u aplikacijama logike | Microsoft Azure"
    description="Pregled akcijski upit za izvođenje akcija kao što je filtar polja."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Početak rada s akcijski upit

Pomoću akcijski upit, možete raditi s grupama i polja da biste izvršili tijekova rada za:

- Stvaranje zadatka za sve zapise visokog prioriteta iz baze podataka.
- Spremanje svih privitaka PDF-a za e-poštu u blobova platforme Azure.

Da biste počeli koristiti akcijski upit u aplikaciji logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Akcijom upita

Akciju je postupak koji se provodi tako da se tijek rada koji je definiran u aplikaciji logike. [Dodatne informacije o akcije](connectors-overview.md).  

Akcijski upit je trenutno u jednoj operaciji naziva polja filtra predstavljeni u alatu za dizajniranje. Time upita polja, a zatim vratiti skup rezultatima filtriranja.

Evo kako možete ga dodati u aplikaciji logika:

1. Odaberite gumb **Novi korak** .
2. Odaberite **Dodaj akciju**.
3. U okvir za pretraživanje akciju upišite **Filtar** da biste dobili popis akcija **polja filtra** .

    ![Odaberite akciju upita](./media/connectors-native-query/using-action-1.png)

4. Odaberite polja za filtriranje. (Sljedeća slika prikazuje polje rezultata pretraživanja Twitter.)
5. Stvaranje uvjeta za procjenu na svaku stavku. (Sljedeće snimka filtrira tweets s korisnicima koji imaju više od 100 prate).

    ![Dovršite akciju upita](./media/connectors-native-query/using-action-2.png)

    Akcija će izlaz novog polja koja sadrži samo rezultate koje zadovoljiti uvjete filtra.
6. Kliknite gornji lijevi kut alatne trake da biste spremili i aplikacije logike će i spremite i objavite (aktivirati).

## <a name="query-action"></a>Akcijski upit

Evo detalja za akcije koje podržava ovaj poveznik. Poveznik sadrži jedan moguće radnje.

|Akcija|Opis|
|---|---|
|Polja filtra|Procjenjuje uvjeta za svaku stavku u polju i vraća rezultat|

## <a name="action-details"></a>Detalji o akcija

Akcijski upit isporučuje se s jedne moguće radnje. Tablice u nastavku opisuju obaveznih i neobaveznih unosa polja za akcije i odgovarajuće izlaz pojedinosti koje su vezane uz akcija.

### <a name="filter-array"></a>Polja filtra
U nastavku su Ulazna polja za akciju čega HTTP Izlazni zahtjev.
A * znači da je obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Iz *|iz|Polja za filtriranje|
|Uvjet *|gdje|Uvjet se vrednuje za svaku stavku|
<br>

### <a name="output-details"></a>Detalji o Izlaz

Slijede izlaz detalje o HTTP odgovor.

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Filtrirani polja|polja|Polje koje sadrži objekt za svaku filtrirani rezultat|

## <a name="next-steps"></a>Daljnji koraci

Sada, isprobajte platforme i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Druge dostupne poveznika u aplikacijama logike možete istražiti tako da pogledate našem [popisu API-ji](apis-list.md).
