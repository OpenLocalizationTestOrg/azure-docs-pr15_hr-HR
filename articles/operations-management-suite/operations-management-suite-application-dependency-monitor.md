<properties
   pageTitle="Aplikacija ovisnost monitora (Admin) u paketu za upravljanje operacije (OMS) | Microsoft Azure"
   description="Aplikacija ovisnost monitora (Admin) je rješenje operacije upravljanja paket (OMS) koja automatski otkriva aplikacije komponente u sustavima Windows i Linux i mapira komunikaciju između servisa.  Ovaj članak sadrži detalje o implementaciji Admin u okruženju sustava i koriste u različitim scenarijima."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Pomoću aplikacije ovisnost Monitor rješenja u paketu operacije upravljanja (OMS)
![Ikona upozorenja upravljanja](media/operations-management-suite-application-dependency-monitor/icon.png) Aplikacija ovisnost monitora (Admin) automatski otkrije aplikacije komponente u sustavima Windows i Linux i karata komunikaciju između servisa. Omogućuje prikaz poslužitelja kao što mislite da ih – kao međusobno povezanih sustavi za izlaganje ključnih services.  Aplikacija ovisnost Monitor prikazuje veze između poslužitelja, a zatim procesa, i priključke preko bilo koje arhitektura TCP povezani s bez konfiguracijom obavezno osim instalacije agent.

