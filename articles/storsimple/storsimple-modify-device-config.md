<properties 
   pageTitle="Izmjena Konfiguracija uređaja StorSimple | Microsoft Azure" 
   description="U članku se opisuje kako pomoću servisa Upravitelj StorSimple da ponovno konfigurira StorSimple uređaja koji je već uveden." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Korištenje upravitelja StorSimple servisa da biste izmijenili konfiguraciju StorSimple uređaja

## <a name="overview"></a>Pregled 

Azure klasični portal **Konfiguriraj** stranica sadrži sve parametre uređaja možete konfigurirajte na uređaju StorSimple kojim upravlja StorSimple Upravitelj servisa. Pomoću ovog praktičnog vodiča objašnjavaju kako koristiti stranici **Konfiguracija** izvršiti sljedeće zadatke razini uređaja:

- Izmjena postavke audiouređaja 
- Izmjena postavke vremena 
- Izmjena postavke DNS-a 
- Izmjena sučelje mreže
- Zamjena ili ponovna dodjela IP-ovi

## <a name="modify-device-settings"></a>Izmjena postavke audiouređaja

Postavke uređaja obuhvaćaju neslužbeni naziv i opis uređaj uređaja.

StorSimple uređaja koji je povezan sa servisom StorSimple Upravitelj se dodjeljuje zadani naziv. Zadani naziv obično odražava serijski broj uređaja. Na primjer, zadani naziv uređaja koji je 15 znakova, kao što su 8600 SHX0991003G44HT znači sljedeće:

- **8600** – označava model uređaja.
- **SHX** – označava proizvodnje web-mjesta.
- **0991003** - označava određeni proizvod.
- **G44HT**- zadnje 5 znamenki povećava se da biste stvorili jedinstveni serijske brojeve. To može biti uzastopnih skupa.

Azure klasični portal možete koristiti da biste promijenili naziv uređaja i dodjeljivanje jedinstvenih neslužbeni naziv po izboru. Neslužbeni naziv mogu sadržavati znakove, a može biti najviše 64 znaka.

Možete odrediti i opis uređaja. Opis uređaja obično olakšava pronalaženje vlasnika i fizičke mjesto uređaja. Polje opisa mora sadržavati manje od 256 znakova.
 
## <a name="modify-time-settings"></a>Izmjena postavke vremena

Vaš uređaj, morate sinkronizirati vremena da bi se autentičnost kod davatelja usluge za pohranu za oblaka. Na padajućem popisu odaberite vremensku zonu i navedite do dva protokol vrijeme na mreži (NTP) poslužitelja. Primarni NTP poslužitelja potrebna je, a nije naveden kada koristite Windows PowerShell za StorSimple da biste konfigurirali uređaj. Možete odrediti Windows Server zadane **time.windows.com** kao NTP poslužitelja. Možete pogledati primarni konfiguraciju poslužitelja NTP putem portala za Azure klasični, ali morate koristiti sučelja komponente Windows PowerShell da biste ga promijenili.

Sekundarni konfiguraciju poslužitelja NTP nije obavezno. Klasični portal možete koristiti da biste konfigurirali sekundarne NTP poslužitelja. 

Prilikom konfiguriranja NTP poslužitelja, provjerite dopušta li mrežom promet NTP prenesite iz vaše podatkovnog centra s Internetom. Prilikom određivanja javno NTP poslužitelja, provjerite jesu li vatrozidima svoje mreže i na drugim uređajima sigurnost konfigurirani da dopušta promet NTP morali putovati i s mjesta izvan mreže. Ako nije dopuštena Dvosmjeran NTP promet, morate koristiti interni poslužitelj NTP (Ova funkcija sadrži kontroler domene sustava Windows). Ako vaš uređaj nije moguće sinkronizirati vrijeme, možda neće biti moći komunicirati s vaš davatelj usluga za pohranu oblaka.

