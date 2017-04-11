<properties
    pageTitle="Analiza performansi rub u Azure CDN | Microsoft Azure"
    description="Analiza performansi čvor rub u programu Microsoft Azure CDN. Rub performanse Analytics nudi promet i propusnosti korištenja zrnastog informacija na CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analiza performansi čvor rub u Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Pregled
Rub performanse analytics nudi promet i propusnosti korištenja zrnastog informacija na CDN. Ove informacije možete iskoristiti za generiranje trendova Statistika koje omogućuju vam da bi se dobio uvid u kako se vaše imovine predmemorirani i isporučuju klijentima. U nizu, to vam omogućuje da obrazac strategije na kako optimizirati isporuku sadržaja, a da biste odredili što probleme treba tackled da biste bolje leverage u CDN. Zbog toga ne samo će moći da biste poboljšali performanse isporuke podataka, ali i moći smanjiti troškove CDN.

> [AZURE.NOTE] Sva izvješća pomoću UTC-GMT notaciju pri određivanju datuma/vremena.

## <a name="reports-and-log-collection"></a>Prikupljanje zapisnika i izvješća

CDN aktivnosti podataka mora biti prikuplja modul rub performanse analize prije nego što možete generirati izvješća na njemu. Ovaj postupak zbirke pojavljuje se jednom dnevno i pokriva aktivnosti koje bilo tijekom na prethodni dan. To znači da se statističkih podataka u izvješću predstavljaju uzorak dnevnih Statistika trenutku je obrađen, a ne moraju nužno biti sadržavati potpunog skupa podataka za trenutni dan. Funkciju primarni takva izvješća je procijenite performansi. Oni se ne smiju koristiti za naplatu svrhe ili točno numerički Statistika.

> [AZURE.NOTE] Sirovim podacima iz koje se generiraju rub performanse analitički izvješća dostupna je za barem 90 dana.

## <a name="dashboard"></a>Nadzorna ploča

Na nadzornoj ploči analitike performanse rub prati trenutni i prošli CDN prometa na grafikon i statističkih podataka. Koristite ovu nadzornu ploču da biste otkrili nedavnih i Dugoročne trendova na performanse CDN promet za vaš račun.

Ove nadzorne ploče sastoji se od:

* Interaktivni grafikon koji omogućuje vizualizacije ključa metriku i trendova.
* Vremensku crtu koja daje uvid dugoročnu uzoraka za ključne metriku i trendova.
* Ključni metriku i statističkih podataka o našu mrežu CDN poboljšava mjeri se cjelokupni performanse, korištenje i učinkovitosti promet za web-mjesta.

### <a name="accessing-the-edge-performance-dashboard"></a>Pristup na rub nadzorna ploča

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-edge-performance/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Zadržite pokazivač na kartici **analize** , a zatim zadržite pokazivač na **Rub Perfomance analize** Potpaleta.  Kliknite na **nadzornoj ploči**.

    Prikazat će se na nadzornoj ploči analitike čvor ruba.

### <a name="chart"></a>Grafikon

Na nadzornoj ploči sadrži grafikon koji prati metrike putem vremensko razdoblje odabran na vremensku crtu koja se pojavljuje izravno ispod njega.  Vremenske crte gore li grafova do zadnje dvije godine CDN aktivnosti prikazat će se neposredno ispod grafikona.

#### <a name="using-the-chart"></a>Korištenje grafikona

* Prema zadanim postavkama, stope učinkovitosti predmemoriju za zadnjih 30 dana će biti ucrtavaju u grafikon.
* Na grafikonu se generira iz podataka složeno za uvez po danu.
* Zadrži pokazivač na dan na grafikonu retka označava datum i vrijednost metriku na taj datum.
* Kliknite Istakni vikenda da biste se prebacivali te osnovnu sivi okomite crte koje predstavljaju vikenda na grafikon. Ta vrsta preklapanja je korisno za prepoznavanje uzorke promet tijekom vikenda.
* Kliknite prikaz jednu godinu dana da biste se prebacivali te aktivnosti prethodne godine iznad u istom vremenskom razdoblju na grafikon. Ovu vrstu usporedbe daje uvid u Dugoročne uzoraka korištenja CDN. U gornjem desnom kutu grafikona sadrži legendu koji označava kod boje za svaki redak grafikonu.

#### <a name="updating-the-chart"></a>Ažuriranje grafikona

