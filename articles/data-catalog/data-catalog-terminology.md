<properties
   pageTitle="Azure terminologija katalog podataka | Microsoft Azure"
   description="Ovaj članak sadrži Uvod u koncepata i izraze koji se koriste u dokumentaciji Azure kataloga podataka."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure terminologija kataloga podataka

## <a name="catalog"></a>Katalog

Katalog podataka Azure je spremište metapodataka u oblaku u podatke koji se može registrirati resursi i izvora podataka. Katalog služi kao središnje spremanje lokacije strukturnih metapodataka dobivenih iz izvora podataka i opisni metapodataka dodao korisnika.

## <a name="data-source"></a>Izvor podataka

Izvor podataka je sustav ili spremnik koja upravlja imovine podataka. Primjeri baza podataka sustava SQL Server, baze podataka tvrtke Oracle, baze podataka SQL Server Analysis Services (tablični ili višedimenzionalne) i poslužitelja SQL Server Reporting Services.

## <a name="data-asset"></a>Podaci resursa

Resursi podataka su objekti koji se nalazi unutar izvori podataka koje može biti registriran u katalogu. Primjeri tablice sustava SQL Server i prikaze, Oracle tablica i prikaza, SQL Server Analysis Services mjere, dimenzije i ključne pokazatelje uspješnosti i izvješća SQL Server Reporting Services.

## <a name="data-asset-location"></a>Mjesto resursa podataka

Katalog pohranjuje mjesto izvora podataka ili podataka resursa koji se može koristiti za povezivanje s izvorom pomoću klijentska aplikacija. Pojedinosti lokacije i oblika razlikuju se ovisno o vrsti izvora podataka. Ako, na primjer, tablicom sustava SQL Server mogu se prepoznati po nazivu četiri dijela – naziv poslužitelja, naziv baze podataka, naziv sheme, naziv objekta – dok je SQL Server Reporting Services izvješća mogu se prepoznati po njezin URL.

## <a name="structural-metadata"></a>Strukturni metapodataka

Strukturni metapodataka je metapodataka dobivenih iz izvora podataka koji opisuje strukturu podataka resursa. To obuhvaća mjesto resursi, njegov naziv objekta i vrsta i dodatne značajke specifične za vrstu. Ako, na primjer, strukturnih metapodataka za tablice i prikaze sadrži imena i vrste podataka za stupce objekta.

## <a name="descriptive-metadata"></a>Opisna metapodataka

