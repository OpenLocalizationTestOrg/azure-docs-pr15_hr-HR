<properties
    pageTitle="Vodič za brzi početak rada za R jezik za strojnog učenja | Microsoft Azure"
    description="Slijedite ovaj praktični programiranje R da biste započeli brzo pomoću jezika R Azure strojnog učenja Studio stvaranje predviđanja rješenja."
    keywords="brzi početak rada, r jezik, r programski jezik, r programski vodič"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="garye"/>

# <a name="quickstart-tutorial-for-the-r-programming-language-for-azure-machine-learning"></a>Vodič za brzi početak rada za R programskom jeziku za Azure strojnog učenja

Ikona F Elston, Ph.D.

##  <a name="introduction"></a>Uvod

Pomoću ovog praktičnog vodiča za brzi početak rada pomaže vam brzo pokretanje proširivanje Azure strojnog učenja pomoću R programski jezik. Slijedite ovaj R programiranje Praktični vodič da biste stvorili, testirajte i izvršiti kod R unutar Azure strojnog učenja. Tijekom rada kroz Praktični vodič potpuno rješenje predviđanja će stvoriti i pomoću jezika R u Azure strojnog učenja.  

Microsoft Azure strojnog učenja sadrži mnogo Napredna strojnog učenja i podataka rukovanje module. Napredna jezik R je opisan franca dodatno od analize. Zadovoljno, rukovanje analize i podataka u Azure strojnog učenja mogu se proširiti pomoću R. Ovu kombinaciju pruža skalabilnost i jednostavnog implementacije Azure strojnog učenja fleksibilnost i precizno analize R.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


###<a name="forecasting-and-the-dataset"></a>Predviđanje i skup podataka

Predviđanje je široko zapošljavanju i vrlo koristan način analytical. Zajednički koristi u rasponu od procjenjivanje Prodaja sezonski stavki, određivanje razine optimalnih zalihe, da biste procjenjivanje macroeconomic varijabli. Predviđanje obično radite s modelima niza vremena.

Niz vrijeme podaci podataka koja omogućuje vrijednosti vremena indeksa. Vrijeme indeksa može biti pravilnim, npr svakog mjeseca ili svake minute ili nepravilnim. Model niz vrijeme temelji se na vrijeme niza podataka. R programskom jeziku sadrži fleksibilne framework i proširenom analize podataka niza vremena.

U ovom vodiču za brzi početak rada ne možemo bit će rad s Kaliforniji Mliječni proizvodi proizvodnje i cijene podataka. Ti podaci obuhvaćaju i mjesečne informacije na radne nekoliko Mliječni proizvodi proizvoda i cijenu mlijeko fat, usporednih obavješćivanje.

Podaci koji se koriste u ovom članku, zajedno s skripte R može [preuzeti ovdje][download]. Ove podatke je izvorno synthesized iz informacije dostupne na University Wisconsin pri http://future.aae.wisc.edu/tab/production.html.

### <a name="organization"></a>Tvrtke ili ustanove

Ne možemo će proći kroz nekoliko koraka dok učite kako stvoriti, testirajte i izvršavanje analize i podataka rukovanje R koda u okruženju Azure strojnog učenja.  

* Najprije ćemo istražiti osnove korištenja R jezika u okruženju Azure strojnog učenja Studio.

* Zatim ćemo tijek za razgovarate o razne aspekte/i, R kod ili grafike u okruženju Azure strojnog učenja.

* Ne možemo će izgraditi prvi dio naš predviđanja rješenje stvaranjem kod za čišćenje podataka i transformacije.

* S oglednim podacima pripremili smo će pokrenuti analizu korelacija između nekoliko varijabli u našem skup podataka.

* Na kraju, ćemo stvoriti niz sezonski vremena predviđanja model za proizvodnju mlijeko.

##<a id="mlstudio"></a>Interakcija s jezikom R u strojnog učenja Studio

U ovom se odjeljku vodi kroz neke osnove interakcija s R programski jezik u okruženju Studio strojnog učenja. Jezik R nudi naprednih alata za stvaranje prilagođene analize i module za rukovanje podataka u okruženju Azure strojnog učenja.

RStudio će koristiti za razvoj, testiranje i R kod small skalu za ispravljanje pogrešaka. Kod je zatim Izreži i Zalijepi u [Izvršavanje skripte R] [ execute-r-script] modul u Studio strojno učenje spreman za pokretanje.  

###<a name="the-execute-r-script-module"></a>Modul za izvršavanje skripte R

Unutar strojnog učenja Studio, R skripte izvodi unutar [Izvršavanje skripte R] [ execute-r-script] modul. Primjer [Izvršavanje skripte R] [ execute-r-script] modul u strojnog učenja Studio prikazuju se u slika 1.

 ![R programskom jeziku: Modul skripte u izvršavanje R odabran u strojnog učenja Studio][1]

*Slika 1. Okruženje strojnog učenja Studio prikazuje modul izvršavanje skripte R odabran.*

Referenciranje slika 1, pogledajmo neke od glavne dijelove strojnog učenja Studio okruženje za rad s [Izvršavanje skripte R] [ execute-r-script] modul.

- Moduli u pokusa prikazuju se u oknu centra.

- Gornji dio u desnom oknu sadrži prozor za prikaz i uređivanje skripte R.  

- U donjem dijelu desnom oknu prikazuje neka svojstva [Izvršavanje skripte R][execute-r-script]. Zapisnike o pogreškama i izlazni možete vidjeti klikom na odgovarajućim mjestima ovaj okna.

Ne možemo će, Naravno, biti razgovarate o [Izvršavanje skripte R] [ execute-r-script] nešto podrobnije u ostatku dokumenta.

Kada radite s funkcijama složene R, li preporučujemo da uređivanje, testirajte i ispravljanje pogrešaka u RStudio. Kao što je razvoju bilo koji softver sustava proširivanje kod postupno i testiranje na small jednostavne test slučajeve. Zatim izrezivanje i lijepljenje na funkcije u prozor za skripte R [Izvršavanje skripte R] [ execute-r-script] modul. Taj se način omogućuje harness RStudio integrirano razvojno okruženje (IDE) i na potenciju Azure strojnog učenja.  

####<a name="execute-r-code"></a>Izvršavanje kod R

R kod u [Izvršavanje skripte R] [ execute-r-script] modul će se izvoditi kada pokrenete pokusa klikom na gumb **Pokreni** . Prilikom izvršavanja dovrši, kvačicu će se pojaviti na [Izvršavanje skripte R] [ execute-r-script] ikona.

####<a name="defensive-r-coding-for-azure-machine-learning"></a>Za Azure strojnog učenja defensive kodiranje R

Ako razvijate R šifru, recimo, web-servisa pomoću Azure strojnog učenja, trebali biste sigurno planirate kako kod će baviti ulazni neočekivane podatke i iznimke. Da biste zadržali preglednosti, mogu imati obuhvaćen približno in the way of provjeru ili iznimke rukovanja u većini prikazanim primjerima kod. Međutim, kao što je ne možemo nastaviti li steći ćete nekoliko primjera funkcija pomoću iznimku R-rad s mogućnošću pretraživanja kroz razine.  

