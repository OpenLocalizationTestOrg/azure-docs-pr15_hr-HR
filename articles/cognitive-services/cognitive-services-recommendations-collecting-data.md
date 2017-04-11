<properties
    pageTitle="Prikupljanje podataka da biste uvježbati modela: vodič za preporuke API-JA na računalu | Microsoft Azure"
    description="Azure strojnog učenja preporuke - prikupljanja podataka za uvježbati modela"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Prikupljanje podataka da biste uvježbati modela #

Preporuke API Pratit će iz prethodnih transakcije da biste pronašli stavke koje treba imati preporučuje se određeni korisnički.

Kada stvorite model, morat ćete omogućuje dvije informaciju mogli učiniti sve obuka: datoteka kataloga i podataka o korištenju.

>   Ako niste učinili već, preporučujemo da biste dovršili [Vodič za brzi početak rada](cognitive-services-recommendations-quick-start.md).


## <a name="catalog-data"></a>Katalog podataka ##

### <a name="catalog-file-format"></a>Oblik datoteke kataloga ###

Datoteka kataloga sadrži informacije o stavke koja nudi uz prijenos ovlasti.
Katalog podataka morate slijediti sljedećem obliku:

- Bez značajki-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Pomoću značajki-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Ogledna redaka u datoteci kataloga

Bez značajki:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Pomoću značajke:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Detalji o obliku

| Ime | Obavezna | Vrsta |  Opis |
|:---|:---|:---|:---|
| Id stavke |Da | [A-z], [a-z], [0-9], [_] & #40; Donja crta & #41; [–] & #40; crtice & #41;<br> Maksimalna duljina: 50 | Jedinstveni identifikator stavke. |
| Naziv stavke | Da | Bilo koje alfanumeričke znakove<br> Maksimalna duljina: 255 | Naziv stavke. |
| Kategorija artikla | Da | Bilo koje alfanumeričke znakove <br> Maksimalna duljina: 255 | Kategorije koje ta stavka pripada (npr. kuhanje adresari, izražajnost...); može biti prazna. |
| Opis | Ne, osim ako su značajke prikaz (ali može biti prazna) | Bilo koje alfanumeričke znakove <br> Maksimalna duljina: 4000 | Opis tu stavku. |
| Popis značajki | ne | Bilo koje alfanumeričke znakove <br> Maksimalna duljina: 4000; Maksimalan broj značajke: 20 | Popis naziva značajke odvojenih zarezom = vrijednost značajke koje je moguće koristiti za poboljšanje modela preporuke.|

#### <a name="uploading-a-catalog-file"></a>Prijenos datoteke kataloga

Pogledajte [API reference](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) za prijenos datoteka kataloga.  

Imajte na umu da se sadržaj datoteke kataloga treba proslijeđena kao tijelu zahtjeva.

Ako prijenos više datoteka kataloga u istom model s nekoliko pozive, ne možemo će umetnuti samo nove stavke kataloga. Postojeće stavke će ostati s izvorne vrijednosti. Ako koristite taj način ne možete ažurirati kataloga podataka.

>   Napomena: Maksimalna veličina datoteka je 200MB.
>   Maksimalan broj stavki u katalogu podržano je 100 000 stavki.


## <a name="why-add-features-to-the-catalog"></a>Razlozi za dodavanje značajki u katalogu?

Modul za preporuke stvara statističke modelu koji pokazuje stavke koje su vjerojatno sviđa ili kupili koje je korisnik odabrao. Kad imate novi proizvod koji sadrži nikad nije komunicirao ga nije moguće stvoriti model na suautorstvo pojavljivanja samostalno. Recimo da pokrenete koja nudi novu "djece violin" u trgovini, budući da ste nikad prodao te violin prije nego što se ne može odrediti koje druge stavke na preporučujemo da s tog violin.

Koje rečeno, ako modul zna informacije o toj violin (odnosno je musical instrument je za djecu starije od 7 do 10, nije pozivnim violin, itd.), a zatim modul naučiti od drugih proizvoda s slične se značajke. Ako, primjerice, u prošlosti ste je prodao violin na i obično osoba koje kupe violins obično kupite klasični glazbeni CD-ovi i označava glazbu lista.  U sustavu možete pronaći ove veze između značajki i navedite preporuke ovisno o značajkama koje su u svoje nove violin ima malo potrošnju.

Značajke uvoze se kao dio katalog podataka te je njihov položaj (ili važnost značajku u modelu) povezane nakon dovršetka ranga Sastavi. Značajka položaj možete promijeniti prema uzorku podataka o korištenju i vrste stavki. No za dosljedno korištenje/stavki, rang mora sadržavati samo small prilagođavati razlikama. Položaj broja značajke je koji nije negativan broj. Broj 0 označava je li značajka ne rangiranih (u slučaju pozivanje taj API prije dovršetka prvog rank sastavljanje). Datum na kojoj je atribut rang zove svježina rezultat.


