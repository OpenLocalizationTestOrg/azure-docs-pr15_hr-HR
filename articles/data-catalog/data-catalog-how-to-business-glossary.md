<properties
    pageTitle="Upute za postavljanje tvrtke Pojmovnik za mjerodavni označavanje | Microsoft Azure"
    description="S uputama članak isticanje Pojmovnik tvrtke Azure u katalogu podataka dodatka za definiranje i korištenje uobičajenih rječnik tvrtke oznaku registrirane imovine podataka."
    services="data-catalog"
    documentationCenter=""
    authors="steelanddata"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/21/2016"
    ms.author="maroche"/>

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Upute za postavljanje tvrtke Pojmovnik za mjerodavni označavanje

## <a name="introduction"></a>Uvod

Azure katalog podataka pruža mogućnosti za otkrivanje izvora podataka, koji korisnicima omogućuje jednostavno otkrivanje i razumijevanje izvori podataka koje su im potrebne za izvođenje analize i donošenje odluka. Ove mogućnosti otkrivanja provjerite najvećih utjecaj kada korisnici mogu pronaći i razumijevanje imalo raspon dostupnih izvora podataka.

Označavanje se jedna značajka katalog podataka koja promiče veći razumijevanje podataka resursi. Označavanje korisnicima omogućuje pridružiti sredstvo ili stupac, shodno lakše da biste otkrili resursa putem pretraživanje ili pregledavanje, ključne riječi i korisnicima omogućuje jednostavnije razumjeti kontekst i svrhu sredstava.

Međutim, označavanja ponekad može uzrokovati probleme vlastitog. Primjeri probleme koji mogu biti uvedene označavanje su:

1.  Korisnici koji se koriste kratica na neki resursi i proširenog teksta na drugi tijekom označavanje. Nedosljednosti ometa otkrivanje imovine čak i ako je namjerom da biste označili imovine s istom oznakom.
2.  Oznake koje znače različitih stvari u različitim konteksta. Ako, na primjer, oznake pod nazivom "Prihod" na skupu podataka kupca katkad znači prihod prema klijenta, ali istu oznaku na kvartalne prodaje dataset znači tromjesečni prihod za tvrtku.  

Da biste lakše adresa te i druge slične izazove, katalog podataka obuhvaća rječnika tvrtke.

Pojmovnik za tvrtke katalog podataka omogućuje tvrtke i ustanove moraju dokumenata ključnim poslovnim Uvjeti i definicije da biste stvorili uobičajene poslovne rječnik. U ovom upravljanja omogućuje dosljednost korištenja podataka u tvrtki ili ustanovi. Kada su uvjeti definirani u Pojmovnik tvrtke, oni se mogu dodijeliti imovine podataka u katalogu pomoću na isti način kao označavanje, kako bi _mjerodavni za označavanje_.

> [AZURE.NOTE] Funkcija opisane u ovom članku dostupne su samo u standardni izdanje programa Azure kataloga podataka. Besplatni Edition ne nudi mogućnosti za označavanje mjerodavni ili tvrtke rječnika.

## <a name="glossary-availability-and-privileges"></a>Pojmovnik dostupnosti i ovlasti

*Pojmovnik tvrtke dostupna je u standardni izdanje programa Azure kataloga podataka. Besplatni kataloga za Edition podataka ne uključuje rječnika.*

Pojmovnik tvrtke možete pristupiti putem mogućnosti "Pojmovnik" u katalog podataka portal navigacijski izbornik.  

![Pristup Pojmovnik tvrtke](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Administratori katalog podataka i Članovi uloge administratora Pojmovnik možete stvaranje, uređivanje i brisanje Pojmovnik uvjete Pojmovnik tvrtke. Svi korisnici katalog podataka možete pogledati definicije termina, a možete označavati imovine s uvjetima Pojmovnik.

![Dodavanje novog termina Pojmovnik](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Stvaranje Pojmovnik uvjeta

Administratori za katalog podataka i administratori Pojmovnik možete stvoriti nove uvjete Pojmovnik tako da kliknete na novom terminu ' gumb da biste stvorili Pojmovnik uvjete sljedeća polja:

* Definiciju tvrtke termina
* Opis koji snima namjena ili tvrtke pravila za resursa i stupca
* Popis zainteresiranih strana tko zna najviše termina
* Nadređenog termina koji definira organizirani termin hijerarhije


## <a name="glossary-term-hierarchies"></a>Pojmovnik termin hijerarhije

Pojmovnik tvrtke katalog podataka omogućuje opisuje vaš rječnik tvrtke kao hijerarhije uvjeta. Time se omogućuje tvrtke ili ustanove da biste stvorili klasifikacija uvjete koji bolje predstavlja taksonomije svoje tvrtke.

Naziv termina mora biti jedinstven na određenoj razini hijerarhije – dvostrukih naziva nije dopušteno. Nema ograničenja broja razine u hijerarhiji, ali hijerarhije je često više lako razumjeti kad postoje tri razine ili manje.

Korištenje hijerarhije u Pojmovnik tvrtke nije obavezno. Nadređenog ostavite prazno polje termina za uvjete Pojmovnik ćete stvoriti popis ravnomjerne (koji nisu-hijerarhijski) uvjeta u u rječniku.  

## <a name="tagging-assets-with-glossary-terms"></a>Označavanje imovine s uvjetima Pojmovnik

Kada se uvjeti Pojmovnik su definirani u katalogu, sučelje označavanja resursi optimiziran je za pretraživanje u rječniku dok korisnik upisuje svoje oznake. Portal za katalog podataka prikazuje se popis koji se podudaraju Pojmovnik uvjete za korisnika na raspolaganju. Ako korisnik odabere Pojmovnik termina s popisa, ona se dodaje imovine kao oznaku (poznatog Pojmovnik oznaka). Korisnik može odabrati i da biste stvorili novu tako da upišete pojam koji se ne nalazi u rječniku (poznatog oznaka korisnika).

![Podataka resursa označen jednom korisniku oznake i dvije Pojmovnik oznake](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Korisnik oznake su samo vrste oznake koje su podržane u besplatne izdanje sustava kataloga podataka.

### <a name="hover-behavior-on-tags"></a>Postavite pokazivač miša na oznake
Na portalu za katalog podataka vizualno različite, s različitim držanju ponašanja su dvije vrste oznake. Kada korisnik postavi pokazivač miša iznad oznake korisnika mogu vidjeti tekst oznake i korisnika ili korisnike koje ste dodali oznaku. Kada korisnik postavi pokazivač miša iznad oznake Pojmovnik, također vide definiciju Pojmovnik termina i vezu da biste otvorili rječnik tvrtke da biste prikazali cijeli definicija skupa termina.

### <a name="search-filters-for-tags"></a>Filtri pretraživanja za oznake
Pojmovnik oznake i oznake korisnika se mogu pretraživati, a možete se primjenjuju kao filtri u pretraživanju.

## <a name="summary"></a>Sažetak
Pojmovnik za tvrtke u katalog podataka Azure i governed označavanje omogućuje, omogućuju imovine podataka biti označena, upravlja i otkrio dosljedan način. Pojmovnik tvrtke mogu promovirati učenje rječnik tvrtke između korisnika tvrtke ili ustanove te podržava smisleni meta-podataka zabilježene, upućivanje resursa otkrivanje i razumijevanje na Povjetarac.

## <a name="see-also"></a>Vidi također

- [Dokumentacija REST API-JA za tvrtke Pojmovnik operacije](https://msdn.microsoft.com/library/mt708855.aspx)
