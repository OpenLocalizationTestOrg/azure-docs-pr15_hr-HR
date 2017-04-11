<properties 
   pageTitle="Lokalno StorSimple prikvačene količine najčešća pitanja vezana uz | Microsoft Azure"
   description="Navedeni odgovori na najčešća pitanja o StorSimple lokalno prikvačene količine."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Lokalno StorSimple prikvačene količine: najčešća pitanja

## <a name="overview"></a>Pregled

Slijede pitanja i odgovore koje biste mogli imati pri stvaranju StorSimple lokalno prikvačene glasnoće, pretvaranje tiered jedinice lokalno prikvačene jedinicu (i obratno), ili sigurnosno kopiranje i vraćanje lokalno prikvačene glasnoću.

Pitanja i odgovore podijeljene su sljedeće kategorije

- Stvaranje lokalno prikvačene glasnoće
- Sigurnosno kopiranje prikvačene lokalno
- Pretvaranje tiered glasnoću lokalno prikvačene glasnoće
- Vraćanje lokalno prikvačene glasnoće
- Neuspjeh putem lokalno prikvačene glasnoće

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Pitanja o stvaranju lokalno prikvačene glasnoće

**PITANJA.** Što je maksimalna veličina lokalno prikvačene jedinice koje se može stvoriti na uređajima 8000 niz?

**A** možete Dodjela lokalno prikvačene količine do 8,5 TB ili tiered količine do 200 TB na uređaju 8100. Na uređaju 8600 veće, možete Dodjela lokalno prikvačene količine do 22.5 TB ili tiered količine do 500 TB.

**PITANJA.** Nedavno nadogradnje uređaju 8100 ažuriranju 2 i prilikom pokušaja stvorite lokalno prikvačene jedinicu, maksimalna veličina dostupna je samo 6 TB i ne 8.5 TB. Zašto nije moguće stvoriti jedinici s 8,5 TB?

**A** možete Dodjela lokalno prikvačene količine najviše 8.5 TB ili tiered količine do 200 TB na uređaju 8100. Ako vaš uređaj već ima tiered količine, tada će biti proporcionalno manja od maksimalno ograničenje prostora za stvaranje lokalno prikvačene glasnoću. Na primjer, ako je na uređaju 8100 (što je pola tiered kapaciteta) već dodijeljen 100 TB tiered jedinicama, zatim maksimalne veličine lokalnu jedinicu koju možete stvoriti na uređaju 8100 bit će artiklima smanjena za 4 TB (otprilike polovicu najviše lokalno prikvačene kapaciteta jedinice).

Jer lokalni prostor na uređaju koristi se za hostiranje radni skup tiered količine dostupnog prostora za stvaranje lokalno prikvačene glasnoću se smanjuje ako uređaj ima tiered količine. Nasuprot tome, stvaranje lokalno prikvačene glasnoću proporcionalno smanjuje dostupnog prostora za tiered količine. U sljedećoj su tablici navedene dostupan tiered kapacitet na uređajima 8100 i 8600 stvaranja lokalno prikvačene količine.

|Lokalno prikvačene količine dodijeljenu kapaciteta|Dostupni kapacitet biti dodijeljena tiered količine - 8100|Dostupni kapacitet biti dodijeljena tiered količine - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176.5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | N/D | 277.8 TB |
|15 TB | N/D | 166.7 TB |
|22.5 TB | N/D | 0 TB |


**PITANJA.** Zašto je stvaranje lokalno prikvačene glasnoću dugo izvodi operacije? 

**ODGOVORI.** Lokalno prikvačene jedinicama su thickly dodijeljeni resursi. Da biste stvorili prostora na lokalnom razine uređaja, neke podatke iz postojeće tiered jedinicama možda se pomiču u oblak tijekom postupka dodjele resursa. I jer to ovisi o tome veličinu jedinice dodjeli, postojeće podatke o uređaju i raspoložive propusnosti u oblak vrijeme potrebno da biste stvorili lokalnu jedinicu možda nekoliko sati.

