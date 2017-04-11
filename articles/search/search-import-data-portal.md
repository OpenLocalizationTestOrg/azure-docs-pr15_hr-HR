<properties
    pageTitle="Uvoz podataka Azure pretraživanje pomoću indexers na portalu za Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Koristite Azure Čarobnjak za pretraživanje uvoz podataka na portalu za Azure pretraživanja radi indeksiranja podatke iz blobova platforme Azure prostora za pohranu, stroage tablice, SQL baze podataka i SQL Server na Azure VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Uvoz podataka pomoću portala za pretraživanje za Azure

Portal za Azure nudi Čarobnjak za **Uvoz podataka** na nadzornoj ploči za Azure pretraživanja za učitavanja podataka u indeks. 

  ![Uvoz podataka na naredbenoj traci][1]

Interno, čarobnjak konfigurira i poziva *za indeksiranje*, automatizacija nekoliko koraka indeksiranja procesa: 

- Povezivanje s vanjskim izvorom podataka u trenutnom Azure pretplate
- Automatsko generiranje shemom indeksa na temelju struktura izvora podataka
- Stvaranje dokumenata koji se temelji na skup redaka dohvaćeni iz izvora podataka
- Prijenos dokumenata na indeks servisa za pretraživanje

Možete Isprobajte ovaj tijek rada korištenje oglednih podataka u DocumentDB. [Početak rada s Azure pretraživanja na portalu za Azure](search-get-started-portal.md) potražite upute.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Izvori podataka koje podržava čarobnjak za uvoz podataka

Čarobnjak za uvoz podataka podržava sa sljedećim izvorima podataka: 

- Baze podataka Azure SQL
- SQL Server relacijskih podataka na VM programa Azure
- Azure DocumentDB
- Azure blobova (u pretpregled)
- Azure spremište tablica (u pretpregled)

Plošnu skup podataka je obavezan unos. Možete uvesti samo iz jedne tablice, prikaz baze podataka ili struktura ekvivalentan podataka. U ovom struktura podataka potrebno stvoriti prije pokretanja čarobnjaka.

Imajte na umu nekoliko na indexers jesu li i dalje u pretpregledu, što znači da je definiciju indeksiranje sigurnosno verziji pretpregleda u API. [Pregled komponente za indeksiranje](search-indexer-overview.md) potražite dodatne informacije i veze.

## <a name="connect-to-your-data"></a>Povezivanje s podacima

