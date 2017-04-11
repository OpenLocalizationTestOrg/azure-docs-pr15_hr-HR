<properties
    pageTitle="Kako prepoznati scenariji i plan za napredne analize podataka obrada | Microsoft Azure"
    description="Plan za napredne analize, preporučuje se niz ključa pitanja."
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


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Prepoznavanje scenariji i plan za obradu napredne analize podataka

Resursima treba namjeravate uvrstiti prilikom postavljanja okruženju učiniti naprednom analitikom obrade na skup podataka? U ovom se članku predlaže niz pitanja zatražiti koji će olakšati prepoznavanje zadataka i resursima odgovarajući scenariju. Redoslijed više razine korake predvidljivu analitičkih obrubljena je u [što je timu podataka znanstvenog procesa (TDSP)?](data-science-process-overview.md). Određene resursi svaki od ovih koraka potrebno za važne za određeni scenariju zadatke. Ključni pitanja da biste odredili scenariju tiču podataka logistika, karakteristike, kvalitete skupova podataka, kao i alati i jezike koje želite učiniti analizu.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistika pitanja: mjesta podataka i premještanja
Logistika pitanja tiču mjesto **izvora podataka**, u **ciljne odredište** u Azure i preduvjeti za premještanje podataka, uključujući raspored, iznos i resursi koji je uključen. Podatke možda morati premjestiti nekoliko puta tijekom analize. Uobičajeni scenarij je da biste premjestili lokalnih podataka u neki oblik prostora za pohranu na Azure, a zatim u Studio strojnog učenja.

1. **Što je izvor podataka?** Je li lokalnom ili u oblaku? Ako, na primjer:
    - Podaci su javno dostupan na adresi za HTTP.
    - Podaci se nalaze u lokalnoj mreži mjesto datoteke.
    - Podaci u bazi podataka sustava SQL Server.
    - Podaci spremljeni u spremniku programa Azure prostora za pohranu

2. **Što je odredište Azure?** Gdje je mora biti za obradu ili Modeliranje? Ako, na primjer:
    - Spremište blobova platforme Azure
    - Baze podataka SQL Azure
    - SQL Server na Azure VM
    - HDInsight (Hadoop na Azure) ili grozd tablice
    - Azure strojnog učenja
    - Pretvoriti Azure virtualne tvrdi disk.

3. **Kako koju ćete premjestiti podatke?** Postupke i resurse koji su dostupni za ingest ili podatke učitali u raznim različite prostora za pohranu i obrada okruženja su strukturirane u sljedećim temama.

    -  [Podatke učitali u okruženju za pohranu za analizu](machine-learning-data-science-ingest-data.md)
    -  [Uvoz podataka obuka u Azure strojnog učenja Studio iz različitih izvora podataka](machine-learning-data-science-import-data.md).

4. **Podaci moraju li premjestiti redovito ili mijenjati tijekom migracije?** Razmislite o korištenju tvorničke Azure podataka (ADF) kada je podatke potrebno neprestano migrirati, osobito ako scenarija hibridnog koji može pristupiti i na lokaciji i resursi oblaka sudjeluje ili kada podatke je transakcije ili potrebno izmijeniti ili imaju poslovne logike u njega dodali tijeku migrira. Dodatne informacije potražite u članku [Premještanje podataka iz lokalnog sustava SQL server u SQL Azure s tvorničke Azure podataka](machine-learning-data-science-move-sql-azure-adf.md)