Da biste vidjeli popis javno NTP poslužitelja, otvorite [NTP poslužitelja Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Što se događa ako je uređaj implementiran u drugoj vremenskoj zoni?

Ako je uređaj implementiran u drugu vremensku zonu, vremenska zona uređaj će se promijeniti. Given da sigurnosne kopije pravila pomoću uređaja vremensku zonu, sigurnosne kopije pravila će automatski prilagoditi u skladu novu vremensku zonu. Potreban je bez intervencije korisnika.

## <a name="modify-dns-settings"></a>Izmjena postavke DNS-a

DNS poslužitelj koristi se kada uređaj pokušava komunicirati s vaš davatelj usluga oblaka za pohranu. Za visoke dostupnosti su potrebne da biste konfigurirali primarnih i sekundarnih DNS poslužitelji tijekom početne uređaj implementacije. Da biste ponovno konfigurirali primarni DNS poslužitelj, morat ćete koristiti sučelje komponente Windows PowerShell na uređaju StorSimple.

Da biste izmijenili sekundarne DNS poslužitelj, možete koristiti Azure klasični portal.



## <a name="modify-network-interfaces"></a>Izmjena sučelje mreže

Vaš uređaj ima šest mreže sučeljima uređaja, četiri od kojih su 1 GbE i dva od kojih su 10 GbE. Ta sučelja označavaju se kao 0 – 5 podataka podatke. Podaci 0, podaci 1, podataka 4 i 5 podataka su 1 GbE dok podataka 2 i 3 podaci su 10 sučelje GbE mreže.

Konfiguriranje **Postavke mreže sučelje** za svaku sučelja koja će se koristiti. Da biste bili sigurni visoke dostupnosti, preporučujemo da imate barem dva sučelja iSCSI i dva sučelja oblaka omogućena na uređaju. Ne možemo preporučujemo, ali ne zahtijeva Neiskorišteni sučelja onemogućivanja.

Kada konfigurirate bilo koji od sučelje mreže, morate konfigurirati na virtualne IP (VIP).

0 podaci oblaka omogućena prema zadanim postavkama. Prilikom konfiguriranja podataka 0, su potrebna i konfiguriranje dva fiksnim IP adrese, jedan za svaki kontroler. Ove fiksnim IP adrese možete koristiti da biste izravno pristupili kontrolera uređaja, a korisni su kada instalirate ažuriranja na uređaju ili prilikom pristupa kontrolera potrebe za otklanjanje poteškoća.

U StorSimple 8000 niz Update 1 metriku usmjeravanje podataka 0 postavljena prema najmanjem; Dakle, ako je uređaju instaliran StorSimple 8000 niz Update 1, sve promet oblak će usmjerena kroz podataka 0. Zabilježite to ako imate više od jedne oblaka omogućeno mrežno sučelje na uređaju StorSimple.

>[AZURE.NOTE] Fiksni IP adresa za kontroler se koriste za održavanje ažuriranja na uređaju. Stoga na fiksnim IP-ovi mora biti usmjeravati i može se povezati s Internetom.

Za svaki mrežno sučelje prikazuju se sljedećih parametara:

- **Brzina** – ne može konfigurirati korisnika parametra. Podaci 0, podaci 1, podataka 4 i 5 podataka uvijek su 1 GbE dok podataka 2 i 3 podaci su 10 GbE sučelja.

     >[AZURE.NOTE] Brzina i dvostrano su uvijek automatski-dogovori. Jumbo okvira nisu podržane.
 
- **Stanje sučelja** – sučelja možete omogućiti ili onemogućiti. Ako je omogućeno, uređaj će pokušati koristiti sučelje. Preporučujemo da se omogućiti samo sučelja koji su povezani s mrežom i koristiti. Onemogući sve sučelja koju koristite.

- **Vrsta sučelja** – taj parametar omogućuje izdvajanja iSCSI promet iz oblaka promet za pohranu. Ovaj parametar može biti nešto od sljedećeg:

    - **Oblak omogućeno** – kada je omogućen, uređaj će pomoću ovog sučelja možete komunicirati s oblaka.
    - **iSCSI omogućeno** – kada je omogućen, uređaj će koristiti ovo sučelje možete komunicirati s iSCSI glavno računalo.

    Preporučujemo da izdvajanja iSCSI promet iz oblaka promet za pohranu. Imajte na umu i ako je vaš glavnog računala u istoj podmreži uređaj, morate Dodjela pristupnika; Međutim, ako je vaš glavno računalo u različitoj podmreži od uređaju, morat ćete Dodjela pristupnika.

- **IP adresa** – to može biti IPv4 ili IPv6 ili i jedno i drugo. IPv4 i IPv6 adrese linije podržan je sučelje mreže uređaja. Kada koristite IPv4, navesti 32-bitni IP adrese (*xxx.xxx.xxx.xxx*) notacijom točka decimalna. Prilikom korištenja protokola IPv6, jednostavno navesti prefiks 4 znamenke i 128-bitno adresa se za vaš uređaj mrežnog sučelja koji se temelji na toj prefiks generira automatski.

- **Podmreže** – to se odnosi na masku i konfiguriran putem sučelja komponente Windows PowerShell.

- **Pristupnik** – to je zadani pristupnik koje treba koristiti ovo sučelje kada se pokuša komunikaciju čvorove koje se ne nalaze u istom prostor IP adrese (podmreži). Zadani pristupnik mora biti u isti prostor adresa (podmreži) kao sučelje IP adresu, kao što je određeno maska podmreže.

- **Fiksni IP adresa** – ovo polje dostupna je samo dok konfiguriranje podataka 0 sučelja. Za operacije kao što su ažuriranja i otklanjanje poteškoća s uređaja, morate povezati izravno s kontrolerom uređaja. Da biste pristupili aktivnog i pasivni kontroler na uređaju se poslužite nepromjenjivi IP adresa.

Možete konfigurirajte kontroler 0 i 1, kontroler putem portala za Azure klasični.

>[AZURE.NOTE] 
>
>- Da biste provjerili pravilnog, provjerite brzina sučelja i dvostrano na parametar svaki sučelje uređaja povezano s. Promjena sučelja treba ili sklopite s ili je konfigurirati za gigabitna Ethernet (1000 Mb/s), a biti potpuno dvostrano. Sučelja radi u manjoj brzini ili pola dvostrano rezultirat će probleme s performansama.
>
>- Da biste minimizirali to izbjeglo i isključiti, preporučujemo da omogućite portfast na svakom priključaka parametar koji mrežno sučelje iSCSI uređaja će povezati. To će osigurati da veza s mrežom možete uspostaviti brzo slučaju na prebacivanje.
 
## <a name="swap-or-reassign-ips"></a>Zamjena ili ponovna dodjela IP-ovi

Trenutno Ako bilo koji mrežno sučelje na kontroleru je dodijelila VIP koji se koristi (ili neki drugi uređaj u mrežu na istom uređaju), kontrolerom neće uspjeti iznad. Dakle, morate provesti pravilan postupak ako su zamjena VIPs za mrežno sučelje uređaja jer će stvoriti duplicirane IP situaciji.

Izvršite sljedeće korake zamjena ili ponovno dodijelite VIPs za bilo koju od sučelje mreže:

#### <a name="to-reassign-ips"></a>Da biste dodijelili IP-ovi

1. Poništite IP adresa za oba sučelja.

2. Nakon što ste poništili IP adrese, dodijeliti odgovarajući sučelja nove IP adrese.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [konfigurirati MPIO za svoj uređaj StorSimple](storsimple-configure-mpio-windows-server.md).

- Saznajte kako [koristiti StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
     