Opisna metapodataka je metapodataka koji opisuje svrhu ili namjerom podataka resursa. Obično opisni metapodataka dodao kataloga korisnika pomoću portala za Azure katalog podataka, ali ga možete također se izdvajati iz izvora podataka tijekom registracije. Na primjer, alat za registraciju katalog podataka Azure će izdvojiti opise iz svojstvo opis u SQL Server Analysis Services i SQL Server Reporting Services i [ms_description proširena svojstva](https://technet.microsoft.com/library/ms190243.aspx) u bazama podataka za SQL Server, ako tih svojstava popunjeni s vrijednostima.

## <a name="request-access"></a>Traženje pristupa

Opisni metapodataka resursa podataka mogu sadržavati informacije o tome kako zatražiti pristup podacima jednu ili više izvora podataka. Ove informacije mogu se mjesto resursa podataka, a mogu sadržavati najmanje jednu od sljedećih mogućnosti:

- Adresa e-pošte korisnika ili tim odgovoran za dodjelu pristup izvoru podataka.
- URL dokumentirane procesa koje korisnici moraju slijediti ostvariti pristup izvoru podataka.
- URL programa identiteta i pristup alat za upravljanje (kao što je Microsoftov Upravitelj identiteta) koje je moguće koristiti za pristup izvoru podataka.
- Unos osloboditi teksta koji opisuje način na koji korisnici mogu ostvariti pristup izvoru podataka.

## <a name="preview"></a>Pretpregled

Pretpregled u katalog podataka Azure je snimka do 20 zapisa koji se može dobivenih iz izvora podataka tijekom registracije i spremljene u katalogu s metapodacima resursa podataka. Pretpregled omogućuju korisnicima koji otkrivanje podataka resursa bolje razumjeli funkcije i više nije potrebna. Drugim riječima, prikazuje ogledne podatke može biti koristan više od prikazuju samo nazivi stupaca i vrste podataka.
Pretpregled podržani su samo za tablice i prikaze, a morate izričito odabrati korisnik tijekom registracije.

## <a name="data-profile"></a>Podataka profila

Profil podataka u katalogu podataka Azure je snimka razine tablice i razini stupca metapodataka o imovini registriranih podataka koji se može dobivenih iz izvora podataka tijekom registracije i spremljene u katalogu s metapodacima resursa podataka. Podaci profila olakšavate korisnike otkrivanje podataka resursa bolje razumjeli funkcije i više nije potrebna. Slično pretpregleda, profili podataka mora biti izričito odabran korisnik tijekom registracije.

> [AZURE.NOTE] Izdvajanje podataka profila mogu počinjati postupak za velike tablice i prikaze, a može značajno povećati vrijeme potrebno da biste registrirali izvora podataka.

## <a name="user-perspective"></a>Perspektive korisnika

U katalogu podataka Azure, bilo koji korisnik unijeti opisni metapodataka za registrirani podataka resursa. Svaki korisnik ima distinct perspektive na podatke i njezino korištenje. Ako, na primjer, administrator odgovoran za poslužitelj pružati detalja njegov ugovor o razini usluge (SLA) ili sigurnosne kopije windows; Upravitelj podataka može vam dati veze potražite u dokumentaciji za tvrtke obrađuje podržava podataka; te se analitičar možda navedite i opis u odredbama koje su najrelevantniji na druge analitičar analitičar podataka, a koja može biti najviše koristan korisnicima kojima su potrebne da biste otkrili i razumijevanje podataka.

Svaki od tih perspektive su čini koristan i pomoću kataloga podataka Azure svakog korisnika možete unijeti podatke koji vam je smislen njima dok svi korisnici mogu koristiti te podatke za razumijevanje podataka i njezinu svrhu.

## <a name="expert"></a>Stručnjak

Stručnjaka je korisnik koji je označen kao pojavljuju informirali "stručnjak" perspektive imovine podataka. Bilo koji korisnik možete dodati sami ili neki drugi korisnik kao stručnjaka za sredstvo. Koji se prikazuju u obliku stručnjaka značenja sve dodatne ovlasti u katalogu podataka Azure; omogućuje korisnicima da biste lakše pronašli te perspektive koje su najvjerojatnije biti korisna prilikom pregleda za sredstvo opisni metapodataka.

## <a name="owner"></a>Vlasnik

Vlasnika je korisnik koji ima dodatne ovlasti za upravljanje podataka resursa u katalog podataka Azure. Korisnici mogu poduzeti vlasništvo imovine registriranih podataka, a vlasnici drugim korisnicima možete dodati kao zajednički vlasnici. Dodatne informacije potražite [u](data-catalog-how-to-manage.md) članku Upravljanje resursima podataka  
> [AZURE.NOTE] Vlasništvo i upravljanje njima su dostupne samo u standardni izdanje programa Azure kataloga podataka.

## <a name="registration"></a>Registracija

Registracija je čin izdvajanje metapodataka resursa podataka iz izvora podataka i kopiranje sa servisom Azure kataloga podataka. Resursi podataka registriranih možete se dodavati napomene i otkrivanja.

## <a name="see-also"></a>Vidi također

- [Što je katalog podataka Azure?](data-catalog-what-is-data-catalog.md) – Ovaj članak sadrži pregled servisa Azure katalog podataka, vrijednost u njoj nalaze i scenariji podržava.

- [Početak rada s Azure katalog podataka](data-catalog-get-started.md) – u ovom se članku daje završetka do kraja praktičnom vodiču koji pokazuje kako koristiti Azure katalog podataka za otkrivanje izvora podataka.  
