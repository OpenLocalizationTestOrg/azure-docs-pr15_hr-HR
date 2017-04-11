<properties
   pageTitle="Konfiguriranje aplikacije ovisnost monitora (Admin) u paketu za upravljanje operacije (OMS) | Microsoft Azure"
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

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Konfiguriranje rješenja Monitor ovisnost aplikacije u paketu operacije upravljanja (OMS)
![Ikona upozorenja upravljanja](media/operations-management-suite-application-dependency-monitor/icon.png) Aplikacija ovisnost monitora (Admin) automatski otkrije aplikacije komponente u sustavima Windows i Linux i karata komunikaciju između servisa. Omogućuje prikaz poslužitelja kao što mislite da ih – kao međusobno povezanih sustavi za izlaganje ključnih services.  Aplikacija ovisnost Monitor prikazuje veze između poslužitelja, a zatim procesa, i priključke preko bilo koje arhitektura TCP povezani s bez konfiguracijom obavezno osim instalacije agent.

U ovom se članku opisuju detalje o konfiguriranju Monitor ovisnost aplikacije i za uhodavanje agenata.  Informacije o korištenju Admin, potražite u članku [Korištenje monitora ovisnost aplikacije rješenja u paketu operacije upravljanja (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Aplikacija ovisnost Monitor trenutno privatne pretpregled.  Možete zatražiti pristup Admin privatne pretpregled pri [https://aka.ms/getadm](https://aka.ms/getadm).
>
>Tijekom privatne pretpregleda su svi računi OMS Neograničeni pristup ADM.  Čvorovi Admin besplatna, no zapisnika analize podataka za vrste AdmComputer_CL i AdmProcess_CL će se s ograničenim prometom kao što su druge rješenja.
>
>Nakon što Admin unosi javno pretpregled, bit će dostupna samo korisnicima besplatne i plaćenu uvid i analize u na OMS cijene Plan.  Besplatni sloju računi bit će ograničeni na 5 Admin čvorove.  Ako sudjeluju u privatnu pretpregled, a ne upisani u OMS cijene planiranje kad Admin unese javno pretpregled, tada će biti onemogućena Admin. 



## <a name="connected-sources"></a>Povezani izvora
U sljedećoj tablici opisane povezanih izvora koje podržava Admin rješenja.

| Povezani izvora | Podržana | Opis |
|:--|:--|:--|
| [Windows agenata](../log-analytics/log-analytics-windows-agents.md) | Da | Admin analizira i prikuplja podatke iz agent za računala sa sustavom Windows.  <br><br>Imajte na umu da osim OMS agent Windows agenata zahtijeva Microsoft Agent ovisnosti.  Pročitajte članak [podržani operacijski sustavi](#supported-operating-systems) za popis svih verzija operacijskog sustava. |
| [Linux agenata](../log-analytics/log-analytics-linux-agents.md) | Da | Admin analizira i prikuplja podatke s računala agent Linux.  <br><br>Imajte na umu da osim OMS agent Linux agenata zahtijeva Microsoft Agent ovisnosti.  Pročitajte članak [podržani operacijski sustavi](#supported-operating-systems) za popis svih verzija operacijskog sustava. |
| [Grupa Upravljanje SCOM](../log-analytics/log-analytics-om-agents.md) | Da | Admin analizira i prikuplja podatke iz sustava Windows i Linux agenata u grupu za upravljanje povezanim SCOM. <br><br>Potreban je izravnu vezu s računala agent SCOM OMS. Podaci se šalju izravno iz prosljeđuju iz grupe za Upravljanje spremištem OMS.|
| [Račun za Azure prostora za pohranu](../log-analytics/log-analytics-azure-storage.md) | ne | Admin prikuplja podatke s računala agent tako da nema podataka iz nje da biste prikupili iz Azure prostora za pohranu. |

Imajte na umu da Admin podržava samo 64-bitnim platformama.

U sustavu Windows, u programu Microsoft nadzor Agent (MMA) koristi SCOM i OMS da biste prikupili i slanje podataka za nadzor.  (Ovaj agent zove SCOM Agent, OMS Agent, MMA ili izravno Agent ovisno o kontekstu.)  SCOM i OMS nude različite iz okvira verzijama MMA, ali te verzije možete svako izvješće da biste SCOM, OMS ili i jedno i drugo.  Na Linux, OMS Agent za Linux prikuplja i šalje podatke OMS za nadzor.  Admin možete koristiti na poslužiteljima s OMS Izravni agenata ili na poslužiteljima koje su priložene OMS putem SCOM Upravljanje grupama.  Radi ove dokumentacije odnose na sve te agente – li na web-mjesto Linux ili u okvir za Windows, hoće li povezani s SCOM MG ili izravno OMS – kao "OMS Agent", osim ako naziv određene implementacije agenta je potreban za kontekst.

Agent za Admin prenositi same podatke, a ne zahtijeva promjene vatrozida ili priključci.  Podatke o Admin uvijek prenose se po OMS Agent za OMS, izravno ili putem OMS pristupnika.

![Admin agenata](media/operations-management-suite-application-dependency-monitor/agents.png)

Ako ste SCOM klijenta s grupom upravljanje povezan s OMS:

- Ako vaš agenata SCOM možete pristupiti s Internetom da biste se povezali OMS, potrebna je bez dodatna konfiguracija.  
- Ako vaš agenata SCOM ne možete pristupiti OMS putem Interneta, morat ćete konfigurirati pristupnik OMS da biste radili s SCOM.
  
Ako koristite Agent Izravni OMS, morate konfigurirati Agent OMS sam za povezivanje s OMS ili s pristupnikom OMS.  OMS pristupnika možete preuzeti s [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Izbjegavanje dupliciranih podataka

Ako ste kupac SCOM, konfigurirati svoje SCOM agenata komunikaciju izravno u OMS ili podataka će se prijavljivati dvaput.  U Admin, to će rezultirati računala koji se pojavljuje dvaput na popisu računala.

Konfiguriranje OMS će se dogoditi u samo jedan od sljedećih mjesta:

- Na ploči SCOM konzole operacije upravljanja paket za upravlja računala
- Azure konfiguracija radu uvida u MMA svojstva

Pomoću oba konfiguracije na isti radni prostor u svakom uzrokovat će dupliciranje podataka. Korištenje obje s različitim radnim prostorima može rezultirati sukobljenih konfiguracija (jedan s rješenje Admin omogućeno i druge bez) koja se može spriječiti potpuno slijedi Admin podataka.  

Imajte na umu da čak i ako na računalu sam nije naveden u konfiguraciji OMS konzole za SCOM, ako je aktivan granu instancu kao što su "Grupe sustava Windows Server instancama", može i dalje dovesti do strojno primanju OMS konfiguraciju putem SCOM.



## <a name="management-packs"></a>Upravljanje paketi
Kada je Admin aktivirana u radnom prostoru programa OMS, 300KB paket za upravljanje šalje se svi u programu Microsoft nadzor agenti tog radnog prostora.  Ako koristite SCOM agenata u na [povezani grupu upravljanje](../log-analytics/log-analytics-om-agents.md)paket za upravljanje Admin će biti implementirano s SCOM.  Ako na agenata izravno povezani, u MP će se isporučiti OMS.

Na MP pod nazivom Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Da biste ga je napisao *%Programfiles%\Microsoft nadzor Agent\Agent\Health State\Management pakete usluga\*.  Izvor podataka koji se koriste paket za upravljanje *% Program files%\Microsoft praćenja Agent\Agent\Health servisa State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfiguracija
Osim sustava Windows i Linux računalima imati agent spojeni OMS, instalacijski program Agent ovisnost mora biti preuzete iz rješenja Admin i instalirali se kao korijen ili administrator na svakom upravljanih poslužitelju.  Kad instalirate agent Admin na poslužitelju izvješća OMS, Admin ovisnost karte pojavit će se unutar 10 minuta.  Ako imate problema, pošaljite e-pošte [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migracija s BlueStripe FactFinder
Aplikacija ovisnost Monitor isporučiti BlueStripe tehnologija u OMS u fazama. FactFinder još je podržana za postojeće korisnike, ali više nije dostupna za kupnju pojedinačne.  Ova verzija pretpregled agenta ovisnost samo možete komunicirati s OMS.  Ako ste trenutni FactFinder klijenta, provjerite prepoznati skup test poslužitelje Admin koji ne upravlja FactFinder. 

### <a name="download-the-dependency-agent"></a>Preuzimanje ovisnost Agent
Osim Microsoft Agent za upravljanje (MMA) i OMS Linux Agent koji sadrže veze između računala i OMS, sva računala analizirati prema aplikacije ovisnost Monitor morate imati instaliran Agent ovisnosti.  Na Linux, prije Agent ovisnost mora biti instaliran OMS Agent za Linux. 

![Pločica aplikacije ovisnost monitora](media/operations-management-suite-application-dependency-monitor/tile.png)

Da biste preuzeli Agent ovisnost, kliknite **Konfiguriranje rješenja** u pločicu **Aplikacije ovisnost monitora** da biste otvorili plohu **Ovisnost Agent** .  Agent za ovisnost plohu sadrži veze za Windows i agenata Linux. Kliknite odgovarajuću vezu da biste preuzeli svaki pojedini agent. Potražite u sljedećim se odjeljcima detalje o instaliranju agenta na različitim sustavima.

### <a name="install-the-dependency-agent"></a>Instalirajte Agent ovisnost

#### <a name="microsoft-windows"></a>Microsoft Windows
Instalacija i Deinstalacija agenta moraju administratorske ovlasti.

Agent za ovisnost je instaliran na računala sa sustavom Windows s Admin Agent Windows.exe. Ako naiđete na tom izvršna bez sve mogućnosti, zatim će započeti čarobnjak koji mogu slijediti prilikom interaktivno izvršiti instalaciju.  

Poduzmite sljedeće korake da biste instalirali Agent ovisnosti na svakom računalu sa sustavom Windows.

1.  Je li OMS agent instaliran slijedeći upute na računalima povezivanje izravno OMS.
2.  Preuzmite agent za Windows i pokrenite je pomoću sljedeće naredbe.<br>*Admin Agent Windows.exe*
3.  Slijedite čarobnjaka za instaliranje agenta.
4.  Ako Agent ovisnost ne pokreće, u zapisnicima detaljne informacije o pogrešci. Na agenata Windows direktorij za zapisnike je *C:\Program Files\BlueStripe\Collector\logs*. 

Ovisnost Agent za Windows možete deinstalirati administrator putem upravljačke ploče.


#### <a name="linux"></a>Linux
Da biste instalirali ili konfiguriranje agenta potreban je korijenski pristup.

Agent za ovisnost je instalirana na računalima Linux s Admin-Agent-Linux64.bin, Jezgrena skripta s samoizdvojiva binarni. Možete pokrenuti datoteku s p ili dodati izvršavanje dozvole za samu datoteku.
 
Poduzmite sljedeće korake da biste instalirali Agent ovisnosti na svakom računalu Linux.

1.  Je li OMS agent instaliran slijedeći upute na [skupljanje i upravljanje podacima s računala Linux.  To potrebno je instalirati prije ovisnost Agent Linux](https://technet.microsoft.com/library/mt622052.aspx).
2.  Instalirajte agent Linux ovisnosti kao korijen pomoću sljedeće naredbe.<br>*pri Admin Agent Linux64.bin*.
3.  Ako Agent ovisnost ne pokreće, u zapisnicima detaljne informacije o pogrešci. Na agenata Linux direktorij za zapisnike je */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Deinstalacija Agent ovisnosti na Linux
Da biste deinstalira posve Agent ovisnost Linux, morate ukloniti agent sam i proxy koji se instalira automatski uz agenta.  Deinstalirajte oba s jednom naredbu.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Instaliranje iz naredbenog retka
U prethodnom odjeljku sadrži smjernice o instaliranju agent ovisnost Monitor sa zadanim mogućnostima.  U nastavku se odjeljcima navode upute za instalaciju agenta iz naredbenog retka pomoću prilagođene mogućnosti.

#### <a name="windows"></a>Windows
Pomoću mogućnosti iz tablice u nastavku da biste izvršili instalaciju putem naredbenog retka. Da biste vidjeli popis instalacije zastavice pokrenite instalacijski program s na /? Označite na sljedeći način.

    ADM-Agent-Windows.exe /?

| Zastavica | Opis |
|:--|:--|
| /S | Izvođenje tihu instalaciju s bez upita korisnika. |

Datoteka za ovisnost Agent za Windows spremaju se u *C:\Programske Files\BlueStripe\Collector* prema zadanim postavkama.


#### <a name="linux"></a>Linux
Pomoću mogućnosti iz tablice u nastavku da biste izvršili instalaciju. Da biste vidjeli popis instalaciju pokrenite zastavice instalacije programa pomoću - zastavice na sljedeći način.

    ADM-Agent-Linux64.bin -help

| Opis zastavica
|:--|:--|
| -s | Izvođenje tihu instalaciju s bez upita korisnika. |
| – Provjera | Provjerava dozvole i operacijski sustav, ali ne može instalirati agenta. |

Datoteka za Agent ovisnost spremaju se u sljedećim direktorija.

| Datoteka | Mjesto |
|:--|:--|
| Ključne datoteke | /usr/lib/bluestripe-Collector |
| Datoteke zapisnika | /VAR/Opt/Microsoft/Dependency-agent/log |
| Konfiguracijska datoteka | /etc/Opt/Microsoft/Dependency-agent/config |
| Servis izvršne datoteke | /sbin/bluestripe-Collector<br>/sbin/bluestripe-Collector-Manager |
| Binarni prostora za pohranu datoteka | /VAR/Opt/Microsoft/Dependency-agent/Storage |



## <a name="troubleshooting"></a>Otklanjanje poteškoća
Ako naiđete na probleme s monitora ovisnost aplikacije, možete Prikupite informacije o otklanjanju poteškoća s više komponenti pomoću sljedeće informacije.

### <a name="windows-agents"></a>Windows agenata

#### <a name="microsoft-dependency-agent"></a>Ovisnost Microsoft Agent
Da biste generirali podataka o otklanjanju poteškoća s Agent ovisnost, otvorite naredbeni redak kao administrator i pokrenuti skriptu CollectBluestripeData.vbs pomoću sljedeće naredbe.  Možete dodati zastavicu – pomoć da biste vidjeli dodatne mogućnosti.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Paket za podršku podataka sprema se u direktoriju % % korisničkog PROFILA za trenutnog korisnika.  Možete koristiti – datoteku <filename> mogućnost da biste spremili na drugo mjesto.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Ovisnost Microsoft Agent paket za upravljanje MMA
Paket za upravljanje ovisnost Agent pokreće unutar Microsoft Management Agent.  Prima podatke iz Agent ovisnost i prosljeđuje u oblaku Admin.
  
Provjerite je li paket za upravljanje preuzeta pomoću sljedećih koraka.

1.  Potražite datoteku s nazivom Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp u c:\Programske datoteke\Microsoft nadzor Agent\Agent\Health State\Management pakete usluga.  
2.  Ako datoteka ne postoji, a agenta priključen na grupu za upravljanje SCOM, zatim potvrdite da je uvezena u SCOM potvrđivanjem paketi upravljanje u radnom prostoru za administraciju konzole za operacije.

MP Admin zapisuje događaje zapisnika događaja sustava Windows Upravitelj operacije.  Zapisnik može [pretraživati u OMS](../log-analytics/log-analytics-log-searches.md) putem zapisnika rješenje sustava na kojoj možete konfigurirati zapisnika datoteke za prijenos.  Ako je omogućeno ispravljanje pogrešaka događaje, su sastavljene zapisnik događaja aplikacije, s izvorom događaj *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Agent za praćenje
Da biste prikupili dijagnostičke kašnjenja, otvorite naredbeni redak kao administrator, a zatim izvršite sljedeće naredbe: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Kašnjenja zapisuju se c:\Windows\Logs\OpsMgrTrace.  Praćenje s StopTracing.cmd, možete isključiti.


### <a name="linux-agents"></a>Linux agenata

#### <a name="microsoft-dependency-agent"></a>Ovisnost Microsoft Agent
Da biste generirali podataka o otklanjanju poteškoća agenta ovisnost, prijavite se pomoću računa koji ima ovlasti sudo ili korijenski i pokrenite sljedeću naredbu.  Možete dodati zastavicu – pomoć da biste vidjeli dodatne mogućnosti.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Paket podataka podrška se sprema u /var/opt/microsoft/dependency-agent/log (Ako korijenski) ispod direktorija za instalaciju na Agent ili imenika kućni korisnik koji se pokreće skriptu (ako nisu korijenski).  Možete koristiti – datoteku <filename> mogućnost da biste spremili na drugo mjesto.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Ovisnost Microsoft Agent Fluentd dodatak za Linux
Dodatak za Fluentd ovisnost Agent pokreće unutar Agent za OMS Linux.  Prima podatke iz Agent ovisnost i prosljeđuje u oblaku Admin.  

Zapisnici zapisuju se sljedeće dvije datoteke.

- /VAR/Opt/Microsoft/omsagent/log/omsagent.log
- /VAR/log/messages

#### <a name="oms-agent-for-linux"></a>OMS Agent za Linux
Otklanjanje poteškoća resursa za povezivanje Linux poslužitelje OMS nalazi se ovdje: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

U zapisnicima OMS Agent za Linux nalaze se u */var/uključivanje/microsoft/omsagent/zapisnika/*.  

U zapisnicima omsconfig (agent konfiguracije) nalaze se u */var/uključivanje/microsoft/omsconfig/zapisnika/*.
 
U zapisniku OMI i SCX komponente koje omogućuju podataka o performansama metriku nalaze se u */var/uključivanje/omi/zapisnika/* i */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Prikupljanje podataka
Možete očekivati da se svaki pojedini agent za prijenos otprilike 25MB po danu, ovisno o tome kako složene su vaše ovisnosti sustava.  Admin ovisnost podataka je poslao svaki pojedini agent svakih 15 sekundi.  

Agent za Admin obično troši 0,1% sistemske memorije i 0,1% sustava procesora.


## <a name="supported-operating-systems"></a>Podržani operacijski sustavi
U sljedećim se odjeljcima navode podržani operacijski sustavi za Agent ovisnosti.   32-bitni arhitekturi nisu podržani za operacijske sustave.

### <a name="windows-server"></a>Windows Server
- Windows Server 2012 R2
- Windows Server 2012.
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Radnu površinu sustava Windows
- Napomena: Windows 10 još nije podržana
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Crvena je vaša Enterprise Linux, CentOS Linux i Linux Oracle (uz otklanjanje RHEL)
- Podržani su samo zadani i izdanja otklanjanje SMP Linux.
- Nestandardne otklanjanje izdavanje, kao što su PAE i Xen, nisu podržani za neki distribuciju Linux. Na primjer, sustavu izdanje niz "2.6.16.21-0.8-xen" nije podržana.
- Prilagođeni jezgre, uključujući recompiles standardne jezgre nisu podržane
- Otklanjanje centos Plus nije podržana.
- Oracle Unbreakable otklanjanje (UEK) opisana su u nastavku različite sekcije.


#### <a name="red-hat-linux-7"></a>Crvena je vaša Linux 7
| Verzija OS-a | Otklanjanje verzija |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7,2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Crvena je vaša Linux 6
| Verzija OS-a | Otklanjanje verzija |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6,3 | 2.6.32-279 |
| 6,4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6,7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Crvena je vaša Linux 5
| Verzija OS-a | Otklanjanje verzija |
|:--|:--|
| 5,8 | 2.6.18-308 |
| 5,9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux w/ Unbreakable otklanjanje (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Verzija OS-a | Otklanjanje verzija
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6,3 | Oracle 2.6.39-200 (UEK R2) |
| 6,4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Verzija OS-a | Otklanjanje verzija
|:--|:--|
| 5,8 | Oracle 2.6.32-300 (UEK R1) |
| 5,9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Verzija OS-a | Otklanjanje verzija
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Verzija OS-a | Otklanjanje verzija
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Dijagnostika i korištenje podataka
Microsoft automatski prikuplja korištenje i performanse podataka putem korištenje servisa aplikacija ovisnost Monitor. Microsoft koristi ove podatke pružanje i poboljšanju kvalitete, sigurnost i integritet servisa aplikacija ovisnost Monitor. Podataka sadrži informacije o konfiguraciji vašeg softvera kao što je operacijski sustav i verzija i i IP adrese, naziv DNS-a i naziv radne stanice omogućili točni i učinkovito mogućnosti za otklanjanje poteškoća. Ne možemo prikupiti imena, adrese i druge podatke za kontakt.

Dodatne informacije o prikupljanje podataka i korištenje potražite u [Microsoft Online Services Izjava o zaštiti privatnosti](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Daljnji koraci
- Saznajte kako jednom [pomoću aplikacije ovisnost Monitor](operations-management-suite-application-dependency-monitor.md) ga je uvesti i konfiguriran.