###<a name="features-are-categorical"></a>Značajke koje su Categorical

To znači da biste trebali stvoriti značajke koje oblik sličan kategorije. Na primjer, cijena = 9.34 categorical značajka nije. S druge strane, značajke kao što su priceRange = Under5Dollars je categorical značajka. Drugi česta pogreška je da biste koristili naziv stavke kao značajka. To može prouzročiti naziv stavke moraju biti jedinstvene da bi opisuju kategorije. Provjerite je li značajke predstavljaju kategorije stavki.


###<a name="how-manywhich-features-should-i-use"></a>Kako više/which značajke trebao koristiti?


Konačni preporuke sastavljanje podržava stvaranje modela do 20 značajke. Više od 20 značajke može dodijeliti stavke u katalogu, ali se očekuje da učinite rang Sastavi, a zatim odaberite samo značajke rangirane visoka. (Značajka s položaj broja 2.0 ili više njih je zaista dobar značajku tako da koristi!). 


###<a name="when-are-features-actually-used"></a>Kada su značajke zapravo koristi?

Značajke koriste model kada nema dovoljno transakcije podataka možete unijeti preporuke o transakcije samostalno. Stoga značajke maksimalnog učinka dodijelit će se na "Hladna stavke" – stavke s nekoliko transakcije. Ako sve stavke imati dovoljno transakcije podataka možda morati obogaćivanje modela pomoću značajke.


###<a name="using-product-features"></a>Korištenje značajki proizvoda

Korištenje značajki kao dio sustava Sastavi morate:

1. Provjerite je li katalog sadrži značajke koje prilikom prijenosa.

2. Pokretanje rang Sastavi. To će učiniti analizu na važnost/rang značajke.

3. Pokretanje preporuke Sastavi, postavljanjem sljedeće sastavljanje parametara: useFeaturesInModel postavite na true, allowColdItemPlacement TRUE, a modelingFeatureList mora biti postavljena na popisu odvojenih zarezom značajki koje želite koristiti za poboljšanje modela. Dodatne informacije potražite [preporuke sastavljanje vrstu parametara](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Korištenje podataka ##
Korištenje datoteka sadrži informacije o korištenja tih stavki ili transakcija iz vaše tvrtke.

#### <a name="usage-format-details"></a>Informacije o korištenju oblik
Korištenje datoteka je CSV datoteke (vrijednosti odvojenih zarezom) pri čemu svaki redak u datoteci za korištenje predstavlja interakciju između korisnika i stavke. Svaki redak, oblikovano je na sljedeći način:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Ime  | Obavezna | Vrsta | Opis
|-------|------------|------|---------------
|Id korisnika|         Da|[A-z], [a-z], [0-9], [_] & #40; Donja crta & #41; [–] & #40; crtice & #41;<br> Maksimalna duljina: 255 |Jedinstveni identifikator korisnika.
|Id stavke|Da|[A-z], [a-z], [0-9], [& #95;] & #40; Donja crta & #41; [–] & #40; crtice & #41;<br> Maksimalna duljina: 50|Jedinstveni identifikator stavke.
|Vrijeme|Da|Datum u obliku: gggg-MM/ddThh (npr. 2013/06/20T10:00:00)|Vrijeme podataka.
|Događaja|ne | Nešto od sljedećeg:<br>• Kliknite<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Za kupnju| Vrsta transakcije. |

#### <a name="sample-rows-in-a-usage-file"></a>Ogledna redaka u datoteci za korištenje

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Prijenos datoteke za korištenje

Pogledajte [API reference](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) za prijenos datoteka za korištenje.
Imajte na umu da morate proći sadržaj korištenje datoteke kao tijela HTTP poziv.

>  Napomena:

>  Maksimalna veličina datoteke: 200MB. Nekoliko korištenje datoteke ne mogu prenijeti.

>  Morate prenijeti datoteke kataloga prije nego što počnete dodavanje podataka o korištenju u modelu. Koristit će se samo stavke u datoteci kataloga tijekom faze obuku. Sve stavke će je zanemariti.

## <a name="how-much-data-do-you-need"></a>Koliko je podataka potrebno?

Kvaliteta modela ovisi o intenzivnog kvalitete i količine podataka.
Sustav Pratit će kada korisnici kupite različite stavke (smo poziva ovaj zajednički pojavljivanja). Za izgradi FBT također je važno je znati koje se nabavljaju iste transakcije. 

Dobar pravilo palca je za većinu stavki u 20 transakcije ili više njih, pa ako imate 10 000 stavki u katalogu, bi preporučujemo da imate barem 20 puta taj broj transakcije ili spremate 200,000 transakcije. I opet, to je pravilo palca. Morat ćete Poigrajte se sa svojim podacima.

Kada ste stvorili model, na raspolaganju vam [izvanmrežne procjenu](cognitive-services-recommendations-buildtypes.md) da biste provjerili koliko će se dobro je vjerojatno da biste izvršili modela.
