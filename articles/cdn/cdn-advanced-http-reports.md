<properties
    pageTitle="Azure CDN dodatna izvješća HTTP | Microsoft Azure"
    description="Dodatna izvješća za HTTP-a u programu Microsoft Azure CDN. Ta izvješća pružaju podrobne informacije o CDN aktivnosti."
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

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Dodatna izvješća HTTP-a u programu Microsoft Azure CDN

## <a name="overview"></a>Pregled

Ovaj dokument objašnjava napredne HTTP izvješćivanje o pogreškama u programu Microsoft Azure CDN. Ta izvješća pružaju podrobne informacije o CDN aktivnosti.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Pristup dodatna izvješća HTTP

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Zadržite pokazivač na kartici **analize** , a zatim zadržite pokazivač na Potpaleta **Napredne HTTP izvješća** .  Kliknite **veliki platformu HTTP**.

    ![Portal za upravljanje CDN - izbornik dodatna izvješća](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Prikazuju se mogućnosti izvješća.

## <a name="geography-reports-map-based"></a>Zemljopis izvješća (temeljenih na karti)

Postoji pet izvješća koja iskoristiti karte da biste naznačili područja iz kojeg je Tražena sadržaj. Ta izvješća su svijeta karte, karta Sjedinjenih Američkih Država, Kanada karte, karta Europe i karte Pacifička Azija.

Svako izvješće temeljenih na karti rangira geografske entiteti (odnosno Državama, stanja i provinces) prema postotku pristupa koji potječu iz to područje. Uz to, kartu dostupni su vam vizualizaciju mjesta iz kojeg je Tražena sadržaj. Preporučuje se moći učiniti tako da dostupnijima svaku regiju prema količinu zahtjev iskusnih u tom području. Lighter osjenčani područja označavaju donjem zahtjev za sadržaj, dok tamnije područja označili više razine zahtjev za sadržaj.

Detaljne informacije o promet i propusnosti za svako područje navedeni su neposredno ispod kartu. Time se omogućuje vam da biste vidjeli ukupan broj pristupa, postotak pristupa, a zatim ukupnu količinu podataka prenose (gigabajta) te postotak podataka prenijeli za svako područje. Za svaki od tih metriku pogledati opis. Na kraju, kada pokazivač miša postavite iznad područja (odnosno države, država ili županija), naziv i postotak pristupa do kojih je došlo u regiji prikazat će se kao naziv alata.

Kratak opis je navedene u nastavku za svaku vrstu izvješća temeljenih na karti Zemljopis.

Naziv izvješća | Opis
------------|------------
Karta u svijetu | Izvješće omogućuje prikaz diljem svijeta zahtjev za CDN sadržaj. Svakoj državi označena je različitim bojama na karti svijeta da biste naznačili postotka pristupa koji potječe od to područje.
Karta Sjedinjenih Američkih Država | Izvješće omogućuje prikaz zahtjev za CDN sadržaj u Sjedinjenim Američkim Državama. Svakog pojedinog statusa je označena bojom na tu mapu da biste naznačili postotka pristupa koji potječe od to područje.
Kanada karte | Izvješće omogućuje prikaz zahtjev za CDN sadržaj u Kanadi. Svaki županija je označena bojom na tu mapu da biste naznačili Postotak uspješnih koji potječe od to područje.
Karta Europe | Izvješće omogućuje prikaz zahtjev za CDN sadržaj u Europi. Svakoj državi je označena bojom na tu mapu da biste naznačili Postotak uspješnih koji potječe od to područje.
Azija Pacifik karte | Izvješće omogućuje prikaz zahtjev za sadržaj CDN Azija. Svakoj državi je označena bojom na tu mapu da biste naznačili postotka pristupa koji potječe od to područje.

## <a name="geography-reports-bar-charts"></a>Zemljopis izvješća (trakaste)

Postoje dva izvješća dodatne koji omogućuju statističke podatke prema Zemljopis koji su gradovi vrha i vrha države. Ta izvješća rangiranje gradova i zemljama, odnosno prema broj pristupa koji potječe od tih područja. Nakon generiranje ovu vrstu izvješća na trakastom grafikonu označava gornji 10 gradova ili država koje su zatražili sadržaja putem određene platforme. U ovom trakasti grafikon omogućuje brzo procijenite regije koje generiranje najveći broj zahtjeva za sadržaj.

Lijeve strane grafikona (os y) upućuje na to koliko pristupa došlo je do u području navedeni. Neposredno ispod na grafikonu (os x), za svaku gornju 10 područja tražit će natpis.

### <a name="using-the-bar-charts"></a>Korištenje trakasti grafikoni

* Ako zadržite pokazivač na traku, naziv i ukupan broj uspješnih do kojih je došlo u regiji prikazat će se kao naziv alata.
* Zaslonski opis za izvješće vrha gradova označava grad tako da države/županije, naziv i kratica država.
* Ako nije moguće odrediti grad ili regije (odnosno Savezna država/pokrajina) iz koje potječe zahtjev, pa ga označava da su Nepoznato. Ako je država Nepoznato, a zatim dva upitnik (odnosno??), će se prikazati.
* Izvješća mogu sadržavati metriku za "Europe" ili "Države Pacifičke regiju." Te stavke nisu namijenjenu navedite statističke podatke na svim IP adresama u ta područja. Umjesto toga mogu primijeniti samo na zahtjeve za koji potječu iz IP adrese koje su širenje putem Europe ili države/Pacifičke umjesto da biste određeni grad i država.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Tamo ćete pronaći ukupan broj pristupa, Postotak uspješnih, količinu podataka prenose (gigabajta) te postotak podataka prenijeli regije vrha 250. Za svaki od tih metriku pogledati opis.

Za obje vrste izvješća u nastavku navedeni kratak opis.

Naziv izvješća | Opis
------------|------------
Vrh gradova | Izvješće rangira gradova prema broj pristupa koji potječu iz to područje.
Gornji države | Izvješće rangira države prema broj pristupa koji potječu iz to područje.

## <a name="daily-summary"></a>Dnevni sažetak

Dnevni sažetak omogućuje da biste vidjeli ukupan broj pristupa i podaci koji se prenose putem određenu platformu po danu. Ove informacije mogu se brzo razlikovati uzorke CDN aktivnosti. Na primjer, izvješće omogućuju otkriva koji su dani iskusnih veća ili manja od očekivanih promet.

Nakon generiranje ovu vrstu izvješća na trakastom grafikonu pronaći ćete vizualno upućuju kao količinu ovisne zahtjev iskusnih po danu tijekom vremenskog razdoblja obuhvaćenog izvješćem. To ćete učiniti prikazom trake za svaki dan u izvješću. Ako, na primjer, odaberite vremensko razdoblje pod nazivom "Tjedan" generirat će trakasti grafikon s trakama sedam. Svaka traka označava ukupan broj uspješnih iskusnih na taj dan.

Lijeve strane grafikona (os y) upućuje na to koliko pristupa došlo je do određenog datuma. Neposredno ispod na grafikonu (os x), vidjet ćete oznaku koja označava datum (oblik: gggg-MM-DD) za svaki dan uvrstiti u izvješće.

> [AZURE.TIP] Ako zadržite pokazivač na traku, prikazat će se ukupan broj uspješnih nastalih na taj datum kao naziv alata.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Tamo ćete pronaći ukupan broj uspješnih i količinu podataka koji se prenose (gigabajta) za svaki dan obuhvaćenog izvješćem.

## <a name="by-hour"></a>Sat

Izvješće po satu omogućuje da biste vidjeli ukupan broj pristupa i podaci koji se prenose putem određenu platformu na osnovi svaki sat. Ove informacije mogu se brzo razlikovati uzorke CDN aktivnosti. Na primjer, izvješće omogućuju otkriti vremenska razdoblja tijekom dana koje je manji od očekivanih promet ili noviji.

Nakon generiranje ovu vrstu izvješća na trakastom grafikonu dat će vizualno upućuju kao količinu ovisne zahtjev naišao na svaki sat temelj tijekom vremenskog razdoblja obuhvaćenog izvješćem. To će učiniti prikazom trake za svaki sat prekriveni izvješća. Na primjer, odaberete 24 sata vremensko razdoblje generirat će trakasti grafikon s trakama od dvadeset četiri. Svaka traka označava ukupan broj uspješnih iskusnih tijekom tog sat.

Lijeve strane grafikona (os y) upućuje na to koliko pristupa došlo je do na određeni sat. Neposredno ispod na grafikonu (os x), vidjet ćete oznaku koja označava datuma i vremena (oblik: gggg-MM-DD hh) za svaki sat uvrstiti u izvješće. Vrijeme prijavljuje pomoću obliku 24 sata, a zatim je naveden pomoću UTC-GMT vremensku zonu.

> [AZURE.TIP] Ako zadržite pokazivač na traku, ukupan broj uspješnih kojih je došlo tijekom tog sat prikazat će se kao naziv alata.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Tamo ćete pronaći ukupan broj uspješnih i količinu podataka koji se prenose (gigabajta) za svaki sat prekriveni izvješća.

## <a name="by-file"></a>Datoteka

Izvješće o datoteka za tako da omogućuje prikazivanje količinu zahtjev i promet nastale tijekom određenog platformu za većinu tražene resursi. Nakon generiranje ovu vrstu izvješća trakastog grafikona bit će načinjena na 10 najčešće tražene imovinu tijekom određenog razdoblja.

> [AZURE.NOTE] Za potrebe ovog izvješća, rub CNAME URL-ovi pretvaraju se u ekvivalentne CDN URL-ove. Time se točne bilježi za ukupan broj uspješnih pridružene sredstvo bez obzira na to CDN ili rub CNAME URL-a koriste ga zatražiti.

Lijeve strane grafikona (os y) označava broj zahtjeva za svaku imovinu tijekom određenog razdoblja. Izravno ispod grafikona (os x) pronaći ćete oznaku koja označava naziv datoteke za svaki od 10 tražene resursi.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Postoji vidjet ćete sljedeće informacije za svaki od 250 tražene imovine: relativni put, ukupnog broja pristupa, postotka pristupa, količinu podataka koji su preneseni (u gigabajta) i prenijeti postotka podataka.

## <a name="by-file-detail"></a>Datoteka svim proizvodima

Izvješće datoteke detalja omogućuje prikaz količinu zahtjev i promet nastale tijekom određenog platformu za određene resursa. Pri vrhu ovog izvješća je mogućnost datoteke detalje za. Ta mogućnost daje popis najčešće tražene imovine na odabrani platformi. Da biste generirali izvješće datoteke detalj, morat ćete odaberite željeni imovinu putem mogućnosti datoteka detalje za. Nakon kojeg, na trakastom grafikonu će vas upozoriti količinu dnevni zahtjev koji je generirao tijekom određenog razdoblja.

Lijeve strane grafikona (os y) pokazuje ukupan broj zahtjeva da je sredstvo naišao na određeni dan. Neposredno ispod na grafikonu (os x), vidjet ćete oznaku koja označava datum (oblik: gggg-MM-DD) za koje CDN prijavljena je zahtjev za sredstvo.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Tamo ćete pronaći ukupan broj uspješnih i količinu podataka koji se prenose (gigabajta) za svaki dan prekriveni izvješća.

## <a name="by-file-type"></a>Prema vrsti datoteke

Izvješće po vrsti datoteka omogućuje prikaz količinu zahtjev i promet nastale prema vrsti datoteke. Nakon generiranje ovu vrstu izvješća, prstenastih grafikona će vas upozoriti Postotak uspješnih generira gornji vrste datoteku 10.

> [AZURE.TIP] Ako pokazivač isječak prstenastih grafikona Internet media vrste vrsta datoteke će se prikazati kao naziv alata.

Podaci koji je korišten za generiranje prstenastih grafikona možete prikazati ispod njega. Tamo ćete pronaći vrstu medijskih sadržaja proširenje/Internet naziv datoteke, ukupan broj uspješnih, postotak pristupa, količinu podataka prenose (gigabajta) te postotak podataka prenijeli za svaku od vrha 250 vrsta datoteka.

## <a name="by-directory"></a>Po direktorija

Izvješće direktorija za tako da omogućuje prikaz količinu zahtjev i promet nastale tijekom određenog platformu za sadržaja s određenim direktorija. Nakon generiranje ovu vrstu izvješća na trakastom grafikonu će vas upozoriti ukupan broj uspješnih generira sadržaja u gornjoj 10 direktorija.

### <a name="using-the-bar-chart"></a>Trakasti grafikon možete koristiti

* Postavite pokazivač miša iznad trake da biste pogledali relativni put do odgovarajuće direktorija.
* Sadržaj pohranjen u podmapu direktorij ne broji pri izračunu zahtjev po direktorija. Ovaj izračun ovisi isključivo s broj zahtjeva za sadržaj koji su pohranjenu u direktoriju stvarni generira.
* Za potrebe ovog izvješća, rub CNAME URL-ovi pretvaraju se u ekvivalentne CDN URL-ove. Time se točne bilježi za sve statistiku pridružene sredstvo bez obzira na to CDN ili rub CNAME URL-a koriste ga zatražiti.

Lijeve strane grafikona (os y) pokazuje ukupan broj zahtjeva za sadržaj koji je spremljen u vašem direktorija prvih 10. Svaka traka na grafikonu predstavlja direktorij. Koristite shemu color-coding tako da odgovara trake direktorij navedene u odjeljku vrha 250 cijelog direktorija.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Postoji vidjet ćete sljedeće informacije za svaki od vrha 250 direktorija: relativni put, ukupnog broja pristupa, Postotak uspješnih, količinu podataka koji su preneseni (u gigabajta) i prenijeti postotak podataka.

## <a name="by-browser"></a>U pregledniku

Izvješće preglednika tako da omogućuje prikaz kojim preglednicima su se koristile zatražite sadržaj. Nakon generiranje ovu vrstu izvješća tortni grafikon će vas upozoriti postotak zahtjeva za obrađuje Najčešći web-preglednici 10.

### <a name="using-the-pie-chart"></a>Korištenje tortni grafikon

* Prijeđite pokazivačem preko isječak na tortnom grafikonu da biste vidjeli naziv i verzija preglednika.
* Za potrebe ovog izvješća, svaku kombinaciju jedinstveni/verziju preglednika smatra se neki drugi preglednik.
* Odsječak pod nazivom "Ostalo" prikazuje postotak zahtjeva za obrađuje drugim preglednicima i verzije.

Podataka koji je korišten za generiranje tortnog grafikona možete prikazati ispod njega. Tamo ćete pronaći broj vrsta/verzije preglednika, ukupan broj uspješnih i postotak pristupa za svaku od 250 preglednika.

## <a name="by-referrer"></a>Po Referrer

Izvješće Referrer tako da omogućuje prikazivanje Najbolji preporučitelji sadržaja na odabrani platformi. Na referrer označava iz kojeg je generirana zahtjeva za naziv glavnog računala. Nakon generiranje ovu vrstu izvješća na trakastom grafikonu označava količinu zahtjev (odnosno pristupa) generira Najbolji preporučitelji 10.

Lijeve strane grafikona (os y) pokazuje ukupan broj zahtjeva da iskustvom sredstvo za svaku referrer. Svaka traka na grafikonu predstavlja na referrer. Koristite shemu color-coding tako da odgovara trake da biste referrer navedene u odjeljku Referrer 250 vrha.

Podaci koji je korišten za generiranje na trakastom grafikonu mogu se pregledati ispod njega. Tamo ćete pronaći URL-a, ukupan broj uspješnih i Postotak uspješnih stvorene iz svih Najbolji preporučitelji 250.

## <a name="by-download"></a>Preuzeti

Izvješće, preuzmite omogućuje da biste analizirali uzoraka preuzimanje za većinu tražene sadržaj. Vrh izvješće sadrži trakasti grafikon koji uspoređuje pokušaja preuzimanja s dovršene preuzimanja za 10 tražene resursi. Svaka traka označena je različitim bojama prema tome je na pokušaj preuzimanjem (plava) ili dovršene preuzimanje (zelena).

> [AZURE.NOTE] Za potrebe ovog izvješća, rub CNAME URL-ovi pretvaraju se u ekvivalentne CDN URL-ove. Time se točne bilježi za sve statistiku pridružene sredstvo bez obzira na to CDN ili rub CNAME URL-a koriste ga zatražiti.

Na lijevoj strani graph (os y) označava naziv datoteke za svaki od 10 tražene resursi. Neposredno ispod grafikona (os x) tražit će oznake koje označavaju ukupni broj pokušaja/dovršiti preuzimanja.

Izravno ispod trakasti grafikon sljedeći podaci će se prikazati 250 tražene imovine: relativni put (uključujući naziv datoteke), broj koji ste preuzeli do završetka posla, broj koji je to od vas traži i postotak zahtjeva za koja je rezultirala dovrši preuzimanje.

> [AZURE.TIP] Naš CDN se obavještavaju klijent HTTP (odnosno preglednik) kada sredstvo sadrži potpuno preuzeta. Zbog toga imamo da biste izračunali li sredstvo sadrži potpuno preuzeta prema šiframa statusa i bajt raspon zahtjeva. Najprije ćemo potražite pri stvaranju tog izračuna je li zahtjev rezultira kod 200 stanja u redu. Ako je tako, zatim ćemo pogledajte bajt raspon zahtjeva da bi oni pokrivaju cijelu resursa. Na kraju, ne možemo usporedite količinu podataka koji su preneseni na veličinu tražene resursa. Ako se podaci koji se prenose jednaka ili veća od veličine datoteke, a zahtjeva za bajt raspon prikladna za tog resursa, na rezultat će broji kao dovršeno preuzimanje.
>
>Zbog interpretive prirode izvješće koje treba Imajte na umu sljedeće točke koje mogu mijenjati dosljednost i točnosti izvješća.
>
>* Uzorci promet ne može se točno predviđati kada se drugačije ponašaju korisničkih agenata. To može dati dovršeni Preuzmi rezultate koje su veće od 100%.
>* Sredstva koja iskoristili HTTP progresivno preuzimanje možda nije precizno predstavljen brojem izvješća. To su korisnici traže na različita mjesta u videozapisu.

## <a name="by-404-errors"></a>Po 404 pogreške

Izvješće 404 pogreške vam omogućuje da utvrdite vrste sadržaja koje generira najveći broj šifre statusa 404 nije pronađeno. Vrh izvješće sadrži trakasti grafikon prvih 10 imovine za koje je vratio 404 nije pronađeno Šifra stanja. U ovom trakasti grafikon uspoređuje ukupan broj zahtjeva zahtjevima koja je rezultirala 404 nije pronađeno kod stanja te imovine. Svaka traka je bojom. Da biste naznačili zahtjev rezultirala 404 nije pronađeno Šifra stanja koristi se žutu traku. Da biste naznačili ukupan broj zahtjeva za sredstvo koristi se crvenu traku.

> [AZURE.NOTE] Za potrebe ovog izvješća, imajte na umu sljedeće:
>
>* Na uspješnosti predstavlja svaki zahtjev za sredstvo bez obzira na to Šifra stanja.
>* Rub CNAME URL-ovi pretvaraju se u njihove ekvivalentne CDN URL-ova. Time se točne bilježi za sve statistiku pridružene sredstvo bez obzira na to CDN ili rub CNAME URL-a koriste ga zatražiti.

Na lijevoj strani graph (os y) pokazuje naziv datoteke za svaki unos u gornjoj 10 tražene imovine koja je rezultirala 404 nije pronađeno Šifra stanja. Neposredno ispod na grafikonu (os x), vidjet ćete oznake koje označavaju ukupni broj zahtjeva i broj zahtjeva koja je rezultirala 404 nije pronađeno Šifra stanja.

Izravno ispod trakasti grafikon sljedeći podaci će se prikazati 250 tražene imovine: relativni put (uključujući naziv datoteke), broj zahtjeva koja je rezultirala 404 nije pronađeno Šifra stanja, ukupan broj puta zatraženo je imovina i postotak zahtjeva za koja je rezultirala 404 nije pronađeno Šifra stanja.

## <a name="see-also"></a>Vidi također
* [Pregled Azure CDN](cdn-overview.md)
* [U stvarnom vremenu stat u Microsoft Azure CDN](cdn-real-time-stats.md)
* [Nadjačavanje zadano HTTP ponašanje pomoću modul pravila](cdn-rules-engine.md)
* [Analiza performansi rub](cdn-edge-performance.md)
