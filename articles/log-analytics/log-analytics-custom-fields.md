<properties
   pageTitle="Prilagođena polja u prijava analitiku | Microsoft Azure"
   description="Značajka prilagođenih polja analize zapisnika omogućuje stvaranje pretraživanja polja iz OMS podataka koje se dodaju svojstva prikupljene zapis.  U ovom se članku opisuje postupak stvaranja prilagođenog polja i nudi detaljni vodič za uzorak događaj."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Prilagođena polja u zapisnik Analytics

Značajka **Prilagođenih polja** analize zapisnika omogućuje da biste proširili postojeće zapise u spremištu OMS dodavanjem pretraživanja polja.  Prilagođena polja automatski se unose iz podataka dobivenih iz ostalih svojstava u istom zapisu.

![Pregled prilagođenih polja](media/log-analytics-custom-fields/overview.png)

Na primjer, na primjer zapis ima korisne podatke negdje u opis događaja.  Izdvajanje te podatke u zasebnom svojstva, on postaje dostupan za pretraživanje kao akcije kao sortiranje i filtriranje.

![Gumb za pretraživanje zapisnika](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] U pretpregledu ste ograničeni su na 100 prilagođena polja u radnom prostoru.  To ograničenje će se proširiti kada značajka je dostigne Općenito dostupan.

## <a name="creating-a-custom-field"></a>Stvaranje prilagođenog polja

Prilikom stvaranja prilagođenog polja prijava analitiku morate razumjeti podatke koje želite koristiti za popunjavanje njegovom vrijednošću.  Koristi tehnologiju iz Microsoft Research naziva FlashExtract brzo prepoznavanje te podatke.  Umjesto potrebno unijeti eksplicitnih upute, zapisnika Analytics Pratit će o podacima koje želite izdvojiti iz primjera koje navedete.

Sljedeći odjeljci sadrže postupak stvaranja prilagođenih polja.  Pri dnu ovog članka je vodič uzorka izdvajanjem.

> [!NOTE] Prilagođeno polje se popunjava kako se dodaju u spremište podataka OMS zapise koji odgovaraju navedene kriterije tako da se pojavljuju samo na zapise koji se prikupljaju nakon stvaranja prilagođenog polja.  Prilagođeno polje neće biti dodano sa zapisima koji se već nalaze u spremištu podataka prilikom stvaranja.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Korak 1 – izdvojili zapise koji će prilagođeno polje
Prvi korak je radi određivanja zapisa koji će se prilagođenog polja.  Započnite [standardne zapisnika pretraživanja](log-analytics-log-searches.md) , a zatim odaberite zapis koji će poslužiti kao model koji prijava analitiku saznat ćete iz.  Kada odredite koje namjeravate izdvojiti podatke u prilagođena polja, **Čarobnjak za izdvajanje polje** otvorit će se mjesto Provjerite valjanost i suzite kriterij.

2. Idite na **Zapisnika pretraživanja** i koristiti [upit za dohvaćanje zapisa](log-analytics-log-searches.md) koji će imati prilagođenog polja.
2. Odaberite zapis prijava analitiku pomoću kojega će poslužiti kao model za izdvajanje podataka za popunjavanje prilagođena polja.  Navodite podatke koje želite izdvojiti iz tog zapisa, a prijava analitiku će koristiti te podatke da biste odredili logike za popunjavanje prilagođena polja za sve slične zapise.
3. Kliknite gumb s lijeve strane neko svojstvo tekst zapisa pa odaberite **Izdvoji polja iz**.
4. U **Glavnom primjeru** stupac prikazuje se **otvoren Čarobnjak za izdvajanje polja**i zapisa koji ste odabrali.  Prilagođeno polje će se definirati za zapise s iste vrijednosti u svojstvima koja nije odabrana.  
5. Ako je odabir u potpunosti ono što želite, odaberite dodatna polja da biste suzili kriterije.  Da biste promijenili vrijednosti polja za kriterije, morate prekinuti i odaberite drugi zapis koji odgovaraju kriterijima koji želite.