Ako vam je potrebna potpuniji pokusa od rukovanje iznimku R, li preporučujemo da čitate primjenjivo sekcije knjige tako da Wickham na popisu [Dodatak B – daljnje čitanje](#appendixb).


####<a name="debug-and-test-r-in-machine-learning-studio"></a>Ispravljanje pogrešaka i testiranje R u strojnog učenja Studio

Reiterate, li preporučuju testirajte i ispravljanje pogrešaka u kodu R small skalu u RStudio. No postoje slučajevi gdje ćete morati Utvrđivanje uzroka problema kod R u [Izvršavanje skripte R] [ execute-r-script] sam. Osim toga, dobro je da biste provjerili rezultate u Studio strojnog učenja.

Izlaz iz izvođenja koda R i na platformi Azure strojnog učenja nalazi se prvenstveno u output.log. Nekih dodatnih informacija će se prikazivati u error.log.  

U slučaju pogreške u strojnog učenja Studio prilikom pokretanja kod R, možete pogledati error.log mora biti na prvom tečaja akcije. Ove datoteke mogu sadržavati korisne poruke o pogreškama da biste bolje razumjeli i ispravite pogrešku. Da biste pogledali error.log, kliknite **Prikaz evidencije pogrešaka** u **oknu svojstva** za [Izvršavanje skripte R] [ execute-r-script] s pogreškom.

Ako, na primjer, li pokrenete sljedeći kod R s programa Nedefinirano varijable y u [Izvršavanje skripte R] [ execute-r-script] modula:

    x <- 1.0
    z <- x + y

Kod ne izvrši, rezultira pogrešku. Klikom na **Prikaz evidencije pogrešaka** u **oknu svojstva** daje prikaz prikazano slika 2.

  ![Poruka o pogrešci skočnim][2]

*Slika 2. Poruka o pogrešci skočni.*

Čini se da moramo Pogledaj u output.log da biste vidjeli poruku o pogrešci R. Kliknite na [Izvršavanje skripte R] [ execute-r-script] pa kliknite na stavci **Prikaz output.log** u **oknu svojstva** udesno. Otvorit će se novi prozor preglednika, a mogu vidjeti sljedeće.


    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found
    
    
    object 'y' not found
    ----------- End of error message from R -----------

Poruka o pogrešci sadrži bez iznenađenja i jasno označava problem.

Provjerite vrijednost bilo koji objekt u R, možete ispisati te vrijednosti output.log datoteku. Pravila za ispitivanje vrijednosti objekt su biti jednaki onima sesiju interaktivne R. Ako, na primjer, ako upišete naziv varijable u retku, vrijednost objekt bit će ispisane output.log datoteku.  

####<a name="packages-in-machine-learning-studio"></a>Paketi u strojnog učenja Studio

U sklopu Azure strojnog učenja preko 350 predinstaliranim R jezičnih paketa. Možete koristiti sljedeći kod u [Izvršavanje skripte R] [ execute-r-script] modula za dohvaćanje popisa unaprijed instaliranih paketa.

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

Ako trenutno ne razumijete zadnji redak kod, nastavite čitati. U ostatku dokumenta ne možemo opsežno obrađuje pomoću R u okruženju Azure strojnog učenja.

### <a name="introduction-to-rstudio"></a>Uvod u RStudio

RStudio je široku IDE za R. Će koristiti RStudio za uređivanje, testiranje i ispravljanje pogrešaka neke R kod koji se koristi u ovom vodiču za brzi početak rada Kod R je testirano i spremni, jednostavno izrezivanje i iz uređivača RStudio zalijepite strojnog učenja Studio [Izvršavanje skripte R] [ execute-r-script] modul.  

Ako nemate instaliran na vašem stolnom računalu R programski jezik, li preporučujemo da to učinite sada. Besplatno preuzeti Otvori izvor R jezika dostupnih na na potpun R arhiva mreže (CRAN) pri [http://www.r-project.org/](http://www.r-project.org/). Su dostupne za Windows, Mac OS i Linux/UNIX preuzimanja. Odaberite blizini zrcalne i slijedite upute za preuzimanje. Osim toga, CRAN sadrži mnoštvo korisne analize i podataka rukovanje paketa.

Ako ste novi korisnik RStudio, potrebno je preuzmite i instalirajte verzije za stolna računala. Možete pronaći na RStudio preuzimanja za Windows, Mac OS i Linux/UNIX pri http://www.rstudio.com/products/RStudio/. Slijedite upute da biste instalirali RStudio na stolnom računalu.  

Praktični vodič Uvod u RStudio dostupan je na https://support.rstudio.com/hc/sections/200107586-Using-RStudio.

Sadrže neke dodatne informacije o korištenju RStudio u [dodatak A][appendixa].  

##<a id="scriptmodule"></a>Dohvaćanje podataka i modul izvršavanje skripte R

U ovom odjeljku ne možemo obrađuje kako dohvaćanje podataka u i Odjava iz njega [Izvršavanje skripte R] [ execute-r-script] modul. Ne možemo pregledati i kako rukovati čitati u i Odjava iz njega [Izvršavanje skripte R] različite vrste podataka[ execute-r-script] modul.

Dovršavanje kod za ovaj odjeljak je u zip datoteke prethodno preuzeti.

###<a name="load-and-check-data-in-machine-learning-studio"></a>Učitavanje i provjeru podataka u strojnog učenja Studio

####<a id="loading"></a>Učitavanje skup podataka

Ne možemo počet će učitavanjem **csdairydata.csv** datoteku u Azure strojnog učenja Studio.

- Pokrenite okruženje za Azure strojnog učenja Studio.

- Kliknite __+ NOVO__ u donjem lijevom kutu zaslona pa odaberite **skup podataka**.

- Odaberite **Iz lokalne datoteke**, a zatim **Pregledaj** da biste odabrali datoteku.

- Provjerite je li odaberete **generički CSV datoteku s uklonjenim zaglavljem (.csv)** kao vrstu skupu podataka.

- Kliknite kvačicu.

- Nakon prijenosa skupu podataka trebali biste vidjeti novi skup podataka tako da kliknete na kartici **skupova podataka** .  

####<a name="create-an-experiment"></a>Stvaranje pokusa

Kada imamo neke podatke u strojnog učenja Studio, moramo stvaranje pokusa za analizu.  

- Kliknite __+ NOVO__ u donjem lijevom i odaberite **eksperiment**, a zatim **Prazna eksperiment**.

- Vaš eksperiment možete nazvati tako da odaberete i izmjena naslova **eksperiment stvoreno...** pri vrhu stranice. Da biste **Analizirali Mliječni proizvodi CA**, na primjer, promjena.

- Na lijevoj strani stranice eksperiment, proširite **Spremili skupova podataka**, a zatim **Moje skupova podataka**. Trebali biste vidjeti **cadairydata.csv** koji ste prethodno prenijeli.

- Povucite i ispustite **csdairydata.csv dataset** premjestite pokusa.

- U okvir **pretraživanje Poigrajte stavki** pri vrhu stranice u lijevom oknu upišite [Izvršavanje skripte R][execute-r-script]. Prikazat će se modul koji se pojavljuju na popisu Pretraži.

- Povucite i ispustite [Izvršavanje skripte R] [ execute-r-script] modula na vašem paleta.  

- Izlazni **skup podataka csdairydata.csv** povezati krajnje lijeve unos (**Dataset1**) [Izvršavanje skripte R][execute-r-script].

- **Nemojte zaboraviti kliknite "Spremi"!**  

Sada na eksperiment bi morao izgledati slično slika 3.

![Mliječni proizvodi analizu CA Eksperimentirajte s skup podataka i modul izvršavanje skripte R][3]

*Slika 3. Mliječni proizvodi analizu CA isprobati skup podataka i izvoditi skripte R modul.*

####<a name="check-on-the-data"></a>Provjera podataka

Pogledajmo susret smo ste učitava u našem eksperiment podatke. U pokusa, kliknite izlaz **cadairydata.csv dataset** i odaberite **vizualizaciju**. Trebali biste vidjeti nešto poput slika 4.  

![Sažetak cadairydata.csv dataset][4]

*Slika 4. Sažetak cadairydata.csv skupu podataka.*

U ovom prikazu dobivamo mnogo korisne informacije. Vidimo prvih nekoliko redaka od tog skupa podataka. Ako ne možemo odaberite stupac, u odjeljku Statistika prikazuje dodatne informacije o stupcu. Ako, na primjer, redak vrste značajke prikazuje nam s kojim je vrstama podataka Azure strojnog učenja Studio dodijeljene stupac. Dobar sanity potvrdite pojavljuju brze izgleda ovako je prije ne možemo pokrenuti da biste to napravili ozbiljne.

### <a name="first-r-script"></a>Prvi skripte R

Stvaranje jednostavne prvi R skripta za eksperimentiranje u Azure strojnog učenja Studio. Imate stvara i testira sljedeću skriptu u RStudio.  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Sada želim prenijeti ovu skriptu Azure strojnog učenja Studio. Mogu se jednostavno Izreži i Zalijepi. Međutim, u ovom slučaju će prenijeti Moje skripte R putem zip datoteku.

### <a name="data-input-to-the-execute-r-script-module"></a>Unos podataka modul izvršavanje skripte R

Pogledajmo o čemu susret unosa [Izvršavanje skripte R] [ execute-r-script] modul. U ovom primjeru smo će pročitati Kaliforniji Mliječni proizvodi podatke u [Izvršavanje skripte R] [ execute-r-script] modul.  

Postoje tri moguće unosa za [Izvršavanje skripte R] [ execute-r-script] modul. Možete koristiti neku ili sve od ovih unosa ovisno o aplikaciji. Ujedno savršeno pametnije je pomoću skripte R koja ne zahtijeva unos uopće.  

Pogledajmo svaki od tih unosa na početak slijeva nadesno. Nazive svih ulaza možete vidjeti tako da postavite pokazivač miša iznad unos i naziv alata za čitanje.  

#### <a name="script-bundle"></a>Paket za skripte

Paket za skripte za unos omogućuje ulaze sadržaj zip datoteku u [Izvršavanje skripte R] [ execute-r-script] modul. Da biste pročitali sadržaj zip datoteke u kodu R možete koristiti jedan od sljedećih naredbi.

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [AZURE.NOTE] Azure strojnog učenja datoteka u zip tretira kao da su na src / imeničkog da morate prefiks naziva datoteka tog naziva direktorija. Ako, primjerice, zip sadrži datoteke `yourfile.R` i `yourData.rdata` u korijenu zip bi adresa kao `src/yourfile.R` i `src/yourData.rdata` prilikom korištenja `source` i `load`.

Ne možemo već spominju skupove podataka učitavanja učitavanje [skupu podataka](#loading). Kada ste stvorili i testirati skripte R prikazano u prethodnom odjeljku, učinite sljedeće:

1. Spremanje skripte R u u. R datoteka. Li poziva Moje skriptna datoteka "simpleplot. "R". Ovdje je sadržaj.

        ## Only one of the following two lines should be used
        ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
        ## If in RStudio, use the second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## The following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')

2.  Stvaranje zip datoteku i kopirajte skriptu u zip datoteku. U sustavu Windows, možete desnom tipkom miša kliknite datoteku i odaberite __Premjesti u__, a zatim __komprimiranu mapu__. To će stvoriti novu zip datoteku koja sadrži "simpleplot. R"datoteka.

3.  Dodajte datoteku **skupove podataka** u Studio učenje računala, koji određuje vrstu kao **poštanski broj**. Sada trebali biste vidjeti zip datoteke u svoje skupove podataka.

4.  Povucite i ispustite zip datoteke iz **skupova podataka** na **ML Studio platna**.

5.  Izlaz ikonu **zip podataka** povezati unos **Skripte paket** [Izvršavanje skripte R] [ execute-r-script] modul.

6.  Vrsta na `source()` funkcija uz naziv datoteke zip u prozoru koda za [Izvršavanje skripte R] [ execute-r-script] modul. U slučaju da moje upisana `source("src/simpleplot.R")`.  

7.  Provjerite je li kliknite **Spremi**.

Kada dovršite korake u nastavku, [Izvršavanje skripte R] [ execute-r-script] modul će izvršiti skripte R u zip datoteke prilikom pokretanja pokusa. Sada na eksperiment bi morao izgledati slično slici 5.

![Isprobajte ih pomoću skripte zipane R][6]

*Slika 5. Isprobajte ih pomoću skripte zipane R.*

#### <a name="dataset1"></a>Dataset1

Pravokutni tablicu s podacima možete proslijediti u kodu R pomoću Dataset1 unos. U našem jednostavnu skriptu na `maml.mapInputPort(1)` funkcija čita podatke iz priključka 1. Naziv varijable dataframe u kodu zatim dodijeljene te podatke. U našem jednostavnu skriptu prvi redak kod izvodi dodjelu.

    cadairydata <- maml.mapInputPort(1)

Izvršavanje vaše eksperiment klikom na gumb **Pokreni** . Kada se dovrši izvođenje, kliknite [Izvršavanje skripte R] [ execute-r-script] modul pa kliknite **prikaz zapisnika izlaz** u oknu svojstva. Nova stranica se trebale prikazati u pregledniku prikazuje sadržaj datoteke output.log. Kada se pomičete prema dolje trebali biste vidjeti otprilike ovako.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Ono prema dolje po stranici je detaljnije informacije o stupcima, izgledat će otprilike ovako.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

Ove rezultati su uglavnom kako treba, s 228 opažanja i 9 stupce u dataframe. Vidimo nazive stupaca, vrsta podataka R i uzorak svakog stupca.

> [AZURE.NOTE] Ovaj isti ispisanom dokumentu je pogrešna dostupna radi lakšeg iz izlaza R uređaj [Izvršavanje skripte R] [ execute-r-script] modul. Ćemo obrađuje izlaze [Izvršavanje skripte R] [ execute-r-script] modul u sljedećem odjeljku.  

####<a name="dataset2"></a>Dataset2

Ponašanje unos Dataset2 jednak onome Dataset1. Pomoću ovog unosa možete proslijediti drugi pravokutni tablicu s podacima u kodu R. Funkcija `maml.mapInputPort(2)`, s argumentom 2 se koristi za prosljeđivanje te podatke.  

###<a name="execute-r-script-outputs"></a>Izvršavanje izlaze skripte R

####<a name="output-a-dataframe"></a>Izlaz na dataframe

Izlaz sadržaj programa dataframe R kao pravokutni tablicu putem priključka Dataset1 rezultat pomoću na `maml.mapOutputPort()` (opis funkcije). U našem jednostavnu skriptu R to je izvršavati sljedeći redak.

    maml.mapOutputPort('cadairydata')

Nakon izvođenja pokusa, kliknite na izlazni priključak Dataset1 rezultat, a zatim na **Vizualiziraj**. Trebali biste vidjeti nešto poput 6 slici.

![Vizualizacija izlaz Kaliforniji Mliječni proizvodi podataka][7]

*6 slici. Vizualizacija izlazne podatke Mliječni proizvodi Kaliforniji.*

Ovaj izlaz izgleda jednako unos, točno onako kako smo očekivali.  

### <a name="r-device-output"></a>Izlaz R uređaja

Izlazni uređaj [Izvršavanje skripte R] [ execute-r-script] modul sadrži izlazne poruke i grafike. Oba standardne izlazne i standardnu pogrešku poruke r šalju izlazni priključak R uređaja.  

Da biste pogledali izlaz R uređaja, kliknite na priključak, a zatim na **Vizualiziraj**. Vidimo standardni izlazni i standardnu pogrešku iz skripte R u slici 7.

![Standardni izlazni i standardnu pogrešku iz priključka R uređaja][8]

*Slika 7. Standardni izlazni i standardnu pogrešku iz priključka R uređaja.*

Prema dolje smo vidjeli grafike Izlaz iz naših R skripte u slika 8.  

![Grafika izlaza iz priključak R uređaja][9]

*Slika 8. Izlaz iz priključka R uređaj grafike.*  

##<a id="filtering"></a>Filtriranje podataka i transformaciju.

U ovom odjeljku smo će izvršiti neke osnovne filtriranje podataka i transformacije operacije u Kaliforniji Mliječni proizvodi podatke. Do kraja u ovom se odjeljku ćemo imati podatke u obliku prikladna za stvaranje analitičkih modela.  

Preciznije, u ovom odjeljku ne možemo će poduzeti nekoliko uobičajenih podataka čišćenje i transformacije aktivnosti: upišite transformaciju, filtriranje na dataframes, dodavanje novi izračunati stupci, a vrijednost transformacije. Tu pozadinu pridonose baviti mnoge varijacije u probleme stvarnog života.

Dovršavanje R kod za ovaj odjeljak je dostupna u zip datoteke prethodno preuzeti.

### <a name="type-transformations"></a>Pretvorbe vrsta

Nakon što smo mogu čitati Kaliforniji Mliječni proizvodi podatke u kod R u [Izvršavanje skripte R] [ execute-r-script] modula, moramo provjerite imaju li podaci u stupcima svrhu unos i oblikovanje.  

R je dinamički upisanog jezik, što znači da vrste podataka su mogao prisilno smjestiti iz jedne u drugu prema potrebi. Vrste atomske podataka u R sadrže numeričke, logičke i znakova. Vrsta faktor koristi se za compactly pohranu categorical podataka. Mnogo detaljnije informacije o vrstama podataka možete pronaći u referenci u [Dodatak B – dodatno za čitanje](#appendixb).

Kada tablične podatke je za čitanje u R iz vanjskog izvora, uvijek je dobro provjeriti vrstu rezultata u stupcima. Možda želite da se u stupcu Vrsta znakova, ali u većini slučajeva to prikazat će se kao faktor ili obrnuto. U drugim slučajevima stupca mislite da mora biti numeričke predstavlja znak podatke, primjerice "1.23" umjesto 1.23 kao plutajući broj točaka.  

Srećom, jednostavno je za pretvaranje jednu vrstu u drugu, pod uvjetom da je moguće mapiranje. Ako, na primjer, "Kojeg" nije moguće pretvoriti u numeričku vrijednost, ali možete pretvoriti varijance (categorical varijabla). Drugi primjer možete pretvoriti numeričke 1 znaka "1" ili faktor.  

Vidjet ćete da sintaksa za bilo koju od tih pretvorbe je jednostavno: `as.datatype()`. Ove funkcije pretvorbe vrsta obuhvaćaju sljedeće.

* `as.numeric()`

* `as.character()`

* `as.logical()`

* `as.factor()`

Pogled na vrste podataka stupaca ne možemo unos u prethodnom odjeljku: sve stupce su vrste numeričkih, osim stupca s oznakom "Mjesec", što je vrsta znaka. Pogledajmo to pretvorili faktor i testiranje rezultate.  

Izbrisao sam redak koji stvara scatterplot matrice i dodali redak pretvaranje faktor stupac "Mjesec". U moje eksperiment će upravo sam Izreži i zalijepite kod R u prozoru koda [Izvršiti skripte R] [ execute-r-script] modul. Nije moguće ažurirati zip datoteke i prijenos Azure strojnog učenja Studio, ali to u svega nekoliko koraka.  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check the result
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Pogledajmo izvršavanje kod i pogledajte izlazni zapisnik skripte R. Relevantne podatke iz zapisnika prikazuju se u slici 9.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Slika 9. Sažetak dataframe uz faktor tjednog prikaza kalendara.*

Vrsta za mjesec sada trebalo bi pisati "**Faktor w/ razine 14**". To je problem jer se samo 12 mjeseci u godini. Možete provjeriti i vrstu u **Vizualiziraj** priključak Dataset rezultat je "**Categorical**".

Problem je da stupac "Mjesec" sadrži ne je kodira sustavno. U nekim slučajevima mjesec zove travnju i u drugim skraćen kao tra. Ne možemo problem možete riješiti tako da skraćivanje nizu u znakove 3. Redak koda sada izgleda ovako:

    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

Ponovno pokrenite pokusa i prikaz zapisnika izlaz. Očekivanih rezultata prikazuju se u slici 10.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Slika 10. Sažetak dataframe s točan broj razina faktor.*

Naš varijabla faktor sada ima željeni 12 razine.

###<a name="basic-data-frame-filtering"></a>Osnovni okvira filtriranje

R dataframes podržava naprednih mogućnosti filtriranja. Skupove podataka može biti podskup pomoću logičkih filtara na retke ili stupce. U mnogim slučajevima, složeni kriteriji će se zatražiti. Reference u [Dodatak B – dodatno čitanje](#appendixb) sadrže proširenom Primjeri filtriranje dataframes.  

Postoji jedan bitne filtriranja smo trebao učiniti na našem skup podataka. Ako pogledate stupaca u cadairydata dataframe, vidjet ćete dva nepotrebnih stupaca. Prvi stupac sadrži samo broj redaka koji je vrlo koristan. Drugi stupac, Year.Month, sadrži suvišnih informacija. Ne možemo jednostavno možete isključiti ti stupci pomoću sljedeći kod R.

> [AZURE.NOTE] Od tog trenutka u ovom odjeljku li samo vidjet ćete dodatni kod dodavanja u [Izvršavanje skripte R] [ execute-r-script] modul. Svaki novi redak **prije nego** li dodat će se `str()` funkcija. Da biste provjerili Moje rezultira Azure strojnog učenja Studio koristiti ove funkcije.

Kako dodati sljedeći redak u moj R kod u [Izvršavanje skripte R] [ execute-r-script] modul.

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

Pokrenite kod u vašem eksperiment i provjeriti rezultat iz zapisnika za izlaz. Ovi Rezultati prikazuju se u slici 11.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Slika 11. Uklanjaju se sažetak dataframe s dva stupca.*

Dobre vijesti! Ne možemo dobiti očekivane rezultate.

###<a name="add-a-new-column"></a>Dodavanje novog stupca

Da biste stvorili modelima niza vremena bit će praktično da stupac koji sadrži mjeseci od početka niza vremena. Ne možemo će stvoriti novi stupac 'Month.Count'.

Da biste lakše organizirali kod ćemo će stvoriti naš prvi jednostavne funkcija `num.month()`. Ne možemo će primijeniti ovu funkciju da biste stvorili novi stupac u na dataframe. Nova šifra je na sljedeći način.

    ## Create a new column with the month count
    ## Function to find the number of months from the first
    ## month of the time series
    num.month <- function(Year, Month) {
      ## Find the starting year
      min.year  <- min(Year)

      ## Compute the number of months from the start of the time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute the new column for the dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

Sada pokrenuti ažurirane pokusa i koristiti izlazni zapisnik za prikaz rezultata. Ovi Rezultati prikazuju se u 12 slici.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Slika 12. Sažetak dataframe s dodatni stupac.*

Čini se da funkcionira sve. Imamo novi stupac s vrijednostima očekivani u našem dataframe.

###<a name="value-transformations"></a>Vrijednost transformacije

U ovom odjeljku ne možemo će izvršiti neke jednostavne transformacije vrijednosti u stupce naše dataframe. Jezik R podržava transformacije gotovo proizvoljne vrijednost. Reference u [Dodatak B – daljnje čitanje](#appendixb) sadrže proširenom primjere.

Ako pogledate vrijednosti u sažetaka naše dataframe vidjet ćete nešto neparni ovdje. Je li više ice cream od mlijeka proizvodi u Kaliforniji? Ne, Naravno, kao što je to vam ne odgovara, sud kao u ovom fact možda nije nekim nam ice cream lovers. Jedinica se razlikuju. Cijena u jedinicama NAM je funtama, mlijeko u jedinicama milijun funtama SAD-a, ice cream je u jedinicama 1000 NAM galoni i cottage Sir je u jedinicama 1000 funtama SAD-a. Pod pretpostavkom da ice cream weighs oko 6.5 funtama po galon, jednostavno možemo množenja da biste pretvorili te vrijednosti tako da su sve u jednako jedinicama 1000 funtama.

Za naše predviđanja model koristimo multiplicative model za trend i sezonski prilagođavanje tih podataka. Zapisnik transformaciju omogućuje nam da biste koristili linearni model pojednostavljivanje ovaj postupak. Ne možemo možete primijeniti transformaciju zapisnika u istu funkciju koju je primijenjena množitelj.

U sljedeći kod mogu definirati novoj funkciji `log.transform()`, i primijenite ga na redaka koji sadrže numeričke vrijednosti. Nakon R `Map()` funkcija koristi da biste primijenili na `log.transform()` funkcija odabrane stupce u dataframe. `Map()`slično je `apply()` , ali omogućuje više popisa argumenata funkcije. Imajte na umu da popis množitelja dohvaćaju drugi argument u `log.transform()` (opis funkcije). Na `na.omit()` se funkcija koristi kao malo čišćenje da biste bili sigurni nismo vrijednosti koje nedostaju ili su definirana na dataframe.

    log.transform <- function(invec, multiplier = 1) {
      ## Function for the transformation, which is the log
      ## of the input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments to function log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check the input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap the multiplication in tryCatch
      ## If there is an exception, print the warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply the transformation function to the 4 columns
    ## of the dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

Postoji prilično na bitne događa u na `log.transform()` (opis funkcije). Većina kod provjerava potencijalne probleme s argumentima ili Postupanje s iznimke, možete i dalje biste tijekom na computations. Samo nekoliko redaka kod zapravo učinite na computations.

Cilj defensive programiranja je da biste spriječili pogreške jednu funkciju koja onemogućuje obrada nastavak. Neuspio oštrih analize dugoročnih može biti vrlo frustrirati korisnike. Da biste izbjegli u ovom slučaju, zadanih vrijednosti za povrat morate odabrati koji će ograničiti oštećenje do obrada. Poruka je Proizvodi i upozorenja korisnicima da nešto je nestalo pogrešne.

Ako se ne koriste defensive programiranje u R, taj kod mogu prestati malo izvršilo. Koje će vas voditi kroz važnih koraka:

1. Definira se vektor četiri poruka. Te poruke se koriste za komunikaciju informacije o nekim od mogućih pogrešaka i iznimke koje se može pojaviti prilikom kod.

2. Li moguće vratiti vrijednost NA svakom slučaju. Postoje brojne mogućnosti koje možda imaju manje strani efekata. Li nije moguće vratiti vektor od nule ili izvorni unos vektorski, primjerice.

3. Provjerava se izvoditi na argumenata funkcije. U svakom slučaju, ako dođe do pogreške, vraća se zadana vrijednost i poruke koje je stvorio u `warning()` (opis funkcije). Koristim `warning()` umjesto `stop()` kao drugu mogućnost će prekinuti izvođenja, točno što pokušavam da biste izbjegli. Imajte na umu da se ste napisali kod u sljedećem mjestu stil, kao u ovom slučaju funkcionalni pristup izgledao složene i otkrivanja.

4. Zapisnik computations su umotan u `tryCatch()` tako da iznimke će uzrokovati oštrih zastoj na obradu. Bez `tryCatch()` Većina pogreške upućuju rezultat funkcije R Zaustavi signal koji upravo to ne.

Izvršavanje R kod u vašem eksperiment, a imate pogled na ispisanom dokumentu je pogrešna u datoteci output.log. Sada će vidjeti transformiranih vrijednosti četiri stupca u zapisniku, kao što je prikazano na slici 13.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Slika 13. Sažetak podataka koji su vrijednosti u dataframe.*

Vidimo pretvoreni vrijednosti. Sada radnog mlijeko znatno premašuje sve druge Mliječni proizvodi proizvoda proizvodne, opozovite pri Logaritamsko mjerilo se sada tražite.

Sada očistiti podatke, a ne možemo spremni neke Modeliranje. Pogled na vizualizaciju sažetka za izlazni rezultat Dataset naš [Izvršavanje skripte R] [ execute-r-script] modula, vidjet ćete stupac "Mjesec" je "Categorical" s 12 jedinstvene vrijednosti, ponovno, baš kao smo željeli.

##<a id="timeseries"></a>Objekti niza vremena i analizu korelacije

U ovom odjeljku ne možemo će nekoliko osnovnih R vrijeme niz objekata proučavanje i analiza korelacija između neke varijabli. Naš cilj je izlaz dataframe para korelacije podacima na nekoliko lags.

Dovršavanje R kod za ovaj odjeljak je u zip datoteke prethodno preuzeti.

###<a name="time-series-objects-in-r"></a>Vrijeme niz objekata u R

Kao što je već su spomenuti, vrijeme nizovi skup vrijednosti podataka indeksirati prema vremenu. R vrijeme niz objekti se koriste za stvaranje i upravljanje indeksa vrijeme. Nekoliko je prednosti korištenja objekata niza vremena. Objekti niza vremena besplatne iz više pojedinosti o upravljanju vremenske vrijednosti niza indeks koji su encapsulated u objektu. Uz to, vrijeme niz objekti dopustiti da koristite brojne načine niza vremena za iscrtavanje, ispis Modeliranje i itd..

Klasa niza vremena POSIXct obično se koristi i relativno jednostavno. U ovom vremenski niz klase mjere vrijeme od početka epoch, siječanj 1, 1970. Koristit ćemo POSIXct vrijeme niz objekata u ovom primjeru. Ostale široku R vrijeme niz objekt klase obuhvaćaju zoo i xts, a zatim extensible vremenski niz.
<!-- Additional information on R time series objects is provided in the references in Section 5.7. [commenting because this section doesn't exist, even in the original] -->

### <a name="time-series-object-example"></a>Primjer za objekt niza vremena

Počnimo s radom u našem primjeru. Povucite i ispustite **Novi** [Izvršavanje skripte R] [ execute-r-script] modul u vašem eksperiment. Povezivanje izlazni priključak rezultat Dataset1 postojeće [Izvršavanje skripte R] [ execute-r-script] priključak za novu [Izvršavanje R skripte] za unos modul u Dataset1[ execute-r-script] modul.

Kao prvi primjer kao što smo proći kroz primjeru, na neki upućuje li prikazat će samo rastuće dodatni reci R kod prilikom svakog koraka.  

####    <a name="reading-the-dataframe"></a>Na dataframe za čitanje

Kao prvi korak, recimo čitati u na dataframe i provjerite je li ćemo dobiti očekivane rezultate. Sljedeći kod biste trebali učiniti posao.

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check the results

Pokrenite pokusa. Zapisnik novi oblik izvršavanje skripte R trebao izgledati kao slika 14.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*Slika 14. Sažetak dataframe u modulu za izvršavanje R skripte.*

U ovom podaci očekivane vrste i oblika. Primijetite da je vrsta faktor stupac "Mjesec" i sadrži očekivani broj razina.

####<a name="creating-a-time-series-object"></a>Stvaranje objekta niza vremena

Moramo dodati objekt niza vremena za naše dataframe. Zamijenite koda trenutnog sljedeće dodaje novi stupac klase POSIXct.

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check the results

Sada u zapisniku. To će izgledati kao slika 15.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Slika 15. Sažetak dataframe s objektom niza vremena.*

Ne možemo možete vidjeti sažetak koji je zapravo klase POSIXct novi stupac.

###<a name="exploring-and-transforming-the-data"></a>Proučavanje i pretvorba podataka

Pogledajmo neke varijabli u ovaj skup podataka. Matrica scatterplot izvrstan je način da bi proizveo Kratak pregled. Mogu se zamjene u `str()` funkcija prethodne kod R s sljedeći redak.

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

Pokreni kod, a zatim u odjeljku što se događa. Crtanja Proizvodi na uređaju R priključak trebao izgledati kao slika 16.

![Matrica Scatterplot odabrane varijabli][17]

*Slika 16. Matrica Scatterplot odabrane varijabli.*

Je odnose između tih varijabli neke odd-looking strukturu. Možda to nastaje iz trendova u podacima i činjenica da ćemo imati ne standardizirani priručniku varijabli.

###<a name="correlation-analysis"></a>Analiza korelacije

Za analizu korelacije moramo deaktivirali trend i standardizirati varijabli. Ne možemo jednostavno može koristiti u R `scale()` funkciju, koja pogoni i mijenja veličinu varijabli. Ova funkcija može dobro brže izvoditi. Međutim, je li da bi se prikazala primjera defensive programing u R.

Na `ts.detrend()` funkcija prikazano u nastavku izvodi obje te operacije. Sljedeća dva retka koda deaktivirali trend podatke i standardizirati vrijednosti.

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function to de-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                    'ERROR: ts.detrend requires argument ts to be of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
    )
      # Create a vector of zeros to return as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # The input arguments are not of the same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If the ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If the ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that the Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend the time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply the detrend.ts function to the variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot the results to look at the relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

Postoji prilično na bitne događa u na `ts.detrend()` (opis funkcije). Većina kod provjerava potencijalne probleme s argumentima ili Postupanje s iznimke, možete i dalje biste tijekom na computations. Samo nekoliko redaka kod zapravo učinite na computations.

Ne možemo već ste spominju primjera defensive programiranje u [vrijednost transformacije](#valuetransformations). Oba blokovi Izračuni su umotan u `tryCatch()`. Za neke pogreške smisla da biste se vratili izvorne unos vektorski i u drugim slučajevima li moguće vratiti vektor od nule.  

Imajte na umu da linearna regresija koji se koriste za deaktivirali popularne regresiji niza vremena. Varijabla predictor je objekt niza vremena.  

Jednom `ts.detrend()` definiran ćemo ga primijeniti i na varijable važna mjesta u našem dataframe. Ćemo morate prinude nastalom popisu stvorio `lapply()` za dataframe podataka pomoću `as.data.frame()`. Zbog defensive aspekte `ts.detrend()`, postupak nešto varijabli nije uspjelo ne sprječava ispravna obrada na druge.  

Konačni redak koda stvara para scatterplot. Nakon izvođenja kod R, rezultati na scatterplot prikazuju se u 17 slici.

![Para scatterplot deaktivirali trend i standardizirani vremenski niz][18]

*Slika 17. Para scatterplot niza deaktivirali trend i standardizirani vrijeme.*

Možete usporediti te rezultate onima prikazanim na slici 16. Trend ukloniti i varijable standardizirani priručniku, dobivamo mnogo manje strukture u odnosu između tih varijabli.

Kod za izračunavanje korelacija kao objekti R ccf je na sljedeći način.

    ## A function to compute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of the pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute the list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

Pokretanje kod daje zapisnika prikazan na slici 18.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*18 slici. Popis ccf objekata iz analizu para korelacije.*

Postoji korelacije vrijednost za svaku kašnjenja. Ništa od te vrijednosti korelacije nije dovoljna biti vrlo. Ne možemo možete stoga zaključite da ne možemo možete modela svakoj varijabli neovisno.

###<a name="output-a-dataframe"></a>Izlaz na dataframe

Ne možemo su izračunati para korelacija kao popis R ccf objekte. To predstavlja malo problema izlazni priključak rezultat Dataset zaista potrebno je dataframe. Nadalje, ccf objekt sam popis i želimo samo vrijednosti u prvom elementu ovaj popis korelacija na različitim lags.

Sljedeći kod izdvaja kašnjenja vrijednosti s popisa ccf objekata koje su sami popise.

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with the row names column and the
    ## correlation data frame and assign the column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## The following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

U prvom retku kod je malo evidencija, a neke objašnjenje mogu pomoći uvid u to. Rad s unutrašnji izvan imamo sljedeće:

1.  Na '**[[**' operatora s argumentom "**1**" za prvi element popisu objekta ccf odabire vektorski korelacija pri na lags.

2.  Na `do.call()` funkcija odnosi se na `rbind()` funkcija iznad elemenata na popisu prikazuje po `lapply()`.

3.  Na `data.frame()` funkcija Prinudno određuje rezultata koje je stvorio `do.call()` da biste na dataframe.

Imajte na umu da su retka imena u stupcu s dataframe. Način tako čuva retku imena kada su izlaza iz [Izvršavanje skripte R][execute-r-script].

Pokreće kod daje izlaz prikazan na slici 19 kada se **Vizualiziraj** Izlaz na priključak skupa rezultata. Redak imena su u prvom stupcu, kako je predviđeno.

![Rezultati Izlaz iz analizu korelacije][20]

*Slika 19. Izlaz iz analizu korelacije rezultata.*

##<a id="seasonalforecasting"></a>Primjer niza vremena: sezonski predviđanja

Oglednim podacima sada je u obrascu prikladna za analizu, a ne možemo zaključili postoje bez značajan korelacija između varijabli. Pogledajmo premjestili na i stvorite vremenski niz predviđanje modela. Pomoću ovog modela smo će predviđanja proizvodnje mlijeko Kaliforniji za 12 mjeseci 2013.

Naš predviđanja modela će imati dvije komponente, komponentu trend i sezonski komponentu. Dovršavanje predviđanje je proizvod tih dviju komponenata. Ove vrste modela zove multiplicative modela. Druga je additive modela. Već su primijenjene zapisnika transformaciju varijablama koja vas zanima što čini ovu analizu tractable.

Dovršavanje R kod za ovaj odjeljak je u zip datoteke prethodno preuzeti.

### <a name="creating-the-dataframe-for-analysis"></a>Stvaranje dataframe za analizu

Započnite dodavanjem **nove** [Izvršavanje skripte R] [ execute-r-script] modul vaše eksperiment. Povezivanje izlazni **Rezultat Dataset** postojeće [Izvršavanje skripte R] [ execute-r-script] modul unos **Dataset1** novi modul. Rezultat bi morao izgledati slično 20 slici.

![Eksperimentiranje s novi modul izvršavanje skripte R dodali][21]

*20 slici. Eksperimentiranje s novi modul izvršavanje skripte R dodali.*

Kao s analizu korelacije smo upravo dovršili moramo da biste dodali stupac s objektom niz POSIXct vremena. Sljedeći kod će samo to učiniti.

    # If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

Pokreni kod, a zatim pogledajte u zapisnik. Rezultat će izgledati kao slika 21.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Slika 21. Sažetak na dataframe.*

Uz taj rezultat ćemo spremni za početak naš analize.

###<a name="create-a-training-dataset"></a>Stvaranje skupa podataka za osposobljavanje

S dataframe konstruirana moramo stvaranje obuka skup podataka. Ti podaci neće sadržavati stavku sve opažanja osim posljednjeg 12 godini 2013, koji je naš test skup podataka. Sljedeće kod podskupova na dataframe i stvara iscrtavaju Mliječni proizvodi varijabli proizvodnje i cijene. Zatim stvaranje iscrtavaju četiri proizvodnje i cijene varijabli. Anonimni funkcija koristi se za definiranje neke augments za crtanje i ponavljanje na popisu argumenata druge dvije s `Map()`. Ako su mislim na koji je za petlja bi ste radili precizno ovdje, koje ispravni. No Budući da R je funkcionalni jezika koje se prikazuje funkcionalni pristup.

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

Pokreće kod stvara niz vrijeme niz iscrtava iz izlaza R uređaja prikazano slika 22. Imajte na umu da os vremenske u jedinicama bolje prednost vrijeme niz iscrtati način datuma.

![Prvi iscrtavaju niza vremena Kaliforniji Mliječni proizvodi proizvodnje i cijena podataka](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Druge iscrtavaju niza vremena Kaliforniji Mliječni proizvodi proizvodnje i cijena podataka](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Treća iscrtavaju niza vremena Kaliforniji Mliječni proizvodi proizvodnje i cijena podataka](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Četvrti iscrtavaju niza vremena Kaliforniji Mliječni proizvodi proizvodnje i cijena podataka](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

*Slika 22. Crtanje niza vremena Kaliforniji Mliječni proizvodi proizvodnje i podatke o cijeni.*

### <a name="a-trend-model"></a>Trend modela

Imate stvorili objekt za niza vremena i pritom imali susret podatke, Započnimo za sastavljanje trend modela za podatke radnog mlijeko Kaliforniji. Ne možemo možete učiniti s regresiji niza vremena. Međutim, je Očisti iz crtanja koje ćemo će trebate više na slope i intercept da biste točno modela opaženih trenda obuka podataka.

Uz small skale podatke, li će sastavljanje model za trend u RStudio i zatim Izreži i zalijepite dobivene modela Azure strojnog učenja. RStudio pruža interaktivnu okruženje za tu vrstu interaktivne analize.

Kao prvi pokušaj pokušam će Polinomna regresije s potencije najviše 3. Postoji realni opasnost od previše približite ove vrste modela. Dakle, najbolje je da biste izbjegli visoke redoslijed uvjeta. Na `I()` funkcija inhibits tumačenja sadržaja (tumači sadržaj "kakav jest") i omogućuje vam da biste napisali doslovno interpreted funkcija u regresijska Jednadžba.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

Stvara sljedeće.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

Iz vrijednosti P (cijena (> | t |)) u ovom izlaza Vidimo da kvadrata termina možda neće biti vrlo. Koristit ću na `update()` funkcija da biste izmijenili ovaj model tako da ispustite kvadrata termina.

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

Stvara sljedeće.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

Izgleda bolje. Sve uvjete su značajan. Međutim, vrijednost 2e 16 zadana je vrijednost i trebali biste ne mogu prenijeti previše utjecati.  

Test sanity, recimo upućivati grafički prikaz niza vremena Mliječni proizvodi radnog podataka Kaliforniji krivulje trend prikazano. Ste dodali sljedeći kod u Azure strojno učenje [Izvršavanje skripte R] [ execute-r-script] model (ne RStudio) da biste stvorili modela i provjerite grafički prikaz. Rezultat je prikazan na slici 23.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kaliforniji mlijeko radnog podataka s modelom trend prikazano](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

*Slika 23. Podaci radnog Kaliforniji mlijeko s modelom trend prikazano.*

Čini se da modela trend prilično dobro pristaje podacima. Nadalje, postoji ne čini se da dokaz prekoračenja staviti kao što su odd wiggles krivulji modela.  

###<a name="seasonal-model"></a>Sezonski modela

S modelom trend u rukom, moramo automatske i uključiti sezonski efekata. Koristit ćemo mjesec u godini kao sustava varijabla u linearnom modelu da biste snimili efekt mjeseca u mjesec. Imajte na umu da kada predstavljanje faktor varijable u model, sjecište morate ne izračunati. Ako to ne učinite, formula previše naveden i R će neki od željenog čimbenika ispuštanje uz zadržavanje intercept termina.

Budući da imamo zadovoljavajuće trend model možete koristiti u `update()` funkcija za dodavanje novih termina u postojećem modelu. -1 u formuli ažuriranje izostavlja intercept termina. Nastavite RStudio na trenutak:

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

Stvara sljedeće.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,   Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

Ne možemo vidjeti model više ne sadrži programa intercept termina te sadrži 12 čimbenika značajan mjesec. To je upravo ono što smo htjeli da biste vidjeli.

Recimo da bi drugi crtanja niza vremena Mliječni proizvodi radnog podataka Kaliforniji da biste vidjeli kako dobro funkcionira sezonski modela. Ste dodali sljedeći kod u Azure strojno učenje [Izvršavanje skripte R] [ execute-r-script] da biste stvorili modela i provjerite grafički prikaz.

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

Pokretanje kod u Azure strojnog učenja daje crtanja prikazan na slici 24.

![Proizvodne mlijeko Kaliforniji s modelom uključujući sezonski efekata](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

*Slika 24. Kaliforniji mlijeko radni model uključujući sezonski efekte.*

Prilagodi podaci prikazani u 24 slika je radije encouraging. Trend i sezonski efekt (mjesečno varijacije) pogledajte pametnije.

Kao drugi potvrdite na našem modelu Pogledajmo susret s ostaci. Sljedeći kod formula izračunava predviđene vrijednosti iz naših dva modela, formula izračunava ostataka sezonski modela i iscrtava te ostataka za obuku podatke.

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot the residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

Razlike crtanja prikazan je slika 25.

![Ostataka sezonski modela za podatke za osposobljavanje](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

*Slika 25. Ostataka sezonski modela za obuku podatke.*

Pogledajte ove ostaci pametnije. Postoji nema određenu strukturu, osim efekt recession 2008 2009 koji našem modelu račun za osobito dobro.

Crtanja 25 slici prikazano je korisno za otkrivanje bilo koje vrijeme ovisne uzoraka u na ostaci. Eksplicitno pristup izračunavanje i Crtanje ostataka mogu koristiti smješta na ostataka redoslijedom vrijeme na na crtež. Ako se s druge strane, li imali iscrtati `milk.lm$residuals`, na crtež bi su redoslijedom vremena.

Možete koristiti `plot.lm()` čime se dobiva niz dijagnostičkih crtanje.

    ## Show the diagnostic plots for the model
    plot(milk.lm2, ask = FALSE)

Kod stvara niz dijagnostičkih iscrtavaju prikazan na slici 26.

![Prvi dijagnostičkih iscrtavaju sezonski modela](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Druge dijagnostičkih iscrtavaju sezonski modela](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Treća dijagnostičkih iscrtavaju sezonski modela](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Četvrti dijagnostičkih iscrtavaju sezonski modela](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

*Slika 26. Dijagnostika iscrtava sezonski modela.*

Postoji nekoliko iznimno utjecajno točaka prepoznati te se iscrtavaju, ali ništa uzrokuju važno. Nadalje, možemo vidjeti iz crtanja Q Q normalno jesu li u ostataka zatvori normalno raspodijeljen važne pretpostavci za linearne modele.

###<a name="forecasting-and-model-evaluation"></a>Procjena predviđanja i modela

Postoji samo jednu dodatne stvari koje treba učiniti da biste dovršili našeg primjera. Moramo izračunati predviđanja i mjere pogrešku protiv stvarnih podataka. Naš predviđanje će biti 12 mjeseci 2013. Ne možemo možete izračunati mjeru pogreške za ovaj predviđanja u stvarnih podataka koja nije dio naš tečaj skup podataka. Osim toga, ne možemo možete usporediti performanse 18 godina obuka podatke u roku od 12 mjeseci testiranje podataka.  

Broj metriku koriste se za mjerenje performansi modela niza vremena. U slučaju da naš koristit ćemo pogreške kvadratni korijen sredina (RMS). Funkciju sljedeća formula izračunava pogreške RMS između dva niza.  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function to compute the RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                    "ERROR: Input vector to function RMS.error is too short",
                    "ERROR: Input vectors to function RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check the arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
        warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate the values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute the RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

Kao i u `log.transform()` funkcija ćemo se spominju u odjeljku "Vrijednost transformacije" je prilično mnogo Provjera i iznimke oporavak Šifra pogreške u ovu funkciju. Načela zapošljavanju su uvijek jednake. Rad obavlja na dva mjesta umotan u `tryCatch()`. Najprije vremenski niz su exponentiated, jer smo rad sa zapisnicima vrijednosti. Drugo, izračunati se stvarni RMS pogreške.  

Opremljeno funkcije za mjerenje RMS pogreške, recimo Sastavljanje i izlaz dataframe koji sadrže pogreške RMS-a. Ne možemo neće sadržavati stavku uvjete za samostalno model trend i dovršavanje model s sezonski čimbenike. Sljedeći kod ne posla pomoću dva linearne modele ćemo imati konstruirana.

    ## Compute the RMS error in a dataframe
    ## Include the row names in the first column so they will
    ## appear in the output of the Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

Izvodi kod dati izlaz prikazan na slici 27 pri izlazni priključak skupa rezultata.

![Usporedba RMS pogrešaka u modelima][26]

*Slika 27. Usporedba RMS pogrešaka u modelima.*

U rezultatima te možemo vidjeti da dodavanje sezonski čimbenika u model smanjuje pogreške RMS znatno. Ne previše surprisingly pogrešku RMS za obuku podatke moguće je na bit manji od za predviđanje.

##<a id="appendixa"></a>DODATAK A: Vodič kroz RStudio

RStudio prilično dobro navedenih tako da u ovom dodatak mogu pronaći ćete neke od veza na ključa odjeljci RStudio dokumentaciju za početak.

1.  Stvaranje projekata

    Možete organizirati i upravljanje R kod u projekte pomoću RStudio. Dokumentacija koja se koristi projekata možete pronaći na https://support.rstudio.com/hc/articles/200526207-Using-Projects.

    Preporučujemo I da slijedite ove upute i stvaranje projekta primjere koda R u ovom dokumentu.  

2.  Uređivanje i izvršavanja kod R

    RStudio nudi okruženje za uređivanje i izvršavanja R kod. Dokumentacija možete pronaći na https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.

3.  Ispravljanje pogrešaka

    RStudio obuhvaća Napredne mogućnosti za ispravljanje pogrešaka. Dokumentacija za te značajke nalazi se na https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.

    Značajke otklanjanja poteškoća točku prekida se navedenih pri https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.

##<a id="appendixb"></a>B: DODATAK dodatno čitanja

Ovaj vodič programiranje R pokriva osnove što vam je potrebno za korištenje jezika R s Azure strojnog učenja Studio. Ako niste upoznati s R, dostupne na CRAN su dvije Uvod:

- R za početnike po Emmanuel Paradis dobro je mjesto da biste krenuli od http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.  

- Uvod u R po W. N. Venables postavljanje. AL. ulazi u veću dubinu, pri http://cran.r-project.org/doc/manuals/R-intro.html.

Postoji mnogo knjige R koji vam mogu pomoći da započnete. Evo nekoliko pronaći korisne:

- Crteža R programiranje: vodič od statističkih softver dizajna tako da Norman Matloff je odličan Uvod u programiranje u R.  

- Kuharica R po Teetor paula nudi problemima i rješenjima pristup pomoću R.  

- R u akciji po Robert Kabacoff je drugi korisne uvodnom adresara. Na web-mjesto za brzi R pomoćnom koristan je resurs pri http://www.statmethods.net/.

- R Inferno po opekline Patrika je surprisingly duhovit knjiga koja se bavi broj Evidencija i teško teme koje možete naići kada programiranje u R. Knjige dostupna je besplatno na http://www.burns-stat.com/documents/books/the-r-inferno/.

- Ako želite precizno dive u dodatne teme u R, imati susret adresara dodatno R po Hadley Wickham. Internetske verzije Ova knjiga dostupna je besplatno na http://adv-r.had.co.nz/.

Catalogue niz paketa R vrijeme moguće je pronaći u prikaz zadatka CRAN za analizu niza vremena: http://cran.r-project.org/web/views/TimeSeries.html. Informacije o određeno vrijeme niz objekt paketa, pogledajte dokumentaciju za taj paket.

Knjiga uvodni vremenski niz R paula Cowpertwait i promjena Metcalfe pruža Uvod u korištenje R za analizu niza vremena. Mnogo više theoretical tekstova sadrže primjere R.

Neke sjajne resurse za internet:

- DataCamp: DataCamp ručica R u udobnosti preglednika s video predavanja i odbijanje vježbe. Postoje interaktivni vodiči za na najnovije R tehnike i paketa. Preuzimanje besplatne interaktivni vodič R pri https://www.datacamp.com/courses/introduction-to-r  

- Na brzi R vodič po Kelly crno iz http://www.cyclismo.org/tutorial/R/ Clarkson University

- 60 + R resursa navedeni na http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
