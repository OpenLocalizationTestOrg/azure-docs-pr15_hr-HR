<properties 
    pageTitle="Korištenje linearna regresija u strojnog učenja | Microsoft Azure" 
    description="Usporedba linearna regresija modela u programu Excel i u Azure strojnog učenja Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Korištenje linearna regresija u Azure strojnog učenja

> *Kate Baroni* i *Ben Boatman* su projektantima rješenje enterprise u Microsoftovu podataka uvida centar organizaciji. U ovom se članku se opisuju svoje okruženje migracije postojećeg paketa regresijsku analizu u oblaku rješenje pomoću Azure strojnog učenja.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Cilj

Naš projekta rada s dvije ciljeve na umu:  

1. Poboljšajte točnost našu tvrtke ili ustanove mjesečnog prihoda projekcija pomoću predvidljivu analytics  
2. Pomoću Azure ML potvrdi, optimiziranje, povećati brzinu, i omjere naš rezultata.  

Kao što su mnoge se tvrtke našu tvrtke ili ustanove prolazi kroz mjesečnog prihoda predviđanje postupak. Naš tim za small od poslovni analitičar analitičar podataka je tasked putem strojnog učenja podržavaju proces i poboljšanja točnosti predviđanja.  U tim potrošeno nekoliko mjeseca prikupljanje podataka iz više izvora i atribute podataka primjene statističku analizu prepoznavanje atributi ključa za predviđanje prodaje servisa.  Daljnji koraci je da biste započeli prototyping statistike regresije modelima podataka u programu Excel.  Unutar nekoliko tjedana smo imao regresije model programa Excel koja je outperforming trenutno polje i financije predviđanje procesa. To su označeni kao rezultat predviđanje osnovne.  


Ne možemo zatim trajalo na sljedeći korak premještati naše predvidljivu analize Azure ML da biste saznali kako se Azure ML poboljšanju predvidljivu performansi.


## <a name="achieving-predictive-performance-parity"></a>Postizanje slične značajke predvidljivu performansi

Naš prednost je da biste postigli slične značajke između modelima regresije Azure ML i Excel.  Dao točno iste podatke, a isti Podjela za obuku i testiranje podataka ne možemo željeli da biste postigli slične značajke predvidljivu performanse između programa Excel i Azure ML.   Na početku Nismo uspjeli. Model programa Excel outperformed modela Azure ML.   Neuspjeh je zbog nedostatka razumijevanja postavku osnovni alata u Azure ML. Nakon sinkronizacije s Azure ML timu za proizvod, ne možemo stekli bolje razumjeli osnovni postavljanje potrebni za naše skupova podataka i postići slične značajke između dva modela.  

### <a name="create-regression-model-in-excel"></a>Stvaranje regresijskog modela u programu Excel
Naš regresije Excel koristi model standardne linearna regresija pronaći u aplikaciji Analysis ToolPak u programu Excel. 

Ne možemo izračunati *znače apsolutne % pogreške* i koristiti kao mjere performansi modela.  Potrebno 3 mjeseca stigne u radu modela pomoću programa Excel.  Ne možemo ne unese velik dio učenje u pokusa Azure ML koji konačni je korisni za razumijevanje preduvjeti.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Stvaranje usporediti eksperiment u Azure strojnog učenja  
Ne možemo iza ove korake da biste stvorili naš eksperiment u Azure ML:  

1.  Prenijeli skupu podataka kao csv Azure ML (vrlo malen datoteka)
2.  Stvara novu eksperiment i koristiti [Odabir stupaca u skupu podataka] [ select-columns] modul da biste odabrali iste značajke podataka koji se koriste u programu Excel   
3.  Koristili [Razdijeljeni podaci] [ split] modula ( *Relativni izraz* način) podjele podatke u točno istom vlaku skupova kao što je prodao obavljeno u programu Excel  
4.  Experimented s [Linearna regresija] [ linear-regression] modul (zadane mogućnosti samo) navedenih i uspoređuju rezultate u našem regresije model programa Excel

### <a name="review-initial-results"></a>Pregled početne rezultata
Model programa Excel isprva jasno outperformed Azure ML modela:  