5. **Koliko je podataka će se premjestiti u Azure?** Skupove podataka koji su vrlo velike prekoračiti kapacitet mnogim okruženjima za pohranu. Na primjer, pogledajte rasprave ograničenja veličine za strojnog učenja Studio u sljedećem odjeljku. Uzorak podataka u tim slučajevima može se koristiti tijekom analizu. Pojedinosti o tome kako dolje uzorka skup podataka u različitim okruženjima Azure potražite u članku [ogledne podatke u postupku znanstvenog timu podataka](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Podataka značajke pitanja: vrsta, oblik i veličinu
Ta pitanja su vam ključne za planiranje prostora za pohranu i obrada okruženja, od kojih svaka su za različite vrste podataka i koje imaju određene ograničenja.

1. **Što su vrste podataka?** Ako, na primjer:
    - Numeričke
    - Categorical
    - Nizovi
    - Binarni

2. **Kako se oblikuje podataka?** Ako, na primjer:
    - Točke sa zarezom (CSV) ili tabulatorom (TSV) plošna datoteke
    - Sažete ili dekomprimirati
    - Azure blob-ova
    - Hadoop grozd tablice
    - Tablice sustava SQL Server

2. **Kako velike se vaši podaci?**
    - Mala: manje od 2GB
    - Srednja: Veće od 2GB i manje od 10GB
    - Veliki: Veći od 10GB

Okruženje za Azure strojnog učenja Studio uzimaju na primjer:

- Popis oblika podataka i koje podržava Azure strojno učenje Studio potražite u članku [oblici i podataka vrste podataka podržane](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) sekcije.
- Informacije o ograničenja podataka za Azure strojnog učenja Studio potražite u članku na **veličinu skup podataka može za moj module?** dio [Uvoz i izvoz podataka za učenje za računala](machine-learning-faq.md#machine-learning-studio-questions)

Informacije o ograničenjima drugih servisa za Azure koristi u postupku analize potražite u članku [Azure pretplate i ograničenja servisa, kvote, i ograničenja](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Pitanja kvalitete podataka: prije obradu i istraživanje

1. **Što vam je znati o vašim podacima?** Istraživanje podataka kada je potrebno da bi se dobio programa objašnjenje karakteristika osnovni. Što uzoraka ili trendova se prikazuje, što je outliers sadrži ili nedostaju koliko ima vrijednosti. Ovaj korak nije važno za određivanje opsegu stara obrada potrebne za formulating hipotezu koji bi mogli Predložite značajku najprikladnije ili upišite analize i formulating tarife za prikupljanje dodatnih podataka. Opisna statistika izračunavanje i crtanje vizualizacije su korisne postupaka za provjeru podataka. Pojedinosti o tome da biste istražili skup podataka u različitim okruženjima Azure, potražite u članku [Istraživanje podataka u postupku znanstvenog timu podataka](machine-learning-data-science-explore-data.md).

2. **Ne podatke potrebne predobradu ili čišćenje?**
Predobradu i čišćenje podataka su važne zadatke koje obično mora se provoditi prije skup podataka mogu se koristiti učinkovito za strojnog učenja. Sirovim podacima najčešće oscilirajuće i nepouzdanih, a možda nema vrijednosti. Pomoću tih podataka za Modeliranje može proizvesti netočni rezultate. Opis, potražite u članku [učenje zadaci Priprema podataka za poboljšane računalo](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Alati i jezici pitanja
Postoje razne mogućnosti koje su ovdje ovisno o tome koji Jezici i okruženja za razvoj ili Alati potrebno ili najčešće conformable pomoću.

1. **Koje jezike želite koristiti za analizu?**  
    - R
    - Python
    - SQL

2. **Koje alate morate koristiti za analizu podataka?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) – skripte jezik koji se koristi za administriranje Azure resurse skripte jeziku.
    - [Azure strojnog učenja Studio](machine-learning-what-is-ml-studio/)
    - [Revolucija Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python alate za Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter bilježnica](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Prepoznavanje naprednom analitikom scenariju
Nakon što ste ime odgovorili na pitanja iz prethodnog odjeljka, spremni ste za određivanje koji scenarij koji najbolje odgovara slučaj. Ogledni Scenariji su strukturirane u [scenariji za napredne analize u Azure strojnog učenja](machine-learning-data-science-plan-sample-scenarios.md).
