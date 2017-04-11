<properties
   pageTitle="Ugrađene Microsoft Power BI – povezivanje s izvorom podataka"
   description="Power BI ugrađene, povezivanje s izvorima podataka"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Povezivanje s izvorom podataka

Pomoću **Dodatka Power BI ugrađene**, moguće je ugraditi izvješća u vlastiti aplikaciju. Kad umetnete izvješća servisa Power BI u aplikacije, izvješće povezuje podatke u podlozi tako da **uvezete** kopiju podataka ili **izravno povezivanje** s izvorom podataka pomoću **DirectQuery**.

Ovdje su razlike između korištenja **Uvoz** i **DirectQuery**.

|Uvoz | DirectQuery
|---|---
|Tablice, stupce *i podataka iz* uvezene ili kopirali u skupu podataka u izvješću. Da biste vidjeli promjene da biste podatke u podlozi, morate osvježiti ili uvoz dovršen, trenutni skup podataka ponovno.|*Tablica i stupaca* uvesti ili kopirali u skupu podataka u izvješću. Uvijek vidjeti najnovije podatke.
Pomoću dodatka Power BI ugrađene koje možete koristiti DirectQuery s izvorima podataka oblaka ali ne lokalnim izvorima podataka trenutno.

## <a name="benefits-of-using-directquery"></a>Prednosti korištenja DirectQuery

Postoje dvije primarni pogodnosti prilikom korištenja **DirectQuery**:

   -    **DirectQuery** omogućuje vam stvaranje vizualizacija putem vrlo velike skupove podataka, gdje u suprotnom će biti unfeasible za uvoz prve svi podaci.
   -    Podlozi promjene podataka možete zatražiti osvježavanja podataka, a za neke izvješća, morate prikazati trenutne podatke možete zatražiti prijenosi velike podataka, čime se unfeasible ponovno uvoz podataka. Za razliku od toga **DirectQuery** izvješća uvijek koristi trenutne podatke.

## <a name="limitations-of-directquery"></a>Ograničenja DirectQuery

   Postoji nekoliko ograničenja korištenja **DirectQuery**:

   -    Sve tablice mora potjecati iz jedne baze podataka.
   -    Ako je upit pretjerano složene, pojavit će se pogreška. Da biste riješili pogrešku morate refactor upit tako da bude manje složene. Ako na quuery mora biti složene, morat ćete uvoz podataka umjesto korištenja **DirectQuery**.
   -    Filtriranje odnos ograničeno je na jednom smjeru umjesto oba smjera.
   -    Ne možete promijeniti vrstu podataka stupca.
   -    Prema zadanim postavkama, ograničenja smještaju se na DAX izrazi dopušteni u mjere. Potražite u članku [DirectQuery i mjera](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery i mjera

Za upite koji se šalju u temeljnom izvoru podataka provjerite imaju li prihvatljiva performanse, ograničenja koje su na mjere. Kada pomoću **Dodatka Power BI Desktop**Napredni korisnici možete zaobići to ograničenje tako da odaberete **Datoteka > Mogućnosti i postavke > Mogućnosti**. U dijaloškom okviru **Mogućnosti** odaberite **DirectQuery**, a zatim odaberite željenu mogućnost **Dopusti neograničeni mjere u načinu rada DirectQuery**. Kada je odabrana ta mogućnost, bilo koji DAX izraz koji vrijedi za mjera može se koristiti. Korisnici moraju imati na umu; Međutim, da neki izrazi koji izvršiti vrlo dobro prilikom uvoza podataka može uzrokovati vrlo sporo upiti s izvorom pozadinskog kada se nalazite u načinu rada **DirectQuery** . 

## <a name="see-also"></a>Vidi također
- [Uvod u Microsoft Power BI ugrađeni](power-bi-embedded-get-started.md)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
