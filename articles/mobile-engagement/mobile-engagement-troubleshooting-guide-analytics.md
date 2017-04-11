<properties 
   pageTitle="Azure mobilne radnje vodič – analitičke podatke za otklanjanje poteškoća" 
   description="Otklanjanje problema s analize, nadzor, segmente i nadzorne ploče u Azure Mobile radnje" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Vodič za probleme s analize, nadzor, segmente i nadzorne ploče za otklanjanje poteškoća

Slijede mogući problemi koji se mogu pojaviti s kako Azure Mobile radnje prikuplja informacije o aplikacijama, uređaji i korisnicima.

## <a name="missingdelayed-information"></a>Podaci koji nedostaju/Delayed

### <a name="issue"></a>Problem
- Informacije o kasni u koji se pojavljuje u analize, segmente ili nadzorne ploče.
- Nadzor nedostaje informacija.
- Nedostaju podaci analize, segmente ili nadzorne ploče.
- Odlazak segmente ograničenja.

### <a name="causes"></a>Uzroci

- Možete koristiti API analize, Monitor API-JA i segmenata API da biste vidjeli ako sve podatke koje nedostaju korisničko Sučelje je vidljiva kroz API-ji.
- Ako radnje SDK za Azure Mobile nije ispravno integriranom aplikacije pa koje nećete moći prikaz podataka u analize, segmente, nadzor ili nadzorne ploče.
- Segmenata nije moguće promijeniti nakon stvaranja, segmenata mogu se samo "kloniranu" (koja se kopira) ili "uništava" (izbrisani). Segmenata može sadržavati samo 10 kriterija.
- Najbolji način da biste testirali podataka koji nedostaju iz nadzor je uređaj test za postavljanje, deinstalirajte i/ili ponovno instalirajte aplikaciju na uređaju test.
- Informacije o osvježavanja svaka 24 sata za analize, segmente ili nadzorne ploče.
- Informacije u nove segmente možda neće prikazati do 24 sata nakon stvaranja čak i ako segment temelji se na prethodne informacije.
- Filtriranje analize podataka u korisničkom Sučelju prikazivat će se svim primjerima za tu vrstu bez obzira na verziju aplikacije (npr. "ruši" filtrirani prema nazivu prikazat će od 1 i 2 verzije aplikacije).
- Razdoblje za analizu temelji se na datum iz postavke uređaja korisnika, tako da korisnika čije telefona sadrži datum i pogrešno prikazuju nije u redu vremensko razdoblje.
- Ih gura bez strani poslužitelja podataka prijavljen je kada koristite gumb za "testiranje", podataka samo traje realni automatske kampanja.

## <a name="cant-locate-items-in-ui"></a>Ne možete pronaći stavke u korisničkom Sučelju

### <a name="issue"></a>Problem
- Nije moguće stvoriti segmenata na temelju određenih ugrađen u web-mjesta ili informacije prilagođene aplikacije označavanje kriterija.
- Ne mogu pronaći određenih ugrađen u web-mjesta i informacije prilagođene aplikacije označavanje kriterija u analize, nadzor ili nadzorne ploče.
- Ne mogu protumačiti podatke u analize, nadzor, segmente ili nadzorne ploče.

### <a name="causes"></a>Uzroci

- Ugrađena neke stavke, a oznake aplikacije informacije dostupne samo kao kriterija za slanje, ali možda neće biti dodane segmentu vama ili vidljivu analize, nadzor ili nadzorne ploče. 
- Ugrađeni stavki i oznaka informacije aplikacije koje se ne mogu dodati segment, morat ćete popis postavljanje ciljanja kriterija u svakom kampanje da biste izvršili funkciju isti kao ciljanja koji se temelji na segment.
- Kontekstni izbornici u odjeljcima analize, nadzor, segmente i nadzornih ploča Azure Mobile radnje korisničkog sučelja dodatnu pomoć potražite u članku te kako informacije.

## <a name="crash-troubleshooting"></a>Ruši se za otklanjanje poteškoća

### <a name="issue"></a>Problem
- Aplikacija zatvara se pojavljuju se u analize, nadzor ili nadzorne ploče.

### <a name="causes"></a>Uzroci

- Otklanjanje poteškoća s aplikacije ruši se vidjeti u analize, nadzor ili nadzornoj ploči provjerite da biste provjerili napomene o poznatim problemima sa starijim verzijama SDK-a.
- Za daljnje rješavanje problema s računala ruši izvesti događaj iz test uređaj s instaliranu aplikaciju i potražili uređaju ID-a u odjeljku "Monitor – događaje" korisničkog sučelja Azure Mobile radnje. Izvedite događaja koji je uzrok aplikaciju ruši i potražiti dodatne informacije u odjeljku "Monitor – pad" korisničkog sučelja Azure Mobile radnje. 

 