### <a name="step-2---perform-initial-extract"></a>Korak 2: izvođenje početne Izdvoji.
Nakon što ste označena zapise koji će imati prilagođenog polja, odredite podatke koje želite izdvojiti.  Prijava analitiku koristit će se ti podaci da biste odredili uzorke koje su slične u sličnih zapisa.  U koraku nakon toga će se da biste provjerili rezultate i sadrže dodatne pojedinosti za analizu zapisnika da biste koristili u njegov analiza.

1. Istaknite tekst u uzorka zapis koji želite popuniti prilagođenog polja.  Koju zatim prikazat će se dijaloški okvir za Navedite naziv polja i izvođenje početne Izdvoji.  Znakovi ** \_CW** će se automatski dodati.
2. Kliknite **Izdvoji** tema prikupljene zapisa.  
3. U odjeljcima **Sažetak** i **Rezultata pretraživanja** prikazati rezultate u izdvojiti tako da možete provjeriti njezinu točnost.  **Sažetak** prikazuje kriterija koji se koristi za prepoznavanje zapisa i ukupan za svaku vrijednost podataka prepoznala.  **Rezultati pretraživanja** nudi detaljni popis zapisa koji odgovaraju kriterijima.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Korak 3 – Provjerite točnost na izdvojiti i stvaranje prilagođenih polja

Kada obavite početne izdvojiti prijava analitiku prikazat će se rezultatima na temelju podataka već prikupljene.  Ako je rezultat izgleda točne možete stvoriti prilagođeno polje s daljnji rad.  Ako nije, zatim Suzite rezultate tako da se prijava analitiku možete poboljšati njegov logike.

2.  Ako sve vrijednosti u početne izdvojiti nisu točne, kliknite ikonu **Uređivanje** pokraj netočnih zapisa i odaberite **Izmijeni ovaj Označi** da biste izmijenili odabira.
3.  Stavka se kopira do odjeljka **Dodatne primjere** ispod **Glavne primjer**.  Isticanje Ovdje možete prilagoditi radi analize zapisnika razumijevanje odabira treba ste napravili.
4.  Kliknite **Izdvoji** u te nove podatke koristiti za procjenu postojeće zapise.  Rezultati se mogu izmijeniti za zapise koji nije ona koju ste upravo izmijenili koji se temelji na ovom novi obavještavanje.
5.  Nastavite s dodavanjem ispravaka dok sve zapise u izdvojiti pravilno prepoznavanje podataka za popunjavanje Nova prilagođena polja.
6. Kada ste zadovoljni rezultatom, kliknite **Spremi Izdvoji** .  Prilagođeno polje sada je definiran, ali se neće dodati sve zapise još.
7.  Pričekajte nove zapise koji se podudaraju navedene kriterije koji se prikupljaju i zatim ponovno pokrenite pretraživanje zapisnika. Novi zapisi moraju imati prilagođenog polja.
8.  Korištenje prilagođenog polja kao što su druga svojstva zapisa.  Možete koristiti za zbrajanja i grupiranje podataka i čak ga koristiti za stvaranje nove uvide.


## <a name="viewing-custom-fields"></a>Prikaz prilagođena polja
Možete prikazati popis svih prilagođenih polja u grupi Upravljanje iz pločice **Postavke** OMS nadzorne ploče.  Odaberite **podatke** , a zatim **Prilagođena polja** popis sva prilagođena polja u radnom prostoru.  

