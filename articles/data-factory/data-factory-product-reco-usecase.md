<properties 
    pageTitle="Korištenje predmeta tvorničke podataka – preporuke proizvoda" 
    description="Saznajte više o slučaj koristi implementiran putem tvorničke Azure podataka zajedno s ostalim servisima." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Korištenje predmeta – preporuke proizvoda 

Azure tvorničke podataka jedan je od mnoge servise za implementaciju paket obavještavanje Cortana ubrzivača rješenja.  Potražite u članku [Cortana obavještavanje paket](http://www.microsoft.com/cortanaanalytics) stranice pojedinosti o ovom paket. U ovom dokumentu, ne možemo opisuju uobičajena slučaja korištenje Azure korisnici već otkloniti i implementirati pomoću tvorničke Azure podataka i druge usluge komponenti obavještavanje Cortana.

## <a name="scenario"></a>Scenarij

Online trgovaca na malo najčešće želite potaknuti svoje kupce kupiti proizvodi tako da ih prezentaciju s proizvodima koje su vjerojatno će vas zanimati i stoga najvjerojatnije kupiti. Da biste to postigli, online trgovaca na malo morate li prilagoditi svojim korisničkog sučelja za online pomoću prilagođenih proizvoda preporuke za tog korisnika određene. Te preporuke personalizirane je potrebno utemeljene na svom trenutni i prošli košarice podaci o ponašanju, informacije o proizvodu, upravo uvedeni marke i korisničkoj segmente podataka.  Osim toga, možete unijeti preporuke proizvoda korisnika na temelju analize ponašanja cjelokupan korištenje svih njihove korisnika koji se spajaju.

Cilj te lanaca je optimizirati za pretvorbe kliknite Prodaja korisnika, a zatim stjecanje veći prihod od prodaje.  Oni postignete pretvorbe izlaganja preporuke kontekstne, ponašanje proizvoda koji se temelji na interesima kupaca i akcije. Za taj slučaj koristi kao primjer tvrtki koje se želite optimizirati za svoje kupce koristimo online trgovaca na malo. Međutim, tih načela primjenjuju se na sve poslovne koji želi sudjelovati svojih klijenata oko svojih proizvoda i usluga i poboljšavaju svoje kupce kupnje s preporuke personalizirane proizvoda.

## <a name="challenges"></a>Izazove

Postoje mnoge izazove te nominalne online trgovaca na malo prilikom pokušaja implementirati ovu vrstu koristi slučaj. 

Najprije se podaci s različitim veličinama i oblicima mora ingested iz više izvora podataka, obje lokalnog i u oblaku. Ove podatke sadrži podatke o proizvodu, podaci o ponašanju povijesne klijenta i korisničkih podataka kao što je korisnik traži internetska maloprodaja web-mjesta. 

Preporuke za drugi personalizirane proizvoda koji se moraju biti razumno te točno izračunati i predviđene. Uz proizvod, marke i podatke o klijentu ponašanje i preglednik online trgovaca na malo ćete morati uključiti povratnu informaciju na zadnjih Nabava da biste faktor u određivanja najbolje proizvoda preporuke za korisnika. 

Treći, preporuke mora biti odmah rezultata korisniku objedinjenog pregledavanja i kupnju sučelje i pružanja preporuke nedavne i relevantnim oznakama. 

Na kraju, trgovaca na malo morati mjerenje učinkovitosti njihove pristup praćenjem Ukupna prodaja gore unakrsno pošiljke kliknite pretvorbe prodaje uspjeha i prilagoditi svojim buduće preporuke.

## <a name="solution-overview"></a>Pregled rješenja

Ovaj primjer koristi slučaj otkloniti je i implementirati korisnici real Azure pomoću tvorničke Azure podataka i ostale servise obavještavanje Cortana komponente, koji je uključujući [HDInsight](https://azure.microsoft.com/services/hdinsight/) i [Power BI](https://powerbi.microsoft.com/).

Online prodavača koristi u spremište blobova platforme Azure programa lokalnog sustava SQL server, baze podataka SQL Azure te relacijskih podataka čavanje kao njihove mogućnosti pohrane podataka u tijeku rada.  Spremište blobova platforme sadrži informacije o klijentu, ponašanje podatke o klijentu i podatke informacije o proizvodu. Proizvod kataloga pohranjuju lokalno u SQL warehouse podataka i podatke informacije o proizvodu sadrži informacije o proizvodu marku. 

Svi podaci se kombinirati i umeće proizvoda sustava preporuke izlaganje personalizirane preporuke na temelju interesima kupaca i Akcije, dok korisnik traži proizvodi u katalogu web-mjesta. Klijenti i potražite u članku proizvode koje su vezane uz proizvod su pogledate na temelju uzoraka korištenja cjelokupnog web-mjesta koji se odnose na bilo kojem jednog korisnika.

![Korištenje Dijagram predmeta](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabajta datoteka zapisnika neobrađenog web generiraju se svakodnevno s web-mjesta online prodavača kao djelomično strukturirani datoteke. Zapisničke datoteke neobrađenog web i kataloga informacije klijenta i proizvod je ingested redovito u programa Azure blobova pomoću premještanje podataka tvorničke globalno distribuiranih podataka kao usluga. Neobrađenog zapisničke datoteke za dan imaju (po godini i mjesecu) u spremište blobova platforme za Dugoročne prostora za pohranu.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) koristi se za particija neobrađenog zapisničke datoteke u spremištu blobova platforme i obrada zapisnike ingested na razini korištenje grozd i Svinja skripti. Particioniranom web zapisnike podataka zatim obrađuju da biste izdvojili potrebne unosa za strojnog učenja sustava preporuke za generiranje preporuke personalizirane proizvoda.

Preporuka sustav za strojnog učenja u ovom primjeru je se Otvori izvor strojnog učenja preporuke platformu iz [Apache Mahout](http://mahout.apache.org/).  Bilo koji [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) prilagođene model možete zatvoriti ili scenarij.  Mahout model koristi se za predviđanje sličnosti između stavki na temelju cjelokupan uzoraka korištenja web-mjestu, a da biste generirali personalizirane preporuke temeljene na pojedinim korisnicima.

Na kraju, skup rezultata za preporuke personalizirane proizvoda se premješta u relacijskih podataka čavanje za potrošnju prodavača web-mjesta.  Skup rezultata nije također biti pristupiti izravno iz spremišta blobova neku drugu aplikaciju ili premještene dodatne služi za pohranu za drugi korisnici i korištenje slučajeve.

## <a name="benefits"></a>Prednosti

Optimiziranje njihove strategije preporuke proizvoda i poravnavanje s poslovnim ciljevima, rješenje zadovoljeni merchandising online prodavača i marketing ciljeva. Uz to, su mogućnost operationalize i upravljanje tijeka rada za preporuke proizvoda učinkovitog, pouzdanog i troškovima učinkovit način. Pristup grupa postala računala u kojem će se ažurirati njihove modela i Prilagodba njegov učinkovitosti na temelju mjera prodajne uspjeha pretvorbe kliknite. Pomoću Azure podataka tvorničke su bile moći ćete njihove Upravljanje resursima dosta vremena i pozivnim ručno oblaka, a zatim premjestite Upravljanje resursima oblak na zahtjev. Stoga su možete spremiti novac, vrijeme i smanjivanje njihove vremena za implementacije rješenja. Podaci o podrijetlu prikaza i stanje servisa radu postala lako vizualni prikaz i otklanjanje poteškoća s intuitivno podataka tvorničke nadzor i upravljanje dostupna na portalu Azure korisničkog Sučelja. Njihova rješenja možete sada zakazano i upravlja tako da dovršeni podataka pouzdano proizvodi i isporučena korisnicima te podatke i obrada ovisnosti automatski upravljati bez Ljudski intervencije.

Unosom personalizacije kupovinu online prodavača stvorili više konkurencije, privlačne kupca iskustva i stoga povećajte zadovoljstvo prodaje i cjelokupan klijenata.



  