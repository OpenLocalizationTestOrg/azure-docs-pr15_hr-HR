<properties
    pageTitle="Lucene Primjeri upita za pretraživanje Azure | Pretraživanje sustava Microsoft Azure"
    description="Lucene sintaksa upita za pretraživanje u mutno, Blizina pretraživanja, povećanje termina, uobičajeni izraz pretraživanja i pretraživanje zamjenskih znakova."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Primjeri sintakse Lucene upita za sastavljanje upita u pretraživanju Azure

Tijekom izgradnje upita za pretraživanje Azure, možete koristiti kao zadanu [sintaksu upita za jednostavne](https://msdn.microsoft.com/library/azure/dn798920.aspx) ili zamjenski [Lucene analizator upit u pretraživanju Azure](https://msdn.microsoft.com/library/azure/mt589323.aspx). Analizator upita Lucene podržava složenije stavki za upit, kao što su polja opsegu upita, mutno pretraživanja, Blizina pretraživanja, povećanje termina i reqular izraz pretraživanja.

U ovom se članku korak primjera koje se prikazuju Lucene sintaksu upita i rezultate usporedno. Primjeri pokrenuti unaprijed učitani indeks pretraživanja u [JSFiddle](https://jsfiddle.net/), uređivač online kod testiranja skripte i HTML. 

Desnom tipkom miša kliknite upit primjer URL-ove da biste otvorili JSFiddle u zasebnom prozoru preglednika.

> [AZURE.NOTE] Sljedeći primjeri pod utjecajem indeks pretraživanja na koji se sastoji od poslove dostupne na temelju dataset nudi inicijativa [Grad New York OpenData](https://nycopendata.socrata.com/) . Ti podaci ne treba smatrati trenutnu ili dovršeno. Indeks je pruža Microsoft servis za testiranje. Ne morate Azure pretplatu ili Azure pretraživanja možete pokušati tih upita.

## <a name="viewing-the-examples-in-this-article"></a>Prikaz se primjerima u ovom članku

Sve se primjerima u ovom članku odredite analizator upita Lucene putem pretraživanja parametar**queryType** . Kada koristite analizator upita Lucene iz koda, **queryType** ćete navesti na svaki zahtjev.  Valjane vrijednosti obuhvaćaju **jednostavne**|**puni**s **jednostavne** kao zadane i **cijeli** za upit analizator Lucene. Pojedinosti o parametrima upita potražite u članku [Pretraživanje dokumenata (Azure pretraživanje servisa REST API -JA)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

**Primjer 1** – desnom tipkom miša kliknite isječak da biste ga otvorili u novu stranicu preglednika koja se učitava JSFiddle i izvodi upit za upite na sljedeći način:
- [& queryType = potpuni & pretraživanje = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Ovaj upit vraća dokumente iz naših poslove indeksa (učitati servis za testiranje)

U novom prozoru preglednika, vidjet ćete JavaScript izvorni i HTML izlaz usporedno. Skripta poziva upit, koji pruža primjer URL-ovi u ovom članku. Na primjer, na **primjer 1**, upita u pozadini je na sljedeći način:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Obavijest o upit koristi konfiguriranog indeks pretraživanja za Azure pod nazivom nycjobs. Parametar **searchFields** ograničava pretraživanje samo u polje naslov tvrtke. **QueryType** postavljen je na **cijeli**koji upućuje Azure pretraživanja da biste koristili analizator upita Lucene za ovaj upit.

### <a name="fielded-query-operation"></a>Operacija fielded upita

Primjeri u ovom se članku možete izmijeniti navođenjem izgradnju **fieldname:searchterm** da biste definirali operacije fielded upita, pri čemu je polje jednu riječ, a pojam za pretraživanje i jednu riječ ili izraz, po želji s logički operatori. Primjeri obuhvaćaju sljedeće:

- business_title:(senior NOT junior)
- Država: ("Zagreb" i "Nova Jersey")

Ne zaboravite da biste umetnuli više nizovi unutar navodnika ako želite da se oba nizovi koji se procjenjuje kao jedan entitet, kao u ovom slučaju traženje dva različita gradova u polju mjesto. Također provjerite je li operator je slovom kao što vidite s ne i AND.

Polje naveden u **fieldname:searchterm** mora biti polje pretraživanja. Informacije o korištenja atribute indeksa u definicije polja potražite u članku [Stvaranje indeksa (Azure pretraživanje servisa REST API -JA)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Mutno pretraživanja

Mutno pretraživanje pronalazi podudaranja rječnikom koji imaju slične izgradnju. Po [Lucene dokumentaciju](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)mutno pretraživanja temelje se na [Damerau Levenshtein udaljenost](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Izvršite mutno pretraživanje pomoću na tilda "~" simbol na kraju jedne riječi s neobavezan parametar, vrijednost između 0 i 2, koja određuje udaljenost uređivanje. Ako, na primjer, "plavo ~" ili "plavom ~ 1" vratiti Plava, blues i lijepljenje.

**Primjer 2** – desnom tipkom miša kliknite sljedeće isječak upita da biste isprobajte sami. Ovaj upit traži naslove tvrtke s više termina u im, ali ne niži:

- [& queryType = potpuni & pretraživanje = business_title:senior ne niži](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Blizina pretraživanja

Blizina pretraživanja koji se koristi za pronalaženje uvjeta koje su u blizini međusobno u dokumentu. Umetanje na tilda "~" simbol na kraju izraz slijedi broj riječi koje stvorite Blizina granicu. Na primjer, "hotelski zračna luka" ~ 5 tražit će hotelski uvjete i zračna luka unutar 5 riječi međusobno u dokumentu.

**Primjer 3** – desnom tipkom miša kliknite sljedeće isječak upita. Ovaj upit traži poslove s Pridruži termina (gdje je pogrešno napisana):

- [& queryType = potpuni & pretraživanje = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Primjer 4** – desnom tipkom miša kliknite upit. Traženje poslove s termina "viši analitičar" gdje ih razdvojite točkom više od jedne riječi:

- [& queryType = potpuni & pretraživanje = business_title: "viši analitičar" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Primjer 5** – pokušajte ponovno uklanjanje riječi između "viši analitičar" termina.

- [& queryType = potpuni & pretraživanje = business_title: "viši analitičar" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Termina Boosting

Povećanje termina se odnosi na rangiranje veća ako sadrži boosted pojam odnosu dokumente koji sadrže izraz dokumenta. Razlikuje se od bilježenje rezultata profila iz tog bilježenja rezultata profili povećajte određenih polja, umjesto određenim pojmovima. U sljedećem primjeru olakšava ilustracija razlike.

Razmislite o bilježenja rezultata profil koji se pojačava odgovara određenim polja, kao što su **stupca** u primjeru musicstoreindex. Povećanje izraz može koristiti da biste dodatno uvjete veća od ostalih uvećali određene pretraživanja. Na primjer, "rock ^ 2 elektronička" dokumente koji sadrže pojmova za pretraživanje u polje **stupca** veće od drugih pretraživanja polja u indeks će se povećati. Osim toga, dokumente koji sadrže izraz za pretraživanje "rock" će biti rangiranih veća od drugih izraz za pretraživanje "elektroničke" uslijed pojačavanje vrijednost termina (2).

Povećajte termina, koristiti karet, "^", simbol uz faktor pojačavanje (broj) na kraju razdoblja pretražujete. Veći faktor Pojačavanje, više odgovarajućih termina bit će odnosu druge pojmove za pretraživanje. Prema zadanim postavkama faktor pojačavanje je 1. Iako faktor pojačavanje mora biti pozitivan, to može biti manji od 1 (na primjer 0,2).

**Primjer 6** – desnom tipkom miša kliknite upit. Traženje poslove s termina "računalo analitičar" gdje Vidimo nema rezultata s računala riječi i analitičar još analitičar poslovi su pri vrhu rezultata.

- [& queryType = potpuni & pretraživanje = business_title:computer analitičar](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Primjer 7** – pokušajte ponovno, povećanje vrijeme rezultata s računalom termina putem analitičar termina ako obje riječi ne postoji.

- [& queryType = potpuni & pretraživanje = business_title:computer ^ 2 analitičar](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Uobičajeni izraz

Uobičajeni izraz pretraživanje pronalazi match na temelju sadržaja između kose "/", kao što je navedeni u [predmete RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Primjer 8** – desnom tipkom miša kliknite upit. Traženje poslove s bilo termina više ili osnovnu i srednju.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

URL-a u ovom primjeru će pravilno prikazati na stranici. Kao zaobilazno rješenje, kopirajte URL u nastavku i zalijepite ga u pregledniku URL adresa:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Pretraživanje zamjenskih znakova

Koristite obično prepoznati sintaksa za više (\*) ili jedan pretraživanja za zamjenskih znakova (?). Imajte na umu analizator upita Lucene podržava korištenje ove simbole s jednom termina, a ne izraz.

**Primjer 9** – desnom tipkom miša kliknite upit. Pretraživanje za zadatke koje sadrže prefiks "prog", koji sadrži naslove tvrtke s uvjetima programiranje i programerskom u njoj.

- [& queryType = cijelog & $select = business_title & pretraživanje = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Ne možete koristiti u * ili? Simbol kao prvi znak pretraživanja.


## <a name="next-steps"></a>Daljnji koraci

Pokušajte navesti analizator upita Lucene u kodu. Na sljedećim vezama objašnjavaju kako postaviti upita za pretraživanje za .NET i REST API-JA. Veze pomoću kao zadanu sintaksu za jednostavne bit će potrebno da biste primijenili ono što ste naučili iz ovog članka da biste odredili **queryType**.

- [Indeks pretraživanja Azure pomoću .NET SDK za upite](search-query-dotnet.md)
- [Indeks pretraživanja Azure pomoću REST API-JA za upite](search-query-rest-api.md)


 