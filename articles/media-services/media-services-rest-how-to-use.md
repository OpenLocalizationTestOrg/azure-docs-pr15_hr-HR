<properties 
    pageTitle="Pregled Media Services REST API-JA | Microsoft Azure" 
    description="Pregled Media Services REST API-JA" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Pregled Media Services REST API-JA 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Servisa Microsoft Azure Media Services je servis koji prihvaća zahtjeve utemeljen na OData HTTP, a možete odgovoriti na opširno JSON ili atom + pub. Budući da Media Services u skladu smjernice za Azure dizajna, nema skup potrebna HTTP zaglavlja svakog klijenta mora koristiti prilikom povezivanja s Media Services, kao i skup neobavezno zaglavlja koje je moguće koristiti. U sljedećim odjeljcima opisuju zaglavlja i glagola HTTP možete koristiti pri stvaranja zahtjeva za primanje odgovore od Media Services.

##<a name="considerations"></a>Razmatranja 

Imajte na umu sljedeće primjenjuju se prilikom korištenja ostalih.

- Prilikom postavljanja upita entiteti, postoji ograničenje od 1000 entiteti vraća odjednom jer javno OSTALE v2 Ograničava rezultate upita 1000 rezultate. Morate koristiti **Preskoči** i **iskoristite** (.NET) / **Vrh** (REST) kao što je opisano u [ovom primjeru .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [u ovom se primjeru REST API -JA](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 

- Kada pomoću JSON i navođenjem da biste koristili **__metadata** ključnu riječ u zahtjevu za (na primjer, da biste reference povezani objekt) morate postaviti zaglavlje **Prihvati** [JSON opširno](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) oblik (pogledajte u sljedećem primjeru). OData ne razumije svojstvo **__metadata** u zahtjevu za osim ako je postavljena za opširno.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardni HTTP zahtjev zaglavlja podržava Media Services

Za svaki poziv pretvoriti Media Services, postoji skup potrebna zaglavlja morate uključiti u zahtjevu za, a i skup neobavezno zaglavlja možda želite uvrstiti. U sljedećoj su tablici navedeni potrebna zaglavlja:


Zaglavlje|Vrsta|Vrijednost
---|---|---
Autorizacija|Nošenja|Nošenja je mehanizam samo prihvaćenom autorizacije. Vrijednost mora obuhvaćati nudi ACS token za pristup.
x ms-verzija|Broja decimalnih mjesta|2.11
DataServiceVersion|Broja decimalnih mjesta|3.0
MaxDataServiceVersion|Broja decimalnih mjesta|3.0



>[AZURE.NOTE] Budući da Media Services koristi OData da biste vidjeli njezin podlozi spremište metapodataka resursa kroz REST API-ji, zaglavlja DataServiceVersion i MaxDataServiceVersion valja obuhvatiti zahtjev; Međutim, ako nisu, zatim trenutno Media Services pretpostavlja da vrijednost DataServiceVersion koristi je 3.0.

Skup neobavezno zaglavlja je:

Zaglavlje|Vrsta|Vrijednost
---|---|---
Datum|RFC 1123 datuma|Vremenska oznaka zahtjeva
Prihvaćanje|Vrste sadržaja|Potrebne vrste sadržaja za odgovor kao što je sljedeće:<p> – aplikacije/json; odata = opširno<p> – aplikacije/atom + xml<p> Odgovori možda različite vrste sadržaja, kao što su dohvaćanje s blob gdje uspješno odgovor sadržavat će strujanje blob kao opseg.
Prihvaćanje šifriranje|Gzip, umanjivanje|GZIP i DEFLATE kodiranja, kada je primjenjivo. Napomena: Za velike resurse Media Services može Zanemari tog zaglavlja i vraćanje noncompressed podataka.
Prihvaćanje jezika|"kratki", "es" i tako dalje.|Određuje Preferirani jezik za odgovor.
Prihvaćanje skup znakova|Vrsta skup znakova kao što su "UTF-8"|Zadana vrijednost je UTF-8.
X HTTP metoda|HTTP metoda|Omogućuje klijente ili vatrozid koji ne podržavaju HTTP metode kao što su STAVI ili Izbriši da biste koristili ovih metoda tunneled putem poziva GET.
Vrsta sadržaja|Vrste sadržaja|Vrsta tijela zahtjev u zahtjevima za STAVI ili objavu sadržaja.
klijent id-zahtjeva|Niz|Pozivatelja definirana vrijednost koja služi za identifikaciju zahtjev za dani. Ako navedeni, tu vrijednost uvrstiti u poruku odgovora kao način da biste mapirali zahtjev. <p><p>**Važno**<p>Vrijednosti moraju biti capped pri 2096b (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standardni HTTP odgovor zaglavlja podržava Media Services

Slijedi skup zaglavlja kojih može doći vam ovisno o resursa su Traži i namijenjen za izvođenje akcija.


Zaglavlje|Vrsta|Vrijednost
---|---|---
id zahtjeva|Niz|Jedinstveni identifikator za trenutnu operaciju servisa generira.
klijent id-zahtjeva|Niz|Identifikator određen pozivatelja u izvorni zahtjev, ako postoji.
Datum|RFC 1123 datuma|Datum koji nije obrađen zahtjev.
Vrsta sadržaja|Mijenja se|Vrsta sadržaja tijela odgovor.
Kodiranje za sadržaj|Mijenja se|Gzip ili udubljeno, sukladno situaciji.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standardni glagoli HTTP koje podržava Media Services

Slijedi popis svih glagoli HTTP koje je moguće koristiti kad upućivanje HTTP zatraži:


Glagolski|Opis
---|---
POČETAK|Vraća trenutnu vrijednost objekta.
OBJAVA|Stvara objekt na temelju podataka navedene ili šalje naredbu.
POSTAVLJANJE|Zamjenjuje objekta ili stvara imenovani objekt (kada je primjenjivo).
BRISANJE|Brisanje objekta.
SPAJANJE|Promjena svojstava imenovani ažurira postojećeg objekta.
ZAGLAVLJE|Vraća metapodataka objekta DOBIVANJE odgovora.

##<a name="limitation"></a>Ograničenje

Prilikom postavljanja upita entiteti, postoji ograničenje od 1000 entiteti vraća odjednom jer javno OSTALE v2 Ograničava rezultate upita 1000 rezultate. Morate koristiti **Preskoči** i **iskoristite** (.NET) / **Vrh** (REST) kao što je opisano u [ovom primjeru .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [u ovom se primjeru REST API -JA](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 


## <a name="discovering-media-services-model"></a>Otkrivanje modela Media Services

Da biste lakše dostupnima entiteti Media Services, mogu se koristiti operacija $metadata. Omogućuje dohvatiti sve vrste valjani entitet, svojstva entitet, pridruživanja, Funkcije, akcije i tako dalje. Sljedeći primjer prikazuje način za sastavljanje URI: https://media.windows.net/API/$ metapodataka.

Trebali biste dodati "? api-version=2.x" na kraj URI ako želite prikazati metapodatke u pregledniku ili Uključi zaglavlje x ms verzija u zahtjevu za.



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
