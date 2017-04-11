<properties
   pageTitle="Kako otkrili izvore podataka | Microsoft Azure"
   description="Isticanje kako otkriti resursi registriranih podataka pomoću kataloga podataka Azure, uključujući pretraživanja i filtriranja i pomoću uspješnosti isticanje mogućnosti kataloga podataka Azure portala je članak s uputama."
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

# <a name="how-to-discover-data-sources"></a>Kako otkriti izvora podataka

## <a name="introduction"></a>Uvod
**Katalog podataka za Microsoft Azure** je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Drugim riječima, **Katalog podataka Azure** je pomoć osobe otkrivanje, razumijevanje i korištenje izvora podataka i tvrtkama ili ustanovama pomoć da biste dobili dodatne vrijednost iz svoje postojeće podatke. Kada registrirani izvora podataka pomoću **Kataloga podataka Azure**, njegove metapodatke indeksirati servis, tako da korisnici mogu jednostavno pretraživati da biste otkrili koje su im potrebne podatke.

## <a name="searching-and-filtering"></a>Pretraživanje i filtriranje

Otkrivanje u **Katalog podataka Azure** koristi dvije primarni mehanizme: pretraživanje i filtriranje.

Pretraživanje namijenjen intuitivno i naprednih – po zadanom, pojmova za pretraživanje odgovaraju protiv neko svojstvo u katalogu, uključujući primjedbe navedene za korisnika.

Filtriranje namijenjen je nadopunjuju pretraživanje. Korisnici mogu odabrati određene značajke kao što su stručnjake, vrsta izvora podataka, Vrsta objekta i oznake da biste vidjeli samo imovine odgovarajuće podatke, a da biste ograničili rezultate pretraživanja na odgovarajuće kao i resursi.

Korisnicima možete brzo prijeći izvora podataka registriranih **Azure katalog podataka** da biste otkrili izvore podataka koje su im potrebne pomoću kombinacije pretraživanja i filtriranja.

## <a name="search-syntax"></a>Sintaksa za pretraživanje

Iako je zadano pretraživanje slobodni tekstni jednostavne i intuitivan, korisnici možete koristiti sintaksa za pretraživanje **Kataloga podataka Azure**korisnika da bi veću kontrolu nad rezultatima pretraživanja. Pretraživanje **Kataloga podataka Azure** podržava sljedeće tehnike:

| Postupak                 | Korištenje                                                                                                                                     | Primjer                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Osnovno pretraživanje              | Osnovno pretraživanje pomoću jednog ili više pojmova za pretraživanje. Rezultati su neki resursi koji odgovaraju na neko svojstvo s jednim ili više uvjete navedene. | Podaci o prodaji                                                |
| Određivanje opsega svojstvo          | Vratiti samo gdje se izraz za pretraživanje uparuje s navedeno svojstvo izvora podataka                                                   | Naziv: financije                                              |
| Logički operatori         | Proširili ili Suzite pretraživanje pomoću logičkih operacija                                                                                     | financije ne tvrtke                                     |
| Grupiranje s zagrada | Korištenje zagrada u grupi dijelovi upita da biste postigli logičke odvajanja, posebice u kombinaciji s logički operatori              | Naziv: financije i (oznaka: Q1 ili oznaka: Q2) |
| Operatori usporedbe      | Korištenje usporedbe osim jednakosti za svojstva koje sadrže numeričke i datum vrste podataka                                                | modifiedTime > "11/05/2014."                                 |

Dodatne informacije o pretraživanje **Kataloga podataka Azure** potražite u članku [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Kliknite isticanje
Kada pregledavate rezultate pretraživanja, bit će istaknuti prikazana svojstva koje zadovoljavaju navedene uvjete pretraživanja – kao što su naziv resursa podataka, opis i oznake – da biste lakše prepoznali Zašto podataka resursa je vratila na pretraživanju.

> [AZURE.NOTE] Korisnike možete uključiti uspješnosti isticanje isključivanje po želji pomoću parametar "Isticanje" na portalu za **Azure kataloga podataka** .

Kada pregledavate rezultate pretraživanja, nije uvijek možda očite Zašto podataka resursa uključen, čak i uz omogućeno uspješnosti isticanje. Budući da se sva svojstva pretraživati prema zadanim postavkama, podataka resursa, rezultat zbog podudaranje na svojstvu razini stupca. I jer više korisnika može dodavati opaske registriranih podataka imovine s vlastite oznake i opise, sve metapodataka mogu se prikazati na popisu rezultata pretraživanja.

U zadanom pločica prikaz, svaku pločicu prikazane u rezultatima pretraživanja neće sadržavati stavku ikonom "odgovara prikazu pojam za pretraživanje", koji omogućuje korisniku da biste brzo prikazali broj rezultata i njihovo mjesto, a da biste se prebacili na njih po želji.

 ![Kliknite isticanje i pretraživanje odgovara na portalu za Azure kataloga podataka](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Sažetak
Registracija izvora podataka pomoću **Kataloga podataka Azure** olakšava taj izvor podataka da biste otkrili i razumijevanje kopiranjem strukturnih i opisni metapodatke iz izvora podataka u katalogu servis. Kada registrirani izvor podataka, korisnici mogu otkriti pomoću filtriranja i pretraživanje s portala za **Azure kataloga podataka** .

## <a name="see-also"></a>Vidi također
- [Početak rada s katalog podataka Azure](data-catalog-get-started.md) vodič za detaljne upute kako otkrili izvore podataka.
