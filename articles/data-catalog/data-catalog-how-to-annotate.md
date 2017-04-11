<properties
   pageTitle="Kako označiti izvore podataka | Microsoft Azure"
   description="Upute u članku se isticanje kako označiti resursima podataka u katalog podataka Azure, uključujući neslužbeni imena, oznake, opis i stručnjaka."
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


# <a name="how-to-annotate-data-sources"></a>Kako označiti izvora podataka

## <a name="introduction"></a>Uvod
**Katalog podataka za Microsoft Azure** je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Drugim riječima, katalog podataka je pomoć osobe otkrivanje, razumijevanje i korištenje izvora podataka i tvrtkama ili ustanovama pomoć da biste dobili dodatne vrijednost iz svoje postojeće podatke. Kada registriran izvora podataka pomoću kataloga podataka, njegov metapodataka je kopirali i indeksirati servis, no priče ne postoji završili. Katalog podataka korisnicima omogućuje prikaz vlastitih opisni metapodataka – kao što su opisi i oznake – dodatku izjavi metapodataka dobivenih iz izvora podataka i izvora podataka provjerite više razumljiv još osoba.

## <a name="annotation-and-crowdsourcing"></a>Opaske i crowdsourcing
Svi ima svoje mišljenje. I to je dobro.
Katalog podataka prepoznaje različitim korisnicima uključeno različite perspektive tvrtke izvora podataka, a da svaki od tih perspektive može biti koristan. Zamislite sljedeće:

* Administrator sustava zna na razini ugovor o usluzi za poslužitelje ili usluge glavnog izvora podataka.
* Administratora baze podataka zna raspored sigurnosnog kopiranja za svaku bazu podataka i windows dopuštene obrada ETL.
* Vlasnik sustava zna postupka za korisnike da biste zatražili pristup izvoru podataka.
* Upravitelja podataka zna kako mapiranje resursi i atribute u izvoru podataka u podatkovni model enterprise.
* Na analitičar zna korištenja podataka u kontekstu poslovnih procesa on podržava.

Svaki od tih perspektive je koristan, a katalog podataka koristi crowdsourcing pristup metapodataka koja omogućuje svaki od njih da biste bilježe i upotrijebiti za davanje potpuna slika izvora podataka registriranih. Pomoću portala za katalog podataka svakog korisnika možete dodati i uređivanje svoju vlastitu primjedbe tijekom moći pregledavati primjedbe koje ste dobili od drugih korisnika.

## <a name="different-types-of-annotations"></a>Različite vrste primjedbe
Katalog podataka podržava sljedeće vrste napomene:

| Opaske     | Bilješke                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Neslužbeni naziv  | Neslužbeni nazivi možete navesti na razini podataka resursa, da biste imovine podataka više lako razumjeti. Neslužbeni su nazivi najkorisniji kada je podlozi naziv objekta prekomplicirano, skraćeni ili u suprotnom neće smisleni korisnicima.                                                                                                                            |
| Opis    | Opisi mogu biti iz podataka resursa i atributa / razine stupca. Opisi su primjedbe prostoručni Kratki tekst koji opisuje korisnika perspektivi resursa podataka ili njezino korištenje.                                                                                                                                                              |
| Oznake (korisnik oznake)          | Oznaka koje će biti navedena na podataka resursa i atributa / razine stupca. Korisnik oznake su korisnički definirane oznake koje se mogu koristiti za kategorizaciju podataka imovine ili atribute.                                                                                                                                                                                                    |
| Oznake (Pojmovnik oznake)          | Oznaka koje će biti navedena na podataka resursa i atributa / razine stupca. Pojmovnik oznake su središnje definirani Pojmovnik uvjete koji se mogu koristiti za kategorizaciju imovine podatke ili atribute pomoću uobičajene poslovne taksonomije. Dodatne informacije potražite u članku [upute za postavljanje tvrtke Pojmovnik za mjerodavni označavanje](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Stručnjaci        | Stručnjaci mogu biti navedena na razini podataka resursa. Stručnjaci odredite korisnike ili grupe s expert perspektive na podacima možete poslužiti kao točke kontakta za korisnike koji otkrivanje izvora podataka registriranih i imate pitanja na koja ne odgovara tako da postojeći primjedbe.  |
| Traženje pristupa | Informacije o zahtjeva za pristup možete navesti na razini podataka resursa. Te su informacije za korisnike koji otkrijte izvor podataka koji se još nemaju dozvole za pristup. Korisnike možete unijeti adresu e-pošte korisniku ili grupi koja daje pristupa, URL procesa ili alata koje korisnici moraju da biste pristupili, ili možete unijeti postupka sam kao tekst. |
| Dokumentacija | Dokumentacija može biti navedena na razini podataka resursa. Dokumentacija resursa je obogaćenog teksta informacije koje se mogu sadržavati veze i slike, a nude sve informacije ne kroz opise i oznake. |


## <a name="annotating-multiple-assets"></a>Dodavanje opaski više resursi
Prilikom odabira više imovine podataka na portalu za katalog podataka, korisnici može dodavati opaske imovini odabrane u jednoj operaciji. Opaske će se primijeniti na sve odabrane imovine, što olakšava odaberite i dati dosljedan opis i skupova oznake i stručnjaka za imovinu povezanih podataka.

> [AZURE.NOTE] Oznake i stručnjaka također se može pružati prilikom registracije podataka koju koristite podataka katalog podataka izvora alat za registraciju.

Kada odaberete više tablica i prikaza, samo stupaca sve odabrane podatke imovine imaju zajedničke prikazat će se na portalu kataloga podataka. Time korisnici moraju unijeti oznake i opise za sve stupce s istim nazivom za sva sredstva odabranog.

## <a name="annotations-and-discovery"></a>Opaske i otkrivanje
Baš kao i metapodataka dobivenih iz izvora podataka tijekom registracije se dodaje u indeks pretraživanja katalog podataka, korisnik metapodataka također će se indeksirati. To znači da ne samo primjedbe olakšavaju korisnicima da brzo razumijevanje podataka mogu otkriti, primjedbe i olakšavaju korisnicima da biste otkrili imovine označenim podataka pretraživanjem uz uvjete navedene da odgovara.

## <a name="summary"></a>Sažetak
Registracija izvora podataka pomoću kataloga podataka poboljšava tih podataka vidljivim kopirate strukturnih i opisni metapodatke iz izvora podataka u servis kataloga. Kada registrirani izvor podataka, korisnici mogli dati opaske da biste olakšali da biste otkrili i razumijevanje iz unutar portala kataloga podataka.

## <a name="see-also"></a>Vidi također
- [Početak rada s katalog podataka Azure](data-catalog-get-started.md) vodič za detaljne upute kako označiti izvora podataka.
