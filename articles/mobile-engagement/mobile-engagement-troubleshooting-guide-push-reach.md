<properties 
   pageTitle="Azure mobilne radnje vodič – automatske/razgovor za otklanjanje poteškoća" 
   description="Otklanjanje poteškoća korisnika interakcije i obavijesti o problemima u Azure Mobile radnje" 
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

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Vodič za otklanjanje poteškoća s automatske i dođete do problema

Slijede moguće probleme koji se mogu pojaviti s kako Azure Mobile radnje šalje obavijest o tome svojim korisnicima.
 
## <a name="push-failures"></a>Automatske neuspjeha

### <a name="issue"></a>Problem
- Ih gura ne funkcioniraju (u aplikaciji iz aplikacije ili i jedno i drugo).

### <a name="causes"></a>Uzroci
- Više puta automatske došlo je znak da Azure Mobile radnje, razgovor i druge napredne značajke Azure Mobile radnje nije ispravno integriran ili da SDK potrebno nadogradnju da biste riješili Poznati problem s novu platformu OS ili uređaj.
- Testirajte samo za slanje u aplikaciji i samo na automatske van zaslona aplikacije da biste utvrdili je li nešto u aplikaciji ili izvan aplikacije problem.
- Testirajte korisničkog Sučelja i na API kao otklanjanje poteškoća korak da biste vidjeli koje dodatne informacije o pogrešci dostupna oba mjesta.
- Iz aplikacije ih gura neće funkcionirati ako Azure Mobile radnje i razgovor integrira u SDK-a.
- Ih gura neće funkcionirati ako certifikati nisu valjane, ili koristite RN nasuprot Razvojni pravilno (samo za iOS). (**Bilješke:** "iz aplikacije" automatske obavijesti možda nije moguće isporučiti za iOS, ako su oba razvoja (Razvojni), a zatim proizvodnje (grupa) verzija aplikacije instalirana na istom uređaju jer sigurnosni token pridružen certifikata koje se možda nevažeći po Apple. Da biste riješili taj problem, deinstalirajte i Razvojni i radni verziju aplikacije i ponovno instalirati samo jedne verzije na uređaju.)
- Iz aplikacije automatske broji rukuje se drukčije u različitim platformama (iOS prikazuje manje informacija od Android ako izvorni ih gura onemogućuju na uređaju, u API-JA možete unijeti dodatne informacije od korisničko Sučelje na automatske stat).
- Iz aplikacije ih gura možete blokirati korisnika na razini OS (iOS i Android).
- Iz aplikacije ih gura će se prikazivati kao onemogućena u korisničkom Sučelju radnje Mobile Azure ako nisu ispravno integriran, no možda neće uspjeti tihu iz API Sučelja.
- U aplikaciji ih gura neće funkcionirati ako Azure Mobile radnje i razgovor integrira u SDK-a.
- GCM i Admin ih gura neće funkcionirati ako Azure Mobile radnje i poslužitelj za određene integrira u SDK (samo za Android).
- U aplikacije i o ih gura treba testirati zasebno da biste vidjeli je li automatske ili razgovor problem.
- U aplikaciji ih gura zahtijevaju da se aplikacija Otvori moguće primiti.
- U aplikaciji ih gura često su postavljanje filtriraju po informacije oznaku aplikacije izborno ili isključivanja.
- Ako koristite prilagođenu kategoriju u razgovor da biste prikazali obavijesti u aplikaciji, ćete morati slijedite odgovarajuće-vijek obavijesti o jer inače obavijesti neće se izbrisati kada korisnik odbacite je.
- Ako pokrenete s kampanjom bez datuma završetka, a uređaj prima aplikaciju u obavijesti, ali ne ne prikazuj je još, korisnik će i dalje primati obavijesti kada se sljedeći put na mogu se prijaviti u aplikaciju, čak i ako ručno završili kampanje.
- Ima li problema s automatske API potvrdite koje zaista želite koristiti API automatske umjesto API dosegne (jer API dosegne služi češće) i su zbunjujućim parametara "tereta" i "obavijesti".
- Testirajte kampanje automatske s obje uređaj koji je povezan putem Wi-Fi veze i 3G da biste uklonili vezu s mrežom kao izvor mogućih problema.

## <a name="push-testing"></a>Automatske testiranje

### <a name="issue"></a>Problem
- Ih gura može poslati određeni uređaj ovisno o uređaju ID.

### <a name="causes"></a>Uzroci

- Testiranje uređaji su postavljanje drugačije za svaki platformu, ali uzrokuje događaja u aplikaciji na uređaju test i tražim identifikacijskog Broja za uređaj na portalu surađivati da biste pronašli svoj uređaj ID za sve platforme.
- Testiranje uređaji funkcioniraju drukčije s IDFA nasuprot IDFV (samo za iOS).


## <a name="push-customization"></a>Automatske prilagodbe

### <a name="issue"></a>Problem
- Napredne automatske sadržaja neće funkcionirati stavke (iskaznice, preusmjeravaju, vibracija, slike, itd.).
- Veze iz ih gura ne funkcioniraju (iz aplikacije, u aplikaciji za web-mjesta, na mjesto u aplikaciji).
- Automatske statistike prikazuje da je automatske nije poslana proizvoljan broj osoba u skladu s očekivanjima (previše ili nema dovoljno).
- Automatske duplicirati i primljenih dvaput.
- Ne može registrirati uređaj test za radnje Azure Mobile ih gura (s vlastitim katalog ili Razvojni aplikacija).

