<properties
    pageTitle="Pregled usporednih za Azure SQL baze podataka"
    description="U ovoj se temi opisuju usporednih baze podataka SQL Azure koristi u mjerenje performanse baze podataka SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Pregled usporednih za Azure SQL baze podataka

## <a name="overview"></a>Pregled
Baza podataka Microsoft Azure SQL nudi tri [servisa razine](sql-database-service-tiers.md) s više razina performansi. Svaka razina performanse nudi rastuće skup resursa ili "power', namijenjenu izlaganje smanjuju propusnost.

Važno je da će moći brojčano kako će se povećavati power svake razine performanse prevodi u performanse povećana baze podataka. Da biste učinili ovaj Microsoft je razvio Azure SQL baza podataka usporednih (ASDB). Na usporednih exercises kombinacije osnovne operacije u svim radnih opterećenja OLTP. Ne možemo Izmjerite propusnost postići za baze podataka radi u svaku razinu performansi.

Resurse i power svaki servis sloju i performanse razine su izražena [Jedinice transakcije baze podataka (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs omogućuje opisuju relativni kapacitet razinu performansi na temelju mjera stopljena procesora, memorije i čitanje i pisanje stope nudi svaku razinu performansi. Udvostručavanja ocjena DTU baze podataka daje rezultat u obliku udvostručavanja power baze podataka. Na usporednih zatražit procijenite utjecaj na performanse baze podataka rastuće napajanja za svaku razinu performanse nudi exercising stvarni postupaka baze podataka, prilikom promjene veličine Veličina baze podataka, broj korisnika i stope transakcije razmjerno resursa za bazu podataka.

Prema kojem se iznosi ono propusnost sloju osnovni servis pomoću transakcije po sati, sloju standardnog servisa pomoću transakcije po minuti i sloju servisa Premium pomoću transakcije po sekundi, on olakšava brzo povezivanje performanse potencijalne od svakog servisa sloju preduvjeti za aplikaciju.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Correlating rezultate usporednih stvarnog svijeta performanse baze podataka
Važno da biste shvatili da ASDB, kao što je sve jednonitnih je predstavniku i indikativnih samo je. Transakcije tečajeve postići pomoću aplikacije usporednih neće biti jednaki onima koje možda postići s drugim aplikacijama. Na usporednih sastoji se od zbirka različite transakcije vrste pokrenuti sa shemom koja sadrži raspon tablice i vrste podataka. Dok se usporednih exercises iste osnovne operacije koje su zajedničke svih radnih opterećenja OLTP, predstavljaju neki određeni klasa baze podataka ili aplikacije. Cilj na usporednih je pružanje pametnije vodič kroz relativni performanse bazu podataka koja se može očekivati kada skaliranje prema gore ili dolje između razina performansi. U stvari, baze podataka pripadaju različitim veličinama i složenosti, naići na različitim smjese radnih opterećenja i će odgovoriti na različite načine. Na primjer, aplikaciju IO ćete morati usko može odrediti kliknite IO pragovi ili procesora ćete morati usko aplikacije može odrediti kliknite procesora ograničenja. Postoji ne jamči da će sve određenu bazu podataka skaliranje na isti način kao usporednih u odjeljku povećanje učitavanja.

Na usporednih i njegov methodology se podrobnije opisan u nastavku.

## <a name="benchmark-summary"></a>Sažetak usporednih
ASDB mjeri performanse kombinacije osnovni postupaka baze podataka koje se najčešće pojavljuju u mrežna transakcija obrade radnih opterećenja (OLTP). Iako u usporednih namijenjen s oblaka računalstvo u umu, shemu baze podataka, populacije podataka, a namijenjena transakcije treba na njih svim korisnicima Dopusti predstavniku elemenata osnovni najčešće koriste u radnih opterećenja OLTP.

## <a name="schema"></a>Shema
Shema namijenjen je dovoljno različite i složenosti za podršku širok raspon operacije. Na usporednih pokreće se sastoji od šest tablica bazi podataka. Tablica se nalaziti u tri kategorije: dugotrajne veličine, veličine i rast. Postoje dvije tablice – veličina; tri tablice skaliranja; i jednu tablicu sve veći. Veličina tablice imaju stalan broj redaka. Skaliranja tablica ima cardinality proporcionalna performanse baze podataka, ali ne mijenjaju tijekom na usporednih. Sve veći tablice veličinom kao što su skaliranja tablica učitavanja početne, ali zatim promjene cardinality tijeku pokretanjem na usporednih redaka su umetati i izbrisati.

Shema sadrži kombinacije vrste podataka, uključujući cijeli broj, numeričke, znakova i datuma/vremena. Shema uključuje primarnih i sekundarnih ključeva, ali ne vanjski ključevi – to jest, postoje bez ograničenja referencijalni integritet između tablica.

Program za generiranje podataka generira podatke za početne baze podataka. Cijeli broj i numeričke podatke je generirao različite strategije. U nekim slučajevima vrijednosti distributed će se slučajno u rasponu. U drugim slučajevima skup vrijednosti se slučajno permuted da bi se održava određene distribuciju. Tekstna polja generiraju ponderiranog popis riječi da bi proizveo realističan izgleda podataka.

Veličinom baze podataka na temelju "skaliranje faktor." Faktor promjene veličine (skraćeno naziva SF) određuje cardinality skaliranja i rast tablice. Prema uputama u nastavku u odjeljku korisnici i Pacing, veličina baze podataka broj korisnika i maksimalne performanse sve skaliranje razmjerno međusobno povezani.

## <a name="transactions"></a>Transakcije
Povećavaju sastoji se od devet vrste transakcija, kao što je prikazano u tablici u nastavku. Da biste istaknuli određeni skup značajki sustava u bazu podataka modul i sustava hardver, s jakog kontrasta iz druge transakcije osmišljene su pojedine transakcije. Taj se način olakšava procijenite utjecaj na različitim cjelokupnoj izvedbi komponente. Ako, na primjer, "Čitanje podebljanom" transakcije daje značajan broj operacije čitanja s diska.

| Vrsta transakcije | Opis |
|---|---|
| Ograničeno čitanje | ODABERITE; u memoriji; samo za čitanje |
| Čitanje srednje | ODABERITE; uglavnom u memoriji; samo za čitanje |
| Tamni za čitanje | ODABERITE; ne uglavnom u memoriji; samo za čitanje |
| Ažuriranje ograničeno | AŽURIRANJE; u memoriji; čitanje i pisanje |
| Ažuriranje podebljano | AŽURIRANJE; ne uglavnom u memoriji; čitanje i pisanje |
| Umetanje ograničeno | UMETANJE; u memoriji; čitanje i pisanje |
| Umetanje podebljano | UMETANJE; ne uglavnom u memoriji; čitanje i pisanje |
| Brisanje | BRISANJE; kombinacije u memoriji, a ne u memoriji; čitanje i pisanje |
| Tamni CPU-a | ODABERITE; u memoriji; relativno podebljanom opterećenje procesora; samo za čitanje |

## <a name="workload-mix"></a>Radno opterećenje mix
Transakcije nasumično su odabrani iz ponderiranog razdiobu sa sljedećim cjelokupan mix. Cjelokupan mix ima omjer za čitanje/pisanje približno 2:1.

| Vrsta transakcije | % Mix |
|---|---|
| Ograničeno čitanje | 35 |
| Čitanje srednje | 20 |
| Tamni za čitanje | 5 |
| Ažuriranje ograničeno | 20 |
| Ažuriranje podebljano | 3 |
| Umetanje ograničeno | 3 |
| Umetanje podebljano | 2 |
| Brisanje | 2 |
| Tamni CPU-a | 10 |

## <a name="users-and-pacing"></a>Korisnici i pacing
Radno opterećenje usporednih pokreću iz alata koji šalje transakcije u skupu rezultata veze u programu Publisher ponašanje broja Istodobni korisnika. Iako sve veze i transakcije strojno generirani, zbog jednostavnosti nazivamo ove veze "korisnika". Iako se svaki korisnik pristajete neovisno o drugim korisnicima, svi korisnici izvedite ciklusu iste korake u nastavku:

1. Uspostaviti vezu s bazom podataka.
2. Ponavljajte sve dok ne signaled da biste izašli iz:
    - Odaberite transakcije nasumično (iz ponderiranog raspodjele).
    - Izvođenje odabrane transakcije i mjerenje vremena odgovor.
    - Pričekajte pacing odgode.
3. Zatvorite vezu s bazom podataka.
4. Izlaz.

Nasumično odabran pacing vrijeme (u koraku 2c), ali uz razdiobu s koje je prosjek 1.0 sekundi. Stoga svaki korisnik može, u generirati najviše jedan transakcije sekundi.

## <a name="scaling-rules"></a>Promjena veličine pravila
Broj korisnika određuje Veličina baze podataka (u faktor skaliranja jedinice). Postoji jedan korisnik za svaku pet jedinice faktor skaliranja. Zbog pacing odgode jednog korisnika mogu uzrokovati najviše jedan transakcije sekundi, u.

Na primjer, na skaliranje dvofaktorska analiza varijance od 500 (SF = 500) baza podataka će imati 100 korisnika i možete postići Maksimalna brzina od 100 TPS. Na pogon veći TPS stopa zahtijeva više korisnika i veće baze podataka.

U tablici u nastavku prikazuje broj korisnika zapravo osigurale trajne za svaki servis sloju i performanse razinu.

| Servis sloju (performanse razina) | Korisnici | Veličina baze podataka |
|---|---|---|
| Osnovni | 5 | 720 MB |
| Standardna (S0) | 10 | 1 GB |
| Standardna (S1) | 20 | 2.1 GB |
| Standardna (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6 na P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Mjerenje trajanja
Valjani usporednih pokrenuti zahtijeva steady stanja mjerenje trajanja barem jedan sat.

## <a name="metrics"></a>Mjerenja
Ključni metriku u na usporednih su vrijeme propusnost i odgovor.

- Propusnost je mjera ključna performanse u na usporednih. Propusnost prijavljuje u transakcije po jedinica--vremena, brojeći sve vrste transakcije.
- Reakcija je mjera širine predvidljivost performansi. Ograničenje vremena odgovor ovisi o predmete servisa s veći klase servisa imate više stroga preduvjet odgovor vremena, kao što je prikazano u nastavku.

| Klase pružanja usluge  | Propusnost mjera | Zahtjev za vrijeme odgovora |
|---|---|---|
| Premium | Transakcije u sekundi | 95. percentil pri 0,5 sekundi |
| Standardna | Transakcije u minuti | 90. percentil pri 1.0 sekundi |
| Osnovni | Transakcije sat | 80. percentil pri 2.0 sekundi |

## <a name="conclusion"></a>Zaključak
Usporednih za baze podataka SQL Azure mjeri relativni performanse baze podataka SQL Azure izvodi preko raspona raspoloživ servis razine i razina performansi. Na usporednih exercises kombinacije osnovni postupaka baze podataka koje se najčešće pojavljuju u mrežna transakcija obrade radnih opterećenja (OLTP). Po mjerenje stvarni performanse, na usporednih pruža smisleniji procjenu utjecaja na propusnost promijenite razinu performanse nego što je moguće navođenjem samo resursi nudi svaku razinu kao što je brzina procesora, memorije i IOPS. U budućnosti, nastavit ćemo poslovanja usporednih da biste proširili svog djelokruga i proširili navedene podatke.

## <a name="resources"></a>Resursi
[Uvod u SQL baze podataka](sql-database-technical-overview.md)

[Razine usluge i razine performansi](sql-database-service-tiers.md)

[Performanse smjernice za jednu baze podataka](sql-database-performance-guidance.md)