* Vremenski raspon: Učinite nešto od sljedećeg:
    * Odaberite željeno područje na vremenskoj crti. Grafikon će se ažurirati s podacima koji odgovara odabranom vremenskom razdoblju.
    * Dvokliknite željeni grafikon da biste prikazali sve dostupne povijesne podatke maksimalno dvije godine do.
* Mjerenje: Kliknite ikonu grafikona koji će se prikazati pokraj željenog metriku. Grafikon i vremenske trake će se osvježiti s podacima za odgovarajuće metriku.


### <a name="key-metrics-and-statistics"></a>Ključni metriku i statističkih podataka

#### <a name="efficiency-metrics"></a>Učinkovitosti mjerenja

Svrha ove metriku je da biste vidjeli hoće li se predmemorije učinkovitosti mogu poboljšati. Pogodnosti izvedene iz predmemorije učinkovitosti su:

* Smanjene opterećenje poslužitelja polazište što može dovesti do:
    * Bolje performanse web poslužitelja.
    * Smanjene radu troškove.
* Poboljšani ubrzanja isporuku podataka Budući da zahtjevi će posluživanje izravno iz sustava CDN.

Polje | Opis
------|------------
Predmemorija učinkovitosti | Prikazuje postotak podataka koji se prenose koja je služila iz predmemorije. Ovaj metričkim mjere kada predmemorirane verzije tražene sadržaja je poslužena izravno iz CDN (edge poslužitelji) requesters (npr., web-preglednik)
Kliknite stopa | Prikazuje postotak zahtjevi za koje su poslužena iz predmemorije. Ovaj metričkim mjere kada predmemorirane verzije traženi sadržaja je poslužena izravno iz CDN (edge poslužitelji) da biste requesters (npr., web-preglednik).
% Udaljenu bajtova - bez Config predmemorije | Prikazuje postotak prometa čije je slanje s poslužitelja polazište CDN (edge poslužitelji) ne spremit će se kao rezultat značajku zaobilazak predmemorije (HTTP pravila modul).
% Udaljene bajtova - istekle predmemorije | Prikazuje postotak prometa čije je slanje s poslužitelja polazište CDN (edge poslužitelji) kao rezultat zastarjele revalidation sadržaja.

#### <a name="usage-metrics"></a>Korištenje mjerenja

Svrha ove metriku jest uvid u sljedeće mjere trošak izrezivanje:

* Minimiziranje radu troškove kroz na CDN.
* Smanjivanje CDN potrošite putem predmemorije učinkovitosti i sažimanja.

> [AZURE.NOTE] Promet glasnoću brojevi predstavljaju prometa čije je koristiti u izračunima od omjera i postotaka i možda prikazuje samo dio ukupni promet za kupce visoku glasnoću.

Polje | Opis
------|------------
Spremi bajtova izgleda | Označava Prosječan broj bajtova prenijeti za svaki zahtjev za slanje iz CDN (edge poslužitelji) tražitelju (npr., web-preglednik).
Nema stopa Config bajt predmemorije | Prikazuje postotak prometa slanje CDN (edge poslužitelji) tražitelju (npr., web-preglednik) koji će se spremiti zbog značajku zaobilazak predmemoriju.
Komprimirana bajt stopa | Prikazuje postotak prometa poslali CDN (edge poslužitelji) requesters (npr., web-preglednik) u obliku komprimirane.
Bajtova izgleda | Označava količinu podataka u bajtova koje su isporučene s CDN (edge poslužitelji) tražitelju (npr., web-preglednik).  
Bajtova u | Označava količinu podataka bajtovima šalje requesters (npr., web-preglednik) CDN (edge poslužitelji).
Alat za analizu daljinske bajtova | Označava količinu podataka u poslali CDN i kupca polazište poslužitelje CDN (edge poslužitelji).

#### <a name="performance-metrics"></a>Performanse mjerenja

Svrha ove metriku je da biste pratili cjelokupnoj izvedbi CDN za prometa.

Polje | Opis
------|------------
Brzinu prijenosa | Označava Prosječna brzina na kojoj se sadržaj prijenosa iz na CDN na tražitelju.
Trajanje | Označava Prosječno vrijeme u milisekundama, ali je isporuka sredstvo tražitelju (npr., web-preglednik).
Komprimirana zahtjev stopa | Prikazuje postotak uspješnih koje su isporučene s CDN (edge poslužitelji) tražitelju (npr., web-preglednik) u obliku komprimirane.
Stopa 4xx pogreške | Prikazuje postotak uspješnih koji je generirao 4xx kod stanja.
Stopa 5xx pogreške | Prikazuje postotak uspješnih koji je generirao 5xx kod stanja.
Pristupa | Upućuje na to koliko zahtjeva za CDN sadržaj.

