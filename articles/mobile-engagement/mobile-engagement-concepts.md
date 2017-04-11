<properties
    pageTitle="Radnje Mobile koncepata | Microsoft Azure"
    description="Azure koncepata Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure koncepata Mobile radnje

Korištenje mobilne definira koncepata nekoliko uobičajenih sve podržane platforme. U ovom se članku ukratko opisuje te koncepata.

Ovaj je članak dobro start ako ste novi korisnik mobilnog radnje. I provjerite je li pročitajte dokumentaciju specifične za platformu koristite, kao što je će suzite koncepata opisane u ovom članku s više pojedinosti i primjeri kao i moguće ograničenja.

## <a name="devices-and-users"></a>Uređaji i korisnika
Korištenje mobilne označava korisnicima tako da generira Jedinstveni identifikator za svaki uređaj. Ovaj identifikator zove identifikator uređaja (ili `deviceid`). Generira se na način sve pokretanje aplikacija na istom uređaju zajednički koristiti iste identifikator uređaja.

Implicitno, to znači da Mobile radnje smatra jedan uređaj pripadati točno jednog korisnika i na taj način korisnicima i uređajima su konceptima ekvivalent.

## <a name="sessions-and-activities"></a>Sesije i aktivnosti
Sesije je jedan koristite aplikacije obavlja korisnika, od vremena korisnik pokrene, korištenje do tabulatora korisnika.

Aktivnost je jedan koristite navedeni dodatni dijela aplikacije obavlja jedan korisnik (to je obično na zaslon, ali to može biti bilo što odgovarajuću aplikaciji).

Korisnik može izvoditi samo jedan aktivnosti odjednom.

Aktivnost označena prema nazivu (ograničeno 64 znaka) i po želji možete ugraditi neki dodatni podaci (u ograničenje 1024 bajtova).

Sesije automatski se izračunati iz slijeda aktivnosti korisnicima. Sesije se pokreće kada korisnik pokrene svoje prve aktivnosti i prestaje kada se dovrši on svoje zadnje aktivnosti. To znači da sesije ne morate izričito pokrenuti ili zaustaviti. Umjesto toga, aktivnosti su izričito pokrenuti ili zaustaviti. Ako nema aktivnosti prijavljuje, bez sesiju prijavljuje.

## <a name="events"></a>Događaji
Događaji koriste se za izvješća izravne akcija (kao što je gumb pritiska na tipku i članci pročitati korisnika).

Događaj može upućivati na trenutnoj sesiji izvodi posao, ili može biti samostalno događaju.

Događaj je označena prema nazivu (ograničeno 64 znaka), a po želji možete ugraditi neki dodatni podaci (u ograničenje 1024 bajtova).

## <a name="error"></a>Pogreška
Pogreške koriste se za prijave problema pravilno otkrio aplikaciju (kao što je netočan korisničkim akcijama, ili neuspješna poziv API-JA).

Pojavila se pogreška može biti povezan s trenutna sesija u izvodi posao ili može biti samostalne pogreške.

Pogreška označena prema nazivu (ograničeno 64 znaka) i po želji možete ugraditi neki dodatni podaci (u ograničenje 1024 bajtova).

## <a name="job"></a>Zadatak
Zadaci koji se koristi za izvješća akcije koje se pojavljuju trajanje (kao što su trajanja API poziva, prikaz vremena reklame, trajanje pozadinske zadatke ili trajanja akcije korisnika).

Zadatak je povezan sa sesijom jer zadatak može izvoditi u pozadini, bez interakcije s korisnikom.

Zadatak je označena prema nazivu (ograničeno 64 znaka) i po želji možete ugraditi neki dodatni podaci (u ograničenje 1024 bajtova).

## <a name="crash"></a>Pad
Ruši se automatski izdala SDK radnje Mobile da biste prijavili pogrešaka aplikacije problemi koji se ne nalaze u aplikaciji odabirati ga pad.

## <a name="application-information"></a>Informacije o aplikaciji
Informacije o aplikaciji (ili aplikaciju informacije) da biste označili korisnika, odnosno se koristi za pridruživanje nekih podataka koji će korisnici aplikacije (to je slično web kolačići, osim što informacije aplikacije pohranjen na strani poslužitelja na platformi Azure Mobile radnje).

Informacije o aplikaciji mogu biti registrirani pomoću SDK API-JA radnje Mobile ili pomoću platforme radnje mobilni uređaj API-JA.

Informacije o aplikaciji je par ključa vrijednosti povezane s uređajem. Ključno je naziv informacije aplikacije (ograničeno na 64 ASCII slova [a-zA-Z], [0-9] brojeve i podvlake [_]). Vrijednost (ograničeno na 1024 znakova) može biti bilo koji niz, cijeli broj, datum (gggg-MM-dd) ili Booleove vrijednosti (true ili false).

Bilo koji broj informacije aplikacije mogu se povezati s uređajem, unutar ograničenja definira uvjete cijene Mobile radnje. Za dani tipku, radnje Mobile samo evidentira najnovije skup vrijednosti (ne povijest). Postavljanje i promjena vrijednost u aplikaciji informacije navodi radnje Mobile da biste ponovno procijenili publike kriterija postavljen na aplikaciju info (ako ih ima) što znači da se taj aplikacije informacije koje se mogu koristiti za pokretanje ih gura Realno vrijeme.

## <a name="extra-data"></a>Dodatni podaci
Dodatni podaci (ili dodataka) je proizvoljne podatke koji se mogu priložiti događaje, pogreške, aktivnosti i zadacima.

Dodaci strukturirane su na sličan način JSON objekte: su izvršene stabla parove ključa vrijednosti. Strelicama ograničeni su na 64 ASCII slova [a-zA-Z], [0-9] brojeve i podvlake [_]) i ukupnu veličinu dodataka ograničen je na 1024 znaka (jednom kodirana JSON po radnje SDK Mobile).

Cijelo stablo ključa vrijednosti parove pohranjen kao objekt JSON. Ipak, je decomposed da bi mogao izravno pristupiti neke napredne funkcije kao što su segmenata samo prva razina tipke/vrijednosti (na primjer, možete jednostavno definirati segmenta pod nazivom "SciFi sporta" koji je stvoren svih korisnika koji se pojavljuju poslane najmanje 10 puta događaj pod nazivom "content_viewed" s na vrlo ključa "content_type" postavite na vrijednost "scifi" prošli mjesec). Stoga preporučujemo da biste poslali samo dodataka sastoji od jednostavne popise parove ključa vrijednosti pomoću skalarna vrijednosti (na primjer, nizove, datumi, cijelih brojeva ili Booleove vrijednosti).

## <a name="next-steps"></a>Daljnji koraci

- [Pregled Windows univerzalni SDK za Azure Mobile radnje](mobile-engagement-windows-store-sdk-overview.md)
- [Pregled Windows Phone Silverlight SDK za Azure Mobile radnje](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK za Azure Mobile radnje](mobile-engagement-ios-sdk-overview.md)
- [Android SDK za Azure mobilne radnje](mobile-engagement-android-sdk-overview.md)
