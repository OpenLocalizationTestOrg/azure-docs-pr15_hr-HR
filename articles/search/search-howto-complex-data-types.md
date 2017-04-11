<properties
    pageTitle="Kako se model vrstama složenih podataka u pretraživanju Azure | Pretraživanje sustava Microsoft Azure"
    description="Ugniježđene funkcije ili katalog strukture hijerarhijske podatke možete modeliran u indeks pretraživanja Azure pomoću plošnu skup redaka i vrsta zbirke podataka."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Kako se model vrstama složenih podataka u pretraživanju Azure

Vanjski skupove podataka koristi za popunjavanje indeks pretraživanja Azure ponekad obuhvaćaju hijerarhijski ili ugniježđene substructures koji ne raščlanjuju jednostavno u tabličnom skup redaka. Primjeri takvih strukture možda obuhvaćaju više mjesta i telefonski brojevi za jednog klijenta, više boja i veličina za jednu SKU više autora u jednom pojavit će se i tako dalje. U Modeliranje uvjeta, možda će strukture te koje se nazivaju i *vrstama složenih podataka*, *zbirni vrste podataka*, *vrstama složenih podataka*ili *aggregate vrste podataka*, nekoliko.

Vrstama složenih podataka nativno nisu podržane u pretraživanju Azure, ali dokazana zaobilazno rješenje obuhvaća dva koraka od stapanja strukturu, a zatim koristite **zbirke** vrstu podataka da biste reconstitute unutrašnjosti strukturu. Slijedeći postupak opisan u ovom članku omogućuje sadržaja za pretraživanje, slojevito, filtriranja i sortiranja.

## <a name="example-of-a-complex-data-structure"></a>Primjeri složenih podataka strukture

Obično se u pitanju nalaze podaci kao skup JSON ili XML dokumenata ili stavki u spremištu NoSQL kao što su DocumentDB. Strukturno, problem stems onemogućite više podređenih stavki koje su potrebne za pretraživanja i filtriranja.  Kao početnu točku za ilustraciju rješenje, poduzmite sljedeće JSON dokument koji navodi skup kontakata kao primjer:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Dok polja pod nazivom "id", "naziv" i "tvrtke" možete jednostavno je mapirati ikone kao polja u indeks pretraživanja Azure, polje 'mjesta' sadrži polje mjesta koji se pojavljuju oba skupa mjesto ID-a, kao i opise mjesto. Given da Azure pretraživanje ne sadrži vrstu podataka koja je to podržava, moramo drugi način za to modela u pretraživanju Azure. 

> [AZURE.NOTE] Ovu tehniku i opisane pomoću Kirk Evans u unosu u blog [Indeksiranje DocumentDB pomoću Azure pretraživanja](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), koji prikazuje tehnika pod nazivom "stapanja podatke," whereby promijenile poljem pod nazivom `locationsID` i `locationsDescription` koje su i [zbirki](https://msdn.microsoft.com/library/azure/dn798938.aspx) (ili polja nizova).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Dio 1: Stopi polja u pojedinačna polja

Da biste stvorili indeks pretraživanja Azure koji odgovara ovaj skup podataka, stvorite pojedinačna polja za ugniježđene podstrukturnih: `locationsID` i `locationsDescription` s vrstom podataka [zbirke](https://msdn.microsoft.com/library/azure/dn798938.aspx) (ili polja nizova). U ovim poljima želite indeksirati vrijednosti "1" i "2" u na `locationsID` polja za Ivica Novak i vrijednosti '3' i '4' u `locationsID` za Jen Campbell.  

Podataka unutar Azure pretraživanja izgleda ovako: 

![Ogledni podaci, 2 retka](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Dio 2: Dodavanje zbirke polja u definicija indeksa

U shemi indeks definicije polja može izgledati slično kao u ovom primjeru.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Provjerite valjanost ponašanja pretraživanja i po želji proširivanje indeksa

Pod pretpostavkom da ste stvorili indeks i učitavanja podataka, možete odmah testirati rješenja da biste provjerili izvršavanje upita za pretraživanje na temelju skupu podataka. Svako polje **zbirke** treba **pretraživati**, **koje je moguće filtrirati** i **facetable**. Trebali biste moći pokretanje upita kao što su:

* Pronalaženje svih osoba koje rade na "Sjedišta Adventureworks".
* Brojanje broj osoba koje rade u "Polazno Office".  
* Osoba koje rade u "Polazno Office", prikažite koje ureda funkcioniraju zajedno s ukupan broj osoba na svakom mjestu.  

Gdje pada ovu tehniku razlike je kada morate učiniti pretraživanja koje kombinira i id lokacije, kao i opis lokacije. Ako, na primjer:

* Pronaći sve osobe kojima imaju Polazno Office i ima ID lokacije 4.  

Ako je opozvati izvornog sadržaja pokušavali ovako:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Međutim, nakon što smo ste razdvojeni podatke u zasebnim poljima, imamo nije moguće znati ako Home Office za Jen Campbell odnosi `locationsID 3` ili `locationsID 4`.  

Rukovanje taj slučaj, definirajte drugo polje u indeks koji spaja sve podatke u jednu zbirku.  Za našeg primjera ćemo podići ovo polje `locationsCombined` i odvojiti sadržaja pomoću programa `||` Iako možete odabrati sve razdjelnik koji mislite da bi bio Jedinstveni skup znakova za sadržaj. Ako, na primjer: 

![Ogledni podaci, 2 retka s razdjelnika](./media/search-howto-complex-data-types/sample-data-2.png)

Korištenje to `locationsCombined` polja, ne možemo možete sada bi odgovarao još više upita, kao što su:

* Prikaži broj osoba koje rade s 'Polazno sustava Office"s lokacijom Id"4".  
* Traženje osoba koje rade na "Polazno Office" s lokacijom Id "4". 

## <a name="limitations"></a>Ograničenja

Ovu tehniku je korisno za broj scenariji, ali nije primjenjivo svaki velikim slovom.  Ako, na primjer:

1. Ako nemate statične skup polja u vrstu složenih podataka, a nije bilo načina za mapiranje sve moguće vrste jedno polje. 
2. Ažuriranje ugniježđene objekata zahtijeva neke dodatni posao da biste odredili točno što je potrebno ažurirati u indeksu pretraživanja Azure

## <a name="sample-code"></a>Ogledni kod

Primjer možete vidjeti kako indeksirati složene JSON skupa podataka u pretraživanje Azure i izvođenje broj upita ovaj skup podataka na [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Sljedeći korak

[Glasovanja Ugrađena podrška za vrstama složenih podataka](https://feedback.azure.com/forums/263029-azure-search) na UserVoice pretraživanje Azure stranice i navedite dodatni unos koji želite nam treba uzeti u obzir vezane uz značajku implementacije. Koje možete i stupili meni izravno na Twitteru na @liamca.


 