#### <a name="secure-traffic-metrics"></a>Sigurna promet mjerenja

Svrha ove metriku je za praćenje performansi CDN za HTTPS promet.

Polje | Opis
------|------------
Sigurna predmemorije učinkovitosti | Prikazuje postotak podataka koji se prenose HTTPS zahtjeva koja su služila za iz predmemorije. Ovaj metričkim mjere kada predmemorirane verzije tražene sadržaja je slanje izravno iz CDN (edge poslužitelji) requesters (npr., web-preglednik) putem HTTP.
Sigurne brzinu prijenosa | Označava Prosječna brzina kojom sadržaj prijenosa iz CDN (edge poslužitelji) requesters (npr., web-poslužiteljima) putem HTTP.
Prosječna sigurne trajanje | Označava Prosječno vrijeme u milisekundama, ali je isporuka sredstvo tražitelju (npr., web-preglednik) putem HTTP.
Zaštitu pristupa | Upućuje na to koliko HTTPS zahtjeva za CDN sadržaj.
Sigurna bajtova izgleda | Označava količinu HTTPS prometa, u bajtova koje su isporučene s CDN (edge poslužitelji) tražitelju (npr., web-preglednik).

## <a name="reports"></a>Izvješća

Svako izvješće u ovom modulu sadrži grafikon i statističkih podataka o korištenju propusnost i promet za različite vrste metriku (npr., HTTP kodovima stanja, kodovi stanja predmemorije, zatražite URL-a, itd.). Ove informacije mogu se koristiti delve dublju u kako sadržaj je u tijeku slanje klijentima i Prilagodba ponašanja CDN radi poboljšanja performansi za isporuku podataka.

### <a name="accessing-the-edge-performance-reports"></a>Pristup izvješća o izvedbi rub

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-edge-performance/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Zadržite pokazivač na kartici **analize** , a zatim zadržite pokazivač na **Rub Perfomance analize** Potpaleta.  Kliknite na **HTTP velike objektu**.

    Prikazat će se zaslon rub čvor analize izvješća.

