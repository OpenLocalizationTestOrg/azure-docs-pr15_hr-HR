<properties
   pageTitle="Kako registrirati izvore podataka | Microsoft Azure"
   description="Upute u članku se isticanje kako registrirati izvorima podataka pomoću kataloga podataka Azure, uključujući polja metapodataka dobivenih tijekom registracije."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-register-data-sources"></a>Kako registrirati izvora podataka

## <a name="introduction"></a>Uvod
**Katalog podataka za Microsoft Azure** je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Drugim riječima, **Katalog podataka Azure** je pomoć osobe otkrivanje, razumijevanje i korištenje izvora podataka i tvrtkama ili ustanovama pomoć da biste dobili dodatne vrijednost iz svoje postojeće podatke. I u prvom koraku izradi izvora podataka vidljivim putem **Azure katalog podataka** da biste registrirali taj izvor podataka.
## <a name="registering-data-sources"></a>Registracija izvora podataka
Registracija je postupak izdvajanje metapodataka iz izvora podataka i kopiranje tih podataka u servisa **Azure kataloga podataka** . Ostaje podataka pri čemu je trenutno nalazi, a ostaje u odjeljku kontrola administratori i pravila trenutni sustav.

Da biste registrirali izvor podataka, jednostavno pokrenite Registracija alat **Katalog podataka Azure** izvora podataka na portalu **Azure kataloga podataka** . Zapisnik pomoću rad ili školskog računa (iste Azure Active Directory vjerodajnice koju koristite za prijavu portal) i zatim odaberite izvor podataka koji želite registrirati.
[Početak rada s katalog podataka Azure](data-catalog-get-started.md) vodič dodatne detaljne informacije potražite u članku.

Kada registrirani izvora podataka, katalog prati položaju i indeksi njegove metapodatke tako da korisnici mogu pretraživati, Pregledaj, i otkrivanje izvora podataka i pomoću položaju povezati putem aplikacije ili alat za po izboru.

## <a name="sources-supported"></a>Izvori koje podržava
Pogledajte [DSC katalog podataka](data-catalog-dsr.md) za popis trenutno podržanih izvora podataka.
<br/>


## <a name="structural-metadata"></a>Strukturni metapodataka
Kada ste Registracija izvor podataka, alat za registraciju će izvlačenje informacija o strukturu objekata odaberete – to je predani da biste kao strukturnih metapodataka.

Za sve objekte ovaj strukturnih metapodataka neće sadržavati stavku mjesto objekta da bi korisnike otkrijte podaci mogli koristiti te podatke možete povezati objekt u klijentskim alatima po izboru. Drugih strukturnih metapodataka sadrži objekt naziv i vrsta, a upišite naziv atributa/stupca i podaci.

## <a name="descriptive-metadata"></a>Opisna metapodataka
Osim core strukturnih metapodatke dobivenih iz izvora podataka, alat za registraciju izvora podataka će izdvojiti kao i opisni metapodataka. SQL Server Analysis Services i SQL Server Reporting Services, to je preuzet iz svojstva opis koji prikazuje tih servisa. Za SQL Server će se izdvajati vrijednosti navedene pomoću ms_description proširena svojstva. Za bazu podataka Oracle, alat za registraciju izvora podataka će izdvojiti stupcu KOMENTARA u prikazu ALL_TAB_COMMENTS.

Osim opisni metapodataka dobivenih iz izvora podataka, korisnici mogu unijeti opisni metapodataka pomoću alata za registraciju izvora podataka. Korisnicima možete dodavati oznake, a možete prepoznati stručnjaka za objekte registracije. Svi ti opisni metapodaci se kopira sa servisom **Azure katalog podataka** uz strukturnih metapodataka.

## <a name="including-previews"></a>Uključujući pretpregleda

Prema zadanim postavkama, samo metapodataka se izdvajati iz izvora podataka i kopirali sa servisom **Azure katalog podataka** , ali objašnjenje izvora podataka često postala je jednostavnije tako da prikazuje uzorak podataka sadrži.

Alat za registraciju izvora podataka **Azure katalog podataka** omogućuje korisnicima da biste dodali pretpregled snimke podatke u svaku tablicu i prikaz koji je registriran. Ako korisnik odabere u da biste dodali pretpregled tijekom registracije, alat za registraciju neće sadržavati do 20 zapise iz svaku tablicu i prikaz. U katalogu uz strukturnih i opisni metapodatke zatim kopirati te snimke.


> [AZURE.NOTE]  Širine tablice s velikim brojem stupaca može imati manje od 20 zapisa koji su uključeni u svoje pretpregled.


## <a name="including-data-profiles"></a>Uključujući podataka profila

Baš kao i uključujući pretpregledi pružaju koristan kontekst za korisnike pretraživanje za izvore podataka u **Katalogu podataka Azure**, uključujući podatke profila i olakšavaju da biste shvatili otkriven izvore.

Alat za registraciju izvora podataka **Azure katalog podataka** omogućuje korisnicima obuhvaćaju podataka profila za svaku tablicu i prikaz koji je registriran. Ako korisnik odabere da biste dodali podatke profila tijekom registracije, alat za registraciju neće sadržavati stavku prikupljanje statističkih podataka o podatke u svaku tablicu i prikaz, uključujući:

* Broj redaka i veličina podataka u objektu
* Rok za najnovija ažuriranja podataka i shemi objekta
* Broj null zapisa i različitih vrijednosti za stupce
* Minimum, maksimum, average i standardnu devijaciju vrijednosti stupaca

Te statistike zatim kopiraju se u katalogu uz strukturnih i opisni metapodatke.

> [AZURE.NOTE]  Tekst i datum stupaca neće sadržavati prosjeka ili standardne devijacije Statistika u svoj profil podataka.

## <a name="updating-registrations"></a>Ažuriranje registracije

Registracija izvora podataka će vam tako biti vidljiv u **Katalog podataka Azure** pomoću metapodataka i neobavezna pretpregled dobivenih tijekom registracije. Ako izvor podataka mora se ažurirati u katalogu (, na primjer, ako je promijenio se shema objekta, ili tablica izvorno izostaviti potrebno obuhvatiti ili korisnik želi da biste ažurirali podatke koji su uključeni u pretpregleda) alat za registraciju izvora podataka možete ponovno pokrenuti.

Ponovno Registriranje izvora podataka koji je već registriran izvodi operaciju spajanja "upsert": postojećih objekata će se ažurirati dok stvorit će se nove objekte. Sve metapodatke nudi korisnicima putem portala za **Azure katalog podataka** će se zadržavaju.

## <a name="summary"></a>Sažetak
Registracija izvora podataka pomoću **Kataloga podataka Azure** olakšava taj izvor podataka da biste otkrili i razumijevanje kopiranjem strukturnih i opisni metapodatke iz izvora podataka u katalogu servis. Kada registrirani izvor podataka, ga možete pa se dodavati napomene, upravljanim i otkriven pomoću portala za **Azure kataloga podataka** .

## <a name="see-also"></a>Vidi također
- [Početak rada s katalog podataka Azure](data-catalog-get-started.md) vodič detaljne informacije o kako registrirati izvora podataka.
