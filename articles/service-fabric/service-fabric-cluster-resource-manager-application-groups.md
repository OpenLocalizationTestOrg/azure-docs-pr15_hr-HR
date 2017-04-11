<properties
   pageTitle="Servis tkanina klaster Voditelj resursa - grupe aplikacija | Microsoft Azure"
   description="Pregled funkcija grupa aplikacije u tkanina klaster resursa Upravitelj servisa"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introduction-to-application-groups"></a>Uvod u grupe aplikacija
Voditelj resursa za servis tkanina klaster obično upravlja klaster resursi širenjem opterećenje (predstavljene putem metriku) ravnomjerno cijeloj klaster. Servis tkanina upravlja i kapacitet čvorovi u klasteru i klaster cijela kroz notion kapaciteta. To funkcionira odlično za mnogo različitih vrsta radnih opterećenja, ali uzorke koje čine intenzivnog korištenja instanci drugi servis tkanina aplikacije ponekad unijeti dodatni preduvjeti. Neki dodatni preduvjeti obično su:

- Mogućnost rezervirati kapaciteta za instancu aplikacije usluge na određeni broj čvorove
- Mogućnost da biste ograničili ukupan broj čvorove zadani skup servisa unutar aplikacije dopuštenog pokrenuti
- Definiranje kapaciteta instanci aplikacije same da biste ograničili broj ili ukupni resursa utrošak servisa unutar njega

Da bi se zadovoljava te preduvjete, ne možemo razvio podrška za što smo poziva za grupe aplikacija.

## <a name="managing-application-capacity"></a>Upravljanje aplikacije kapaciteta
Kapacitet računala može se koristiti da biste ograničili broj čvorove rastezati aplikacija, kao i ukupno metričkim opterećenje koji na aplikacije instanci pojedinačne čvorove. Može se koristiti i za rezerviranje resursa u klaster za aplikaciju.

Kapacitet računala može se postaviti za nove aplikacije prilikom stvaranja; može se ažurirati i za postojeće aplikacije koje su stvorene bez navođenja kapaciteta aplikacije.

### <a name="limiting-the-maximum-number-of-nodes"></a>Ograničavanje maksimalni broj čvorove
Najjednostavniji koristi slučaj kapaciteta aplikacije je kada je instancijacije aplikacije mora biti ograničeni na određene maksimalni broj čvorove. Ako nema kapaciteta aplikacija nije navedena, Voditelj resursa za servis tkanina klaster će stvoriti instancu replike prema običnom pravilima (opterećenja ili defragmentaciju), koje obično znači da je uslugama će proširite preko svih dostupnih čvorovi u klasteru, ili ako je na nekim proizvoljne, ali manji broj čvorove uključena defragmentaciju.

Sljedeća slika prikazuje potencijalne položaj instance aplikacije bez maksimalni broj čvorove definirani i zatim isti aplikacijom maksimalni broj čvorove postavite. Imajte na umu da ne postoji bez jamstva o koji replike ili instanci koje servisa će se nalaziti zajedno.

![Instance aplikacije koji definira maksimalni broj čvorove][Image1]

U lijevom primjer aplikacija ne sadrži aplikacije kapaciteta postavite i ima tri servisa. CRM učinio logičke odluku da želite proširenje svih replike preko šest dostupna čvorove da biste postigli najbolje saldo u klasteru. U ovom primjeru desnom dobivamo iste aplikacije koji je ograničeno na tri čvorove i gdje CRM tkanina servisa sadrži postići najbolje saldo za replike aplikacije servisa.

Parametar koji određuje takvo ponašanje zove MaximumNodes. Taj parametar možete postaviti tijekom stvaranja aplikacije ili za aplikacije instancu koja je već pokrenut, u kojem slučaju servisa tkanina CRM ograničili replike aplikacije servisa za definirani maksimalni broj čvorove ažurirati.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Metriku aplikacije, učitavanje i kapaciteta
Aplikacija grupe omogućuju da odredite metriku pridružene navedeni aplikacije instancu, kao i kapacitet aplikacije pogledu te metriku. Tako, primjerice možete definirati koji kao mnoge servise koje želite nije moguće stvoriti u

Za svaki metriku postoje 2 vrijednosti koje možete postaviti opisuje kapaciteta za tu instancu aplikacije:

-   Ukupno aplikacije kapaciteta – predstavlja ukupni kapacitet aplikacije za određenu metriku. Servis tkanina CRM će pokušati ograničiti zbroj metričkim opterećenje ove aplikacije servisa za određenu vrijednost; Osim toga, ako u aplikacijske usluge već zauzimaju Učitaj na ovo ograničenje, Voditelj resursa za servis tkanina klaster onemogućava stvaranja nove servise ili particije koji bi uzrokuju ukupni da biste otišli na to ograničenje.
-   Maksimalnog kapaciteta čvor – određuje najveći ukupni učitavanje replike na aplikacije servisa na jedan čvor. Ako je ukupna opterećenje čvor prelazi preko taj kapacitet, servis tkanina CRM će pokušati premjestiti replike ostale čvorove tako da je ograničenje kapaciteta poštuju.