Izvješće | Opis
-------|------------
Dnevni sažetak | Omogućuje vam da biste vidjeli dnevnih trendova promet tijekom određenog vremenskog razdoblja. Svaka traka na ovom grafikonu predstavlja određenog datuma. Veličina trake pokazuje ukupan broj uspješnih nastalih na taj datum.
Svaki sat sažetka | Omogućuje vam da biste vidjeli zaračunava promet trendova tijekom određenog vremenskog razdoblja. Svaka traka na ovom grafikonu predstavlja jedan sat na određeni datum. Veličina trake pokazuje ukupan broj uspješnih kojih je došlo tijekom tog sat.
Protokola | Prikazuje analitički promet između HTTP i HTTPS protokola. Prstenastih grafikona prikazuje postotak uspješnih nastalih za svaku vrstu protokol.
HTTP metode | Omogućuje saznati brzi koje HTTP metode koriste da bi se zatražila podataka. Su obično najčešće metode zahtjev HTTP GET, GLAVE i objavu. Prstenastih grafikona prikazuje postotak uspješnih nastalih za svaku vrstu HTTP zahtjev način.
URL-ovi | Sadrži grafikon koji prikazuje 10 tražene URL. Za sve URL-ove prikazat će se na trake. Visina trake upućuje na to koliko pristupa koji određeni URL generirao tijekom vremenskog razdoblja obuhvaćenog izvješćem. Statistika za 100 najvećih zatražio URL-ovi prikazuju se neposredno ispod ovom grafikonu.
CNAMEs | Sadrži grafikon koji prikazuje prvih 10 CNAMEs koristi da bi se zatražila imovine s vremenom obuhvaćaju u izvješću. Statistika za 100 najvećih zatražio CNAMEs prikazuju se neposredno ispod ovom grafikonu.
Drugačijeg izvora | Sadrži grafikon koji se prikazuje na vrhu 10 CDN ili kupca polazište poslužitelje imovine iz koje su zatražili navedenom vremenskom razdoblju. Statistika za 100 najvećih zatražio CDN ili korisniku polazište poslužitelje prikazuju se neposredno ispod ovom grafikonu. Poslužitelji polazište kupca prepoznaju po nazivu definirani naziv direktorija mogućnost.
Iskače zemlj. | Prikazuje koliko je prometa usmjeruje putem na određeni točke-od-značajke prisutnosti (POP). Tri slova skraćenicu predstavlja POP u našem CDN mreži.
Klijenti | Sadrži grafikon koji prikazuje gornji 10 klijenti potrebne imovine tijekom određenog razdoblja. Za potrebe ovog izvješća, sve zahtjeve za koja proizlazi iz istu IP adresu smatraju se putem klijentskog programa isti. Statistika za gornje 100 klijenata prikazuju se neposredno ispod ovom grafikonu. Izvješće koristan je za određivanje preuzimanje aktivnosti uzoraka gornji klijentima.
Statusi predmemorije | Daje detaljne analitički ponašanja predmemorije, koji mogu otkriti postupke za poboljšanje ukupan doživljaj krajnjeg korisnika. Budući da se najbrže performanse potječe iz predmemorije pristupa, možete optimizirati podataka isporuku brzine minimiziranje neuspjele akcije u predmemoriji i pronalaženje objekata u predmemoriji istekle.
Nema detalja | Sadrži grafikon koji se prikazuje u gornjoj 10 URL-ova za resursi za koju se predmemorije sadržaja svježina je provjerena navedenom vremenskom razdoblju. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
Detalji o CONFIG_NOCACHE | Sadrži grafikon koji prikazuje prvih 10 URL-ova za resursi koji su predmemorirani zbog konfiguracije CDN klijenta. Ove vrste amortizacije su poslužena izravno s poslužitelja polazište. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
UNCACHEABLE pojedinosti | Sadrži grafikon koji prikazuje gornji URL-ova za 10 za resursi koji nije moguće predmemorirani podaci zaglavlja zahtjev. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
Detalji o TCP_HIT | Sadrži grafikon koji prikazuje gornji 10 URL-ove za sredstva koja se odmah služila iz predmemorije. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
Detalji o TCP_MISS | Sadrži grafikon koji se prikazuje u gornjoj 10 URL-ova za sredstva koja imaju status predmemorije TCP_MISS. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
Detalji o TCP_EXPIRED_HIT | Sadrži grafikon koji prikazuje gornji 10 URL-ove za zastarjele sredstva koja su služila izravno iz sustava POP. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
Detalji o TCP_EXPIRED_MISS | Sadrži grafikon koji se prikazuje u gornjoj 10 URL-ova za zastarjele resursi za koje je nova verzija morali biti dohvaćeni iz izvorni poslužitelj. Statistika za gornje URL-ova za 100 za te vrste sadržaja prikazuju se neposredno ispod ovom grafikonu.
Detalji o TCP_CLIENT_REFRESH_MISS | Sadrži trakasti grafikon koji prikazuje prvih 10 URL-ovi za resursi nisu dohvaćeni iz izvorni poslužitelj zbog bez predmemorije zahtjeva putem klijentskog programa. Statistika za gornje URL-ova za 100 za sljedeće vrste zahtjeve prikazuju se neposredno ispod na grafikonu.
Vrstama klijenata za zahtjev | Navodi vrstu zahtjeva za koje je načinio klijenti HTTP (npr., preglednicima). Izvješće sadrži prstenastih grafikona koji omogućuje osjećaj kao kako se obrađuju zahtjeve. Informacije o propusnosti i promet za svaku vrstu zahtjev prikazuje se ispod grafikona.
Korisnički Agent | Sadrži prikaz gornje agenata 10 korisnika da bi se zatražila sadržaj putem naš CDN trakasti grafikon. Korisnički agent obično je web-preglednik, reproduktor ili mobilni telefon preglednika. Statistika za gornje 100 korisničkih agenata prikazuju se neposredno ispod na grafikonu.
Preporučitelji | Sadrži prikaz Najbolji preporučitelji 10 sadržaja na kojoj se pristupa putem naš CDN trakasti grafikon. Obično je referrer je URL web-stranicu ili resursa koja je povezana s sadržaj. Detaljne informacije je namijenjeno ispod grafikon 100 najvećih preporučitelji.
Vrste sažimanja | Sadrži prstenastih grafikona koji raščlanjuje tražene imovine po tome su spojene po naše edge poslužitelji. Postotak Komprimirana imovine podijeljene po vrsti sažimanja koristi. Detaljne informacije nalaze se dolje na grafikonu za svaku vrstu sažimanja i status.
Vrste datoteka | Sadrži trakasti grafikon koji prikazuje gornji vrste datoteku 10 koje tražene kroz naš CDN za vaš račun. Za potrebe ovog izvješća, vrstu datoteke definira datotečni nastavak sredstva i vrsta medija Internet (– primjerice, .html \[tekst/html\], .htm \[tekst/html\], .aspx \[tekst/html\], itd.). Detaljne informacije nalaze se dolje na grafikonu za gornje vrste 100 datoteka.
Jedinstveni datoteke | Sadrži grafikon koji iscrtava ukupan broj jedinstvenih imovine koje su potrebne za određeni dan tijekom određenog razdoblja.
Sažetak tokena provjere autentičnosti | Sadrži tortni grafikon koji sadrži kratak pregled na tome su tražene imovine zaštićen provjere autentičnosti utemeljene na Token. Zaštićeni imovine prikazuju se na grafikonu prema rezultata njihovih koju ste pokušali izvršiti provjeru autentičnosti.
Detalji o Nemoj dopustiti tokena provjere autentičnosti | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje 10 zahtjeve uskraćivanje zbog provjere autentičnosti utemeljene na Token.
HTTP odgovor kodovima | Nudi razrada svega HTTP kodovi stanja (npr 200 u redu, 403 zabranjeno, 404 nije pronađeno, itd.) koje su isporučene klijentima HTTP po naše edge poslužitelji. Tortni grafikon omogućuje brzo procijenite koliko su poslužena svoje imovine. Detaljne Statistički podaci navedeni su za svaku šifru odgovora ispod grafikonu sustava.
404 pogreške | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje zahtjeva za 10 koja je rezultirala 404 nije pronađeno kod odgovor.
403 pogreške | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje zahtjeva za 10 koja je rezultirala 403 zabranjeno kod za odgovor. Šifru za odgovor 403 zabranjeno pojavljuje kada zahtjeva odbio korisnički poslužitelj(i) za izvor ili rubni poslužitelj na našem POP.
4xx pogreške | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje zahtjeva za 10 koja je rezultirala kod odgovor u rasponu od 400. Izuzeti iz izvješća su 403 nije pronađen i 404 kodovi zabranjeno odgovor. Obično kod za odgovor 4xx pojavljuje kada je zahtjev je zabranjen uslijed pogreška klijenta.
504 pogreške | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje zahtjeva za 10 koja je rezultirala 504 kod odgovor Prekoračenje vremena za pristupnik. Šifru za odgovor 504 Prekoračenje vremena za pristupnik pojavljuje kada dođe do vremenskog ograničenja prilikom HTTP proxy pokušaja komunikacije s drugi poslužitelj. Slučaju naš CDN 504 kod odgovor Prekoračenje vremena za pristupnik obično događa kada rubni poslužitelj ne može uspostaviti komunikacije s poslužiteljem polazište kupca.
502 pogreške | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje zahtjeva za 10 koja je rezultirala 502 kod odgovor neispravan pristupnik. 502 kod odgovor neispravan pristupnik se pojavljuje kada dođe do neuspio protokol HTTP između poslužitelja i HTTP proxy. Slučaju naš CDN 502 kod odgovor neispravan pristupnik obično događa kada korisnički poslužitelj(i) za polazište vraća koji nisu valjani odgovor rubni poslužitelj. Odgovor nije valjan ako se ne mogu raščlaniti ili nije potpun.
5xx pogreške | Sadrži trakasti grafikon koji omogućuje prikazivanje gornje zahtjeva za 10 koja je rezultirala kod odgovor u rasponu od 500.  Izuzeti iz izvješća su 502 neispravan pristupnik i 504 kodovi odgovor Prekoračenje vremena za pristupnik.

## <a name="see-also"></a>Vidi također
* [Pregled Azure CDN](cdn-overview.md)
* [U stvarnom vremenu stat u Microsoft Azure CDN](cdn-real-time-stats.md)
* [Nadjačavanje zadano HTTP ponašanje pomoću modul pravila](cdn-rules-engine.md)
* [Napredni HTTP izvješća](cdn-advanced-http-reports.md)
