<properties 
    pageTitle="Izvršavanje Python strojnog učenja skripte | Microsoft Azure" 
    description="Strukture dizajna načela podlozi podrška za skripte Python Azure strojnog učenja i scenariji za korištenje osnovne, mogućnosti i ograničenja." 
    keywords="Python strojnog učenja, pandas, python pandas, skripte python izvršavanje python skripti"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Izvršavanje Python strojnog učenja skripte u Azure strojnog učenja Studio

U ovoj se temi opisuje način dizajna podlozi trenutni podrška za skripte Python u Azure strojnog učenja. Glavni mogućnosti i istaknuti su, uključujući podršku za uvoz postojeći kod, izvoz vizualizacije i na kraju, su navedene neke od ograničenja i tijeku rada.

[Python](https://www.python.org/) je nezaobilaznim alatom u chest alat od mnogo fizičari podataka. Sadrži:

-  Sintaksa elegantna i sažet, 
-  Podrška za različite platforme 
-  veliku zbirke snažna biblioteka, a 
-  Alati za starijih razvoj. 

Python se koristi u svim faze tijeka rada obično se koristi u strojnog učenja Modeliranje, iz podataka ingest i obradu, značajka izgradnju i obuka modela i zatim provjere valjanosti i implementaciju modela. 

Azure strojnog učenja Studio podržava ugrađivanje Python skripte u raznim dijelovima strojnog učenja eksperiment te i jednostavno objavljivanje kao prilagodljivi, operationalized web-servisi na Microsoft Azure.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Dizajniranje načela Python skripte u strojnog učenja
Primarni sučelje za Python u Azure strojnog učenja Studio je putem [Skripte za izvršavanje Python] [ execute-python-script] modulu prikazan u slika 1.

![slika1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![slika2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Slika 1. Modul za **Izvršavanje Python skripte** .

[Izvršavanje skripte Python] [ execute-python-script] modul prihvaća do tri unosa i daje do dva izlaze (spominju ispod), baš kao i njegov R analognih, [Izvršavanje skripte R] [ execute-r-script] modul. Kod Python izvršavanje se unosi u okvir za parametra kao imenovani posebno ulaza funkcije pod nazivom `azureml_main`. Evo načela ključa dizajna za implementaciju ovom modulu:

1.  *Mora biti idiomatic Python korisnicima.* Većina korisnika Python faktor kodu, kao funkcije unutar module, pa je relativno rijetko umetanja mnogo izvršna naredbe u modulu najviše razine. Zbog toga okvir skripta vodi posebno imenovani funkcija Python umjesto samo slijeda naredbi. Objekti koji se prikazuje u funkciji su standardne vrste Python biblioteke kao što su [Pandas](http://pandas.pydata.org/) podataka okvira i [NumPy](http://www.numpy.org/) polja.
2.  *Morate imati visoka kvaliteta između lokalne i izvršavanja u oblaku.* Pozadinski koristiti za izvršavanje kod Python temelji se na [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1, široku različite platforme znanstvenom Python distribuciju. Dolazi s blizu 200 najčešće Python paketa. Stoga fizičari podataka možete ispraviti pogreške i ispunjavate uvjete kodu, u njihove lokalnom okruženju strojno Azure kompatibilan s preglednikom učenje Anaconda. Da biste pokrenuli kao dio pokusa Azure strojnog učenja s povjerljivi koristiti postojeće okruženju za razvoj kao što su [IPython](http://ipython.org/) bilježnice ili [Python alate za Visual Studio](http://aka.ms/ptvs) . Dodatno, u `azureml_main` ulaza je vanilla funkcija Python i moguće je autor bez Azure strojnog učenja određene koda ili instaliran SDK-a.
3.  *Mora biti jednostavno composable s drugim modula Azure strojnog učenja.* [Izvršavanje skripte Python] [ execute-python-script] modul prihvati, kao unosa i izlaze, standardne Azure strojnog učenja skupova podataka. Temeljni framework proziran i učinkovitije premosti Azure strojnog učenja i Python runtimes (podrške značajke kao što su vrijednosti koje nedostaju). Python stoga se mogu koristiti u kombinaciji s postojećim tijekovima rada Azure strojnog učenja, uključujući one koje mogu pozivati u R i SQLite. Jedan envisage stoga tijekova rada koji:
  * Korištenje Python i Pandas podataka predobradu i čišćenje, 
  * sažetak sadržaja podataka SQL transformaciju, pridruživanje više skupova podataka značajki obrasca 
  * obuka modela pomoću opsežan skup algoritama u Azure strojnog učenja, a 
  * Ocjenjivanje i naknadno obradi rezultate pomoću R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Osnovno korištenje scenariji u strojnog učenja Python skripti
U ovom ćete odjeljku smo pitanja iz upitnika neki osnovni koristi [Izvršavanje skripte Python] [ execute-python-script] modul.
Kao što je rečeno ranije, prikazat će se sve unose modul Python kao okvira Pandas podataka. Dodatne informacije o Python Pandas i kako ga koristiti za rukovanje podataka učinkovito i učinkovitije pronaći ćete u *Python za analizu podataka* (O'Reilly 2012) tako da Srednja McKinney. Funkcija mora vratiti jedan okvir podataka Pandas pakirat unutar Python [niz](https://docs.python.org/2/c-api/sequence.html) kao što je n-torke, popisa ili NumPy polja. Prvi element kombinacija zatim vraća u prvom izlazni priključak modula. Ovu shemu prikazan je slika 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Slika 2. Mapiranje priključke za parametara za unos i vraćaju vrijednost izlazni priključak.

Detaljnije semantiku mapiranje unos priključke za parametre u `azureml_main` funkcija prikazuju se u tablica 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tablica 1. Mapiranje unos priključke za parametri funkcije.

Mapiranja između ulaznih priključaka i parametri funkcije je Pozicijske. Prvi povezanog ulazni priključak mapirano na prvi parametar funkcije, a drugi unos (Ako je s njom povezani) mapirano na drugi parametar funkciju.

## <a name="translation-of-input-and-output-types"></a>Prijevod ulazni i izlazni vrste
Kao što je opisano ranije unos skupove podataka u Azure strojnog učenja pretvaraju se u podatke u okvire u Pandas i izlazne podatke okvira pretvaraju se natrag u skupove podataka Azure strojnog učenja. Sljedeće Pretvorba se izvodi:

1.  Niz i numeričke stupaca će se pretvoriti u-je i vrijednosti koje nedostaju u skup podataka pretvaraju se u ' N/d' vrijednosti u Pandas. Isti pretvorbe se događa na putu natrag (vrijednosti n/d u Pandas pretvaraju se u vrijednosti koje nedostaju u Azure strojnog učenja).
2.  Indeks vektori u Pandas nisu podržane u Azure strojnog učenja. Sve okvire ulaznih podataka u funkciji Python uvijek imati 64-bitni numeričke indeks od 0 do broj redaka minus 1. 
3.  Azure strojnog učenja skupove podataka ne smije sadržavati duplikate naziva stupaca i nazivi stupaca koji nisu nizova. Ako je okvir izlaz podataka sadrži stupce numerička, framework poziva `str` na nazive stupaca. Isto tako, sve duplikate naziva stupaca su automatski oštećeni da biste osigurali jedinstvenih imena. Nastavak (2) dodaje se na prvi duplikat (3) u drugi duplikat, itd.

## <a name="operationalizing-python-scripts"></a>Operationalizing Python skripti
Kod [Izvršavanje skripte Python] [ execute-python-script] moduli koji se koriste u bilježenja rezultata eksperiment nazivaju se prilikom objavljivanja kao web-servisa. Ako, na primjer, slika 3 prikazuje bilježenja rezultata eksperiment koji sadrže kod za procjenu jedan Python izraz. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Slika 3. Web-servisa za procjenu Python izraz.

Web-servisa stvorene iz ovog eksperiment uzima kao unos izraza Python (kao što je niz), šalje tumačenja Python i vraća tablicu koja sadrži izraz i provjeriti u odnosu rezultat.

## <a name="importing-existing-python-script-modules"></a>Uvoz postojeće moduli Python skripte
Zajednički koristite-slučaja za mnoge fizičari podataka je ugraditi postojeće Python skripte u eksperimenata Azure strojnog učenja. Umjesto Ulančavanje i zalijepite sav kod u okviru jednu skriptu [Izvršavanje skripte Python] [ execute-python-script] modul prihvaća treći ulazni priključak na koji se mogu povezati zip datoteku koja sadrži Python module. Datoteka pa raspakirana po framework izvođenja prilikom izvođenja i sadržaj dodat će se na biblioteku put tumačenja Python. Na `azureml_main` funkcija možete uvesti te moduli izravno točka unosa.

Na primjer, razmislite o datoteke Hello.py koji sadrži jednostavne funkciju "Zdravo, svijete".

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Slika 4. Korisnički definirana funkcija.

Zatim ćemo stvoriti datoteku koja sadrži Hello.py Hello.zip:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Slika 5. Poštanski broj datoteka koja sadrži korisnički definiranu Python kod.

Nakon toga prenijeti to kao skup podataka u Azure strojnog učenja Studio. Stvaranje i pokretanje jednostavne eksperiment koja koristi kod Python u datoteci Hello.zip Pridruživanjem treći ulazni priključak Python skripte izvršavanje kao što je prikazano na ovoj slici.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

6 slici. Ogledni eksperimentiranje s korisnički definirane kod Python prenesene kao zip datoteku.

Modul izlaz prikazuje zip datoteke je unpackaged i funkciju `print_hello` uistinu pokrenuti.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Slika 7. Korisnički definirana funkcija koristi unutar [Izvršavanje skripte Python] [ execute-python-script] modul.

## <a name="working-with-visualizations"></a>Rad s vizualizacijama
Crtanje pomoću MatplotLib koji se mogu vizualizirati na web-pregledniku može vratiti [Izvršavanje skripte Python][execute-python-script]. Ali se iscrtavaju se neće automatski preusmjereni slike kao što su prilikom korištenja R. Stoga korisnika morate izričito spremiti bilo koje se iscrtavaju PNG datoteke ako su vratiti natrag Azure strojnog učenja. 

Da biste generirali slike iz MatplotLib, morate se natječu u nastavku:

* prijeći pozadinski "AGG" s iscrtavanje Qt utemeljen na zadani 
* Stvorite novi objekt slika 
* Početak osi i generiranje sve iscrtavaju u njega 
* spremite na slici u PNG datoteku 

Ovaj postupak je podržali Sljedeća slika 8 koja se stvara pomoću funkcije scatter_matrix u Pandas matrice crtanja raspršeni.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Slika 8. Spremanje slika MatplotLib slike.



Slika 9 prikazuje pokusa koji koristi skripte prikazano prethodno da biste se vratili iscrtava putem drugog izlazni priključak.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Slika 9. Vizualizacija iscrtavaju generirao Python kod.

Nije moguće vratiti više slika tako da ih spremite u različitim slike, izvođenja Azure strojnog učenja odabire sve slike i spaja ih za vizualizaciju.


## <a name="advanced-examples"></a>Dodatne primjere
Okruženje Anaconda instalirali u drugom Azure strojnog učenja sadrži uobičajene paketa kao što je NumPy, SciPy, i Scikits Saznajte i te se mogu se učinkovito za različite zadatke obradu podataka u kanalu za učenje tipičnog računala. Kao primjer, sljedeće eksperiment i skriptu ilustrira korištenje ensemble learners u Scikits Saznajte kako izračunati rezultati važnost značajke za skup podataka. Rezultati mogu pa se izvršiti odabir supervised značajki prije Umetanje u drugi model strojnog učenja.

Funkcija Python za izračunavanje važnost brojčane pokazatelje i redoslijed značajke koji se temelje na njemu je prikazano u nastavku:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Slika 10. Funkcija rank značajke tako da rezultati.
 Sljedeće pokusa zatim formula izračunava i vraća rezultate važnost značajki u skupu podataka "Pima indijskog Diabetes" u Azure strojnog učenja:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Slika 11. Eksperimentirajte rank značajke u skupu podataka Pima indijskog Diabetes.

## <a name="limitations"></a>Ograničenja 
[Izvršavanje skripte Python] [ execute-python-script] trenutno ima sljedeća ograničenja:

1.  *Izvršavanje u memoriji za testiranje.* Izvođenje Python je trenutno u memoriji za testiranje i zbog toga ne dopušta pristup mreži ili na lokalni datotečni sustav Trajni način. Sve datoteke spremljene lokalno su Izolirani i izbrisati nakon što modul Završi. Kod Python ne može pristupiti Većina direktorija na računalu koji se izvodi na, osim trenutnog direktorija i poddirektorije.
2.  *Nedostatak sofisticirane razvoja i podrška za ispravljanje pogrešaka.* Modul Python trenutno podržava IDE značajke kao što su intellisense i ispravljanje pogrešaka. Osim toga, ne uspijete modul prilikom izvođenja, cijeli Praćenje stoga Python dostupna, ali morate je prikazati u zapisniku izlaz za modul. Trenutno preporučujemo da razviti i njihove Python skripte u okruženju kao što su IPython za ispravljanje pogrešaka i zatim uvesti kod modula.
3.  *Jedan podatkovni okvir izlaz.* Da biste se vratili u okvir jedan podatkovni kao rezultat dopušteno samo Python točku unosa. Nije trenutno moguće da biste se vratili proizvoljne Python objekte kao što su izravno obučeni modeli izvođenja Azure strojnog učenja. [Izvršavanje R skripte]kao što su[execute-r-script]koje imaju isti ograničenje, je ipak moguće u mnogim slučajeva pickle objekata u bajt polja, a zatim vrati te unutar okvira podataka.
4.  *Nemogućnost Prilagodi Python instalaciju*. Trenutno je jedini način da biste dodali module prilagođene Python putem mehanizam datoteke zip ranije. Dok je izvedivo za small module, je naporan za velike Module (osobito one s nativni DLL) ili velikog broja module. 


##<a name="conclusions"></a>Zaključaka
[Izvršavanje skripte Python] [ execute-python-script] modul omogućuje podataka pozvana postojeći kod Python ugraditi u tijekove rada oblaka hostira strojnog učenja u Azure strojnog učenja i jednostavno operationalize ih kao dio web-servisa. Modul za skripte Python interoperates prirodan s druge module u Azure strojnog učenja i mogu se koristiti za raspon zadatke iz Istraživanje podataka na stara obradu, za izdvajanje značajki za procjenu i nakon obrade rezultata. Izvođenje pozadinskog koristi za izvršavanje temelji se na Anaconda, dobro provjeriti i široku Python distribuciju. Time se omogućuje jednostavno umjesto vas ploči postojeće imovini kod u oblak.

Očekivanog radi dodatnih funkcionalnosti [Izvršavanje skripte Python] [ execute-python-script] modul kao što je obuka i operationalize modelima u Python i da biste dodali bolje podrške za razvoj i ispravljanje pogrešaka kod u Azure strojnog učenja Studio.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Python](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
