<properties
    pageTitle="Azure strojnog učenja najčešća pitanja vezana uz | Microsoft Azure"
    description="Azure strojnog učenja Uvod: najčešća pitanja vezana uz prekrivajući naplate, mogućnosti i ograničenja neki servis u oblaku pojednostavnjeno predvidljivu Modeliranje."
    keywords="strojnog učenja Uvod, predvidljivu Modeliranje, što je strojnog učenja"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="garye"/>

# <a name="azure-machine-learning-frequently-asked-questions-faq-billing-capabilities-limitations-and-support"></a>Azure strojnog učenja najčešća pitanja: podrška za naplatu, mogućnosti, ograničenja i

U ovim najčešćim Pitanjima navedeni odgovori na pitanja o Azure strojnog učenja, neki servis u oblaku za razvoj predvidljivu modela i operationalizing rješenja putem web-servisi. U ovim najčešćim Pitanjima pokriva pitanja o korištenju servisa, uključujući naplatu model, mogućnosti, ograničenja i podrška.

## <a name="general-questions"></a>Općenita pitanja

**Što je Azure strojnog učenja?**

Azure strojnog učenja je potpuno upravljanih usluga koje možete koristiti za stvaranje, testiranje, raditi i upravljanje predvidljivu analitički rješenja u oblaku. Samo u pregledniku možete prijaviti, prijenos podataka i odmah započeti eksperimenata strojnog učenja. Povlačenje i ispuštanje predvidljivu Modeliranje, velike paleta module i biblioteci početni predložaka čini uobičajene strojnog učenja zadatke jednostavno i brzo.  Dodatne informacije potražite u članku [Pregled servisa Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/). Na koji prekriva ključa terminologija i koncepata učenje Uvod "stroj", potražite u članku [Uvod u Azure strojnog učenja](machine-learning-what-is-machine-learning.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Što je strojnog učenja Studio?**

Strojnog učenja Studio je okruženju workbench pristup putem web-preglednika. Strojnog učenja Studio hostira paleta module sa sučeljem vizualne sastavljanje koji vam omogućuje sastavljanje završetka do kraja, tijek rada znanstvenog podataka u obliku pokusa.

Dodatne informacije o strojnog učenja Studio potražite u članku [što je strojnog učenja Studio?](machine-learning-what-is-ml-studio.md)

**Što je servis strojnog učenja API-JA?**

Servis strojnog učenja API omogućuje implementacija predvidljivu modele, kao što su oni ugrađena strojnog učenja Studio, kao prilagodljivi, pogreške, web-servisi. Web-servisi stvorio servis strojnog učenja API su REST API-ji koji omogućuju sučelje za komunikaciju između vanjskim aplikacijama i vaše modelima predvidljivu analize.

Dodatne informacije potražite u članku [Povezivanje s web-servisa strojnog učenja](machine-learning-connect-to-azure-machine-learning-web-service.md).

**Gdje se navedeni servisima klasične web? Gdje su navedeni Moj novi Voditelj resursa za Azure temelji web-usluge?**

Klasični Web services i novi Upravitelj Azure resursa na temelju Web services navedene su na portalu [Microsoft Azure strojnog učenja Web Services](https://services.azureml.net/) . 

Klasični web-servisi su navedene u [Strojnog učenja Studio](http://studio.azureml.net) na kartici Web services.

## <a name="microsoft-azure-machine-learning-web-service-questions"></a>Web-servisa za Microsoft Azure strojnog učenja pitanja

**Što su Azure strojnog učenja Web Services?**

Strojnog učenja web-servisi omogućuju sučelja između aplikacija i strojnog učenja bilježenja rezultata model za tijek rada. Pomoću web-servisa Azure strojnog učenja vanjsku aplikaciju možete komunicirati s strojnog učenja model bilježenja rezultata tijeka rada u stvarnom vremenu. Strojnog učenja poziva web-servisa vraća rezultate za predviđanje u vanjsku aplikaciju. Da biste web-servisa strojnog učenja poziv, proslijedite ključa API stvoren prilikom implementiran web-servisa. Web-servis strojnog učenja temelji se na OSTALE, odabir popularnih arhitektura za web-mjesto programiranje projekata.

Azure strojnog učenja sadrži dvije vrste usluge:

* Servis za odgovor na zahtjev (RRS) – niske latencije, Visoko skalabilni servis koji nudi sučelje za bez praćenja stanja modela stvoriti i implementirati iz učenje Studio računala.
* Grupe za izvršavanje usluge (BES) – programa asinkronog servisa te brojčane pokazatelje na obradu podataka zapisa.

Korištenje REST API-JA i pristup web-servisa na nekoliko načina. Ako, na primjer, možete napisati aplikacije u C#, R ili Python pomoću koda za uzorka za koje generira kada implementiran web-servisa.

Ogledni kod dostupna je na: U zauzeti stranice za web-servisa na portalu servisa Azure strojnog učenja Web Services.
API stranici pomoć za servis nadzornoj ploči Web u Studio strojnog učenja.

Ili možete koristiti oglednoj radnoj knjizi programu Microsoft Excel stvara (dostupna nadzornoj ploči web servisa u Studio).

**Što su glavni ažurirat novog web-servisi za Azure ML?**

Dodatne informacije o novog web-servisi za Azure strojnog učenja pogledajte [povezane dokumentacije](machine-learning-whats-new.md).

## <a name="machine-learning-studio-questions"></a>Strojnog učenja Studio pitanja

### <a name="importing-and-exporting-data-for-machine-learning"></a>Uvoz i izvoz podataka za učenje za računala

**Što izvora podataka podržava strojnog učenja?**

Podaci mogu biti učitani u eksperiment strojnog učenja Studio na jedan od tri načina: tako da prenesete lokalne datoteke kao skup podataka pomoću modula za uvoz podataka iz oblaka podatkovne usluge ili uvozom skup podataka spremili od druge Poigrajte. Dodatne informacije o oblicima datoteka podržanih potražite u članku [Uvoz podataka obuka u Studio strojnog učenja](machine-learning-data-science-import-data.md).


#### <a id="ModuleLimit"></a>Kako velike skup podataka može za moj module?

Moduli u strojnog učenja Studio podržava skupove podataka do 10 GB dense numeričke podatke za slučaj da se zajednički koristi. Ako modula traje više od jedne unos, 10 GB je zbroj svih unos veličina. Možete uzorak i veće skupove podataka putem grozd ili baze podataka SQL Azure upita ili učenjem po broji predobradu, prije ingestion.  

Sljedeće vrste podataka možete proširiti u veći skupovima podataka tijekom normalizacija značajke i ograničeni su na manje od 10 GB:

- Kratke
- Categorical
- Nizovi
- Binarni podaci

Sljedeće module ograničeni su na skupove podataka manje od 10GB:

- Moduli recommender
- Modul za SMOTE
- Skriptiranje modula: R, Python, SQL
- Moduli pri čemu može biti veći od veličine ulaznih podataka, kao što su spoj ili značajku raspršivanje izlazna veličina podataka.
- Unakrsno provjere valjanosti, ugađanje Hyperparameters Model, regresije redni broj i jedan Dodavanje veze za vanjskih sve Multiclass, kada je broj iteracija je vrlo velike.

Za skupove podataka veća od nekoliko GB, trebali biste prenijeti podatke Azure prostora za pohranu ili baze podataka SQL Azure ili korištenje HDInsight, umjesto izravno prijenos iz lokalne datoteke.


####<a id="UploadLimit"></a>Koja su ograničenja za prijenos podataka?
Za skupova podataka veća od nekoliko GB prenijeti podatke Azure prostora za pohranu ili baze podataka SQL Azure ili korištenje HDInsight, umjesto izravno prijenos iz lokalne datoteke.

**Možete pročitati podatke iz Amazon S3?**

Ako imate mali količinu podataka i želite da biste otkrili putem http URL, a zatim [Uvoz podataka] možete koristiti[ import-data] modul. Za sve veće količine podataka da biste ih najprije prenesite Azure prostora za pohranu, a zatim pomoću [Uvoz podataka] [ import-data] modul možete prenijeti na vašem eksperiment.
<!--
<SEE CLOUD DS PROCESS>
-->

**Postoji mogućnost za unos ugrađenu sliku?**

Informirajte se o slika unos mogućnost [Uvezi slike] [ image-reader] referenca.

### <a name="modules"></a>Moduli

**Algoritam, izvora podataka, oblik podataka ili operacija transformacije podataka koju tražim nije u Azure strojnog učenja Studio. Što mogu učiniti?**

Možete posjetiti [forum za povratne informacije korisnika](http://go.microsoft.com/fwlink/?LinkId=404231) da biste vidjeli zahtjeve za značajke koje ćemo pratite. Dodajte glas zahtjeva ako je već zatražio mogućnost koju tražite. Ako ne postoji mogućnost koju tražite, stvorite novi zahtjev. Možete pogledati status zahtjev na ovom previše. Ne možemo usko pratite ovaj popis i često ažurirati stanje dostupnosti značajki. Uz to, s ugrađenu podršku za R i Python prilagođene transformacije mogu se kreirati prema potrebi.


**I u možete unijeti na moje kodom postojeće strojnog učenja Studio?**

Da, možete postojeći kod R ili Python dohvaćali strojnog učenja Studio, pokretanje u istoj eksperimentiranje s learners Azure strojnog učenja i implementacija rješenja kao web-servisa putem Azure strojnog učenja. Dodatne informacije potražite u članku [Proširi vaše eksperimentiranje s R](machine-learning-extend-your-experiment-with-r.md) i [izvršavanje Python strojnog učenja skripte u Azure strojnog učenja Studio](machine-learning-execute-python-scripts.md).

**Je li moguće koristiti nešto poput [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) da biste definirali model?**

Ne, koji nije podržan, no prilagođeni kod R i Python može se koristiti za definiranje modul.

**Koliko module mogu se izvršiti paralelno u moje eksperiment?**  

Možete izvršiti do četiri moduli paralelno pokusa.


### <a name="data-processing"></a>Obrada podataka

**Je li omogućuje vizualni prikaz podataka (osim vizualizacija R) interaktivno unutar pokusa?**

Klikom na Izlaz iz modula vizualni prikaz podataka i dobivanje statističkih podataka.

**Prilikom pregleda rezultata ili podatke u pregledniku, broj redaka i stupaca ograničen, zašto?**

Budući da se podaci se šalju u preglednik i možda velik, ograničeno da biste spriječili usporava strojnog učenja Studio je veličina podataka. Da biste vizualizacija sve podatke i rezultat je bolje podatke za preuzimanje i korištenje programa Excel ili neki drugi alat.

### <a name="algorithms"></a>Algoritama

**Koje postojeće algoritama podržava strojnog učenja Studio?**

Strojnog učenja Studio nudi algoritama stanje od-na-crteža, kao što su skalabilni Boosted odlukama, sustavi Bayesian preporuke precizno Neural mrežama i Jungles odluka razvio na Microsoft Research. Paketi za učenje skalabilni strojno Otvori izvor kao što je Vowpal Wabbit su uključeni. Strojnog učenja Studio podržava strojnog učenja algoritama za klasifikaciju multiclass i binarni, regresije i Klasteriranje. Pogledajte popis svih [Strojnog učenja module][machine-learning-modules].

**Nemoj automatski prijedlog desno strojnog učenja algoritam za Moji podaci?**

Ne, no postoje razni načini u strojnog učenja Studio za usporedbu rezultata svaki algoritam da biste odredili odgovarajuću domenu za vaš problem.

**Imate li sve smjernice za izdvajanje jedan algoritam preko drugog za dani algoritmima pomoću?**
Saznajte [kako odabrati algoritam](machine-learning-algorithm-choice.md).

**Su navedeni algoritama pisane R ili Python?**

Ne, kompilirane jezike za veće performanse uglavnom zapisati njima.

**Jesu li sve pojedinosti algoritama dobili?**

U dokumentaciji omogućuje neke informacije o algoritmima pomoću, a da biste optimizirali algoritam za upotrebu opisane su navedeni za ugađanje parametri.  

**Je li sve podrška za učenje online?**

Ne, trenutno samo programski retraining podržana.

**Mogu vizualizirati Slojevi Neural neto modela pomoću ugrađenih modul?**

ne.

**Je li moguće stvoriti vlastiti module u C# ili neki drugi jezik?**

Trenutno nove prilagođene moduli se mogu stvoriti samo u R.

### <a name="r-module"></a>Modul R

**Koje paketa R su dostupne u strojnog učenja Studio?**

Strojnog učenja Studio podržava 400 + CRAN R paketa danas, a slijedi [trenutni popis](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) isporučuje se sve pakete. Osim toga, potražite u članku [Proširi vaše eksperimentiranje s R](machine-learning-extend-your-experiment-with-r.md) da biste saznali kako dohvatiti taj popis. Ako je paket koji želite nije na ovom popisu, navedite naziv paketa na [forum za povratne informacije korisnika](http://go.microsoft.com/fwlink/?LinkId=404231).

**Je li moguće sastaviti prilagođeni modul R?**

Da, potražite u članku [Autor prilagođene R module u Azure strojnog učenja](machine-learning-custom-r-modules.md) dodatne informacije.

**Je li okruženje ZAMIJENI za R?**

Ne postoji ne ZAMIJENI okruženja za R u na studio.

### <a name="python-module"></a>Modul Python

**Je li moguće sastaviti prilagođeni modul Python?**

Trenutno ne, ali možete koristiti jednu ili više [Izvršavanje skripte Python] [ python] moduli da biste dobili isti rezultat.

**Je li okruženje ZAMIJENI za Python?**

Jupyter bilježnice možete koristiti u Studio strojnog učenja. Dodatne informacije potražite u članku [Uvod Jupyter bilježnica u Azure strojnog učenja Studio] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Web-servisa

###<a name="retraining-models-programmatically"></a>Programski retraining modela

**Kako Retrain učenje Azure strojno modelima programski?**

Koristite Retraining API-ji. Dodatne informacije potražite u članku [Retrain strojnog učenja modelima programski](machine-learning-retrain-models-programmatically.md). Ogledni kod dostupan je i u [Programu Microsoft Azure strojnog učenja Retraining pokazni videozapis](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Stvaranje

**Možete implementirati model lokalno i u aplikaciji bez veze s Internetom?**

ne.


**Je li osnovnu latenciju se očekuje za sve web-usluge?**

U odjeljku [ograničava Azure pretplate](../azure-subscription-service-limits.md)

### <a name="use"></a>Korištenje

**Kada želite li da se pokreće Moje predvidljivu modela kao servis za obradu izvođenja nasuprot odgovor zahtjev za uslugu?**

Zahtjev za odgovor servis (RRS) je najniža Latencija, Visoko skaliranje web-servisa koji se koristi za pružanje sučelje za bez praćenja stanja modela koje su stvorene i implementirati iz okruženja za početak. Servis za obradu izvođenja (BES) je servis za asinkrono bilježenje rezultata skupine zapisa podataka. Unos za BES je slična unosa podataka u RRS. Glavna razlika je BES čita blok zapise iz raznih izvora, primjerice s Blob i servis tablice u baze podataka SQL Azure, Azure HDInsight (grozd upita) i HTTP izvora. Dodatne informacije potražite [u](machine-learning-consume-web-services.md)članku korištenje web-servise strojnog učenja.

**Kako ažurirati modela za distribuiranih web-usluge?**

Ažuriranje predvidljivu modela servisa za već distribuiranih jednostavan je izmjenu i rerunning pokusa koju ste koristili za stvaranje i spremanje obučeni modela. Nakon što dodate nove verzije obučeni modela dostupna, strojnog učenja Studio vas pita želite li ažurirati web-servisa. Detalje o ažuriranju distribuiranih web-servisa potražite u članku [uvođenja strojnog učenja web-servisa](machine-learning-publish-a-machine-learning-web-service.md).

Možete koristiti i API-ji Retraining.
Dodatne informacije potražite u članku [Retrain strojnog učenja modelima programski](machine-learning-retrain-models-programmatically.md). Ogledni kod dostupan je i u [Programu Microsoft Azure strojnog učenja Retraining pokazni videozapis](https://azuremlretrain.codeplex.com/).

**Kako se praćenje Moje web-servisa u uveden u radnog?**

Kada predvidljivu model implementiran, možete je nadzirati od Azure klasičnog portal (samo klasični web services) ili na portalu servisa Azure strojnog učenja Web Services. Svaki distribuiranih servis ima vlastitu nadzornu ploču gdje možete vidjeti informacije za servis za nadzor. Dodatne informacije o upravljanju servisa distribuiranih web potražite u članku [Upravljanje web-servisa pomoću portala za Azure strojnog učenja Web Services](machine-learning-manage-new-webservice.md) i [Upravljanje radnog prostora programa Azure strojnog učenja](machine-learning-manage-workspace.md).

**Je li na mjesto gdje se mogu vidjeti Izlaz iz moje RRS/BES?**

RRS, odgovor servisa web je obično kojem ćete vidjeti rezultat. Možete je i pisati u spremište blobova platforme Azure. Za BES, rezultat je pisati blob prema zadanim postavkama. Možete pisati izlaz baze podataka ili tablice pomoću [Izvoz podataka] [ export-data] modul.

**Je li moguće stvoriti web-servisa samo iz modela koje su stvorene u strojnog učenja Studio?**

Ne, web services možete stvoriti izravno iz bilježnice Jupyter i RStudio.

**Gdje pronaći informacije o kodovima pogrešaka?**

Popis kodovi pogrešaka i opise potražite u članku [Kodovi pogrešaka za strojnog učenja modul](https://msdn.microsoft.com/library/azure/dn905910.aspx) .

## <a name="scalability"></a>Skalabilnost

**Što je skalabilnost web-usluge?**

Trenutno je zadana krajnja točka tamo uz 20 istovremeni zahtjevi RRS po krajnjoj točki. To promjena veličine na 200 istovremeni zahtjevi po krajnjoj točki i mogu mijenjati veličinu svakog web-servisa 10 000 krajnje točke po web-servisa kao što je opisano u [Skaliranje web-servisa](machine-learning-scaling-webservice.md). Za BES, svaki krajnjoj točki omogućuje obrade zahtjeva za 40 istovremeno i dodatni zahtjevi izvan 40 zahtjeve su u redu čekanja. Ove zahtjeva u redu čekanja se automatski pokrenuti kao drains red.


**R poslove širi preko čvorove?**

ne.  


**Koliko je podataka možete koristiti za obuku?**

Moduli u strojnog učenja Studio podržava skupove podataka do 10 GB dense numeričke podatke za slučaj da se zajednički koristi. Ako modula traje više od jedne unos, ukupnu veličinu za sve unose zajedno je 10 GB. Možete uzorak i veće skupove podataka putem grozd ili baze podataka SQL Azure upita ili tako da prije obrada saznaju [Brojanje] [ counts] moduli prije ingestion.  

Sljedeće vrste podataka možete proširiti u veći skupovima podataka tijekom normalizacija značajke i ograničeni su na manje od 10 GB:

- kratke
- categorical
- nizove
- Binarni podaci

Sljedeće module ograničeni su na skupove podataka manje od 10 GB:

- Moduli recommender
- Modul za SMOTE
- Skriptiranje modula: R, Python, SQL
- Moduli pri čemu može biti veći od veličine ulaznih podataka, kao što su spoj ili značajku raspršivanje izlazna veličina podataka.
- Provjerite valjanost unakrsno, ugađanje Hyperparameters Model, regresije redni broj i jedan Dodavanje veze za vanjskih sve Multiclass, kada je broj iteracija vrlo velike.

Za skupove podataka većih od nekoliko GB, trebali biste prenijeti podatke Azure prostora za pohranu ili baze podataka SQL Azure, ili pomoću HDInsight, umjesto izravno prijenos iz lokalne datoteke.


**Postoje neki vektorski ograničenja veličine?**

Reci i stupci svakog ograničeni su na ograničenje .NET Max Int: 2.147.483.647.

**Može li se veličina virtualnog računala koja se koristi za pokretanje web-servisa prilagoditi?**

ne.  

## <a name="security-and-availability"></a>Sigurnost i dostupnosti

**Tko ima pristup http krajnja točka za web-servis po zadanom? Kako ograničiti pristup krajnjoj?**

Kada je implementiran u web-uslugu, zadana krajnja točka se stvara za servis. Zadana krajnja točka možete pozvati pomoću ključ API-JA. Dodatni krajnje točke mogu se dodati vlastite ključeve s portala za Azure klasični programski pomoću upravljanja API-ji servisa Web. Tipke za pristup su vam potrebne za upućivanje poziva na web-servisa. Dodatne informacije potražite u članku [Povezivanje s web-servisa strojnog učenja](machine-learning-connect-to-azure-machine-learning-web-service.md).


**Što se događa ako nije moguće pronaći moj račun za Azure prostora za pohranu?**

Strojnog učenja Studio ovisi o račun za Azure prostora za pohranu korisnik da biste spremili privremene podataka prilikom izvršavanja tijeka rada. Taj prostor za pohranu je račun pod uvjetom da biste strojnog učenja Studio u trenutku je stvoriti radni prostor. Kada je radni prostor se stvara Ako račun za pohranu se briše i više nije moguće pronaći, radni prostor prestati raditi i sve eksperimenata tog radnog prostora neće uspjeti.

Ako ste slučajno izbrisali račun za pohranu, ponovno stvorite račun za pohranu s istim nazivom na istom području kao račun izbrisane prostora za pohranu. Nakon toga ponovnog sinkroniziranja tipkovni prečac.


**Što se događa ako nije sinkroniziran ključ za pristup računu za pohranu?**

Strojnog učenja Studio ovisi o račun za Azure prostora za pohranu korisnik radi pohrane podataka privremene prilikom izvršavanja tijeka rada. Taj prostor za pohranu je račun pod uvjetom da biste strojnog učenja Studio u trenutku se stvara radnog prostora i pristupnih tipki su povezani s tog radnog prostora. Ako pristupnih tipki se promijeniti nakon stvaranja radnog prostora, radni prostor više ne možete pristupiti računu za pohranu. Će prestati raditi, a sve eksperimenata tog radnog prostora neće uspjeti.

Ako ste promijenili račun za pohranu pristupnih tipki, ponovnog sinkroniziranja pristupnih tipki u radnom prostoru pomoću portala za Azure klasični.  


## <a name="azure-marketplace"></a>Azure Marketplace

U odjeljku [Najčešća Pitanja za objavljivanje i korištenje aplikacija na tržištu strojnog učenja](machine-learning-marketplace-faq.md).

## <a name="support-and-training"></a>Podrška i obuka

**Gdje pronaći tečajeve za učenje strojno Azure?**

[Azure strojno dokumentaciju medijskog](https://azure.microsoft.com/services/machine-learning/) hostira videozapisa vodiče i vodiče s uputama. Vodiče za detaljne navedite Uvod u usluge i voditi kroz podataka znanstvenog životnog ciklusa uvoz podataka, čišćenje podataka, stvaranje predvidljivu modela i implementacija ih u proizvodnju pomoću Azure strojnog učenja.

Ne možemo dodajete nove materijala centar za učenje strojno pridonositi. Možete poslati zahtjeve za dodatne učenje materijal na računalu centar za učenje na [forum za povratne informacije korisnika](https://windowsazure.uservoice.com/forums/257792-machine-learning).

Obuka možete pronaći i na [Microsoftovoj virtualnoj Akademiji](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Kako nabaviti podrška za učenje strojno Azure?**

Da biste dobili tehničke podrške za Azure strojnog učenja, idite na [Podržava Azure](/support/options/) i odaberite **Strojnog učenja**.

Azure strojnog učenja ima forumu zajednice korisnika na MSDN-gdje mogu postavljati pitanja koji se odnose na Azure strojnog učenja. Forum nadzire tima Azure strojnog učenja. Posjetite [Forum za Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Pitanja o naplati

**Kako funkcionira strojnog učenja naplatu?**

Postoje dvije komponente sa servisom Azure strojnog učenja. Strojnog učenja Studio i strojnog učenja Web Services.

Dok su procjene strojnog učenja Studio, možete koristiti besplatne naplate sloju.  Besplatni sloju omogućuje i uvođenje klasičnog web-servisa ograničenog kapaciteta.

Nakon što ste odlučili da Azure strojnog učenja odgovara vašim potrebama, možete se prijaviti za standardne sloju. Da biste se registrirali, morate imati pretplatu na Microsoft Azure.

U standardnom sloju naplaćena mjesečno za svaki radni prostor definirate Studio strojnog učenja. Kada pokrenete pokusa u na studio, se naplatiti za resurse računalnim kad se izvode pokusa. Kada uvođenje klasičnog web-servisa, transakcije i izračunati su sati naplatiti na temelju plaćanje kao vam Idi (PAYG).

Nova web-servisi za strojno učenje predstavljanje naplata tarife kojih za dodatne predvidljivost troškova. Korisnicima koji su potrebni veliku količinu kapaciteta tiered cijene nudi eskontiranu stope.

Prilikom stvaranja plana primijenite na fiksni trošak koji se isporučuju s uključene količinu API računalnim sata i API transakcije. Ako morate uključiti količine, možete dodati dodatne instance planu. Ako vam je potrebna puno više uključene količine, možete odabrati veći sloju plan koji omogućuje znatno više dio količine i bolje eskontiranu stopi.

Kada uključiti količine u pojavljivanja koriste, dodatne korištenje se naplaćuje brzinom pokrivenosti pridružene naplate sloju plan.

Napomena: Količine uključene su reallocated svakih 30 dana i Nekorišteni uključene količine ne odgoditi za sljedeće razdoblje.

Dodatni naplata i informacije o cijenama, potražite u članku [Strojnog učenja cijene](https://azure.microsoft.com/pricing/details/machine-learning/).

**Mora li strojnog učenja besplatnu probnu verziju?**

 Azure strojnog učenja je besplatna pretplata mogućnost (pogledajte [Strojnog učenja cijene](https://azure.microsoft.com/pricing/details/machine-learning/) detalje), a strojnog učenja Studio ima programa 8-satnom brzi procjenu probnu verziju dostupna (prijavite se na [Računalu učenje Studio](https://studio.azureml.net/?selectAccess=true&o=2) za ove probne verzije).

 Osim toga, kada se registrirate za Azure besplatnu probnu verziju, možete ga isprobati Azure servisi za mjesec. Da biste saznali više o Azure besplatnu probnu verziju, posjetite [Azure besplatnu probnu najčešća Pitanja](/pricing/free-trial-faq/).

**Što je transakcija?**

Transakcije predstavlja API poziva koji odgovara Azure strojnog učenja. Transakcije iz servisa odgovor na zahtjev (RRS) i grupe za izvršavanje usluge (BES) poziva pridružuje i naplatiti s planom naplate.

**Mogu li koristiti količine uključene transakcije u plan za RRS i BES transakcije?**

Da, svoje transakcije s RRS i BES se pridružuje i naplaćena s planom naplate.

**Što je jedan sat računalnim API-JA?**

Jedan sat računalnim API je naplate jedinica za pozive API vrijeme poduzeti da biste pokrenuli koristi resurse računalnim ML. Svi vaši se pozivi se pridružuje svrhu naplate.

**Koliko dugo ne poziva uobičajeni radni API poduzeti?**

Radni API poziva vremena mogu se razlikovati znatno, obično rasponu od stotine milisekundama nekoliko sekundi, ali možda ćete morati minuta ovisno o složenosti obradu podataka i strojnog učenja modela. Najbolji način za procjenu radnog API poziva vremena je benchmark modela na servis strojnog učenja.

**Što je sata Studio izračunati?**

Izračunavanje Studio sata je naplate jedinica zbrajanja put koristiti svoje eksperimenata izračunati resursa u studio.

**Novi Web Services, što je razvojni i testiranje sloju namijenjene?**

Azure ML novog web-servisi omogućuju više razine koje možete koristiti Dodjela plan za naplatu. Razvojni i testiranje sloju je razina na kojoj se navode ograničeni uključene količine koje omogućuju ispitivanje vaše eksperiment kao nove web-usluge bez povećavanja troškova. Imate mogućnost "Početak u gume" da biste vidjeli kako funkcionira.

**Postoje li troškova zasebnom prostora za pohranu?**

Strojnog učenja besplatne sloju potrebna ili ne dopušta zasebnu prostora za pohranu. Strojnog učenja standardne sloju zahtijeva da korisnici imaju račun za Azure prostora za pohranu. Azure prostora za pohranu je [naplaćeno zasebno](https://azure.microsoft.com/pricing/details/storage/).

**Kako funkcionira strojnog učenja podršku visoku dostupnost?**

Radni API poziva vremena mogu se razlikovati znatno, obično rasponu od stotine milisekundama nekoliko sekundi, ali možda ćete morati minuta ovisno o složenosti obradu podataka i strojnog učenja modela. Najbolji način za procjenu radnog API poziva vremena je benchmark modela na servis strojnog učenja.

**Potrebne određene vrste resursa računalnim će Moje pozive radnog API pokrenuti?**

Servis strojnog učenja je složene servis, a stvarni računalnim resursa koji se koriste na pozadinski razlikuju se i su optimizirani za performanse i predvidljivost.

### <a name="management-of-new-web-services"></a>Upravljanje novog web-servisa

**Što se događa prilikom brisanja Moja tarifa?**

Plan je uklonjena iz pretplate, a zatim se naplatiti za proporcionalni korištenje.

Napomena: Nije moguće izbrisati plan koji koristi web-servisa. Da biste izbrisali plan, morate novoj tarifi dodijelite web-servisa i brisanje web-servisa.

**Što je instanca komponente plan?**

Planiranje instancu je jedinica uključene količine koje možete dodati na vaša tarifa za naplatu. Kad odaberete naplate sloju za naplatu tarifa, dolazi s jedne instance. Ako morate uključiti količine, možete dodati pojavljivanja odabrane naplate sloju planu.

**Koliko je instanci plan možete dodati?**

Pretplatu možete imati jednu instancu sloju razvojni i testiranje.

Za razine S1, S2 i S3 možete dodati koliko god potrebne.

Napomena: Ovisno o predviđenu upotrebe, možda će biti troškova učinkovitije nadogradnju na noviji sloju uključene količine umjesto dodati instanci trenutni sloju.

**Što se događa kada promijenim plan razine (Nadogradnja / prijeći na nižu)?**

Staru tarifu se briše i trenutnog korištenja naplate na linearnu amortizaciju. Novoj tarifi s puno obuhvaćene količine sloju nadograditi/Vrati se stvara za ostale razdoblja.

Napomena: Količine uključene su dodijeliti razdoblje i količine koje se ne koriste se odgoditi.

**Što se događa kada povećati instance u tarifu?**

Nalaze se na linearnu amortizaciju i količine u roku od 24 sata stupila na snagu.

**Što se događa prilikom brisanja instance komponente tarifu?**

Instancu se uklanja iz pretplate, a zatim se naplatiti za proporcionalni korištenje.


### <a name="signing-up-for-new-web-services-plans"></a>Registracija za nova web-servisi tarife

**Kako se registrirati za tarifu?**

Imate dva načina za stvaranje tarife za naplatu.

Kada prvi put implementacije novog web-servisa, možete odabrati postojeću tarifu ili stvorite novi plan.

Tarife stvorena na taj način su u vašoj regiji zadane i web-servisa će biti implementirano u to područje.

Ako želite da biste implementirali servise regije osim zadane regije, preporučujemo vam da biste definirali planove za naplatu prije implementacije na servisu

U tom slučaju možete prijaviti na portal za Azure strojnog učenja Web Services i idite na stranicu tarife. Iz nje, koje možete dodati i brisanje tarife, kao i izmijeniti postojeću tarifu.

**Koju tarifu odaberite za da biste započeli?**

Preporučujemo da započnete s standardne S1 sloju i praćenje usluge za korištenje. Ako pronađete koristite vaše uključene količine informacija, možete dodati instanci ili premještanje na višu sloju i dobiti bolje eskontiranu stope. Plan za naplatu možete prilagoditi prema potrebi cijeloj vaše ciklusa naplate.

**Koje regija dostupne nove tarife u?**

Nove tarife za naplatu dostupne su u tri radnog područja u kojem podržavamo novog web-servisi:

* Južna središnje SAD-a
* Europa Zapad
* Južna istočnoazijski

**Imam web-servisi u više područja. Moram li plan za svaku regiju?**

Da. Planiranje cijene razlikuje se po regijama. Ako pokrenete web-servisa u drugoj regiji, morate je dodijeliti određeni plan to područje.

### <a name="new-web-services---overages"></a>Nove servise Web - Overages

**Kako provjeriti je li moj korištenja servisa za web u pokrivenost?**

Korištenje možete pregledati na sve tarife na stranici tarife na portalu servisa Azure strojnog učenja Web Services. Prijavite se na portal sustava, a zatim kliknite mogućnost izbornika tarife.

U transakcije i računalnim stupaca tablice, vidjet ćete uključene količine plana i postotak koji se koristi.

**Što se događa pri korištenju gore količine Uključi u sloju razvojni i testiranje?**

Servise koje ste dodijelili ih sloj razvojni i testiranje su zaustavljena do sljedeće razdoblje ili ih premjestite na neki od plaćenu razine.

**Klasični Web Services i Overages od novog web-servisi, kako se cijene izračunavaju za radnih opterećenja zahtjev za odgovor (RRS) i grupe (BES)?**

Za radno opterećenje programa RRS vam se naplatiti za svaku poziv API transakcije koje ste napravili, a put računalnim povezane s tim zahtjevima za. Vaša RRS radnog API transakcija troškova izračunavaju se kao ukupan broj API poziva koje ste provjerite pomnožen po cijenu 1000 transakcije (proporcionalno podijeljen po pojedinačne transakcije). RRS API radnog API izračunati sat troškove izračunavaju se kao što je vrijeme potrebno za svaku API poziv da biste pokrenuli, pomnožen ukupan broj transakcije API množi cijenu po satu za izračun radnog API-JA.

Na primjer za pokrivenost standardne S1 1,000,000 API transakcije koje koriste 0.72 sekundi svaki da biste pokrenuli posljedicu (1.000.000 *transakcije API K $0.50-1) u $500 troškova transakcije API radnog i (1.000.000* 0.72 sec * $2 / hr) $400 radnog API izračunati sata, za Ukupno $900.

BES radno opterećenje vam se naplatiti na isti način, međutim, troškove transakcije API predstavlja broj obrade koje pošaljete i troškove računalnim predstavljaju vrijeme računalnim koji je povezan s tim obrade. Troškove BES radnog API transakcije izračunavaju se kao ukupan broj zadatke koji su poslani množi cijenu 1000 transakcije (proporcionalno podijeljen po pojedinačne transakcije). BES API radnog API izračunati sat troškove izračunavaju se kao vrijeme potrebno za svaki redak posla da biste pokrenuli pomnožen ukupnog broja redaka u vaš posao pomnožen ukupan broj poslove množi cijenu po satu za izračun radnog API-JA. Kada pomoću Kalkulator strojnog učenja metar transakcije predstavlja broj zadataka koje namjeravate poslati, a vrijeme po polju transakcije kombinirane vremena potrebnog za sve retke u svaki zadatak da biste pokrenuli.

Primjerice s standardne S1 pokrivenost ako pošaljete 100 zadataka po danu da svaki consist 500 redaka koji potrajati 0.72 sekundi, a zatim mjesečni pokrivenosti troškove bio (100 zadataka po danu = 3,100 poslove/Premjesti *transakcije API K $0.50-1) $1.55 troškova proizvodnje API transakcije i (500 redaka* 0.72 sec *3,100 poslove* $2/hr) $620 izračunati sata API-JA za radni , za Ukupno $621.55.

### <a name="azure-ml-classic-web-services"></a>Klasični Azure ML web-servisi

**U pokretu za plaćanje kao što je i dalje dostupna?**
Da, klasične web-usluge i dalje su dostupni u Azure strojnog učenja.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Azure strojnog učenja besplatne i standardne sloju

**Što je sve obuhvaćeno Azure strojnog učenja besplatne sloju?**

Azure strojnog učenja besplatne sloju namijenjen je pružanje detaljnije Upoznavanje s Studio za učenje Azure računala. Sve što trebate je Microsoftov račun da biste se registrirali. Besplatni razina obuhvaća besplatnu pristup jedan radni prostor Azure strojnog učenja Studio po [Microsoftov račun](https://www.microsoft.com/account/default.aspx). Obuhvaća mogućnost korištenja veličine do 10 GB prostora za pohranu i mogućnost operationalize modela kao pripremna API-ji. Besplatni sloju radnih opterećenja su prekriveni programa SLA i su namijenjene razvoja ili osobnu upotrebu. Besplatni sloju radnih opterećenja canâ€™ t podataka programa access putem veze s poslužiteljem SQL lokalnog.

**Što je sve obuhvaćeno Azure strojnog učenja standardne sloju i tarife?**

Azure strojnog učenja standardne sloju je plaćenu radnih verzija Azure strojnog učenja Studio. Azure ML mjesečnu naknadu servisa Studio naplaćen na po temelj svakog mjeseca radnog prostora i Proporcionalni djelomične mjeseca. Azure sati eksperiment ML Studio naplatu po satu računalnim za aktivni početak. Naplata proporcionalno je podijeljen djelomične sata.  

Servis za Azure ML API naplate ovisno o tome je li se klasične web services ili novog web-servisa.

Naknade za sljedeće su pridružuje po radnog prostora za vašu pretplatu.

* Strojnog učenja radnog prostora pretplate - strojno učenje radnog prostora je pretplata mjesečnu naknadu koji omogućuje pristup s radnim prostorom ML Studio je potreban za pokretanje eksperimenata u na studio i korištenja radnog API-ji.
* Studio isprobati sati - ova zbrajanja metar sve izračunati troškove akumuliranih eksperimenata u programu ML Studio pokrenete i pokretanjem radnog API poziva u pripremna okruženju.
* Podataka programa Access tako da povezujete s lokalnog sustava SQL server u vašem modelima za vaše obuka i bilježenje rezultata.
* Za klasične web-usluge:
    * API radnih sati - izračunati ovaj metar uključuje troškove računalnim akumuliranih Web Services radi u radni.
    * Transakcije proizvodnje API-JA (u 1000s) – ovaj metar uključuje naknade akumuliranih po poziv na web-servisa na radni.

Osim prethodni naknada slučaju novog web-servisi naknade se pridružuje tarifu odabrana:

* Standardni S1/S2/S3 API-JA plana (jedinice) - ova metar predstavlja vrstu instanca odabrali za nova web-usluge
* Standardne S1/S2/S3 pokrivenosti API-JA za izračun sati - ova metar uključuje troškove računalnim akumuliranih tako da novi Web servise u radnog nakon uključene količine u postojeće instance koriste prema gore. Dodatni korištenje se naplaćuju po na overate stopa pridružene S1/S2/S3 plan sloju.
* Standardni S1/S2/S3 pokrivenost API transakcije (u 1,000s) – ovaj metar obuhvaća troškove akumuliranih po poziv radnog nove web-usluge nakon uključene količine u postojeće instance koriste prema gore. Dodatni korištenje se naplaćuju po na overate stopa pridružene S1/S2/S3 plan sloju.
* Uključeni računalnim API količinu sati – pomoću novog web-servisi ovaj metar predstavlja uključene količinu sati izračunati API-JA
* Uključeni količina API transakcije (u 1,000s) – nova web-servisi, ova metar predstavlja uključene količinu transakcije API-JA


**Kako se registrirati za Azure ML besplatne sloju?**

Sve što trebate je Microsoftov račun. Idite na [početnu stranicu Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/)pa kliknite **Start sada**. Prijavite se pomoću Microsoftova računa i radni prostor u slobodno sloju automatski se stvara. Možete pokrenuti da biste istražili i odmah stvori eksperimenata strojnog učenja.

**Kako se registrirati za Azure ML standardne sloju?**

Najprije morate imati pristup Azure pretplatu za stvaranje radnog prostora standardne ML. Registracija za 30 dana besplatnu probnu Azure pretplatu i noviji Nadogradnja plaćenu pretplatu za Azure ili kupiti Azure pretplatili potpuno. Zatim možete stvoriti radni prostor strojnog učenja na portalu Microsoft Azure klasični nakon pristupe s pretplatom. Prikaz [detaljnih uputa](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Osim toga, možete dobivati pozive vlasnik radnog prostora standardne ML da biste pristupili vlasnika radnog prostora.

**Odrediti vlastitu blobova platforme Azure prostora za pohranu računa za uporabu besplatne sloju?**

Ne, standardne sloju jednako verziju servis strojnog učenja dostupnoj prije uvedene u razine.

**Možete implementirati Moje strojnog učenja modela kao API-ji u besplatne sloju?**

Da, možete operationalize modela učenje računala da biste pripremna API servisa kao dio besplatne sloju. Da biste mogli smjestiti pripremna API servisa u radni i početak radnog krajnje točke za servis operationalized, morate koristiti standardni sloju.

**Koja je razlika između Azure besplatnu probnu i Azure strojnog učenja besplatne sloju?**

[Besplatna probna verzija sustava Microsoft Azure](https://azure.microsoft.com/free/) nudi ekipa koje se mogu primijeniti na bilo koji servis za Azure za jedan mjesec. Azure strojnog učenja besplatne sloju nudi Neprekidan pristup posebno servisa Azure strojnog učenja radnih opterećenja koji nisu proizvodnje.

**Kako premjestiti pokusa iz na slobodno sloju standardne sloju?**

Da biste kopirali vaše eksperimenata na slobodno sloju standardne sloju:

1.  Prijavite Azure strojnog učenja Studio i provjerite je li možete vidjeti slobodnog prostora i standardnog radnog prostora u biraču radnog prostora u gornjoj navigacijskoj traci.
2.  Prijeđite u slobodni prostor ako se nalazite u standardnom radnog prostora.
3.  U prikazu popisa eksperiment odaberite pokusa želite kopirati, a zatim kliknite gumb naredbe Kopiraj.
4.  Odaberite standardni radnog prostora iz skočnog dijaloškog okvira i kliknite gumb Kopiraj.
    Sve povezane skupova podataka, obučeni model, itd kopiraju se zajedno s pokusa u standardni radnog prostora.
6.  Morate ponovno pokrenite pokusa i ponovno objaviti web-servisa u standardni radnog prostora.

### <a name="studio-workspace"></a>Radni prostor Studio

**Prikazuje će drugi računi za različite radne prostore?**

Naknade za radni prostor su do oštećenja zasebno za svaku primjenjivo metar na jedan račun.

**Potrebne određene vrste resursa računalnim će Moje eksperimenata pokrenuti?**

Servis strojnog učenja je složene servis, a stvarni računalnim resursa koji se koriste na pozadinski razlikuju se i su optimizirani za performanse i predvidljivost.

### <a name="guest-access"></a>Pristup gosta

**Što je pristup gosta Azure strojnog učenja Studio?**

Pristup gosta je ograničenim sučelje za probno razdoblje koja omogućuje stvaranje i pokretanje eksperimenata u na Azure strojnog učenja Studio i bez provjere autentičnosti. Sesije goste nisu koje nisu stalni (nije moguće spremiti) i ograničeni na osam sati. Drugih ograničenja obuhvaćaju nedostatak R Python službe za podršku i, nedostatak pripremna API-ji i veličine i pohranu kapaciteta ograničeni skup podataka. Za usporedbu, odaberite da biste se prijavili pomoću Microsoftova računa korisnike imali puni pristup slobodno sloju strojnog učenja Studio opisane iznad koje sadrži stalnih prostor i detaljnije mogućnosti. Odaberite besplatno sučelje za učenje računala klikom na **Početak rada** na [https://studio.azureml.net](https://studio.azureml.net)i odabirom pogoditi Access i prijavite se pomoću Microsoftova računa.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
