
<properties
    pageTitle="Dodavanje HTTP + Swagger akcija u aplikacijama logike | Microsoft Azure"
    description="Pregled HTTP + Swagger akcija i operacije"
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

# <a name="get-started-with-the-http--swagger-action"></a>Početak rada s HTTP + Swagger akcija

Putem HTTP + Swagger akciju, možete stvoriti jednostavno prva liga poveznik za sve OSTALE krajnju točku putem programa [Swagger dokumenta](https://swagger.io). Možete proširiti i logike aplikacije da biste nazvali sve OSTALE krajnjoj točki doživljaj jednostavno prva liga dizajneru logike aplikacije.

Da biste započeli HTTP + Swagger akcija u aplikaciji logike potražite u članku [Stvaranje nove aplikacije logike](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Korištenje HTTP + Swagger kao okidač ili akcija

HTTP + Swagger okidača i akcija funkcija jednaki [HTTP akciju](connectors-native-http.md) , ali omogućuje bolje iskustvo dizajna prikazom oblik API-JA i izlaza u dizajneru iz [Swagger metapodataka](https://swagger.io). Osim toga, možete koristiti HTTP + Swagger kao okidač. Ako želite implementirati okidač ankete, morate slijediti uzorak ankete koji je opisan u [stvaranju prilagođene API-JA za uporabu logike aplikacije](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers).

[Saznajte više o logike aplikacije okidača i akcija.](connectors-overview.md)

Evo primjera kako za korištenje HTTP + Swagger operacija kao akcije u tijeku rada u aplikaciji logike.

1. Odaberite gumb **Novi korak** .
2. Odaberite **Dodaj akciju**.
3. U okvir za pretraživanje akciju upišite **swagger** do popisa HTTP + Swagger akcija.

    ![Odaberite HTTP + Swagger akcija](./media/connectors-native-http-swagger/using-action-1.png)

4. Upišite URL Swagger dokumenta:
    - Da biste se iz aplikacije dizajnera logike, URL mora biti krajnje HTTPS i omogućeno CORS.
    - Ako dokument Swagger ne zadovoljava taj zahtjev, možete koristiti [Za pohranu Azure s CORS omogućen](#hosting-swagger-from-storage) za spremanje dokumenta.
5. Kliknite **Dalje** da biste čitali i prikazivanje iz dokumenta Swagger.
6. Dodajte parametre koji su potrebni za HTTP poziv.

    ![Dovršavanje HTTP akcija](./media/connectors-native-http-swagger/using-action-2.png)

1. Kliknite **Spremi** u gornjem lijevom kutu na alatnoj traci, a vaše logike aplikacija će oba Spremi i Objavi (aktivirati).

### <a name="host-swagger-from-azure-storage"></a>Glavno računalo Swagger iz spremišta Azure

Možda ćete referentni dokument Swagger koji se nalazi ili koji ne zadovoljava sigurnost i više polazište preduvjeti za dizajnera. Da biste riješili taj problem, možete spremiti dokument Swagger u spremište Azure i omogućivanje CORS referentni dokument.  

Evo nekoliko koraka možete stvarati, konfiguriranje i Pohranjujte dokumente Swagger u spremište Azure:

1. [Stvaranje računa za pohranu za Azure s spremište blobova platforme Azure](../storage/storage-create-storage-account.md). (Da biste to učinili, postavljanje dozvola za **pristup javno**.)
2. Omogućiti CORS na blob-om. [Ovu skriptu PowerShell](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) možete koristiti da biste konfigurirali te postavke automatski.
3. Prijenos datoteke Swagger u blob-om. To možete učiniti s [portala za Azure](https://portal.azure.com) ili alata kao što je [Explorer Azure prostora za pohranu](http://storageexplorer.com/).
1. Reference HTTP vezu na dokument u spremište blobova platforme Azure. (Vezu prati oblik `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Tehničke pojedinosti

Slijede detalje o okidača i akcije koje ovaj HTTP + Swagger podržava poveznik.

## <a name="http--swagger-triggers"></a>HTTP + Swagger okidača

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada koji je definiran u aplikaciji logike. [Saznajte više o okidača.](connectors-overview.md) HTTP + Swagger poveznik sadrži jedan okidača.

|Pokretanje|Opis|
|---|---|
|HTTP + Swagger|Upućivanje poziva na HTTP i vratili se sadržaj odgovor|

## <a name="http--swagger-actions"></a>HTTP + Swagger akcije

Akciju je postupak koji se provodi tako da se tijek rada koji je definiran u aplikaciji logike. [Saznajte više o akcije.](connectors-overview.md) HTTP + Swagger poveznik sadrži jedan moguće radnje.

|Akcija|Opis|
|---|---|
|HTTP + Swagger|Upućivanje poziva na HTTP i vratili se sadržaj odgovor|

### <a name="action-details"></a>Detalji o akcija

HTTP + Swagger poveznik isporučuje se s jedne moguće radnje. Sljedeće se informacije o svakoj od akcije, njihove obaveznih i neobaveznih unosa polja i odgovarajuće izlaz pojedinosti koje su vezane uz njihove upotrebe.

#### <a name="http--swagger"></a>HTTP + Swagger

Provjerite odlazne HTTP zahtjev uz pomoć Swagger metapodataka.
Zvjezdica (*), to znači obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Način *|način|Glagolski HTTP-a za korištenje.|
|URI *|URI-ja|URI za HTTP zahtjev.|
|Zaglavlja|zaglavlja|Objekt JSON HTTP zaglavlja da biste dodali.|
|Tijelo|tijelo|Tijelo HTTP zahtjev.|
|Provjera autentičnosti|Provjera autentičnosti|Provjera autentičnosti za zahtjev. [Dodatne informacije potražite u članku HTTP-a](./connectors-native-http.md#authentication).|

**Detalji o Izlaz**

HTTP odgovor

|Naziv svojstva|Vrsta podataka|Opis|
|---|---|---|
|Zaglavlja|objekt|Odgovor zaglavlja|
|Tijelo|objekt|Objekt odgovora|
|Šifra stanja|INT|Šifra stanja HTTP|

### <a name="http-responses"></a>HTTP odgovora

Kada se upućivanje poziva na različite akcije, mogla bi vam se određene odgovore. Slijedi tablicu dodaje oko odgovarajućih odgovora i opisa.

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvatili|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do neočekivane pogreške.|

---

## <a name="next-steps"></a>Daljnji koraci

Isprobajte platforme i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md) sada. Druge dostupne poveznika u aplikacijama logike možete istražiti tako da pogledate našem [popisu API-ji](apis-list.md).
