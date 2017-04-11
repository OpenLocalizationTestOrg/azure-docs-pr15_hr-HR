<properties
    pageTitle="Zadaci koje ćete morati Priprema podataka za poboljšanu računala učenje | Microsoft Azure"
    description="Unaprijed postupak i čišćenje podataka da biste se pripremili za strojnog učenja."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Zadaci koje ćete morati Priprema podataka za poboljšanu računala učenje

Predobradu i čišćenje podataka su važne zadatke koje obično mora se provoditi prije skup podataka mogu se koristiti učinkovito za strojnog učenja. Sirovim podacima najčešće oscilirajuće i nepouzdanih, a možda nema vrijednosti. Pomoću tih podataka za Modeliranje može proizvesti netočni rezultate. Zadaci su dio od u timu podataka znanstvenog procesa (TDSP) i obično slijedite početne Istraživanje od skup podataka koristi da biste otkrili i planiranje prije obrade potrebne. Detaljnije upute o postupku TDSP potražite u članku korake navedene u [Timu podataka znanstvenog postupak](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Obrada stara i čišćenje zadatke poput zadatka Istraživanje podataka, možete provesti u razna okruženja, kao što je SQL ili grozd ili Azure strojnog učenja Studio i pomoću raznih alata i jezicima, na primjer R ili Python, ovisno o pohrane podataka i kako se oblikuje. Budući da je TDSP izračun s iteracijama u prirode, zadaci može obaviti na razne korake u tijeku rada postupka.

U ovom se članku predstavlja različite koncepata obradu podataka i zadatke koji možete obavljeni prije ili nakon ingesting podataka u Azure strojnog učenja.

Primjer Istraživanje podataka i stara obrada gotovo unutar Azure strojnog učenja studio, potražite u članku [predobradu podataka u Azure strojnog učenja Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) videozapis.


## <a name="why-pre-process-and-clean-data"></a>Zašto unaprijed postupak i čišćenje podataka?

Podatke u stvarnom svijetu je prikupili iz različitih izvora i procesa, i to može sadržavati nepravilnosti ili oštećen podataka ugrozi kvalitete skupu podataka. Na tipičnog podatkovnog probleme s kvalitetom do kojih su:

* **Nepotpuna**: podaci nedostaju atributa ili koji sadrži vrijednosti koje nedostaju.
* **Noisy**: podataka sadrži pogrešne zapise ili outliers.
* **Nekonzistentna**: podaci sadrže sukobljenih zapisa ili nepodudaranja.

Podaci za kvalitete preduvjeta za kvalitete predvidljivu modela. Da biste izbjegli "smeća u smeća out" i poboljšanje kvalitete podataka i stoga modela performanse, izvršite na zaslon stanja podataka ranijeg uočiti problemi s podacima i odlučivanje o odgovarajuće obradu podataka i čišćenje korake od izuzetne je.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Što su neke zaslonima stanja tipičnog podatkovnog koji su zapošljavanju?

Potvrđivanjem smo možete provjeriti Općenito kvalitete podataka:

* Broj **zapisa**.
* Broj **(ili **značajke**)** .
* Atribut **vrste podataka** (nominal, redni broj ili neprekinuti).
* Broj **nema vrijednosti**.
* **Formedness za izvor** podataka.
    * Ako se podaci nalaze u TSV ili CSV, provjerite da stupac tisućica i razdjelnike redak uvijek ispravno odvojite stupaca i redaka.
    * Ako su podaci u obliku HTML ili XML, provjerite je li podatke je dobro oblikovan utemeljene na njihove odgovarajući standardima.
    * Raščlanjivanje možda će biti potrebno da biste izdvojili strukturirane podatke iz djelomično strukturirane i nestrukturirane podatke.
* **Zapisi koje nisu usklađene podataka**. Provjerite je li dopušten raspon vrijednosti. Npr. Ako podaci sadrže učenika Prosjek, provjerite je li u Prosjek se određeni raspon izgovorite 0 ~ 4.

Kada pronađete problemi s podacima, **obradu korake** potrebne koji često obuhvaća Čišćenje vrijednosti koje nedostaju, normalizacija podataka, discretization, tekst obrade uklonite i/ili zamjena ugrađene znakova koji mogu utjecati na poravnanja podataka, vrste zajednički koriste različite verzije podataka polja i drugim korisnicima.

**Azure strojnog učenja troši ispravnog tablične podatke**.  Ako se podaci već nalaze u tabličnom obliku, stara obradu podataka mogu se izvršiti izravno s Azure strojno učenje u učenje Studio računala.  Ako podaci ne nalaze u tabličnom obliku, izgovorite je u XML, možda će biti potrebne da biste pretvorili podatke u tabličnom obliku analize.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Što su neke od glavnih zadataka u stara obradu podataka?

* **Čišćenje podataka**: ispunite ili vrijednosti koje nedostaju, otkrivanje i uklanjanje oscilirajuće i outliers.
* **Transformaciju podataka**: normalizacija podataka da biste smanjili dimenzija i Šum.
* **Smanjenje podataka**: uzorak podataka zapisa ili atribute jednostavnije obradu podataka.
* **Discretization podataka**: pretvorite neprekinuti atribute categorical atribute radi jednostavnog korištenja s određene načinima strojnog učenja.
* **Čišćenje teksta**: uklanjanje ugrađene znakova koji mogu uzrokovati nepravilno poravnanje podataka, za – primjerice, ugrađeni kartice u razgraničen tabulatorom podatkovnu datoteku, ugrađeni novih redaka koji se mogu prekinuti zapisa itd.

U odjeljcima detaljno neke od ovih koraka za obradu podataka.

## <a name="how-to-deal-with-missing-values"></a>Kako se baviti vrijednosti koje nedostaju?

Baviti vrijednosti koje nedostaju, preporučuje se da biste najprije otkrivanju razloga vrijednosti koje nedostaju ručicu za bolje problem. Uobičajeni nedostaje vrijednost rukovanje metode su:

* **Brisanje**: uklanjanje zapisa koji sadrže vrijednosti koje nedostaju
* **Zamjena sustava**: zamjena vrijednosti koje nedostaju sustava vrijednošću: _Nepoznato_ za categorical ili 0, npr. za numeričke vrijednosti.
* **Znače zamjena**: Ako je numeričke podatke koji nedostaju, zamijenite vrijednosti koje nedostaju srednje vrijednosti.
* **Česti zamjena**: Ako su podaci nedostaju categorical, zamijenite vrijednosti koje nedostaju Najčešći stavke
* **Zamjena regresije**: zamijenite nedostaju vrijednosti regressed pomoću metode regresije.  

## <a name="how-to-normalize-data"></a>Kako se normalizacija podataka?

Normalizacija podataka ponovno mijenja veličinu numeričke vrijednosti u navedenom rasponu. Popularne podataka normalizacija metode obuhvaćaju sljedeće:

* **Min Max normalizacija**: Linearno pretvaranje podataka u rasponu, izgovorite između 0 i 1, gdje je neproporcionalno minimalne vrijednosti 0 i Maks vrijednost 1.
* **Normalizacija Z rezultat**: skaliranje podataka na temelju srednju vrijednost i standardnu devijaciju: razlika između podataka i srednje vrijednosti podijelite standardnu devijaciju.
* **Decimalni skaliranje**: skaliranje podatke pomicanjem decimalnog zareza vrijednost atributa.  

## <a name="how-to-discretize-data"></a>Kako discretize podataka?

Podatke možete biti discretized pretvaranjem neprekinuti vrijednosti nominal atribute ili intervala. Nekoliko primjera to su:

* **Jednako širine Binning**: dijeljenje raspon svim mogućim vrijednostima parametra atribut u grupe N iste veličine i dodijelite vrijednosti koje se nalaze u smeće sustava s brojem za smeće.
* **Jednake visine Binning**: podijelite raspon svim mogućim vrijednostima parametra atribut grupe N svaki koje sadrže isti broj instanci, a zatim dodijeliti vrijednosti koje se nalaze u smeće sustava s brojem za smeće.  

## <a name="how-to-reduce-data"></a>Kako smanjiti količinu podataka?

Da biste smanjili veličinu podataka za lakše podatke rukovanje različitih načina. Ovisno o veličini podataka i domena, mogu se primijeniti na sljedeće načine:

* **Zapis uzorkovanje**: uzorak podataka zapisa, a samo odaberite predstavniku podskup podataka.
* **Atribut uzorkovanje**: odaberite samo podskup atribute najvažnije iz podataka.  
* **Zbrajanje**: dijeljenje podatke u grupe i pohraniti brojeve za svaku grupu. Na primjer, dnevno prihod brojeve restoran lanac tijekom proteklih 20 godina možete zbrojiti da biste mjesečnog prihoda da biste smanjili veličinu podataka.  

## <a name="how-to-clean-text-data"></a>Kako se očistiti tekstne podatke?

**Tekstna polja u tablične podatke** mogu sadržavati znakove koje utječu na poravnanje i/ili zapis granica stupaca. Za – primjerice, ugrađeni kartice u nepravilno poravnanje tabulatorom uzrok stupac i ugrađene znakove novog retka prekinuti zapisa crte. Rukovanje kodiranja nepravilnom teksta tijekom pisanja/čitanja teksta vodi do gubitka podataka, slučajne Uvod pronašao znakova, npr i vrijednostima null, možda i utječu na tekst Raščlanjivanje. Pažljivo analize i uređivanje može biti potrebno da biste očistili tekstna polja za proper poravnanje i/ili izdvojiti strukturirane podatke iz podataka nestrukturirane ili djelomično Strukturirani tekst.

**Istraživanje podataka** nudi Prijevremeni prikaz podatke. Broj problema s podataka može biti neotkrivenih tijekom ovaj korak, a odgovarajuće metode koje se mogu primijeniti da biste riješili probleme.  Važno je da postavljati pitanja kao što su što je izvor problema i kako se problem može imati uveden. To pomaže odlučite na obradu podataka koraci koje morate poduzeti da biste ih riješili. Vrste uvida nešto Kra jnjeg za dobivanje glavne iz podataka i omogućuje određivanje prioriteta trud obradu podataka.

## <a name="references"></a>Reference

>*Dubinsku analizu podataka: koncepata i tehnika*, treći Edition, Morgan Kaufmann 2011, pretvorba Jiawei, Micheline Kamber i Jian Pei
