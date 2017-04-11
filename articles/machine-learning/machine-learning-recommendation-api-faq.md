<properties 
    pageTitle="Postavljanje i korištenje API za preporuke strojnog učenja | Microsoft Azure" 
    description="Microsoft PREPORUKE API načinjene pomoću Azure strojnog učenja najčešća pitanja vezana uz" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Postavljanje i korištenje strojnog učenja preporuke API najčešća pitanja o


**Što je PREPORUKE?**

>[AZURE.NOTE]Trebali biste početi koristiti Kognitivne servisa za preporuke API umjesto ovu verziju. Servis za preporuke za Kognitivne će zamjene taj servis, a sve nove značajke će razvio postoji. Ima nove mogućnosti kao što su grupnog slanja promjena podršku, bolje Explorer API-JA, izvlačenje, dosljedan prijava/naplata sučelje za čišćenje API, itd.
> Dodatne informacije o [migraciji nove Kognitivne usluge](http://aka.ms/recomigrate)

Organizacije i tvrtke koje ovise o preporuke za unakrsno pošiljke i gore pošiljke proizvodi i servisi za svoje kupce za PREPORUKE u Azure strojnog učenja predstavlja samoposlužnog preporuke modul. Implementacija suradnju filtriranja koji koristi factorization matricu kao njegov algoritam temeljni je. Razvojnim inženjerima mogu pristupati PREPORUKE pomoću REST API-ji. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Što učiniti s PREPORUKE?**

PREPORUKE uzima kao unos stavku ili skup stavki i vraća popis relevantnih preporuke. Na primjer: klijent online prodavača klikne proizvoda. Online prodavača šalje taj proizvod kao ulaz PREPORUKE, može vidjeti popis proizvoda u povrat i odlučuje koju od tih proizvoda koji se prikazuju kupcu. Preporučujemo vam da koristite PREPORUKE za optimiziranje internetske trgovine ili čak i obavijestiti vaš unutar prodaje odjela ili poziv centar.

**Postoje li ograničenja korištenja?**

Preporuke ima sljedeća ograničenja korištenja:
* Maksimalni broj modela po pretplati: 10
* Maksimalan broj stavki koje mogu sadržavati kataloga: 100,000
* Najveći broj točaka korištenje koje se zadržavaju se ~ 5,000,000. Najstarija izbrisat će se ako nove će se prenijeti ili prijavio.
* Maksimalna veličina podataka koje se mogu slati e-pošte (na primjer, Uvoz kataloga podataka, uvoz podataka o korištenju) je 200 MB
* Broj transakcije sekundi (TPS) za sastavljanje za preporuke model koji je aktivan je TPS ~ 2. Sastavljanje za preporuke model koji je aktivan može sadržavati do 20 TPS.

##<a name="purchase-and-billing"></a>Za kupnju i naplata 


**Koliko stoji preporuke troškova tijekom razdoblja pokretanje?**

Preporuke je servis za pretplatu. Puni temelji se na količinu transakcije mjesečno. Možete provjeriti [stranice ponuda] (https://datamarket.azure.com/dataset/amla/recommendations) u programu Microsoft Azure Marketplace za informacije o cijenama.

**Postoje li sve troškove vezane uz pojavljuju preporuke praćenje i pohranu aktivnosti korisnika?**

Ne koristilo.

**Mora li preporuke besplatnu probnu verziju?**

Postoji besplatne trag koji je ograničeno na 10 000 transakcije mjesečno.

**Kada će se naplatiti za preporuke?**

Plaćenu pretplatu je sve pretplate za koju je mjesečnu naknadu. Kada kupite plaćenu pretplatu, vam se naplatiti odmah za korištenje u prvom mjesecu. Vam se naplatiti iznos koji je povezan s ponudu na stranicu pretplate (plus primjenjivo poreza). Trošak mjesečni postala je svakog mjeseca na istom kalendarski datum kao izvornu kupnju dok se otkazati pretplatu. 

**Kako se nadograditi na višu sloju servis?**

Možete kupiti ili obnoviti pretplatu [stranice ponuda] (https://datamarket.azure.com/dataset/amla/recommendations) stranice na web-servisa Microsoft Azure Marketplace.

Pri nadogradnji pretplate:

* Ne dodaju transakcije koje su preostali stare pretplate na novu pretplatu. 
* Platiti cijelog cijenu za novu pretplatu, čak i ako imate Neiskorišteni transakcije stare pretplate.

Postupak nadogradnje pretplate:

* Nevigate stranicu [ponudu] (https://datamarket.azure.com/dataset/amla/recommendations).
* Prijavite se u trgovinu ako već niste prijavljeni.
* U desnom oknu navedene su dostupne tarife. Kliknite izborni gumb za nadogradnju na željenu tarifu.
* Ako želite izvršiti nadogradnju, kliknite **u redu**. Ako ne želite nadograditi, kliknite **Odustani**.

**Važno** Pažljivo pročitajte dijaloški okvir prije nadogradnje jer je naplata i korištenje posljedice.

**Kad će završiti pretplate za preporuke?**

Vaša pretplata dogodit će se ako je otkažete. Želite li za otkazivanje pretplate, pogledajte sljedeće upute.

**Kako otkazati pretplatu preporuke?**

Da biste otkazali pretplatu, poduzmite sljedeće korake. Ako je trenutne pretplate na plaćenu pretplatu, pretplate nastavlja na snazi do kraja trenutno razdoblje naplate. Ako vam je potrebna pretplata otkazana stupila na snagu odmah, obratite nam na [Microsoftovoj službi za podršku](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Napomena** Ako odustanete prije završetka razdoblja za naplatu ili za koje se ne koriste transakcije u razdoblju naplate, izražen je bez povrat novca.

* Otvorite stranicu sustava [ponudu] (https://datamarket.azure.com/dataset/amla/recommendations).
* Prijavite se u trgovinu ako već niste prijavljeni.
* Kliknite **Odustani** da biste s desne strane naziv skupa podataka i status. Možete koristiti ovu pretplatu do kraja trenutno razdoblje naplate ili ograničenje transakcije zbirka dostigne (ovisno o tome što će se).

Ako želite da biste otkazali pretplatu odmah tako da kupite novu pretplatu, datoteka na karata na [Microsoftovoj službi za podršku](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Uvod u preporuke

**Da biste je preporuke za mene?** 

Preporuke u strojnog učenja odnosi se na web-mjesto tvrtke ili ustanove i u okvir za tvrtke koje ovise o preporuke za unakrsno pošiljke i gore Prodaja proizvoda ili usluga svojim korisnicima. Ako imate na korisnike web-mjesta dostupnog, prodajni tim, unutarnji prodajno ili centar za pozive i ako nudi katalog više od nekoliko dozen proizvoda ili usluga, donja crta možda prednosti korištenja preporuke. 

Eksperimentiranja s preporuke namijenjen je prilično jednostavno. Trenutni API-bitna verzija zahtijeva osnovne vještine programiranje. Ako vam je potrebna pomoć, obratite se dobavljača koji je razvio web-mjesta. Ako imate Interna IT odjel ili sesije programiranje, trebali biste moći preporuke za radi za vas. 

**Što su preduvjeti za postavljanje preporuke?**

Preporuke mora biti zapisnik korisnika mogućnosti kao što je to povezano s katalog. Ako t imati takve zapisnik i imate kupca dostupnog web-mjesto, preporuke možete prikupiti aktivnosti korisnika za vas. 

Preporuke zahtijeva i kataloga proizvoda ili usluga. Ako ne t imati kataloga, preporuke pomoću podataka o korištenju stvarni klijent i destiliranje kataloga. Katalog izričitu neće sadržavati stavke koje su se prijavili kao dio transakcije korisnika.

**Kako postaviti preporuke za prvo?**

Nakon [pretplata] (https://datamarket.azure.com/dataset/amla/recommendations) za preporuke, biste trebali koristiti dokumentaciju API [Azure strojnog učenja preporuke Brzi vodič za početak rada](machine-learning-recommendation-api-quick-start-guide.md) da biste postavili servis.

**Gdje pronaći dokumentaciju API-JA?** 

Dokumentacija API je [Azure strojnog učenja preporuke Brzi vodič za početak rada](machine-learning-recommendation-api-quick-start-guide.md).

**Mogućnosti koje ste da biste prenijeli kataloga i korištenje podataka na preporuke?**

Imate dvije mogućnosti za prijenos kataloga i korištenje podataka: možete izvesti podatke iz sustava CRM ili druge zapisnika, a zatim prijenos preporuke ili oznake možete dodati na web-mjesto na koje ćete moći pratiti aktivnosti korisnika. Ako koristite potonjem način, podaci će se spremati Azure.

##<a name="maintenance-and-support"></a>Održavanje i podršku

**Kako velike Moje skup podataka može?**

Svaki skup podataka može sadržavati do 100 000 stavki kataloga i prema gore na 2048 MB podataka o korištenju.
Pretplate može sadržavati i do 10 skupa podataka (modela).

**Gdje pronaći tehničke podrške za preporuke?**

Tehnička podrška dostupna je na web-mjestu za [Podršku za Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Gdje pronaći Uvjeti korištenja?**

[Microsoft Azure strojnog učenja preporuke API uvjete pružanja usluge](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