## <a name="reserving-capacity"></a>Rezervirati kapaciteta
Drugi zajednički koristite za grupe aplikacija je da biste bili sigurni da resursi unutar klaster su rezervirana za dani aplikacije instancu, čak i ako instanci aplikacije za još nema services u njoj ili čak i ako ih ne troše resurse još. Pogledajmo kako će to funkcionira.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Određivanje najmanji broj čvorove i rezervacija resursa
Rezervirati resurse za instancu aplikacije potreban je određivanje nekoliko dodatnih parametara: *MinimumNodes* i *NodeReservationCapacity*

- MinimumNodes - baš kao i određivanje ciljnog maksimalni broj čvorove koje servisa unutar aplikacije mogu se izvoditi na, možete i navesti najmanji broj čvorove koje treba pokrenuti aplikaciju. Ta postavka učinkovito određuje broj čvorove koje resurse moraju biti rezervirana na barem jamstva kapaciteta unutar klaster kao dio stvaranja instance aplikacije.
- NodeReservationCapacity - u NodeReservationCapacity može se definirati za svaku metriku u aplikaciji. Ovim se definira količinu metričkim učitavanja rezervirana za aplikaciju na bilo koji čvor mjesto na kojem se nalazi nešto na replike ili instance servisa unutar njega.

Pogledajmo na primjer kapaciteta rezervacije:

![Instance aplikacije koji definira rezervirane kapaciteta][Image2]

U lijevom primjeru aplikacije imaju bilo koju aplikaciju kapaciteta definirani. Voditelj resursa za servis tkanina klaster će saldo podređeni services replike aplikacije i instance zajedno s onima iz drugih servisa (izvan aplikacije) da biste bili sigurni saldo u klasteru.

U primjeru s desne strane, recimo da je stvorena aplikacija s MinimumNodes postavljena na 2, MaximumNodes postavite 3 i aplikacije metriku definirana pomoću rezervacije 20, maksimalni kapacitet na čvor 50 i ukupan aplikacije kapacitet 100, tkanina servisa rezervirati kapaciteta na dva čvorove plava aplikacije, a ne dopuštaju druge replike klasteru za taj kapacitet. Taj kapacitet rezervirane aplikacija će se uvrstiti u potrošenih i izračunate protiv preostali kapacitet klaster.

Aplikacija stvaranja s rezervacija Voditelj resursa klaster rezervirati kapaciteta jednako MinimumNodes * NodeReservationCapacity klaster, ali će rezervirati kapaciteta na određene čvorove dok se ne stvara i smjestiti replike aplikacije servisa. Time se omogućuje za fleksibilnosti, budući da su čvorove odabrali za nove replike samo kada se stvaraju. Kapacitet je rezervirana na određeni čvor stavljanju barem jedan replike na njemu.

## <a name="obtaining-the-application-load-information"></a>Nabavljanje učitavanja podataka aplikacije
Za svaku aplikaciju da ima kapacitet aplikacije definirani koje možete dobiti informacije o zbrajanja opterećenje dojavi replike njegove servise. Tkanina servis nudi upita PowerShell i upravljani API za tu svrhu.

Na primjer, učitavanja mogu biti dohvaćeni pomoću cmdleta komponente PowerShell sustava sljedeće:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Izlaz iz ovaj upit će sadržavati osnovne informacije o kapaciteta aplikacije koja je određena za aplikaciju, kao što su čvorove minimalne i maksimalne čvorove. Postoji bit će podaci o broju čvorove aplikacija trenutno koristite. Dakle, za svaku metriku učitavanja će se informacije o:
- Metričkim naziv: Naziv mjerenja.
-   Rezervacija kapaciteta: Klaster kapaciteta koja je rezervirana u klasteru za ovu aplikaciju.
-   Učitavanje aplikacije: Učitavanja ukupni replike podređeni ove aplikacije.
-   Kapacitet aplikacije: Najviše dopuštena vrijednost aplikacije opterećenja servisa.

## <a name="removing-application-capacity"></a>Uklanjanje aplikacije kapaciteta
Kada aplikacije kapaciteta parametre postavljene za aplikaciju, ih je moguće ukloniti pomoću ažuriranje aplikacije API-ji ili PowerShell cmdleti. Ako, na primjer:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Ta se naredba uklonit će sve parametre kapaciteta aplikacije iz aplikacije, a Voditelj resursa za servis tkanina klaster počet će ovu aplikaciju tretira kao neke druge aplikacije u skupini koji imaju parametara definirani. Efekt naredbu je trenutna i upravljanja resursima klaster izbrisat će se sve parametre kapaciteta aplikacije za ovu aplikaciju Određivanje ih ponovno je potrebna ažuriranje aplikacije API pozivanje s odgovarajućim parametra.