**PITANJA.** Koliko dugo traje da biste stvorili lokalno prikvačene glasnoću?

**ODGOVORI.** Jer lokalno prikvačene količine su thickly dodjeli, neki postojeći podaci iz tiered količine možda se pomiču u oblak tijekom postupka dodjele resursa. Stoga vrijeme potrebno za stvaranje lokalno prikvačene jedinice ovisi o više čimbenika, uključujući veličinu glasnoće, podatke o uređaju i raspoložive propusnosti. Na freshly instaliranih uređaja s nema jedinica vremena da biste stvorili lokalno prikvačene glasnoću je otprilike 10 minuta po terabajta podataka. Stvaranje lokalne jedinice, međutim, može potrajati nekoliko sati na temelju čimbenika objašnjenje iznad na uređaju koji se koristi.

**PITANJA.** Želim stvoriti lokalno prikvačene glasnoću. Postoje li sve najbolje prakse morate imati na umu?

**ODGOVORI.** Lokalno prikvačene količine su prikladna za radnih opterećenja koji zahtijevaju lokalne jamstva podataka cijelo vrijeme i osjetljivi su na latencies u oblaku. Uzimajući u obzir korištenje lokalne jedinice za bilo koju od radnih opterećenja, imajte na umu sljedeće:

- Lokalno prikvačene količine su thickly dodjeli i stvaranje lokalne jedinice utječe dostupnog prostora za tiered količine. Dakle, predlažemo započeti s manje veličine količine i promjena veličine kao vaše se povećava zahtjeva za pohranu.

- Dodjeljivanje lokalne jedinica je dugo izvodi operacije koje možda obuhvaćaju margina postojeće podatke iz tiered količine u oblak. Kao rezultat, mogli biste primijetiti smanjene performanse te količine.

- Dodjeljivanje lokalne jedinica je dugotrajan postupak. Stvarno vrijeme uvrštene ovisi o više čimbenika: veličinu glasnoću dodjeljuju se Resursi, podaci o uređaju i raspoložive propusnosti. Ako ne sigurnosno vašim postojeće jedinicama u oblak, zatim glasnoću stvaranje je sporije. Predlažemo da iskoristite oblaka snimke vašim postojeće jedinicama prije dodjele resursa na lokalnu jedinicu.
 
- Lokalno prikvačene količine možete pretvoriti u postojeće tiered količine i Pretvorba obuhvaća Dodjeljivanje prostora na uređaju dobivene lokalno prikvačene jedinice (osim da dolje tiered podataka [li] iz oblaka). Ponovno je dugo izvodi operacija koja ovisi o čimbenika smo ste spominju iznad. Predlažemo da stvorite sigurnosnu kopiju postojeće diskova prije pretvaranja tijekom postupka bit će čak i sporije postojeće količine se sigurnosno kopirali. Vaš uređaj i možda primijetiti smanjene performansi tijekom ovog postupka.
    