|   |Excel|Azure ML|
|---|:---:|:---:|
|Performanse|   |  |
|<ul style="list-style-type: none;"><li>Prilagođeni R kvadrat</li></ul>| 0.96 |N/D|
|<ul style="list-style-type: none;"><li>Koeficijent <br />Utvrđivanje</li></ul>|N/D|   0.78<br />(niska točnost)|
|Znače apsolutne pogreške |  $9. 5M|  M $ 19.4|
|Znače apsolutne pogreške (%)|   6.03%|  12,2%

Kada u timu Azure ML naišli naš proces i rezultati razvojni inženjeri i fizičari podataka, oni brzo navedeni su korisne savjete.  

* Kada koristite [Linearna regresija] [ linear-regression] modul u Azure ML postoje dva načina:
    *  Mrežni Descent prijelaza: Možda nije više prikladan za opsežne problema
    *  Obična najmanjih kvadrata: To je način shvatiti većine kada su čuli linearne regresije. Mali skupova podataka, svakodnevne najmanjih kvadrata može biti više optimalnih odabir.
*  Razmislite o tweaking parametar L2 Regularization težinu radi poboljšanja performansi. Postavlja se na 0,001 po zadanom i za naše small skup podataka, ne možemo je postavite na 0,005 radi poboljšanja performansi.    

### <a name="mystery-solved"></a>Mystery otkloniti!
Kada se primjenjuje preporuke ne možemo postići iste osnovne performanse ML Azure kao s programom Excel:   

|| Excel|Azure ML (inicijal)|Azure ML w/ najmanjih kvadrata|
|---|:---:|:---:|:---:|
|Označen kao vrijednost  |Stvarne vrijednosti (numeričkoj)|isti|isti|
|Učenik  |Excel -> podaci regresije -> Analiza|Linearne regresije.|Linearna regresija|
|Mogućnosti učenik|N/D|Zadane postavke|Obična najmanjih kvadrata<br />L2 = 0,005|
|Skup podataka|26 redaka, 3 značajke i 1 oznake.   Sve numeričke.|isti|isti|
|Podjela: vlaku|Excel obučavanju članova na prvo 18 redaka testirati zadnje 8 redaka.|isti|isti|
|Podjela: Test|Formula regresije zadnje 8 reci u programu Excel|isti|isti|
|**Performanse**||||
|Prilagođeni R kvadrat|0.96|N/D||
|Koeficijent određivanja|N/D|0.78|0.952049|
|Znače apsolutne pogreške |$9. 5M|M $ 19.4|$9. 5M|
|Znače apsolutne pogreške (%)|<span style="background-color: 00FF00;">6.03%</span>|12,2%|<span style="background-color: 00FF00;">6.03%</span>|

Uz to, koeficijente Excel u usporedbi i debljine značajku u modelu Azure obučeni:

||Koeficijenti programa Excel|Azure značajka debljine|
|---|:---:|:---:|
|INTERCEPT/odstupanje|19470209.88|19328500|
|Značajka odgovora|0.832653063|0.834156|
|Značajka B|11071967.08|11007300|
|Značajka C|25383318.09|25140800|

## <a name="next-steps"></a>Daljnji koraci

Ne možemo željeli zauzeti Azure ML web-servisa u programu Excel.  Naš poslovni analitičar analitičar podataka ovise o programu Excel i ne možemo potreban način da biste pozvali web-servisa Azure ML pomoću retka podaci programa Excel ili ih vratite predviđene vrijednosti u Excel.   

Ne možemo i željeli optimiziranje naš modela pomoću mogućnosti i algoritmi dostupne u Azure ML.

### <a name="integration-with-excel"></a>Integracija s programom Excel
Naš rješenje je da biste operationalize model regresije naš Azure ML stvaranjem web-servisa iz obučeni modela.  Za nekoliko minuta, stvaranja web-servisa, a zatim ćemo se poziva izravno iz programa Excel da biste se vratili vrijednost predviđene prihoda.    

U odjeljku *Nadzorne ploče Web Services* sadrži koje je moguće preuzeti radnu knjigu u programu Excel.  Radnu knjigu u sklopu unaprijed oblikovanih web API-JA i sheme podatke o usluzi ugrađen.   Kada kliknete *Preuzmite*radnu knjigu programa Excel, on se otvara i spremite ga u s vašim lokalnim računalom.    