## <a name="restrictions-on-application-capacity"></a>Ograničenja aplikacije kapaciteta
Postoji nekoliko ograničenja aplikacije kapaciteta parametre koje morate poštuju. U slučaju pogreške provjere valjanosti, stvaranje ili ažuriranje aplikacija će biti odbio poruku o pogrešci.
Sve parametre cijeli broj moraju biti brojevi koji nije negativan.
Nadalje, za pojedinačne parametre su ograničenja na sljedeći način:

-   MinimumNodes nikad ne mora biti veći od MaximumNodes.
-   Ako definirate kapaciteta metrike opterećenja, pa ih morate pridržavajte se sljedećih pravila:
  - Kapacitet rezervacija čvor ne smije biti veća od maksimalne čvor kapaciteta. Na primjer, ne možete ograničiti kapacitet metriku "Procesora" čvor 2 jedinice i pokušate rezervirati 3 jedinice na svakom čvor.
  - Ako je naveden MaximumNodes, zatim umnožak MaximumNodes i maksimalnog kapaciteta čvor ne smije biti veći od ukupnog kapaciteta aplikacije. Na primjer, ako ste postavili maksimalnog kapaciteta čvor za mjerenje učitavanja "Procesora" 8 i postavljanje čvorove maksimalno do 10, onda ukupni kapacitet aplikacije mora biti veći od 80 za ovu metriku Učitaj.

Ograničenja provode se i prilikom stvaranja aplikacije (na klijentskoj strani) i tijekom ažuriranja aplikacije (na strani poslužitelja). Tijekom stvaranja, ovo je primjer Očisti krši zahtjevima od MaximumNodes < MinimumNodes i naredba neće uspjeti u klijentu prije nego što zahtjev čak i slanje klaster tkanina servisa:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Slijedi primjer ažuriranje nije valjano. Ako ne možemo snimili postojeće aplikacije i ažuriranje Maksimalna čvorove s nekom vrijednosti, proći kroz ažuriranja:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Nakon toga možete smo pokušaj ažuriranja minimalne čvorove:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Klijent nema dovoljno kontekst o aplikaciji tako da omogućuje ažuriranje za prosljeđivanje klaster tkanina servisa. Međutim, u skupini, tkanina servisa će Provjerite valjanost novi parametar zajedno s postojećeg parametre i neće uspjeti postupak ažuriranja jer čvorove minimalnu vrijednost foe veća od vrijednosti za maksimalnu čvorove. U ovom slučaju aplikacije kapaciteta parametara neće se promijeniti.

Ta ograničenja smještaju se na mjestu redoslijedom za klaster Voditelj resursa da biste mogli unijeti najbolje položaj za replike aplikacije servisa.

## <a name="how-not-to-use-application-capacity"></a>Kako koristiti aplikaciju kapaciteta

-   Korištenje kapaciteta aplikacije da biste ograničili aplikacija za određeni podskup čvorove: iako servisa tkanina provjerite da su Maksimalna čvorove poštuju za svaku aplikaciju koja ima kapacitet aplikacije navedene, korisnici ne možete odlučiti koji se čvorovi će se instancirati na. To se može postići pomoću položaj ograničenja za servise.
-   Da biste bili sigurni da dva servisi iste aplikacije uvijek će se nalaziti duž međusobno pomoću kapaciteta aplikacije. To se može postići pomoću afiniteta odnos između servisa, a afinitet može biti ograničeno samo na servise koje treba zapravo smjestiti zajedno.

## <a name="next-steps"></a>Daljnji koraci
- Za dodatne informacije o drugim mogućnostima dostupnima za konfiguriranje usluge odjavu temu na druge konfiguracije Voditelj resursa klaster dostupne [Dodatne informacije o konfiguriranju Services](service-fabric-cluster-resource-manager-configure-services.md)
- Da biste saznali kako Voditelj resursa klaster upravlja te salda Učitaj u klasteru, pogledajte članak na [za ujednačavanje opterećenja](service-fabric-cluster-resource-manager-balancing.md)
- Počnite s početka i [dobiti Uvod za servis tkanina klaster upravitelj resursa](service-fabric-cluster-resource-manager-introduction.md)
- Dodatne informacije o metriku funkcioniranje Općenito, pročitajte više o [Servisa tkanina učitavanja mjerenja](service-fabric-cluster-resource-manager-metrics.md)
- Voditelj resursa klaster sadrži mnogo mogućnosti za s opisom klaster. Da biste saznali više o njima pogledajte ovaj članak na [s opisom servisa tkanina klaster](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