![Prilagođena polja](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Uklanjanje prilagođenog polja
Da biste uklonili prilagođenog polja na dva načina.  Prvi je mogućnost **Ukloni** za svako polje prilikom pregledavanja cjelovit popis prethodno opisan.  Drugi način je dohvaćanje zapisa, a zatim kliknite gumb s lijeve strane polja.  Na izborniku imat će mogućnost da biste uklonili prilagođenog polja.

## <a name="sample-walkthrough"></a>Vodič za uzorak

U sljedećoj sekciji vodi kroz dovršeno primjer stvaranja prilagođenog polja.  U ovom se primjeru izdvaja naziv usluge u stavkama događaja sustava Windows koje označavaju promjena stanja servisa.  To ovisi o događaje stvoren putem upravitelja kontrole servisa u zapisniku sustava na računala sa sustavom Windows.  Ako želite pratiti u ovom primjeru mora biti [Prikupljanje informacija događaje zapisnika sustava](log-analytics-data-sources-windows-events.md).

Ne možemo unesite sljedeći upit da biste se vratili Svi događaji od upravitelja kontrole servisa koji imaju događaj Identifikacijskim brojem 7036 koja je događaj koji označava pokretanje i Zaustavljanje servisa.

![Upit](media/log-analytics-custom-fields/query.png)

Ne možemo pa odaberite bilo koji zapis s događajem 7036 ID-a.

![Izvor zapisa](media/log-analytics-custom-fields/source-record.png)

Želimo naziv servisa koji će se pojaviti u svojstvu **RenderedDescription** , a zatim odaberite gumb uz to svojstvo.

![Izdvajanje polja](media/log-analytics-custom-fields/extract-fields.png)

**Čarobnjak za izdvajanje polje** otvoren, a polja **zapisniku događaja** i **Iddogađaja** su odabrane u **Glavni primjeru** stupac.  Ukazuje da će se za događaje iz zapisnika sustava s Identifikacijskim događaj brojem 7036 definirati prilagođenog polja.  To je dovoljno tako da mi ne morate da biste odabrali sva ostala polja.

![Glavni primjer](media/log-analytics-custom-fields/main-example.png)

Ne možemo istaknite naziv servisa u svojstvu **RenderedDescription** i korištenje **servisa** za prepoznavanje naziv usluge.  Prilagođeno polje će se pozivati **Service_CF**.

![Polje naslova](media/log-analytics-custom-fields/field-title.png)

Možemo vidjeti da naziv usluge je označena pravilno za neke zapise, ali ne i na drugim korisnicima.   **Rezultati pretraživanja** prikazuju da nije odabrana dio naziva **WMI performanse prilagodnika** .  **Sažetak** prikazuje četiri zapisa sa servisom **DPRMA** neispravno dio dodatni programa word i dva zapisa označena **Instalacijski program za module** umjesto **Windows Installer module**.  

![Rezultati pretraživanja](media/log-analytics-custom-fields/search-results-01.png)

Ne možemo pokrenuti sa zapisom **WMI performanse prilagodnika** .  Ne možemo kliknite njezinu ikonu za uređivanje, a zatim **Izmijeni ovaj isticanja**.  

![Izmjena isticanja](media/log-analytics-custom-fields/modify-highlight.png)

Ne možemo povećati isticanje uvrstite riječ **WMI** i zatim ponovno pokrenite na Izdvoji.  

![Dodatni primjer](media/log-analytics-custom-fields/additional-example-01.png)

Vidimo da ispravljanja stavke za **Prilagodnik WMI performanse** i prijava analitiku koristit te podatke da biste ispravili zapise za **Windows Installer modul**.  Vidimo u odjeljku **Sažetak** Iako **DPMRA** je i dalje neće se označena pravilno.

![Rezultati pretraživanja](media/log-analytics-custom-fields/search-results-02.png)

Pomaknite se do zapisa sa servisom DPMRA ćemo i koristiti isti postupak da biste riješili taj zapis.

![Dodatni primjer](media/log-analytics-custom-fields/additional-example-02.png)

 Kada možemo pokrenuti izdvajanje, možemo vidjeti sve naše rezultata jesu sada.

![Rezultati pretraživanja](media/log-analytics-custom-fields/search-results-03.png)

Vidimo **Service_CF** stvara se, ali još ne dodaje se sve zapise.

![Početni broj](media/log-analytics-custom-fields/initial-count.png)

Nakon nekog vremena pa novo prikuplja događaje, možemo vidjeti da bi polje **Service_CF** sada se dodaje zapisa koji odgovara naš kriterija.

![Konačni rezultat](media/log-analytics-custom-fields/final-results.png)

Sada možete koristiti prilagođene polju zapisa svojstvo.  Da biste to prikazuju ćemo stvoriti upit koji grupira po novog **Service_CF** polja za provjeru koji su servisi većine aktivnog.

![Grupiranje prema upita](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) za sastavljanje upita pomoću prilagođenih polja kao kriterij.
- Praćenje [prilagođene datoteke zapisnika](log-analytics-data-sources-custom-logs.md) raščlaniti pomoću prilagođenih polja.