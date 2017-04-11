<properties
   pageTitle="Logika aplikacije instalirati pristupnik lokalnim podacima | Microsoft Azure"
   description="Informacije o tome kako instalirati pristupnik za lokalnim podacima za korištenje u aplikaciji logike."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Instalacija pristupnik lokalnim podacima za aplikacije logike

## <a name="installation-and-configuration"></a>Instalacija i konfiguracija

### <a name="prerequisites"></a>Preduvjeti

Minimum:

* 4,5 .NET framework
* 64-bitnu verziju sustava Windows 7 ili Windows Server 2008 R2 (ili noviji)

Preporučuje se:

* 8 core procesora
* 8 GB memorije
* 64-bitnu verziju sustava Windows 2012 R2 (ili noviji)

Srodni pitanja vezana uz:

* Pristupnik ne možete instalirati na kontroleru domene.
* Pristupnik bi trebalo instalirati na računalo, kao što prijenosno računalo, koje možda je isključena, stanju mirovanja, ili niste povezani s Internetom jer pristupnika nije moguće pokrenuti kada bilo koji od tih slučajeva. Osim toga, performansama pristupnika možda se putem bežične mreže.

### <a name="install-a-gateway"></a>Instalacija pristupnika

Možete dobiti [instalacijski program za pristupnik lokalnim podacima ovdje](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

Odredite **pristupnik lokalnim podacima** kao načina rada, prijavite pomoću poslovnog ili obrazovne ustanove, i zatim ili konfigurirati pristupnik za novi migrirati, vraćanje ili preuzeti postojeće pristupnika.

* Da biste konfigurirali pristupnika, upišite **naziv** i **oporavak**, a zatim kliknite ili dodirnite **Konfiguriraj**.

    Navedite ključa za oporavak koja sadrži najmanje osam znakova i čuvajte na sigurnome mjestu. Morat ćete taj ključ ako želite migrirati, vratiti ili preuzeti njegov pristupnika.

* Migriranje, vraćanje ili preuzeti postojeće pristupnika, navedite ključa za oporavak naveden prilikom stvaranja pristupnika.

### <a name="restart-the-gateway"></a>Ponovno pokrenite pristupnika

Pristupnik izvodi kao servis Windows i s bilo kojeg drugog servisa Windows, možete započeti i prekid na različite načine. Ako, na primjer, otvorite naredbeni redak s dodatnim dozvolama na računalu na kojem se pokreće pristupnika i pokrenuti obje naredbe:

* Zaustavljanje servisa, pokrenite sljedeću naredbu:

    `net stop PBIEgwService`

* Da biste pokrenuli servis, pokrenite sljedeću naredbu:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfiguriranje vatrozida ili proxy poslužitelja

Informacije o tome da biste dobili informacije proxy poslužitelja za pristupnikom potražite u članku [Konfiguriraj postavke proxyja](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Možete provjeriti je li vatrozid ili proxy poslužitelj, možda blokira veze pokretanjem sljedeće naredbe iz odzivniku komponente PowerShell. To će testirati povezivost tržišta servisa Azure za. Tom samo testiraju veza s mrežom i ne sadrži veze s poslužitelja servisa u oblaku ili pristupnika. Lakše da biste odredili hoće li vaše računalo možete zapravo dobiti s Internetom.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Rezultati trebala bi izgledati slično kao u ovom primjeru. Ako je **TcpTestSucceeded** true, koje možda blokira vatrozid.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Ako želite biti iscrpan zamijeniti **ComputerName** i **priključak** vrijednosti su navedeni u odjeljku [Konfiguriranje priključke](#configure-ports) u nastavku ovog članka.

Vatrozid i blokira veze koje Bus servisa Azure unese centre za Azure podataka. Ako je to slučaj, ćete htjeti whitelist (deblokiranje) sve IP adresa za vašu regiju za centre tih podataka. Možete dobiti popis [Azure IP adrese ovdje](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Konfiguriranje priključci

Pristupnik stvara izlazne veze da biste Bus servisa Azure. Interes na izlazni priključke: TCP 443 (zadano), 5671, 5672, 9350 do 9354. Pristupnik ne zahtijeva ulaznog priključci.

Saznajte više o [hibridnim rješenja](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| NAZIVI DOMENA | IZLAZNI PRIKLJUČCI | OPIS |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671 5672 | Napredne poruke stavljanje Protocol (AMQP) |
| *. servicebus.windows.net | 443, 9350 9354 | Slušače na servis preusmjeravanja Bus putem TCP (zahtijeva 443 za kontrolu pristupa tokena acquisition) |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. core.windows.net | 443 | HTTPS |
| login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Služi za testiranje internetska veza ako pristupnik nije dostupan servis Power BI. |

Ako morate bijeli popis IP adrese umjesto domene, možete preuzeti i koristiti [Microsoft Azure podatkovnog centra IP rasponi popisa](https://www.microsoft.com/download/details.aspx?id=41653). U nekim slučajevima veze Bus servisa Azure bit će s IP adresom umjesto na potpuno kvalificiranih naziva domena.

### <a name="sign-in-account"></a>Račun za prijavu

Korisnici će prijavite ili tvrtke ili obrazovne ustanove. Ovo je vaš račun tvrtke ili ustanove. Ako se prijavili za ponude sustava Office 365, a niste navesti stvarni posao e-pošte, može izgledati kao jeff@contoso.onmicrosoft.com. Vaš račun, unutar servis u oblaku, pohranjuju se u klijentu u Azure Active Directory (AAD). U većini slučajeva UPN računa AAD će odgovarati adresu e-pošte.

### <a name="windows-service-account"></a>Račun servisa Windows

Pristupnik za lokalnim podacima je konfiguriran za korištenje NT SERVICE\PBIEgwService za vjerodajnica za prijavu servisa Windows. Prema zadanim postavkama ima s desne strane zapisnika kao usluga. To je u kontekstu na računalu na kojem ga instalirate pristupnika.

To nije račun koji se koristi za povezivanje s lokalnim izvorima podataka ili račun tvrtke ili obrazovne ustanove s kojom se prijavite na servise u oblaku.

##<a name="frequently-asked-questions"></a>Najčešća pitanja

### <a name="general"></a>Općenito

**Pitanje**: što izvore podataka pristupnika podržava?<br/>
**Odgovor**: na ovom pisanja, SQL Server.

**Pitanje**: moram pristupnik za izvore podataka u oblaku, kao što je SQL Azure? <br/>
**Odgovor**: ne. Pristupnik povezuje se samo lokalnih izvora podataka.

**Pitanje**: što je stvarni servis Windows naziva?<br/>
**Odgovor**: U servise, pristupnika naziva pristupnika servis za Power BI Enterprise.

**Pitanje**: postoje neki unutarnje veze s pristupnikom iz oblaka? <br/>
**Odgovor**: ne. Pristupnik koristi izlazne veze da biste Bus servisa Azure.

**Pitanje**: što se događa ako blokirati izlazne veze? Što je potrebno da biste otvorili? <br/>
**Odgovor**: potražite u članku priključci i domaćini koji koristi pristupnik.


**Pitanje**: ne pristupnika mora biti instaliran na istom računalu kao izvor podataka? <br/>
**Odgovor**: ne. Pristupnik će se povezati s izvorom podataka koristeći podatke o vezi koju ste dobili. Smatrati pristupnika klijentske aplikacije u tom pogledu. Ga samo morat ćete se moći povezati naziva poslužitelja na kojem ste dobili.


**Pitanje**: što je kašnjenje za pokretanje upita s izvorom podataka iz pristupnika? Što je najbolje arhitektura? <br/>
**Odgovor**: da biste smanjili latenciju mreže, instalirajte pristupnika blizu izvora podataka u obliku moguće. Ako instalirate pristupnika na izvoru stvarnih podataka, će minimiziranje Latencija uvodi. Razmislite o središta podataka. Na primjer, ako na servisu koristi podatkovnog centra Zapad SAD-a, a imate SQL Server smješten u programa Azure VM, ćete htjeti kao i imaju VM Azure u Zapad SAD-a. To će minimiziranje Latencija i izbjegavanja naknada za izlazne na Azure VM.


**Pitanje**: ima li sve preduvjete za propusnost mreže? <br/>
**Odgovor**: preporučuje se da bi dobro propusnost za mrežnu vezu. Okruženje za svaku razlikuje se i količinu podataka koji se šalju utječu na rezultate. Korištenje ExpressRoute može pomoći da bi na razinu propusnost između lokalnog i centre za Azure podataka.

Da biste lakše procijeni što je vaš propusnost možete koristiti aplikaciju za testiranje brzine Azure alata drugih proizvođača.


**Pitanje**: možete pokrenuti pristupnika servisa Windows Azure Active Directory računa? <br/>
**Odgovor**: ne. Servis Windows mora imati valjan račun za Windows. Prema zadanim postavkama, funkcionirat će sa servisa ID, NT SERVICE\PBIEgwService.


**Pitanje**: kako se rezultati šalju natrag u oblak? <br/>
**Odgovor**: to možete učiniti putem Bus servisa Azure. Dodatne informacije potražite u članku kako se to radi.


**Pitanje**: gdje su moje vjerodajnice pohranjene? <br/>
**Odgovor**: vjerodajnice koje ste unijeli za izvor podataka spremaju šifrirane u oblaku pristupnika. Vjerodajnice su dešifrirati pri na pristupnika lokalnog.

### <a name="high-availabilitydisaster-recovery"></a>Oporavak visoke dostupnosti/Izrada

**Pitanje**: ima li svaki plan za omogućivanjem scenarija visoke dostupnosti s pristupnika? <br/>
**Odgovor**: to je u vodič, ali ne možemo vremenske crte još nemate.


**Pitanje**: koje su mogućnosti dostupne za oporavak Izrada? <br/>
**Odgovor**: koristite ključa za oporavak da biste vratili ili premještanje pristupnik. Kada instalirate pristupnika, navedite ključa za oporavak.


**Pitanje**: što je prednost ključa za oporavak? <br/>
**Odgovor**: ona omogućuje migrirati ili oporavak postavki pristupnika nakon na Izrada.

### <a name="troubleshooting"></a>Otklanjanje poteškoća

**Pitanje**: mjesto na kojem se zapisnici pristupnika? <br/>
**Odgovor**: u odjeljku Alati u nastavku ovog članka.


**Pitanje**: Kako mogu vidjeti što su upiti koji se šalju lokalni izvor podataka? <br/>
**Odgovor**: možete omogućiti praćenje upit koji će sadržavati upita koja se šalje. Imajte na umu da biste promijenili natrag na izvorne vrijednosti po završetku otklanjanje poteškoća. Ostavite upit omogućeno praćenje uzrokovat će zapisnike biti veća.

Možete pogledati i alata koji je izvor podataka za praćenje upite. Ako, na primjer, možete koristiti prošireni događaje ili SQL Profiler za SQL Server i komponente Analysis Services.

## <a name="how-the-gateway-works"></a>Kako funkcionira pristupnika

Kada korisnik stupi u interakciju s element koji je povezan s lokalnim izvorom podataka:

1. Servis u oblaku stvara upit, zajedno sa šifriranim vjerodajnica za izvor podataka i šalje upit red za pristupnik za obradu.
1. Servis analizira upita i ih gura zahtjev za Bus servisa Azure.
1. Pristupnik za lokalnim podacima polls na Azure Bus servisa za, na čekanju zahtjeva.
1. Pristupnik dobiti upit, dešifrira vjerodajnice i povezuje s izvorima podataka pomoću tih vjerodajnica.
1. Pristupnik šalje upit izvora podataka za izvršavanje.
1. Rezultati se šalju iz izvora podataka natrag pristupnika, a zatim na servis u oblaku. Servis koristi rezultate.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="update-to-the-latest-version"></a>Ažuriranje na najnoviju verziju

Mnogo probleme može ponuditi kada verzija pristupnika nije istekao rok.  Nije dobro Općenito da biste bili sigurni da ste na najnoviju verziju.  Ako još niste ažurirali pristupnik za mjesec, ili produljili, preporučujemo vam da razmislite o instalacija najnovije verzije pristupnika i vidjeti ako pokušate reproducirati problem.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Pogreška: Nije uspjelo dodavanje korisnika u grupu. (-2147463168 PBIEgwService performanse zapisnika korisnika)

Ta se pogreška možete dobiti ako pokušavate instalirati pristupnika na kontroler domene koja nije podržana. Morat ćete za implementaciju pristupnika na računalo koje nije kontroler domene.

## <a name="tools"></a>Alati

### <a name="collecting-logs-from-the-gateway-configurator"></a>Prikupljanje zapisnika iz Konfiguratora pristupnika

Možete prikupiti zapisnike nekoliko pristupnika. Uvijek započinju zapisnike!

#### <a name="installer-logs"></a>Instalacijski program zapisnika

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfiguriranje zapisnika

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise pristupnika servisa zapisnika

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Zapisnike događaja

Pristupnik za upravljanje podacima i PowerBIGateway zapisnike su prisutne u odjeljku **aplikacije i servise zapisnika**.

### <a name="fiddler-trace"></a>Praćenje fiddler

[Fiddler](http://www.telerik.com/fiddler) je alat za besplatne iz Telerik koji nadzire HTTP promet.  Možete vidjeti na stražnjoj i iz dodatka Power bi servisa na klijentskom računalu. To se može prikazivati pogreške i drugih povezanih informacija.

## <a name="next-steps"></a>Daljnji koraci
- [Stvorite vezu na lokalni logike aplikacije](app-service-logic-gateway-connection.md)
- [Značajki integracije Enterprise](app-service-logic-enterprise-integration-overview.md)
- [Logika aplikacije poveznika](../connectors/apis-list.md)