<properties
   pageTitle="Nadzor stanja u servis tkanina | Microsoft Azure"
   description="Uvod u stanju tkanina servisa Azure nadzor modela koji omogućuje praćenje klaster i aplikacija i servisa sustava."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Uvod u praćenje stanja servisa tkanina
Azure tkanina servisa predstavlja stanja modelu koji omogućuje procjenu obogaćenog, fleksibilne i extensible stanja i izvješćivanje o pogreškama. Model omogućuje blizu--sadržajima praćenje stanja klaster i servise u njoj. Jednostavno možete dobiti informacije o stanju i ispravljanje potencijalni problemi prije nego što ih kaskadno zbog čega pretraživanje velikog kvarove. U standardne modelu services slanje izvješća na temelju njihovih lokalne prikaza i da se podaci dodaje možete unijeti na Ukupno klaster razini prikaza.

Servis tkanina komponente koristiti ovaj model obogaćenog stanja da biste prijavili njihovo trenutno stanje. Možete koristiti isti mehanizam stanje izvješća iz aplikacije. Ako ulažete u stanju visoke kvalitete izvješćivanju koji opisuje uvjete prilagođene možete otkriti i otklanjanje problema vezanih uz pokrenute aplikacije mnogo lakše.

> [AZURE.NOTE] Ne možemo pokrenuti podsustav stanja da biste riješili potrebe za nadziranim nadogradnji. Servis tkanina nudi nadziranim aplikacije i klaster nadogradnje osiguraj puni, bez nedostupnost i Minimalni broj stranica bez intervencije korisnika. Da biste postigli te ciljeve nadogradnje provjerava stanja na temelju konfigurirana nadogradnje pravila, a omogućuje nadogradnju da biste nastavili samo kada stanja poštuje željeni pragovi. U suprotnom, Nadogradnja je automatski vraćen ili pauzirano da bi se dobilo administratori vjerojatnost da biste riješili probleme. Dodatne informacije o nadogradnji aplikacije, pročitajte [Ovaj članak](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Stanje spremišta
Spremište stanja zadržava vezanih stanju informacije o entiteti u skupini lakšeg dohvaćanja i procjenu. Implementira se kao što je servis tkanina dosljedan s praćenjem stanja servisa da biste bili sigurni visoke dostupnosti i skalabilnost. Spremište stanja je dio na **tkanina: / sustava** aplikacije, a je dostupna kad je klaster prema gore i pokrenut.

## <a name="health-entities-and-hierarchy"></a>Stanje entiteti i hijerarhije
Stanje entiteti su organizirane u logička hijerarhija koji opisuje interakcije i ovisnosti među različite entiteti. Entiteti i hijerarhije automatski ugrađen u spremištu stanja na temelju izvješća poslao tkanina servisa komponente.

Stanje entiteti odražavati entiteti tkanina servisa. (, Na primjer, **stanja aplikacije entitet** odgovaraju instance aplikacije implementiran u skupini, u **Stanje čvor entitet** odgovara čvor klaster servisa tkanina.) Hijerarhija stanja snima interakcije Sistemski entiteti te je temelj za procjenu napredne stanja. Koje dodatne informacije o koncepata tkanina servisa u [servis tkanina Tehnički pregled](service-fabric-technical-overview.md). Dodatne informacije o aplikaciji potražite u članku [servis tkanina modelu](service-fabric-application-model.md).

Stanje entiteti i hijerarhije omogućuju klaster i aplikacije mogu učinkovito prijavljena, debugged i nadzirati. Stanje model omogućuje programa točne, *zrnastog* prikaz stanja mnogo premještanje dijelova u klasteru.

![Stanje entiteti.][1]
Entiteti stanja organizirane u hijerarhiji na temelju nadređeno-podređene odnose.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Entiteti stanje su:

- **Klaster**. Predstavlja stanje servisa tkanina klaster. Izvješća o stanju klaster opisuju uvjete koje utječu na cijelu klaster, a ne narrowed prema dolje za jedan ili više dobro djece. Primjeri početnog klaster razdvajanje zbog problema s particija ili komunikacije mreže.

- **Čvor**. Predstavlja stanje servisa tkanina čvor. Izvješća o stanju čvor opisuju uvjeta koje utječu na funkciju čvor. Obično utječu distribuiranih entiteti izvodi na njemu. Primjeri kada čvor nema dovoljno prostora na disku (ili drugi svojstvo razini računala, kao što su memorije, veze), a kada je čvor prema dolje. Entitet čvor ima naziv čvora (niz).

- **Aplikacija**. Predstavlja stanje instance aplikacije izvodi u klasteru. Izvješća o stanju aplikacije opisuju uvjeta koje utječu na stanje aplikacije. Oni ne može biti narrowed prema dolje na pojedinačne podređene (servise ili distribuiranih aplikacije). Primjeri interakcije završetka do kraja među različitim servisima u aplikaciji. Aplikacija entitet određuje naziv aplikacije (URI).

- **Servis**. Predstavlja stanje servisa koji se izvodi u klasteru. Izvješća o stanju servisa opisuju uvjeta koje utječu na stanje servisa pa ih ne može biti narrowed particije ili kopiju. Primjeri servisa konfiguracije (primjerice priključak ili zajedničko korištenje vanjskim datoteka) koji je uzrok problema vezanih uz sve particije. Servis entitet određuje naziv usluge (URI).

- **Particije**. Predstavlja stanje servisa particije. Izvješća o stanju particija opisuju uvjeta koje utječu na cijelu replike skupa. Primjeri kada je broj replike ispod cilj count i kada je particija kvorum gubitak. Entitet particija određuje particija ID (GUID).

- **Replike**. Predstavlja stanju s praćenjem stanja servisa replike ili instancu bez praćenja stanja servisa. Najmanja jedinica watchdogs i komponente sustava za aplikaciju možete prijaviti na. Za s praćenjem stanja servisa Primjeri primarni replike izvješćivanje o pogreškama kad se ne može replicirati operacije secondaries i kada replikacija nije prelaska na polje očekivane brzinom. Osim toga, bez praćenja stanja instance možete je prijaviti kad nema dovoljno resursa ili ima problema s povezivanjem. Entitet replike je označena particija ID (GUID) i ID replike ili instancu (dugi).

- **DeployedApplication**. Predstavlja stanja programa *aplikacija izvodi na čvor*. Izvješća o stanju distribuiranih aplikacije opisuju uvjeta specifične za aplikaciju na čvor ne narrowed prema dolje da biste pakete usluga implementiran na istom čvor. Primjeri kada Aplikacijski paket nije moguće preuzeti na tom čvor, a kada postoji problem postavljanja aplikacije sigurnosni upravitelji na čvor. Distribuiranih aplikacija je označena naziv aplikacije (URI) i naziv čvora (niz).

- **DeployedServicePackage**. Predstavlja stanje servisa paket sustavom čvor u klasteru. Opisuje uvjete specifične za paket servisa koje utječu na druge usluge pakete na isti čvor iste aplikacije. Primjeri paket kod u paket servisa koji se ne može pokrenuti i konfiguracija paket koji se ne može pročitati. Paket distribuiranih servis je označena naziv aplikacije (URI), čvor naziva (niz) i naziv manifesta usluge (niz).

Preciznosti modela stanja olakšava otkrivanje i ispravljanje problema. Ako, na primjer, ako servisa ne reagira, je izvedivo da biste prijavili je li dobro instanci aplikacije. Izvješćivanje o pogreškama na razini nije li idealna, međutim, jer je problem možda neće biti utječe na sve servise te aplikacije. Izvješće treba primijeniti dobro servisa ili određene podređene particije, ako više informacija upućuje na tom particije. Podaci automatski površine kroz hijerarhiji, a dobro particija postala je vidljiva na razini usluge i aplikacije. U ovom zbrajanja olakšava pinpoint i brže riješiti uzrok problema.

Hijerarhija stanja se sastojati od nadređeno-podređene odnose. Klaster sastoji se od čvorove i aplikacije. Aplikacije su servise i implementirati aplikacije. Distribuiranih aplikacije Uveli pakete usluga. Usluga imati particije, a svaki particija sadrži jednu ili više replike. Postoji posebno odnosa između tablica i čvorove distribuiranih entiteti. Ako je čvor dobro kao dojavi njegove komponente sustava za izdavanje certifikata (servis za prebacivanje Upravitelj), utječe na distribuiranih aplikacije, pakete usluga i replike implementiran na njemu.

Hijerarhija stanja predstavlja najnovije stanje sustav na temelju najnovija izvješća o stanju koje je informacija gotovo u stvarnom vremenu.
Interne i vanjske watchdogs mogu se prijaviti na isti entiteti na temelju logike specifičnim aplikacijama ili prilagođeni nadziranim uvjeta. Korisnička izvješća postojati zajedno sa sustava izvješća.

Planiranje ulažete u izvješće i odgovaranje na stanje tijekom dizajna servise u oblaku velike da biste olakšali servis za ispravljanje pogrešaka, praćenje i raditi.

## <a name="health-states"></a>Stanje stanja
Servis tkanina koristi tri stanja stanja opisuje je li entitet dobar ili ne: u redu, upozorenja i pogreške. Svako se izvješće šalje u trgovini stanja morate navesti jedno od tih stanja. Rezultat izvođenja stanja je jedan od ta stanja.

Mogući [statusi stanje](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) su:

- **OK**. Dobar je entitet. Ne postoje Poznati problemi prijavili na i podređenim mu objektima (kada je primjenjivo).

- **Upozorenje**. Entitet iskustvo neke od problema, ali još nije dobro (na primjer, postoje kašnjenja, ali ne uzrokuju probleme funkcionalni još). U nekim slučajevima sam uvjet upozorenje može riješiti bez intervencije posebno pa je korisna uvid u što se događa. U drugim slučajevima uvjet upozorenje biti slabije loši problema bez intervencije korisnika.

- **Pogreška**. Dobro je entitet. Akcija treba poduzeti da biste riješili problem stanje entitet, jer ne može ispravno funkcionirati.

- **Nepoznato**. Entitet ne postoji u spremištu stanja. Sljedeći rezultat biti dobivenog raspodijeljeno upite koji spajanje rezultata iz većeg broja komponenti. Na primjer, dobiti čvor upit bude **FailoverManager** i **HealthManager**, upit aplikaciju get vodi do **ClusterManager** i **HealthManager**. Tih upita spajanje rezultata iz većeg broja komponente sustava. Ima li druge komponente sustava vraća entitet koji nije dostigao ili je očistiti iz trgovine stanja, spojenih rezultata Nepoznato stanja.

## <a name="health-policies"></a>Stanje pravila
Spremište stanja primjenjuje pravila stanja da biste utvrdili je li entitet dobar na temelju njegove izvješća i podređenim mu objektima.

> [AZURE.NOTE] Pravila stanja možete navesti manifest klaster (procjene klaster i čvor stanja) ili u programski manifest (za procjenu aplikacije i svih podređenih). Zahtjevi za procjenu stanja i prenositi za procjenu pravila prilagođenih stanja koji se koristi samo za taj izračun.

Po zadanome, servis tkanina primjenjuje ista pravila (sve mora biti dobar) za nadređenih i podređenih hijerarhijski odnos. Ako je još jedan od podređenih jedan dobro događaj, nadređenog smatra dobro.

### <a name="cluster-health-policy"></a>Pravilo stanja klaster
[Pravilo stanja klaster](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) koristi se za procjenu klaster stanja i čvor stanja stanja. Pravilnik može se definirati u manifestu klaster. Ako ga nema, koristi se zadani pravilnik (nula dopušteni neuspjeha).
Pravilo stanja klaster sadrži:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Određuje je li tretira upozorenje o stanju izvješća kao pogreške tijekom izvođenja stanja. Zadani: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Određuje najveći dopušteni postotak aplikacije koje mogu biti dobro prije klaster smatra se prikazuje pogreška.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Određuje najveći dopušteni postotak čvorove koje mogu biti dobro prije klaster smatra se prikazuje pogreška. U velikim klastere neke čvorove su uvijek prema dolje ili prema van za popravke, pa postotkom mora biti konfigurirana tako da tolerate koji.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Karte pravila stanja vrsta računala može se koristiti tijekom izvođenja stanja klaster opisuje vrsta posebno aplikacija. Prema zadanom sve aplikacije se staviti u zajedničko područje, a koji se izračunava s MaxPercentUnhealthyApplications. Ako neke vrste aplikacija treba je tretirati drugačije, oni se može preuzeti iz globalne skup. Umjesto toga se procjenjuju protiv postotaka pridružene njihov naziv vrste aplikacija na karti. Ako, na primjer, klasteru postoje tisuće aplikacije različitih vrsta i nekoliko kontrola aplikacije instance vrsta posebno aplikacije. Aplikacije za kontrolu nikad ne smiju biti pogrešku. Možete navesti globalni MaxPercentUnhealthyApplications 20% tolerate neke pogreške, ali za vrstu aplikacije "ControlApplicationType" postavite na MaxPercentUnhealthyApplications na 0. U slučaju da su neke mnoge aplikacije dobro, ali ispod globalni dobro postotak klaster bi moguće vrednovati kao upozorenje. Stanja za upozorenje utjecati klaster Nadogradnja ili druge nadzor koji se prikazuje kada stanja pogreške. No aplikacije čak i jednu kontrolu s pogreškom bi klaster dobro, koji se pokreće Vrati ili zaustavlja nadogradnje klaster, ovisno o konfiguraciji za nadogradnju.
Za aplikacije vrste definiran na karti, sve instance aplikacije uzimaju se iz globalne skup aplikacije. Se procjenjuju na temelju ukupan broj aplikacije vrste računala, pomoću određenih MaxPercentUnhealthyApplications iz mape. Sve ostale aplikacije ostati u globalni i vrednuju se s MaxPercentUnhealthyApplications.

Sljedeći primjer je u isječak iz klaster manifest. Da biste definirali stavke na karti vrsta aplikacije, prefiks naziv parametra s "ApplicationTypeMaxPercentUnhealthyApplications-", a zatim upišite naziv aplikacije.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Pravilnik o stanju aplikacije
[Pravilnik o stanju aplikacije](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) opisuje kako učiniti vrednovanju događaja i podređenih stanja zbrajanja za aplikacije i svoje podređenim objektima. Može se definirati u aplikaciji manifestu, **ApplicationManifest.xml**u Aplikacijski paket. Ako su navedeni bez pravilnika, tkanina servisa pretpostavlja da je entitet dobro ako je izvješće o stanju ili podređeni pri stanja upozorenja ili pogreške.
Može se konfigurirati pravila su:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Određuje je li tretira upozorenje o stanju izvješća kao pogreške tijekom izvođenja stanja. Zadani: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Određuje najveći dopušteni postotak distribuiranih aplikacije koje mogu biti dobro prije aplikacije smatra se prikazuje pogreška. Postotak izračunava se dijeljenjem broja dobro distribuiranih aplikacije putem broj čvorove koji aplikacije trenutno su raspoređeni na u klasteru. Za izračunavanje se zaokružuje naviše na tolerate jedan pogrešku na mali broj čvorove. Zadani postotak: nula.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Određuje zadani servis vrsta stanja pravilnik, koja zamjenjuje zadani pravilnik o stanju za sve vrste servisa u aplikaciji.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Sadrži kartu pravila za stanje servisa po vrsti servisa. Ta pravila zamijeniti stanja pravila zadane vrste servisa za svaku vrstu navedeni servis. Ako, na primjer, ako aplikacije sadrži vrstu pristupnika bez praćenja stanja servisa i vrstu servisa s praćenjem stanja modul, drugačije možete konfigurirati pravila stanja njihove procjene. Kada odredite pravila po vrsti servisa, možete dobiti precizniji kontrolu nad stanja servisa.

### <a name="service-type-health-policy"></a>Pravilo za stanje servisa vrstu
[Pravilo za stanje servisa vrstu](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) određuje način procijeniti i prikupljati servise i podređenih servisa. Sadrži pravila:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Određuje najveći dopušteni postotak dobro razdjeljivanja prije servisa smatra dobro. Zadani postotak: nula.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Određuje najveći dopušteni postotak dobro replike prije particije smatra dobro. Zadani postotak: nula.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Određuje najveći dopušteni postotak dobro servisa prije aplikacija se smatra dobro. Zadani postotak: nula.

Sljedeći primjer je u isječak iz programski manifest:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Procjena stanja
Korisnici i automatiziranog services može vrednovati stanja za svaki entitet u bilo kojem trenutku. Da biste procijenili stanja neki entitet, trgovine zbrajanja stanja sve stanja izvješća na entitet i procjenjuje svih podređenih (kada je primjenjivo). Zbrajanje algoritam stanja koristi pravila stanja koji određuju kako se vrednuje izvješća o stanju i aggregate podređeni stanja državama (kada je primjenjivo).

### <a name="health-report-aggregation"></a>Zbrajanje izvješća o stanju sustava
Jedan entitet može imati više izvješća o stanju poslao različite reporters (komponente sustava ili watchdogs) na različita svojstva. Skupljanja u posebno koristi pravila pridružene stanja ConsiderWarningAsError član aplikacije ili klaster pravilo stanja. ConsiderWarningAsError određuje način za procjenu upozorenja.

Zbrojeno stanja aktivira *najgorim* izvješća o stanju na entitet. Ako je barem jedan izvješće o pogrešci stanja, Zbrojeno stanja je pogreška.

![Stanje izvješća zbrajanja izvješće o pogrešci.][2]

Stanje entitet koja sadrži jedan ili više pogreške izvješća o stanju vrednuje kao pogreška. Isto vrijedi i za izvješće o stanju mirovanja, bez obzira na stanje stanja.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Ako nema izvješća o pogreškama i jedno ili više upozorenja, Zbrojeno stanja je upozorenja ili pogreške, ovisno o ConsiderWarningAsError zastavica pravila.

![Zbrajanja izvješća o stanju s upozorenjem izvješća i ConsiderWarningAsError false.][3]

Zbrajanje izvješća stanja s upozorenjem izvješća i ConsiderWarningAsError postavite na false (zadano).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Podređeni stanja zbrajanja
Zbrojeno stanja entitet odražava stanje stanja podređeni (kada je primjenjivo). Algoritam za zbrajanje podređeni stanja državama koristi stanja pravilnike primjenjivo ovise o vrsti entitet.

![Podređeni entiteti stanja zbrajanja.][4]

Zbrajanje podređeni na temelju pravila stanja.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Kada je stanje spremište sadrži koji se izračunava sve podređene, objedinjuje njihova stanja stanja konfigurirani maksimalan postotak dobro djece na temelju. Taj postotak koristi se iz pravila ovise o vrsti entitet i podređenih.

- Ako sve podređene stanja u redu, stanja podređeni pridružuje je u redu.

- Ako djece u redu i država upozorenje stanja podređeni pridružuje se upozorenje.

- Postoji li djece s stanja pogreške koja poštuje najveći dopušteni postotak dobro podređenih, Zbrojeno stanja je pogreška.

- Ako podređenih s stanja pogreške poštuje najveći dopušteni postotak dobro podređenih, Zbrojeno stanja se upozorenje.

## <a name="health-reporting"></a>Izvješćivanje o stanju
Komponente sustava, aplikacija sustava tkanina i watchdogs interno/vanjsko mogu se prijaviti protiv entiteti tkanina servisa. Na reporters provjerite *lokalne* determinations stanja nadzirane entiteti, na temelju uvjeta oni su nadzor. Ne morate možete pogledati globalno stanje ni prikupljanje podataka. Na željeni način je da bi jednostavno reporters, a ne složene organisms koje je potrebno pogledati mnogo je toga prepustite programu koji će se podaci poslati.

Slanje podataka stanja u trgovini stanja, na financije mora prepoznavanje problematične entitet i stvaranje izvješća o stanju sustava. Izvješća mogu poslati putem API pomoću [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx), putem komponente PowerShell ili kroz OSTALE.

### <a name="health-reports"></a>Izvješća o stanju
[Izvješća o stanju](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) za svaku entiteti u klasteru sadrže sljedeće informacije:

- **ID-a izvora**. Niz koji služi kao jedinstvena identifikacija financije stanja događaj.

- **Identifikator entitet**. Prepoznaje entitet gdje se primjenjuje izvješća. Razlikuje se ovisno o [entitet upišite](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Klaster. Ništa.

  - Čvor. Naziv čvora (niz).

  - Aplikacija. Naziv aplikacije (URI). Predstavlja naziv instance aplikacije koji je implementiran u klasteru.

  - Servis. Naziv servisa (URI). Predstavlja naziv instance servisa koji je implementiran u klasteru.

  - Particije. ID particije (GUID). Predstavlja jedinstveni identifikator particije.

  - Replike. ID replike s praćenjem stanja servisa ili ID instance bez praćenja stanja servisa (INT64).

  - DeployedApplication. Naziv aplikacije (URI) i naziv čvora (niz).

  - DeployedServicePackage. Naziv aplikacije (URI), čvor naziva (niz) i servis manifesta naziv (niz).

- **Svojstvo**. *Niz* (ne fixed Enumeracije) koji omogućuje financije Kategorizirajte događaj stanja za određene svojstvo entitet. Na primjer, financije odgovora mogu prijaviti stanju svojstvo Node01 "prostora za pohranu" i financije B možete je prijaviti stanja svojstvo Node01 "povezivanje". U trgovini stanja takva izvješća smatraju se događaji zasebnom stanja za entitet Node01.

- **Opis**. Niz koji omogućuje Financije na pružaju podrobne informacije o stanju događaj. **ID-a izvora**, **Svojstva**i **HealthState** mora u potpunosti opišite izvješće. Opis dodaje Ljudski čitljiv informacije o izvješća. Tekst olakšava administratorima i korisnicima da biste shvatili izvješće o stanju.

- **HealthState**. Programa [Enumeracije](service-fabric-health-introduction.md#health-states) koji opisuje stanja izvješća. Prihvaćeni vrijednosti su u redu, upozorenja i pogreške.

- **TimeToLive**. Vremenski raspon koji pokazuje koliko izvješće o stanju je valjan. Povezano s **RemoveWhenExpired**, ona se omogućuje stanja trgovine zna da biste procijenili istekle događaja. Po zadanom je vrijednost beskonačno pa je izvješće valjani zauvijek.

- **RemoveWhenExpired**. Booleove vrijednosti. Ako je postavljen na true, izvješće o istekle stanju automatski uklanjaju iz spremišta stanja i izvješća ne utječe na procjenu entitet stanja. Koristi kada izvješće vrijedi za navedeno razdoblje samo vrijeme, a na financije ne morate izričito isključite. Također se koristi da biste izbrisali izvješća iz trgovine stanje (Ako, na primjer, u watchdog mijenja se i slati izvješća s prethodne izvora i svojstva). To možete poslati izvješće s kratak TimeToLive zajedno s RemoveWhenExpired da biste očistili gore sve prethodno stanje iz trgovine stanja. Ako je vrijednost FALSE, istekle izvješća tretira se kao pogreška na analizu stanja. Vrijednost false signale spremište stanja da izvorišnog web-mjesta potrebno izvješća povremeno na to svojstvo. Ako ne, zatim mora postojati nešto problem s na watchdog. Stanja u watchdog prikazuju se preporučuje se kao pogreška.

- **SequenceNumber**. Pozitivni cijeli broj potrebno ikad povećanje, predstavlja redoslijeda izvješća. Koristi se u spremištu stanja da biste otkrili zastarjele izvješća primljene kašnjenja zbog kašnjenja mreže ili drugi problemi. Izvješće je odbijena ako se redni broj je manja od ili jednaka najčešće nedavno primjenjuje broj za isti entitet, izvor i svojstva. Ako nije naveden, automatski generira se redni broj. Nije potrebno staviti u redni broj samo prilikom prijavljivanja na stanje prijelaze. U tom slučaju izvorišnog web-mjesta mora Imajte na umu uvrštavanje izvješća ga poslati i zadržali podatke za oporavak na prebacivanje.

Ove četiri komada podataka – ID-a izvora, identifikator entitet, svojstva i HealthState – su potrebni za svaku izvješće o stanju. ID-a izvora niz nije dopuštena započeti s prefiksom "**sustava.**", koji je rezervirana za sustav izvješća. Odnosi na isti entitet, postoji samo jedno izvješće za isti izvor i svojstva. Više izvješća za istu izvora i svojstvo nadjačati međusobno povezani, na klijentskoj strani stanje (Ako je oni su odbacivanja) ili o stanju pohraniti strani. Zamjena temelji se na brojeve u nizu; novija izvješća (s veći brojeve u nizu) zamijenite starije izvješća.

### <a name="health-events"></a>Stanje događaja
Interno, trgovine stanja zadržava [stanja događaja](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx)koji sadrže sve podatke iz izvješća i dodatne metapodatke. Metapodataka sadrži vrijeme izvješće dobiven klijentu stanja i vrijeme izmjene na strani poslužitelja. Događaji stanja vraćaju se upiti [stanja](service-fabric-view-entities-aggregated-health.md#health-queries).

Dodane metapodataka sadrži:

- **SourceUtcTimestamp**. Vrijeme izvješće predmetom stanja klijenta (Koordinirano univerzalno vrijeme).

- **LastModifiedUtcTimestamp**. Vrijeme zadnje izmjene izvješća na strani poslužitelja (Koordinirano univerzalno vrijeme).

- **IsExpired**. Zastavica koji određuju hoće li izvješće istekao je prilikom izvršenja upita u spremištu stanja. Događaj može biti istekla samo ako je vrijednost false RemoveWhenExpired. U suprotnom će se događaj je vratio upit i je uklonjena iz trgovine.

- **LastOkTransitionAt**, **LastWarningTransitionAt** **LastErrorTransitionAt**. Zadnji put prijelaza u redu/upozorenje/pogreške. Ta polja dati povijest stanja Prijelazi stanja za događaj.

Polja stanja prijelaza moguće je koristiti za pametnije upozorenja ili podatke o događaju "povijesnim" stanje. Scenariji omogućuju kao što su:

- Upozorenje kada je svojstvo na upozorenje/pogreška za više od X minuta. Provjera uvjeta za određeno vremensko razdoblje izbjegava upozorenja na privremene uvjeta. Na primjer, upozorenja ako stanja sadrži je upozorenje više od pet minuta bilo moguće prevesti (HealthState == upozorenja i – LastWarningTransitionTime > 5 minuta).

- Upozorenja samo na uvjetima koje su promijenjene u zadnjih X minuta. Ako je izvješće već na pogreške prije određenog vremena, možete je zanemariti jer ona se je prethodno već signaled.

- Ako je svojstvo prebacivanja između upozorenja i pogreške, odrediti koliko je dobro (to jest, nije u redu). Na primjer, upozorenje ako je svojstvo nije dobar više od pet minuta bilo moguće prevesti (HealthState! = u redu i – LastOkTransitionTime > 5 minuta).

## <a name="example-report-and-evaluate-application-health"></a>Primjer: Izvješća i procjenu stanja aplikacije
U sljedećem primjeru šalje izvješće o stanju sustava putem komponente PowerShell u aplikaciji **tkanina: / WordCount** iz izvora **MyWatchdog**. Izvješće o stanju sadrži informacije o stanju svojstvo "dostupnosti" u stanju stanja pogreške s beskonačno TimeToLive. Zatim upiti stanja računala, koju vraća pridružuje stanja stanja pogreške i događaje prijavljenim stanja na popisu stanja događaja.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Korištenje modela stanja
Stanje model omogućuje servise u oblaku i temeljni tkanina servisa platforme za promjenu veličine, jer su nadzor i stanje determinations distributed među različitim monitorima unutar klaster.
Drugi sustavi imati servis jedne, središnje razini klaster raščlanjuje sve *potencijalno* korisne informacije čuje Services. Taj se način ometa njihove skalabilnost. I ne omogućuje njihovo prikupljanje određene informacije u rješavanju problema i potencijalne probleme kao blizu uzrok moguće.

Model stanja intenzivnog se koristi za praćenje i Dijagnostika za procjenu klaster i aplikacije stanja te o nadziranim nadogradnjama. Druge servise stanja podatke koristiti za izvođenje automatsko popravaka, sastavljanje klaster stanja povijesti i problema upozorenja na određenim uvjetima.

## <a name="next-steps"></a>Daljnji koraci
[Prikaz izvješća o stanju servisa tkanina](service-fabric-view-entities-aggregated-health.md)

[Korištenje izvješća o stanju sustava za otklanjanje poteškoća](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Upute za izvješća i provjerite stanje servisa](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Dodavanje prilagođenih izvješća o stanju servisa tkanina](service-fabric-report-health.md)

[Praćenje i dijagnosticiranje lokalno services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Nadogradnja aplikacije servisa tkanina](service-fabric-application-upgrade.md)