1. Prijavite se na [Azure Portal](https://portal.azure.com) i otvaranje usluge nadzorne ploče. Možete kliknuti **usluge pretraživanja** na traci Skok da biste prikazali postojeće usluge trenutne pretplate. 

2. Kliknite **Uvoz podataka** na naredbenoj traci da biste slajd otvorili plohu uvoz podataka.  

3. Kliknite **Poveži se s podacima** da biste odredili definiciju izvora podataka koristi indeksiranje. Za izvore podataka intra pretplatu, čarobnjak može obično otkriti i pročitajte informacije o vezi, minimiziranje cjelokupan preduvjeti za konfiguraciju.

| | |
|--------|------------|
|**Postojećeg izvora podataka** | Ako već imate indexers koji su definirani u servis za pretraživanje, možete odabrati postojeću definiciju izvora podataka za drugi uvoz.|
|**Baze podataka Azure SQL** | Naziv usluge, vjerodajnice za bazu podataka korisnik s dozvolama za čitanje, a naziv baze podataka možete navesti na stranici ili putem programa ADO.NET niz za povezivanje. Odaberite mogućnost niz veze da biste pogledali ili Prilagodba svojstva. <br/><br/>Tablica ili prikaz na kojoj se navode u skup redaka mora biti naveden na stranici. Ova mogućnost pojavljuje se nakon uspješnog povezivanja dodjeljivanja padajućeg popisa tako da unesete odabira.|
|**SQL Server na Azure VM** | Navedite naziv u potpunosti kvalificirana usluge, korisnički ID i lozinku i bazu podataka kao niz za povezivanje. Da biste koristili ovaj izvor podataka, morate imati već instaliran certifikat u lokalnom spremištu koji šifrira veze. <br/><br/>Tablica ili prikaz na kojoj se navode u skup redaka mora biti naveden na stranici. Ova mogućnost pojavljuje se nakon uspješnog povezivanja dodjeljivanja padajućeg popisa tako da unesete odabira.
|**DocumentDB** |Preduvjeti obuhvaćaju račun, baze podataka, a zbirke. Svim dokumentima u zbirci uvrstiti u indeks. Možete definirati upita za stopi i filtriranje na skup redaka ili da biste otkrili promijenjene dokumente za operacije osvježavanja podataka za kasnije.|
|**Spremište blobova platforme Azure** | Preduvjeti sadržavati račun za pohranu i spremnik. Po želji, nazivi blob slijede virtualne konvencija imenovanja grupiranja u svrhu, možete odrediti virtualnog direktorija dio naziv kao što je mapa u kontejneru. Dodatne informacije potražite u članku [Indeksiranje blobova (pretpregled)](search-howto-indexing-azure-blob-storage.md) . |
|**Spremište tablica platforme Azure** | Preduvjeti sadržavati račun za pohranu i naziva tablice. Po želji možete navesti upit za dohvaćanje podskup tablice. Dodatne informacije potražite u članku [Indeksiranje spremište tablica (pretpregled)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Cilj indeks za prilagodbu

Preliminarni indeks obično nenamjerna u skupu podataka. Dodavanje, uređivanje ili brisanje polja da biste dovršili u shemi. Uz to, postavite atribute na razini polja da biste odredili njegov ponašanja naknadna pretraživanja.

1. **Prilagodba cilj indeksa**u navedite naziv i **ključ** za identificirati samo svakom dokumentu. Ključ mora biti niz. Ako vrijednosti polja sadrži razmake ili crtice obavezno Postavljanje naprednih mogućnosti **uvoza podataka** izostavlja provjeri valjanosti te znakove.

2. Pregledajte i izmijenite preostala polja. Naziv polja i upišite obično ispunjava umjesto vas. Možete promijeniti vrstu podataka.

3. Postavljanje atributa indeks za svako polje:

 - Veličina vraća polje rezultata pretraživanja.
 - Koje je moguće filtrirati omogućuje polja na koji postavljate referencu u izrazu filtra.
 - Koje je moguće sortirati omogućuje polja koja će se koristiti u sortiranje.
 - Facetable omogućuje polja za slojevito navigaciju.
 - Pretraživanje po cijelom tekstu pretraživanja omogućuje.
  
4. Ako želite navesti analyzer jezika na razini polja, kliknite karticu **za analizu** . Sada možete navesti samo analyzers jezik. Pomoću prilagođenog alata za analizu ili alat za analizu koji nisu jezik kao što su ključnu riječ, uzorak i tako dalje, potrebno je kod.

 - Kliknite **može se pretraživati** odrediti pretraživanja cijelog teksta u polju i omogućuju padajućeg popisa za analizu.
 - Odaberite Alat za analizu koji želite. Detalje potražite u članku [Stvaranje indeksa za dokumente na više jezika](search-language-support.md) .

5. Kliknite **Suggester** da biste omogućili prijedloga za upite dovršena prije vrsta na odabrana polja.


## <a name="import-your-data"></a>Uvoz podataka

1. **Uvoz podataka**, navedite naziv za indeksiranje. Opoziv je li proizvod čarobnjaka za uvoz podataka indeksiranje. Kasnije, ako želite pregledavati ili uređivati, odabrat ćete ga na portalu umjesto rerunning čarobnjaka. 

2. Navedite raspored koji se temelji na regionalne vremensku zonu u kojoj se servis dodjeli.

3. Postavljanje naprednih mogućnosti da biste odredili pragovi na li možete nastaviti indeksiranje ako se prekine dokumenta. Osim toga, možete odrediti hoće li **ključ** polja smiju sadržavati razmake i kose crte.  

## <a name="edit-an-existing-indexer"></a>Uređivanje postojeće komponente za indeksiranje

Na nadzornoj ploči servisa dvokliknite na pločici za indeksiranje slajda out popis svih indexers stvorene za vašu pretplatu. Dvokliknite neku od indexers da biste pokrenuli, uređivanja ili brisanja. Možete indeks zamijeniti postojeću, promjena izvora podataka i postavljanje mogućnosti za pragovi pogreške tijekom indeksiranja.

## <a name="edit-an-existing-index"></a>Uredite postojeći indeks

U pretraživanju Azure strukturnih ažuriranja indeksa potrebno izvođenje te indeksa koji se sastoji od brisanje indeksa, vraćanju indeks i ponovno učitavanje podataka. Strukturni ažuriranja obuhvaćaju Promjena vrste podataka i preimenovanje ili brisanje polja.

Izmjene koje nije potreban je izvođenje obuhvaćaju dodavanjem novog polja, promjenom bilježenja rezultata profila, promjeni suggesters ili promjeni analyzers jezik. Dodatne informacije potražite u članku [Ažuriranje indeksa](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Sljedeći korak

Pregledajte ove veze da biste saznali više o indexers:

- [Indeksiranje baze podataka Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indeksiranje DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Indeksiranje blobova (pretpregled)](search-howto-indexing-azure-blob-storage.md)
- [Indeksiranje spremište tablica (pretpregled)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

