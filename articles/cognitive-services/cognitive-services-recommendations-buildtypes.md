<properties
    pageTitle="Vrste Sastavi i kvalitetu modela: preporuke API | Microsoft Azure"
    description="Azure stroj učenje preporuke – vodič za brzi početak rada"
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
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Stvaranje vrste i kvalitetu modela #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Podržani Sastavi vrste ##

Preporuke API trenutno podržava dvije vrste Sastavi: *preporuke* i *FBT*. Svaki je ugrađena pomoću različitih algoritmi, a svaka ima različite prednosti. Ovaj dokument opisuje svaka od tih izgradi i tehnike za usporedbu kvalitete modela koje generira.

Ako niste učinili već, preporučujemo da dovršite [Vodič za brzi početak rada](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Vrsta Sastavi preporuka ###

Vrsta Sastavi preporuka koristi factorization matrice za preporuke. Generira [latent značajka](https://en.wikipedia.org/wiki/Latent_variable) vektori koji se temelji na svoje transakcije opisuje svaku stavku, a zatim koristi te latent vektori da biste usporedili stavke koje su slične.

Ako obuka model koji se temelji na nabavu izvršene u elektroničkom opremom i navodite Lumia 650 phone kao unos u model, model će vratiti skup stavki koje često kupiti osobama koje su vjerojatno kupiti Lumia 650 telefona. Stavke možda neće biti komplementarnu. U ovom primjeru, moguće je model će vratiti drugim telefonima jer osobama kao što su Lumia 650 možda htjeli drugim telefonima.

Sastavljanje preporuke sastoji se od dviju mogućnosti koje ga čine privlačne:

* *Sastavljanje preporuke podržava *Hladna stavke* položaj **

Stavke koje imaju značajnih korištenje nazivaju Hladna stavke. Na primjer, ako vam se prikaže pošiljci na telefonu nikad prodani prije, sustav nije moguće izvesti preporuke za ovaj proizvod na transakcije samostalno. To znači da sustav informirate iz informacije o proizvodu sam.

Ako želite koristiti Hladna smještaj ćete morati unijeti podatke značajke za sve stavke u katalogu. Ovo je prvi nekoliko redaka katalog možda izgleda (format bilješke ključa = vrijednost za značajke).

>Pro2 6CX 00001, površinski, plošni, upišite = hardver za pohranu = 128 GB, memorije = 4G, proizvođač = Microsoft

>73H-00013, buđenje Xbox 360, igraće,, vrsta = softver, a zatim jezik = engleski, ocjena = starijih

>WAH 0F05, Minecraft Xbox 360, igraće,, * vrsta = softver, a zatim jezik = španjolski, ocjena = djecu

Morate postaviti sljedećih parametara Sastavi:

| Stvaranje parametra         | Bilješke
|------------------     |-----------
|*useFeaturesInModel*     | Postavite na **true**.  Upućuje na to ako se značajke mogu koristiti za poboljšanje modela preporuke.
|*allowColdItemPlacement*   | Postavite na **true**. Upućuje na to ako se preporuke treba i automatske Hladna stavki putem značajke sličnosti.
| *modelingFeatureList*   | Popis odvojenih zarezom značajka imena koja želite koristiti u sastavljanje preporuke za poboljšanje za preporuke. Na primjer, "jezik, za pohranu" za u prethodnom primjeru.

**Sastavljanje preporuke podržava preporuke za korisnika**

Preporuka Sastavi podržava [preporuke za korisnika](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). To znači da možete unijeti personalizirane preporuke za korisnike na temelju njihove transparentnost transakcije. Za preporuke korisnik može omogućiti korisničkog ID-a ili Nedavna povijest transakcije za tog korisnika.

Jedna klasični primjer gdje možda želite primijeniti preporuke korisnika koji se nalazi pri prijavi na stranici dobrodošlice. Postoji možete Promicanje sadržaja koji se primjenjuje na određene korisnike.

Možda želite primijeniti vrstu Sastavi preporuke kada korisnik uskoro će potražite u članku. U tom trenutku imate popis stavki koje je korisnik namjeravate kupiti, a možete unijeti preporuke temelju trenutnog košarica tržištu.

#### <a name="recommendations-build-parameters"></a>Preporuke sastavljanje parametara

| Ime  |   Opis |    Vrsta <br>  Valjane vrijednosti <br> (Zadana vrijednost)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Broj iteracija izvodi modela prikazuje se ukupno vrijeme računalnim i točnost modela. Na viši broj, na točnije model, ali put računalnim traje dulje.  |   Cijeli broj, <br>  10 do 50 <br>(40)
| *NumberOfModelDimensions* |   Broj dimenzija odnosi na broj značajki model će pokušati pronaći podataka. Povećanje broja dimenzije da bi bolje prilagodbom rezultata u manjim klastere. Međutim, previše dimenzija onemogućuje modela pronalaženje korelacija između stavki. |  Cijeli broj, <br> 10 i 40 <br>(20) |
| *ItemCutOffLowerBound* |  Definira Minimalni broj točaka korištenje stavke u trebali bi biti da bi se smatra dio modela. |        Cijeli broj, <br> 2 ili više njih <br> (2) |
| *ItemCutOffUpperBound* |  Određuje najveći broj točaka korištenje stavke u trebali bi biti da bi se smatra dio modela. |  Cijeli broj, <br>2 ili više njih<br> (2147483647) |
|*UserCutOffLowerBound* |   Definira Minimalni broj transakcije korisnik mora obavite smatraju dio modela. | Cijeli broj, <br> 2 ili više njih <br> (2)
| *UserCutOffUpperBound* |  Određuje najveći je broj transakcije korisnik mora imati izvršena smatraju dio modela. | Cijeli broj, <br>2 ili više njih <br> (2147483647)|
| *UseFeaturesInModel* |    Upućuje na to ako se značajke mogu koristiti za poboljšanje modela preporuke. |     Booleove vrijednosti<br> Zadani: True
|*ModelingFeatureList* |    Popis odvojenih zarezom značajka imena koja želite koristiti u sastavljanje preporuke za poboljšanje u preporuke. Ovisi o značajkama koje su važne. |    Niz do 512 znakova
| *AllowColdItemPlacement* |    Upućuje na to ako se preporuke treba i automatske Hladna stavki putem značajke sličnosti. | Booleove vrijednosti <br> Zadani: False
| *EnableFeatureCorrelation*    | Upućuje na to ako se značajke mogu koristiti u zaključivanja. | Booleove vrijednosti <br> Zadani: False
| *ReasoningFeatureList* |  Odvojite ih zarezom popis naziva značajka koja će se koristiti za zaključivanja rečenice, kao što su objašnjenja preporuke. Ovisi o značajkama koje su važne klijentima. | Niz do 512 znakova
| *EnableU2I* | Omogućivanje personalizirane preporuke nazivaju se i korisniku stavke preporuke (U2I). | Booleove vrijednosti <br>Zadani: True
|*EnableModelingInsights* | Određuje hoće li se izvanmrežne izračun trebaju izvesti prikupljanje uvide Modeliranje (to jest, preciznost i raznolikošću metriku). Ako je postavljen na true, podskup podataka će se koristiti za osposobljavanje Budući da je moraju biti rezervirana za testiranje modela. Dodatne informacije o [izvanmrežne procjene](#OfflineEvaluation). | Booleove vrijednosti <br> Zadani: False
| *SplitterStrategy* | Ako je Omogući Modeliranje uvida postavljen na *true*, to je kako se podataka treba podijeliti u svrhu procjene.  | Niz, *RandomSplitter* ili *LastEventSplitter* <br>Zadani: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>Vrsta Sastavi FBT ###

Često bought Sastavi zajedno (FBT) ne analizu koji broji koliko je puta dvije ili tri različite proizvode Suradnja pojavljuju zajedno. Zatim se sortira skupa na temelju funkcije sličnosti (**zajednički pojavljivanja**, **Jaccard**, **Podignite**).

Smatrati **Jaccard** i **Podignite** načini normalizirati zajednički pojavljivanja.  To znači da se stavke će vratiti samo ako su gdje je kupljena zajedno s stavke Početni broj.

U našem primjeru telefon Lumia 650 telefona X vratit će se samo ako je telefon X kupnje u istoj sesiji kao Lumia 650 telefona. Jer to može biti vjerojatno, smo očekivali stavke da biste 650 Lumia koja se vraća komplementarnu na primjer, zaslon zaštitnika ili power prilagodnik za Lumia 650.

Trenutno dvije stavke pretpostavlja da su kupili u istoj sesiji ako se pojavljuju u transakcija s istom korisnički ID i vremenske oznake.

Izgradi FBT ne podržava Hladna stavki jer definition kakvu očekuju dvije stavke koje se kupiti u istom transakcijom. Dok FBT izgradi možete vratiti skupova stavki (skupine), ne podržavaju personalizirane preporuke jer prihvaćaju jedan Početni broj stavke kao unos.


#### <a name="fbt-build-parameters"></a>FBT Sastavi parametara

| Ime  |   Opis |       Vrsta  <br> Valjane vrijednosti <br> (Zadana vrijednost)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Kako običan je model. Broj pojavljivanja dodatnih stavki koje će se smatrati Modeliranje. |  Cijeli broj, <br> 3 50 <br> (6)
| *FbtMaxItemSetSize* | Bounds broj stavki u skupu često korištenih.| Cijeli broj  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Minimalna rezultat koji često korištenih skup imat će biti uključen u opseg vraćenih rezultata. Veći bolje. | Dvaput <br> 0 i noviji <br> (0)
| *FbtSimilarityFunction* | Definira funkciju sličnosti koriste sastavljanje. **Podignite** bolje serendipity, **zajednički pojavu** bolje predvidljivost i **Jaccard** je narušena pouzdanost između dva. | Niz  <br>  <i>cooccurrence Dizalica, jaccard</i><br> Zadani: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Odabir i procjenu međuverzije ##

Ovaj vodič vam mogli pomoći odrediti hoće li koristite preporuke Sastavi ili je FBT Sastavi, ali ne nudi potpuni odgovora u slučajevima gdje možete koristiti bilo koji od njih. Osim toga, čak i ako znate koji želite koristiti vrstu Sastavi FBT, možda želite da biste odabrali **Jaccard** ili **Podignite** kao funkcija sličnosti.

Najbolji način da biste odabrali između dvije različite izgradi je testiranje ih u stvarnom svijetu (online Procjena) i praćenje pretvorbe stopu za različite izdanja. Tečaj nije mjeriti ovisno o klikova preporuke, Nabava stvarni broj iz preporuke prikazano ili čak i na stvarni kupnju iznosi kada su prikazani različite preporuke. Vaš metriku stopa pretvorbe temelju vaš cilj tvrtke može odabrati.

U nekim slučajevima, preporučujemo vam da biste procijenili modela izvanmrežno prije staviti u radni. Tijekom izvanmrežnog procjenu nije zamjena za procjenu online, vam može poslužiti kao metrike.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Izvanmrežna procjenu  ##

Cilj izvanmrežne izvođenja je za predviđanje preciznosti (broj korisnika koji će kupiti preporučene stavki), a raznolikošću preporuke (broj stavki koje se preporučuje se).
Tijekom mjerenja izvođenja preciznost i raznolikošću, sustav pronalazi uzorka korisnika i dijeli transakcije za korisnike u dvije grupe: dataset obuka i testiranje skupu podataka.

> [AZURE.NOTE]Da biste koristili izvanmrežne metriku, morate imati vremenske oznake u vašim podacima za korištenje.
> Vrijeme podaci su potrebne za podjelu korištenje pravilno između obuka i testiranje skupova podataka.

> Također, izvanmrežne procjenu može dati rezultate za small korištenje datoteke. Za procjenu biti temeljito, postoje mora biti najmanje 1000 korištenje točaka u skupu podataka za testiranje.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Preciznost pri k ###
Sljedeća tablica predstavlja izlaz izvođenja izvanmrežne preciznost pri k.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Postotak |   13.75 | 18.04   | 21 |  24.31 | 26.61
|Korisnici u testu |    10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Korisnici koji se smatra | 10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Korisnici koji nisu uzeti u obzir | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
U prethodnoj tablici *k* predstavlja broj preporuke prikazano je korisnik odabrao. U tablici čita na sljedeći način: "Ako tijekom razdoblja test samo jedan preporuke prikazana je kupcima, samo 13.75 korisnika bi ste kupili te preporuke." Izjavu o zaštiti temelji se na pretpostavci da modela je put uvježbavan s podacima za kupnju. Drugi način za to reći jest da je preciznost od 1 13.75.

Primijetit ćete da kao što je više stavki prikazuju se klijentu, vjerojatnost kupca kupnju preporučene stavke pomiče se gore. Za prethodni stalna vjerojatnost gotovo brojevima dvostruke preciznosti postotak 26.61 kada preporučuje se 5 stavki.

#### <a name="percentage"></a>Postotak
Prikazan je postotak korisnicima koji su se barem jedno od preporuke *k* komunicirao. Dijeljenjem broja korisnika koji se komunicirao barem jedan preporuke ukupan broj korisnika koji se smatra se izračunava postotak. Korisnici koji se smatra dodatne informacije potražite u članku.

#### <a name="users-in-test"></a>Korisnici u testu
Podatke u redak predstavlja ukupan broj korisnika u skupu podataka za testiranje.

#### <a name="users-considered"></a>Korisnici koji se smatra
Korisnik smatra se samo ako sustav barem preporučuje *k* stavki na temelju modela stvaraju korištenjem obuka skupu podataka.

#### <a name="users-not-considered"></a>Korisnici koji nisu uzeti u obzir
Podaci u redak predstavljaju svi korisnici koji nisu uzeti u obzir. Korisnici koji nisu primili barem *k* preporučuje stavke.

Korisnik koji se ne smatra = korisnici u testu – smatra korisnika

<a name="Diversity"></a>
### <a name="diversity"></a>Raznolikošću ###
Raznolikošću metriku Izmjerite vrste stavki koji se preporučuje. Sljedeća tablica predstavlja izlaz izvođenja raznolikošću izvanmrežno.

|PERCENTILE grupe |    0 90|  90 99| 99 100
|------------------|--------|-------|---------
|Postotak        | 34.258 | 55.127| 10.615


Ukupno stavki koji se preporučuje: 100,000

Jedinstvenih stavki koji se preporučuje: 954

#### <a name="percentile-buckets"></a>Memorijski blokovi percentil
Svake grupe percentil predstavlja raspon (minimalne i maksimalne vrijednosti u rasponu između 0 i 100). Stavke blizu 100 najpopularnije stavke te stavke blizu 0 najmanje Popularno. Na primjer, ako je postotak vrijednost percentila grupe 99 100 10.6, to znači da 10.6 postotak preporuke vratiti samo gornji jedan postotak najpopularnijih stavki. Grupe minimalnu vrijednost percentila uključivo, a najveću vrijednost isključujući te dvije vrijednosti, osim 100.
#### <a name="unique-items-recommended"></a>Jedinstvenih stavki koji se preporučuje
Jedinstvenih stavki preporučuje metriku prikazuje broj stavki koje su vraćeni procjene.
#### <a name="total-items-recommended"></a>Ukupan broj stavki koji se preporučuje
Ukupan broj stavki preporučuje metriku prikazuje broj stavki koji se preporučuje. Neke se možda duplikate.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Izvanmrežna procjenu mjerenja ###
Preciznost i raznolikošću izvanmrežne metriku može biti korisna kad odaberete koje Sastavi da biste koristili. Sastavljanje trenutku kao dio odgovarajući FBT ili preporuke Sastavi parametara:

-   Sastavljanje parametar *enableModelingInsights* postavite na **true**.
-   Po želji odaberite *splitterStrategy* ( *RandomSplitter* ili *LastEventSplitter*).
*RandomSplitter* dijeli podataka o korištenju u vlaku i testiranje postavlja ovisno o postotak test dani *randomSplitterParameters* i izravnim Početni broj vrijednosti.
*LastEventSplitter* dijeli podataka o korištenju u vlaku i testiranje skupovi koji se temelji na zadnji transakcije za svakog korisnika.

To će pokrenuti Sastavi koristi samo podskup podataka za obuku i koristi ostatak podataka za izračunavanje procjenu metriku.  Po dovršetku sastavljanje da biste dobili rezultat izračuna, morate nazvati na [Početak Sastavi metriku API-JA](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), prosljeđivanje odgovarajući *modelId* i *buildId*.

 Slijedi JSON izlaz za procjenu uzorka.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