![][1]
 
S radne knjige otvorene, kopirajte na unaprijed definirane parametara u odjeljku plava parametar kao što je prikazano u nastavku.  Kada unesete parametre, Excel pozive web-uslugu AzureML pa u predviđene testu dobije oznake će se prikazivati u odjeljku zelenu predviđenim vrijednostima.  Radna knjiga će i dalje da biste stvorili predviđanja parametar koji se temelji na obučeni modela za sve stavke retka koje ste unijeli u odjeljku parametara.   Dodatne informacije o korištenju ove značajke potražite u članku [troše programa Azure strojnog učenja web-servisa iz programa Excel](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimizacija i dodatno eksperimenata
Nakon što smo imali referentne vrijednosti s oglednim model programa Excel, ne možemo premjestiti unaprijed optimiziranje naš Azure ML linearne regresije modela.  Koristi modul [filtar temelji odabir značajki] [ filter-based-feature-selection] da biste poboljšali na našem odabiru početne podatke i elemente pomaže postizanju performanse unaprjeđivanja 4.6% znače apsolutne pogreške.   Za buduće projekte koristit ćemo značajka koji nije moguće spremiti nam tjedana u iterating kroz atribute podataka da biste pronašli desnom skup značajki koje želite koristiti za modeliranja.  

Sljedeći plan da biste dodali dodatne algoritama kao što je [Bayesian] [ bayesian-linear-regression] ili [Boosted odlukama] [ boosted-decision-tree-regression] u našem eksperiment da biste usporedili performansi.    

Ako želite isprobati regresije, dobro skup podataka da biste isprobali je uzorak dataset energiju učinkovitosti regresije koja ima mnogo numeričke atribute. Skup podataka prikazuje se kao dio skupove podataka uzorak u ML Studio.  Možete koristiti raznih učenje module za predviđanje grijanje učitavanja ili hlađenje Učitaj.  Grafikon u nastavku je performanse Usporedba različitih regresije Pratit će odnosu na energiju učinkovitosti dataset procjenjivanje cilj varijable Cooling opterećenja: 

|Model|Znače apsolutne pogreške|Srednja korijenski kvadratne pogreške|Relativna apsolutne pogreške|Zavisne kvadratne pogreške|Koeficijent određivanja
|---|---|---|---|---|---
|Stabla odlučivanja boosted|0.930113|1.4239|0.106647|0.021662|0.978338
|Linearna regresija (prijelaza Descent)|2.035693|2.98006|0.233414|0.094881|0.905119
|Regresije neural mreže|1.548195|2.114617|0.177517|0.047774|0.952226
|Linearna regresija (svakodnevne najmanjih kvadrata)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Ključni Takeaways 

Ćemo naučili mnogo tako da iz izvodi Excel regresije a Azure strojnog učenja eksperimenata paralelno. Stvaranje osnovnog modela u programu Excel i Usporedba s modela pomoću Azure ML [Linearne regresije] [ linear-regression] pomaže nam dodatne Azure ML, a ne možemo otkrio prilike da biste poboljšali performanse odabira i model podataka.         

Također naišli nije Uputno koristiti [filtar temelji odabir značajki] [ filter-based-feature-selection] za ubrzano predviđanje za buduće projekte.  Primjenom odabir značajki podataka možete stvoriti poboljšani model u Azure ML s bolje performanse cjelokupnog. 

Mogućnost da biste prenijeli na predvidljivu analitički predviđanje iz Azure ML u Excel systemically omogućuje značajan povećava u mogućnost uspješno dobivanja rezultata publici općenite poslovnih korisnika.     


## <a name="resources"></a>Resursi
Neki resursi za pomažu u radu s regresije:  

* Regresije u programu Excel.  Ako ste pokušali nikad regresije u programu Excel, pomoću ovog praktičnog vodiča olakšava: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regresije Dodavanje veze za vanjskih predviđanja.  Tyler Chessman napisali članka na blogu s vremenski niz predviđanja u programu Excel koja sadrži opis dobar početnike linearne regresije. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Obična najmanjih kvadrata linearna regresija: Pogreške, probleme i probleme.  Uvod i rasprave regresije: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