### <a name="causes"></a>Uzroci

- Da biste se povezali na određeno mjesto u aplikaciji zahtijeva "Kategorija" (samo za Android).
- Precizno povezivanja sheme preusmjeravanje korisnika na drugo mjesto nakon klika na automatske obavijesti morate stvorene u i izravno upravlja vaše aplikacije i uređaj s operacijskim Sustavom ne po Mobile radnje. (**Bilješke:** iz aplikacije obavijesti ne možete povezati izravno u aplikaciji mjesta s operacijskim sustavom iOS kao što je to moguće uz sa sustavom Android.)
- Slika za vanjske poslužitelje morate moći koristiti HTTP "GET" i "ZAGLAVLJE" za širu sliku ih gura rada (samo za Android).
- U kodu, možete onemogućiti agent za Azure Mobile radnje prilikom otvaranja tipkovnice, a imate kod ponovno aktivirati agent za Azure Mobile radnje kada tipkovnice je zatvoriti tako da se tipkovnice neće utjecati na izgled obavijesti (samo za iOS).
- Neke stavke ne funkcioniraju u test simulations, ali samo realni kampanje (iskaznice, preusmjeravaju, vibracija, slike, itd.).
- Kada koristite gumb za "testiranje" ih gura prijavljen je nema podataka o strani poslužitelja. Realni automatske kampanje samo traje podataka.
- Da biste izdvojili problem, rješavanje problema s: testiranje, zamjenu, i realni kampanje jer se svaki funkcioniraju malo drugačije.
- Trajanja vaše "u aplikaciji" i "uvijek" kampanja zakazana za pokretanje možete efekta isporuke brojeva jer će se samo s kampanjom isporuku za korisnike koji su "u aplikaciji" tijekom kampanje (i korisnicima koji imaju njihove postavke uređaja postavljena za primanje obavijesti o "iz aplikacije").
- Razlike između kako rukovati Android i iOS iz obavijesti aplikacija teško izravno Usporedba automatske statistike između Android i iOS verziju aplikacije. Android navedene su dodatne informacije OS razine obavijesti od iOS. Android izvješća kada izvorni obavijesti je primljena, kliknuli ili izbrisati u centru za obavijesti, ali iOS izvješća te informacije osim ako kliknete obavijesti. 
- Glavna razloga zbog kojeg "pomiču" brojevi razlikuju se od različite od "isporučena" brojevi za dosegne kampanje da "u aplikaciji" i "iz aplikacije" obavijesti broje se drukčije. "U aplikaciji" obavijesti rješava radnje Mobile, ali "iz aplikacije" obavijesti rješava centar za obavijesti u sustavu OS uređaja.

## <a name="push-targeting"></a>Automatske skupine

### <a name="issue"></a>Problem
- Ugrađena ciljanja ne funkcioniraju.
- Određivanje ciljne aplikacije informacije oznaka ne funkcionira prema očekivanjima.
- Određivanje zemlj mjesto ne funkcionira prema očekivanjima.
- Jezične mogućnosti ne funkcioniraju prema očekivanjima.

### <a name="causes"></a>Uzroci

- Provjerite je li koji ste prenijeli aplikacije informacije oznake putem korisničkog Sučelja programa Azure Mobile radnje ili API-JA.
- Ograničavanje automatske brzine ili automatske kvote na razini aplikacije ili ograničavanje publike na razini kampanje možete spriječiti osobe primanje određene automatske čak i ako ih ne ispunjava vaše ciljnih kriterije. 
- Postavljanje "Jezik" razlikuje se od ciljanja na temelju države ili regionalne sheme koja je i razlikuje se od ciljanja ovisno o tome zemlj mjesto na temelju telefona mjesto ili na drugom mjestu GPS.
- Poruka "zadanom jeziku" šalje se na bilo kojem klijentu koji nema svoj uređaj postavite na jednu od druge jezike koji navedete.


## <a name="push-scheduling"></a>Automatske zakazivanje

### <a name="issue"></a>Problem
- Planiranje automatske ne funkcionira očekivani (poslati previše Prijevremeni ili odgođeno).

### <a name="causes"></a>Uzroci

- Vremenske zone možete probleme s planiranje, osobito kada koristite krajnjim korisnicima vremensku zonu.
- Automatske napredne značajke može usporiti ih gura.
- Određivanje koji se temelji na telefonu postavke (umjesto aplikacije informacije oznake) možete odgoditi ih gura jer Azure Mobile radnje možda da biste zatražili podatke u stvarnom vremenu telefona prije slanja na automatske.
- Kampanje stvorena bez završni datum lokalnu pohranu poruka s automatske na uređaju i prikaz rezultata prilikom sljedećeg otvaranja aplikacije čak i ako kampanje ručno je završen.
- Pokretanje više od jedne kampanje u isto vrijeme može potrajati dulje vrijeme da biste pregledali korisnika base (pokušaj samo započnite kampanje s najviše četiri, također cilj samo na aktivni korisnici tako da se stari korisnici ne moraju biti skenirana).
- Ako koristite mogućnost "Publike Zanemari automatske poslat će korisnicima putem na API-JA" u odjeljku "Kampanje" s kampanjom razgovor kampanje će automatski poslati, morat ćete ručno slanje putem API dosegne.
- Ako koristite prilagođenu kategoriju u razgovor da biste prikazali obavijesti u aplikaciji, ćete morati slijedite odgovarajuće-vijek obavijesti jer inače obavijesti neće se izbrisati kada korisnik odbacite je.

 