Dodatne informacije o [stvaranju lokalno prikvačene glasnoće](storsimple-manage-volumes-u2.md#add-a-volume)

**PITANJA.** Je li moguće stvoriti više lokalno prikvačene jedinica u isto vrijeme?

**ODGOVORI.** Da, ali postoje lokalno prikvačene glasnoću stvaranja i proširenja poslovi sekvencijalno obrađuju.

Lokalno prikvačene količine su thickly dodjeli, a to su potrebne za stvaranje lokalne prostora na uređaju (što može dovesti do postojeće podatke s tiered jedinica za pomiču u oblak tijekom postupka dodjele resursa). Dakle, ako je zadatak za dodjele resursa u tijeku, druge zadatke stvaranja lokalnu jedinicu će se u redu čekanja dok se dovrši taj zadatak.

Isto tako, ako postojeće lokalnu jedinicu proširivanja ili tiered glasnoću pretvara lokalno prikvačene jedinicu, zatim stvaranja nove jedinice lokalno prikvačene je u redu čekanja dok se ne prethodnog posla. Proširivanje veličina lokalno prikvačene glasnoću tiče proširenja postojećeg lokalnog prostora za tu jedinicu. Pretvaranje iz tiered za lokalno prikvačene glasnoću također uključuje stvaranje radnog prostora na lokalnom za na rezultat lokalno prikvačene glasnoću. U obje te operacije, stvaranje ili proširenja prostora na lokalnom dugu radi posao.

Ove zadatke možete pogledati na stranici **Poslovi** Azure StorSimple Upravitelj servisa. Zadatak koji je aktivno obrade neprestano ažurira se u skladu s vizualnim tijek prostora dodjele resursa. Preostali poslove lokalno prikvačene glasnoću označen kao pokrenut, ali tijek je zapeo, učinite sljedeće, a izdvajanja u redoslijedu oni su u redu čekanja.

**PITANJA.** Izbrisao sam lokalno prikvačene glasnoću. Zašto ne vidim reclaimed prostora odražavaju u dostupnog prostora kada pokušam da biste stvorili novu jedinicu? 

**ODGOVORI.** Ako izbrišete lokalno prikvačene glasnoće, prostora za nove jedinice možda se neće ažurirati odmah. Servis za upravitelja StorSimple ažurira lokalne dostupnom prostoru približno svaki sat. Predlažemo da pričekati jedan sat prije nego se pokušate stvoriti novu jedinicu.

**PITANJA.** Lokalno prikvačene količine podržani su na uređaj oblak?

**ODGOVORI.** Lokalno prikvačene jedinicama nisu podržane na uređaj oblaka (8010 i 8020 uređaja prethodno nazivaju StorSimple virtualnog uređaja).

**PITANJA.** Mogu li koristiti Azure PowerShell cmdleti za stvaranje i upravljanje lokalno prikvačene količine? 

**ODGOVORI.** Ne, ne možete stvoriti lokalno prikvačene količine putem Azure cmdleta ljuske PowerShell (bilo koje jedinice stvorite PowerShell Azure je tiered). Također predlažemo da ne koristite Azure PowerShell cmdleti da biste izmijenili svojstva lokalno prikvačene jedinice, kao što je imat će neželjenih učinak izmjena vrstu glasnoću na tiered.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Pitanja o sigurnosno kopiranje lokalno prikvačene glasnoće

**PITANJA.** Su lokalni snimke lokalno prikvačene količine podržane?

**ODGOVORI.** Da, možete poduzeti lokalne snimke lokalno prikvačene diskova. Međutim, svakako predlažemo da redovito sigurnosno kopirate lokalno prikvačene diskova s oblaka snimke stanja da biste bili sigurni u eventuality na Izrada zaštićen podataka.

**PITANJA.** Postoje li sve smjernice za upravljanje lokalne snimki za lokalno prikvačene količine?

**ODGOVORI.** Česti lokalne snimke duž visoke rata churn podataka u lokalno prikvačene glasnoću može uzrokovati prostora na lokalnom na uređaju da biste brzo potrošena i rezultirati podatke iz tiered količine koja se pomiču u oblak. Stoga predlažemo da smanjiti broj lokalne snimke.

**PITANJA.** Primio sam upozorenja koja kaže da se možda nevažeći moju lokalnu snimke lokalno prikvačene količine. Kada mogu se to događa?

**ODGOVORI.** Česti lokalne snimke duž visoke rata churn podataka u lokalno prikvačene glasnoću može uzrokovati lokalne prostora na uređaju da biste brzo potrošena. Ako lokalni razine uređaja intenzivnog koriste, do prekida prošireni oblaka može dovesti do uređaju postaje puno i dolazne upisivanje glasnoću može rezultirati poništavanja valjanosti od snimki (kao što je bez razmaka postoji da biste ažurirali snimke stanja da biste se pozvali na starije blokovi podataka koji ste je prebrisati). U takvim situaciji upisivanje glasnoću će i dalje posluživanje, ali lokalne snimke možda nije valjana. Postoji bez utjecaja na postojeće brze snimke oblaka.

Upozorenje upozorenje se prikazuje koja vas obavještava da takve situaciji možete biste i provjerite je li iste adrese pravovremeno pregledavanje rasporede lokalne snimaka da bi manje Česti lokalne snimke ili brisanje starijih lokalne snimke koje više nisu potrebne.

Ako su nevažeći lokalne snimke, primit ćete upozorenje informacije obavijestio da ste je uz popis vremenske oznake lokalne snimki koji su nevažeći nevažeći lokalne snimaka određene sigurnosne kopije pravila. Ove snimke neće biti automatski izbrisan i više neće moći njihov prikaz u **Katalozi sigurnosnu kopiju** stranice na portalu za Azure klasični.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Pitanja o pretvorbi tiered glasnoću lokalno prikvačene jedinicu

**PITANJA.** Koristim opažanja neke sporost programa na uređaju pretvaranju tiered glasnoću lokalno prikvačene glasnoće Zašto se to događa? 

**ODGOVORI.** Pretvorbe uključuje dva koraka: 

  1. Dodjeljivanje prostora na uređaju potražite u uskoro-na--pretvorit lokalno prikvačena glasnoću.
  2. Preuzimanje tiered podatke iz oblaka da biste bili sigurni lokalne jamčiti.

Obje korake u nastavku su dugi postupke koje ovise o veličini glasnoće pretvara, podaci o uređaju i raspoložive propusnosti. Kao što je neke podatke iz postojeće tiered količine možda spill u oblak u sklopu postupka dodjele resursa, uređaju možda primijetiti smanjene performansi tijekom određenog razdoblja. Osim toga, možete biti sporije pretvorbe ako:

- Postojeće količine ne sigurnosno kopirane s oblakom Da bi se predlažemo da sigurnosno kopiranje diskova prije započinjanja pretvorbe.

- Propusnosti regulacije pravila primijenjeni, koji mogu ograničiti raspoložive propusnosti s oblakom Stoga preporučujemo da imate namjenski 40 MB/s ili više veza s oblakom.

- Pretvorbe može potrajati nekoliko sati zbog više čimbenika objašnjeni iznad; Dakle, predlažemo da dovršite ovaj postupak tijekom vremena koje nisu peaks ili na vikenda da biste izbjegli utjecaj na krajnji korisnici.

Dodatne informacije o [Pretvaranje tiered jedinice u lokalno prikvačene glasnoće](storsimple-manage-volumes-u2.md#change-the-volume-type)

**PITANJA.** Mogu li odustati od operacije pretvorbe glasnoću?

**ODGOVORI.** Ne, ne možete Poništi ovaj postupak pretvorbe jednom pokrenuli. Kako je opisano u prethodno pitanje, imajte na umu na potencijalne probleme s performansama da možete naići tijekom postupka i slijede preporučene postupke naveden za planiranje sustava pretvorbe.

**PITANJA.** Što se događa Moje jedinicu pretvorbe ne uspije?

**ODGOVORI.** Pretvorba glasnoću možete neće uspjeti zbog problema s povezivanjem oblaka. Na uređaju možda ipak zaustavljanje pretvorbe nakon niz neuspješnih pokušaja Premjesti dolje tiered podataka iz oblaka. U takvim scenariju jedinica vrsta će i dalje biti jedinica vrsta izvora prije pretvaranja, a:

- Kritično upozorenje se zaokružuje koja vas obavještava o pogreška pretvorbe glasnoću. Dodatne informacije o [upozorenja vezane uz lokalno prikvačene jedinicama](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Ako pretvarate u tiered lokalno prikvačene jedinicu, glasnoću će i dalje nastave svojstva tiered jedinice kao podataka možda još uvijek nalaze na oblaka. Predlažemo da rješavanje problema s povezivanjem i ponovno pokušajte ovaj postupak pretvorbe.
 
- Isto tako, ako pretvorbe iz lokalno prikvačene tiered jedinicu ne uspije, iako glasnoću označit će se kao lokalno prikvačene jedinica, on će funkcionirati kao tiered glasnoću (jer je u oblak može imati spilled podataka). Međutim, on će i dalje proširiti prostora na lokalnom razine uređaja. Taj prostor neće biti dostupno za drugim lokalno prikvačene jedinicama. Predlažemo da vam ponovno pokušajte ovaj postupak da biste bili sigurni da dovršetka pretvorbe glasnoću i lokalne prostora na uređaju možete vraćeno.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Pitanja o vraćanju lokalno prikvačene glasnoće

**PITANJA.** Su lokalno prikvačene količine vratiti trenutačno?

**ODGOVORI.** Da, lokalno prikvačene količine se vraćaju odmah. Čim metapodacima jedinice se povlače iz oblaka kao dio postupak vraćanja, glasnoću je na mreži i mogu pristupiti glavnog računala. Međutim, lokalni jamstva jedinice podataka neće postojati dok sve podatke preuzete iz oblaka, a možete naići smanjene performanse ove jedinice trajanja obnavljanja.

**PITANJA.** Koliko dugo traje da biste vratili lokalno prikvačene glasnoću?

**ODGOVORI.** Lokalno prikvačene jedinicama trenutačno vratiti, a na mreži čim glasnoću metapodataka dohvaća podatke iz oblaka, dok podataka glasnoću nastavlja se preuzimaju u pozadini. Potonjem dio postupak vraćanja – početak natrag lokalne jamstva za podatke glasnoću – je dugo izvodi operacija, a može potrajati nekoliko sati za sve podatke koje želite izvršiti ponovno lokalni. Vrijeme potrebno da biste dovršili isti ovisi o više čimbenika, kao što su veličina glasnoće vraćaju i raspoložive propusnosti. Ako izvorni jedinicu koju vraća izbrisana, znači da je dodatno vrijeme uzimati da biste stvorili lokalnu prostora na uređaju kao dio postupak vraćanja.

**PITANJA.** Morate vratiti postojeću lokalno prikvačene jedinicu starije snimku stanja (izvedena kada je tiered glasnoću). Glasnoću vratit će se kao što je u tom slučaju tiered?

**ODGOVORI.** Ne, glasnoću vratit će se kao lokalne prikvačene jedinica. Iako se snimka datuma u vrijeme kada je tiered glasnoće, prilikom vraćanja postojeće jedinicama StorSimple uvijek koristi vrstu glasnoće na disku kao što je trenutno postoji.

**PITANJA.** Li prošireni moju lokalnu prikvačene glasnoću nedavno, no sada želim vratiti podatke u vrijeme kada je manja veličina glasnoću. Vraćanje promijeniti veličinu trenutne jedinice i hoće li potrebno da biste proširili veličinu jedinice kada se dovrši vraćanja?

**ODGOVORI.** Da, vraćanje će promjena veličine glasnoću i ćete morati proširiti veličinu jedinice po dovršetku vraćanja.

**PITANJA.** Mogu li promijeniti vrstu jedinica tijekom vraćanja?

**A.** Ne, ne možete promijeniti vrstu glasnoću tijekom vraćanja.

- Količine izbrisane vratit će se kao vrstu pohranjene u snimku.

- Postojeće količine se vraćaju ovisno o vrsti trenutni, bez obzira na vrstu pohranjene u snimku (pogledajte prethodna dva pitanja).
 
**PITANJA.** Morate Vrati moje lokalno prikvačene zvuk, ali izdvojiti netočan točke u vrijeme snimke. Možete otkazati trenutni postupak vraćanja?

**ODGOVORI.** Da, možete otkazati postupak vraćanja na početak. Stanje glasnoću bit će vraćen stanje na početku vraćanja. Međutim, sve pisanja koje ste napravili na jedinicu dok vraćanje je u tijeku izgubit će se.

**PITANJA.** Sam započeo postupak vraćanja na moju lokalnu prikvačene količine i vidim snimku u moj katalog zaostale koji se ne recollect stvaranje. Što je to koristiti?

**ODGOVORI.** To je privremeni snimku zaslona koja je stvorena prije no što postupak vraćanja i služi za vraćanje u slučaju Vrati se rada otkazuje ili ne uspije. Brisanje snimke; ga automatski će se izbrisati po dovršetku vraćanja. U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi.

**PITANJA.** Mogu Kloniraj lokalno prikvačene glasnoću?

**ODGOVORI.** Da, možeš. Međutim, lokalno prikvačene glasnoću će biti klonirana kao tiered jedinica prema zadanim postavkama. Dodatne informacije o tome kako [Kloniraj lokalno prikvačene glasnoće](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Pitanja o neuspjeh putem lokalno prikvačene glasnoće

**PITANJA.** Je li nužno neće uspjeti putem uređaja na drugi uređaj fizički. Će Moje lokalno prikvačene količine se nije uspjela preko lokalno prikvačene ili tiered?

**ODGOVORI.** Ovisno o verziji softver ciljni uređaj, lokalno prikvačene količine će se nije uspjela tijekom kao:

- Lokalno prikvačene ako ciljni uređaj radi StorSimple 8000 niz ažuriranje 2
- Tiered ako ciljni uređaj radi StorSimple 8000 niz ažuriranje 1.x
- Tiered ako je uređaj cilj potražite oblaka (ažuriranje verziju programa 2 ili ažurirati 1.x)

Dodatne informacije o [Prebacivanje i DR od lokalno prikvačene količine svim verzijama](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**PITANJA.** Lokalno prikvačene količine trenutačno vraćaju vrijeme Izrada oporavka (DR)?

**ODGOVORI.** Da, lokalno prikvačene količine se vraćaju trenutačno tijekom prebacivanje. Čim metapodacima jedinice se povlače iz oblaka u sklopu postupka prebacivanje, glasnoću je na mreži na ciljni uređaj te im možete pristupiti na glavno računalo. U međuvremenu, glasnoću podataka će se i dalje preuzimanje u pozadini, a možete naići smanjene performanse ove jedinice trajanja na prebacivanje.

**PITANJA.** Prikazuje posao prebacivanje dovršiti kako možete li pratiti napredak lokalno prikvačene jedinicu koja se vraća na ciljni uređaj?

**ODGOVORI.** Tijekom operacije prebacivanje prebacivanje zadatak je označen kao poduzmite jednom količine u skupu prebacivanje su trenutačno vratiti i na mreži na ciljni uređaj. To obuhvaća lokalno prikvačene jedinice koje je pokrenuo putem; Međutim, lokalni jamstva podataka samo će dostupna kad preuzete sve podatke za jedinicu. Ovaj tijek svakoj lokalno prikvačene jedinici koja nije uspio ispočetka tako nadgledanje odgovarajuće poslova vraćanja stvorenih kao dio sustava prebacivanje možete pratiti. Ove zadatke pojedinačne vraćanja stvorit će se samo za lokalno prikvačene količine.

**PITANJA.** Mogu li promijeniti vrstu jedinica tijekom prebacivanje?

**ODGOVORI.** Ne, ne možete promijeniti vrstu glasnoću tijekom na prebacivanje. Ako se ne uspijeva pokazivač na drugi uređaj fizički sa sustavom StorSimple 8000 niz ažuriranje 2, jedinica će se nije uspjela putem ovise o vrsti glasnoću pohranjene u snimku. Prilikom neuspješnih putem uređaja verziju odnose se na pitanje iznad na vrsti pogona nakon na prebacivanje.

**PITANJA.** Mogu li neće uspjeti pokazivač glasnoće kontejner s lokalno prikvačene količine u oblak potražite?

**ODGOVORI.** Da, možeš. Lokalno prikvačene količine će biti nije uspjelo putem kao tiered jedinice. Dodatne informacije o [Prebacivanje i DR od lokalno prikvačene količine svim verzijama](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