U ovom se članku opisuju detalje korištenja aplikacije ovisnost Monitor.  Informacije o konfiguriranju Admin i za uhodavanje agenata potražite u članku [Konfiguriranje monitora računala ovisnost rješenja u paketu operacije upravljanja (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Aplikacija ovisnost Monitor trenutno privatne pretpregled.  Možete zatražiti pristup Admin privatne pretpregled pri [https://aka.ms/getadm](https://aka.ms/getadm).
>
>Tijekom privatne pretpregleda su svi računi OMS Neograničeni pristup ADM.  Čvorovi Admin besplatna, no zapisnika analize podataka za vrste AdmComputer_CL i AdmProcess_CL će se s ograničenim prometom kao što su druge rješenja.
>
>Nakon što Admin unosi javno pretpregled, bit će dostupna samo korisnicima besplatne i plaćenu uvid i analize u na OMS cijene Plan.  Besplatni sloju računi bit će ograničeni na 5 Admin čvorove.  Ako sudjeluju u privatnu pretpregled, a ne upisani u OMS cijene planiranje kad Admin unese javno pretpregled, tada će biti onemogućena Admin. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Korištenje slučajeva: Provjerite vaš IT obrađuje ovisnost umu

### <a name="discovery"></a>Otkrivanje
Admin sastavlja uobičajenih referenca kartu ovisnosti preko poslužitelje, procesa i 3 davatelja usluga.  On otkrije i karata sve TCP ovisnosti, označavanje iznenađenje veze, udaljene 3 strana sustavi ovise o i ovisnosti tradicionalni tamno dijelove mrežom kao što je DNS-a i AD.  Admin otkrije nije uspjelo mrežne veze koje sustavi za upravljane pokušavate učiniti, iskorištavanju prepoznavanje potencijalne konfiguraciji poslužitelja, kvarove servisa i problema s mrežom.

### <a name="incident-management"></a>Upravljanje incidenta
Admin olakšava uklonili nagađanja odvajanja problem s prikazom načina na koji su povezani sustavi i utjecaja na druge.  Osim nije uspjelo veze, informacije o povezanim klijentima pomoći prepoznavanje nisu pravilno konfigurirani učitavanja balancers, surprising ili viškom opterećenje ključnih servise i rogue klijentima poput strojeva za razvojne inženjere radi u produkcijske sustave.  Integrirano tijekovi rada s OMS promjena praćenja i omogućuje vam da vidite li događaj promjene na računalo pozadinske ili servis objašnjava uzrok incident.

### <a name="migration-assurance"></a>Migracija jamstvo
Admin omogućuje učinkovito planiranje, accelerate i provjeriti Azure Migracija jamči da ništa ne ostaje i postoje bez iznenađenja kvarove.  Možete otkriti svim međusobno zavisnih sustavima koje je potrebno migrirati zajedno, procijenite konfiguracija sustava i kapacitet i utvrdite li pokrenutog sustava je i dalje posluživanje korisnika ili kandidata zbog deaktivirati umjesto migracije.  Po završetku premještanje možete provjeriti na identitet da biste potvrdili da se povezujete test sustavi i klijentima i učitavanje klijenta.  Ako podmreže planiranje i vatrozid definicije postoje problemi, nije uspjelo veze na kartama Admin će vam pokažite na sustavi za koje je potrebno povezivanje.

### <a name="business-continuity"></a>Neprekidno poslovanje
Ako koristite oporavak Azure web-mjesta, a zatim je potrebna pomoć za definiranje slijeda oporavak aplikacije okruženju, Admin automatski prikazuje kako sustavi oslanjate međusobno osigurava da vaša tarifa za oporavak je pouzdan.  Odabir ključnih poslužitelja i prikazom svojim klijentima, možete prepoznati sučelja sustava koji treba oporaviti tek kada je taj ključnih poslužitelj vraćene i dostupna.  Nasuprot tome, tako da pogledate ovisnosti pozadinske ključnih server, možete prepoznati te sustavima koji se može oporaviti prije vratit će se vaš sustav fokus.

### <a name="patch-management"></a>Upravljanje zakrpa
Admin poboljšava korištenje OMS sustava ažuriranje rizika tako da pokazuje koja drugim timovima i poslužiteljima ovise o tome na servisu tako da možete obavijestiti ih unaprijed prije nego na sustavima prema dolje za zakrpa.  Admin i poboljšava zakrpu upravljanje u OMS tako da pokazuje servisa jesu li dostupan je i ispravno povezani nakon patched i ponovno pokrenuti. 


## <a name="mapping-overview"></a>Pregled mapiranja
Admin agenata Prikupite informacije o sve TCP povezani procese na poslužitelju na kojem ste instalirali, kao i detalje o vezama ulazni i izlazni za svaki proces.  Pomoću popisa računala na lijevoj strani Admin rješenja, računalima s agenata Admin možete odabrati vizualizacija njihove ovisnosti u odabranom vremenskom rasponu.  Ovisnost strojno karata fokus na određeni stroj te se navode strojeva koje su izravno TCP klijente ili poslužitelje tom računalu.

![Pregled Admin](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Računala može se proširiti na karti da biste prikazali izvodi procesa pomoću aktivni mrežnih veza unutar odabranog vremenskog razdoblja.  Kada udaljeno računalo s agent Admin je proširen da biste prikazali detalje procesa, prikazuju se samo procesa komunikaciju s računala fokus.  Na lijevoj strani procesa koji se mogu povezati s ikonom broj agentless sučelja računalima povezivanje u strojno fokus.  Ako strojno fokus je uspostavljanje veze s računalom pozadinske bez agent, pozadinske predstavlja s čvor u kartu koja prikazuje IPv4 adresa i čvor možete proširiti da bi se prikazala pojedine priključke i servise koje računalo fokus komunicirate.

Po zadanom Admin karte prikazuje posljednjih 10 minuta podaci o ovisnosti.  Pomoću kontrola vrijeme u gornjem lijevom kutu, karte možete može poslati upit za raspone povijesnim vremenom, prema gore za jedan sat wide, da biste prikazali kako pokušavali ovisnosti u prošlosti, primjerice tijekom incident ili prije došlo je do promjena.    Admin podaci se pohranjuju 30 dana u plaćenu radnim prostorima i sedam dana u besplatne radnim prostorima.

![Strojno map sa svojstvima odabranog računala](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Nije uspjelo veze
Nije uspjelo povezivanje prikazuju se na kartama Admin postupke i računalima, zajedno s iscrtanim crvena crta prikazuje ako klijentskim sustavima Nemogućnost dosegne procesa ili priključak.  Nije uspjelo povezivanje prijavljene iz sistemske s distribuiranih agent Admin ako je taj sustav jedan pokušaja nije uspjelo povezivanje.  Admin mjeri to tako da opažanja sockets TCP koje ne prođu uspostaviti vezu.  To može biti vatrozid, konfiguraciji u klijentu ili poslužitelju ili daljinskog servisa nije dostupan u tijeku. 

![Nije uspjelo veze](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Objašnjenje nije uspjelo povezivanje omogućuju s otklanjanjem poteškoća, provjere valjanosti za migraciju, sigurnost analizu i cjelokupnog arhitektonski razumijevanje.  Ponekad nije uspjela bezopasan veze, ali često pokažite izravno na problem, kao što su okruženju prebacivanje iznenada postaje dostupan,.. .ili dva aplikacije razine nemogućnošću objasniti nakon migracije oblaka.  U gornjoj slici IIS i WebSphere i koristite, ali ne mogu povezati. 

## <a name="computer-and-process-properties"></a>Računalo i svojstva procesa
Kada se krećete na kartu Admin, možete odabrati strojeva i procesa da bi se dobio dali kontekst o njihovim svojstvima.  Strojeva nalaze informacije o DNS naziva, IPv4 adrese, procesora i memorije kapacitetu, VM vrsta, verzija operacijskog sustava, vrijeme zadnjeg ponovnog pokretanja računala i ID-ove njihove OMS i Admin agenata.

Detalji o postupku su prikupili iz operacijski sustav metapodataka o pokretanju procesa, uključujući naziv procesa, opis postupka, korisničko ime i domenu (u sustavu Windows), naziv tvrtke, naziv proizvoda, verziju proizvoda, radni direktorij, naredbenog retka i vrijeme početka postupak.

![Svojstva procesa](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Na ploči sažetak postupak pruža dodatne informacije o povezivanje tog procesa, uključujući njegove povezane priključke, ulazni i izlazni veze, a nije uspjela veze. 

![Sažetak postupak](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>Praćenje Integracija OMS promjena
Admin, Integracija s programom evidentiranje promjena odvija se automatski kad omogućiti i konfigurirati u radnom prostoru OMS obiju situacija.

Na ploči sažetak strojno označava je li evidentiranje promjena događaje došlo je do na odabrani računalu unutar odabranog vremenskog razdoblja.

![Ploča za strojno sažetka](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Ploča za evidentiranje promjena strojno prikazuje popis svih promjena s najnovije prvi, uz vezu da biste dubinski analizirati zapisnika pretraživanja za dodatne pojedinosti.
![Ploča za praćenje promjena računala](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Slijedi naniže prikaz događaj promjene konfiguracije nakon što odaberete **Prikaz u zapisnik analize**.
![Konfiguriranje Promjena događaja](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Prijava analitiku zapisa
Admin na računalo i proces zaliha podataka dostupna je za [pretraživanje](../log-analytics/log-analytics-log-searches.md) zapisnika analize.  Time se mogu primijeniti na scenarije uključujući planiranje migracije, kapacitet analizu, otkrivanje i otklanjanje poteškoća s performansama ad-hoc. 

Generira jedan zapis po satu za svako računalo jedinstveni i proces uz zapise kada generira da procesa ili računalo pokreće ili je na-boarded za ADM.  Ti se zapisi imaju svojstva u tablicama u nastavku. 

Postoje interno generirani svojstva možete koristiti da biste odredili jedinstveni procesa i računala:

- PersistentKey_s jedinstveno definira konfiguracija procesa, primjerice naredbeni redak i korisničkog ID-a.  Jedinstveni za dani računalo, no mogu ponoviti svim računalima.
- ProcessId_s i ComputerId_s jedinstvenih globalno u modelu Admin.



### <a name="admcomputercl-records"></a>AdmComputer_CL zapisa
Zapisi s vrstom **AdmComputer_CL** imaju podatke o zalihama za poslužitelje s Admin agenata.  Ti se zapisi imaju svojstva u tablici u nastavku.  

| Svojstvo | Opis |
|:--|:--|
| Vrsta | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Naziv računala za Windows ili Linux |
| CPUSpeed_d | Brzina CPU-a u MHz |
| DnsNames_s | Popis svih imena DNS-a za ovo računalo |
| IPv4s_s | Popis svih IPv4 adresa koristi ovo računalo |
| IPv6s_s | Popis svih IPv6 adrese koristi ovo računalo.  (Admin označava IPv6 adrese, ali otkrijte ovisnosti IPv6.) |
| Is64Bit_b | TRUE ili false ovisno o vrsti OS |
| MachineId_s | Na interni GUID, jedinstveni preko programa OMS prostora  |
| OperatingSystemFamily_s | Windows i Linux |
| OperatingSystemVersion_s | Dugi niz OS verzije |
| TimeGenerated | Datum i vrijeme stvaranja zapisa. |
| TotalCPUs_d | Broj jezgri procesora |
| TotalPhysicalMemory_d | Kapacitet memorije u MB |
| VirtualMachine_b | TRUE ili false ovisno o tome je li OS VM goste |
| VirtualMachineID_g | ID VM Hyper-V |
| VirtualMachineName_g | Naziv VM Hyper-V |
| VirtualMachineType_s | HyperV, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>AdmProcess_CL vrsta zapisa 
Zapisi s vrstom **AdmProcess_CL** imaju podatke o zalihama za TCP povezani procese na poslužiteljima s Admin agenata.  Ti se zapisi imaju svojstva u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Vrsta | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Čitav naredbeni redak postupka |
| CompanyName_s | Naziv tvrtke (iz Windows PE ili Linux RPM) |
| Description_s | Opis dug proces (iz Windows PE ili Linux RPM) |
| FileVersion_s | Izvršna datoteka verzije (iz Windows PE samo u sustavu Windows) |
| FirstPid_d | ID OS procesa |
| InternalName_s | Naziv internog izvršne datoteke (iz sustava Windows PE, samo u sustavu Windows) |
| MachineId_s | Interna GUID jedinstveni preko programa OMS radnog prostora  |
| Name_s | Postupak izvršni naziv |
| Path_s | Put datoteke sustava izvršnu datoteku postupak |
| PersistentKey_s | Interna GUID jedinstveno unutar ovo računalo |
| PoolId_d | Interna ID za zbrajanje procese na temelju slične naredba retke. |
| ProcessId_s | Interna GUID jedinstveni preko programa OMS radnog prostora  |
| ProductName_s | Niz naziv proizvoda (iz Windows PE ili Linux RPM) |
| ProductVersion_s | Niz verzije proizvoda (iz Windows PE ili Linux RPM) |
| StartTime_t | Postupak početno vrijeme na lokalnom računalu sat |
| TimeGenerated | Datum i vrijeme stvaranja zapisa. |
| UserDomain_s | Domene vlasnik procesa (samo za Windows) |
| UserName_s | Ime vlasnika procesa (samo za Windows) |
| WorkingDirectory_s | Postupak radni imenik |


## <a name="sample-log-searches"></a>Ogledna zapisnika pretraživanja

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Popis fizičke memorije kapacitet sva upravljana računala. 
Vrsta = AdmComputer_CL | Odaberite TotalPhysicalMemory_d, ComputerName_s | Dedup ComputerName_s

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Naziv računala na popisu, DNS-a, IP i OS verzija.
Vrsta = AdmComputer_CL | Odaberite ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s | dedup ComputerName_s

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Pronalaženje svih procesa pomoću "sql" u naredbenom retku
Vrsta = AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Nakon prikaza podaci o događaju za dani postupka, njegov ID računalu koristiti za dohvaćanje naziva računala
Vrsta = AdmComputer_CL "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" | DISTINCT ComputerName_s

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Popis svim računalima sa sustavom SQL
Vrsta = AdmComputer_CL MachineId_s u {vrsta = AdmProcess_CL \*sql\* | DISTINCT MachineId_s} | DISTINCT ComputerName_s

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Popis svih verzija jedinstveni proizvoda zakretanja u moj podatkovnim centrom
Vrsta = AdmProcess_CL Name_s = zakretanja | DISTINCT ProductVersion_s

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Stvaranje grupe računala svim računalima sa sustavom CentOS

![Primjer Admin upita](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Dijagnostika i korištenje podataka
Microsoft automatski prikuplja korištenje i performanse podataka putem korištenje servisa aplikacija ovisnost Monitor. Microsoft koristi ove podatke pružanje i poboljšanju kvalitete, sigurnost i integritet servisa aplikacija ovisnost Monitor. Podataka sadrži informacije o konfiguraciji vašeg softvera kao što je operacijski sustav i verzija i i IP adrese, naziv DNS-a i naziv radne stanice omogućili točni i učinkovito mogućnosti za otklanjanje poteškoća. Ne možemo prikupiti imena, adrese i druge podatke za kontakt.

Dodatne informacije o prikupljanje podataka i korištenje potražite u [Microsoft Online Services Izjava o zaštiti privatnosti](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Daljnji koraci
- Dodatne informacije o [Prijavi pretraživanja](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
