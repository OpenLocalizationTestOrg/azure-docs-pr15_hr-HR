<properties
   pageTitle="Servis tkanina arhitektura | Microsoft Azure"
   description="Servis tkanina je raspodijeljeno sustavi za sastavljanje skalabilni, pouzdanog i lako upravlja aplikacije za oblaka. U ovom se članku prikazuje arhitektura tkanina servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Servis tkanina arhitekture

Servis tkanina ugrađena je s Slojeviti podsustava. Ove podsustava omogućuju vam da biste napisali aplikacije koje su:

* Iznimno dostupna
* Skalabilni
* Moguće upravljati
* Testable

Sljedeći dijagram prikazuje glavne podsustava od tkanina servisa.

![Dijagram arhitektura tkanina servisa](media/service-fabric-architecture/service-fabric-architecture.png)

U sustavu za raspodijeljeno, mogućnost sigurnog komunikaciju između čvorove klasteru je presudne. Osnovni stog biti podsustav prometa koji omogućuje sigurno komunikaciju između čvorove. Iznad prijenos podsustav leži podsustav vanjski pristup, koja klaster različite čvorove u jedan entitet (pod nazivom klastere) tako da tkanina servisa možete otkriti pogreške, izvođenje izborno Vodeća crta i dati dosljedan usmjeravanja. Podsustavu pouzdanosti slojevima pri vrhu podsustav vanjski pristup je zadužen za pouzdanost servisa tkanina servisa putem mehanizme kao što su replikacije, Upravljanje resursima i prebacivanje. Vanjski pristup podsustav pozadini i hostiranje i aktivacija podsustavu, koji upravlja životnim ciklusom u aplikaciju na jedan čvor. Upravljanje podsustav upravlja životnim ciklusom aplikacija i servisa. Podsustavu testability olakšava razvojnim inženjerima testirajte svoje usluge putem Simulirani kvarove prije i poslije implementacija aplikacija i servisa radnog okruženja. Servis tkanina pruža mogućnost da biste riješili servisa mjesta putem njegova podsustav komunikacije. Modeli programiranje aplikacije izložen razvojnim inženjerima slojevima pri vrhu ove podsustava uz modela aplikacije da biste omogućili tooling.

## <a name="transport-subsystem"></a>Podsustav prijenosa
Prijenos podsustav implementira point-to-point datagram komunikacije kanala. Ovaj kanal koristi se za komunikaciju unutar servisa tkanina klastere i komunikaciju između servisa tkanina klaster i klijenti. Podržava jednosmjerna i zahtjev za odgovor komunikacijskom, koje služi kao temelj za implementaciju emitiranje i višesmjernog u sloju vanjski pristup. Podsustav prijenosa secures komunikacije pomoću X509 bonovima ili sigurnost sustava Windows. Ovaj podsustav tkanina servis koristi interno i nije moguće izravno pristupiti programerima programirati s računala.

## <a name="federation-subsystem"></a>Vanjski pristup podsustav
Da bi se razloga o skupu čvorove raspodijeljeno sustavu, morate imati dosljedan prikaz sustava. Podsustav vanjski pristup koristi primitives komunikacije koje ste dobili od podsustavu prijenosa i stitches razne čvorove u jednom klaster objedinjenih ga možete razloga o. Pruža primitives raspodijeljeno sustavi trebaju na drugim podsustava - otkrivanje pogreške, izborno Vodeća crta i dosljedni usmjeravanja. Pri vrhu tablice raspodijeljeno raspršivanje razmakom tokena 128-bitno je ugrađena podsustavu vanjski pristup. Podsustav stvara Nazovi topologije putem čvorove sa svakom čvor na popisu koji se dodijeliti podskup tokena prostora za vlasništvo. Za otkrivanje pogreške sloj koristi lizinga mehanizam na temelju pozivanje beating i arbitražu. Vanjski pristup podsustav i jamčiti putem složene spoj i odlaska protokola koje samo jednog vlasnika token postoji u bilo kojem trenutku. To omogućuju izborno Vodeća crta i dosljedni usmjeravanje jamstva.

## <a name="reliability-subsystem"></a>Podsustav pouzdanosti
Podsustav pouzdanosti nudi mehanizam za oslobađanje stanja servisa servis tkanina iznimno pomoću _umnažanje_, _Upravitelj prebacivanje_i _Opterećenja resursa_.

* Na umnažanje osigurava da promjena stanja replike primarni servisa automatski je replicirati na sekundarnog replike održavanje dosljednost između primarnih i sekundarnih replike u skupu replike servisa. Na umnažanje je zadužen za upravljanje kvorum među replike u skupu replike. Ga stupi u interakciju s pomoćnim jedinice da biste dobili popis postupci za replikaciju te je ponovno konfiguriranje agent pruža s konfiguracijom replike. Tu konfiguraciju upućuje na to koji replike operacije potrebno je replicirati. Servis tkanina pruža zadane umnažanje naziva umnažanje tkanina koji se može koristiti tako da programski API model da biste stanje servisa iznimno dostupan je i pouzdan.
* Upravitelj prebacivanje osigurava da kada čvorove dodati ili ukloniti iz skupine, opterećenje je automatski daljnja distribucija preko čvorove dostupna. Ako čvor u klasteru ne uspije, klaster automatski konfigurirajte replike servisa da biste zadržali dostupnost.
* Voditelj resursa postavlja servisa replike preko domene pogreške u klasteru i provjerava jesu li sve jedinice prebacivanje radu. Voditelj resursa salda i resursi za servisnu preko podlozi zajednički skup klaster čvorove da biste postigli optimalne uniform učitavanja raspodjele.

## <a name="management-subsystem"></a>Upravljanje podsustav
Upravljanje podsustav nudi završetka do kraja servisa i upravljanje aplikacijama životni ciklus. PowerShell cmdleti i administratora API-ji omogućuju Dodjela implementacije, zakrpu, Nadogradnja i njezini Dodjela aplikacije bez gubitka dostupnost. Upravljanje podsustav izvodi to putem sljedećih servisa.

* **Upravitelj klaster**: to je primarni servis koji podržava interakciju s pomoćnim Upravitelj iz pouzdanosti da biste postavili aplikacije na čvorove ovisno o ograničenjima servisa položaj. Voditelj resursa u prebacivanje podsustav osigurava da su ograničenja nikad ne prekida. Upravitelj klaster upravlja životnim ciklusom aplikacije iz dodjele deaktivirali Dodjela. To integrira u upravitelju stanja da biste bili sigurni da dostupnosti aplikacija ne gube se iz perspektive semantičkog stanja tijekom nadogradnje.
* **Upravitelj stanja**: ovaj servis omogućuje nadzor stanja aplikacije, servise i entiteti klaster. Klaster entiteti (kao što su čvorove particije servisa i replike) mogu se prijaviti na informacije o stanju, koji se zatim pridružuje u spremište središnje stanje. Ove informacije o stanju nudi stanja ukupni točke-u – vrijeme snimke stanja servisa i čvorove raspodijeliti više čvorove u klaster, što vam omogućuje da iskoristite sve potrebne korektivne akcije. Stanje upita API-ji omogućuju vam poslati upit događaje stanja prijavljivati podsustavu stanja. Upit o stanju API-ji Vrati neobrađenog stanja podatke pohranjene u trgovini stanje ili zbroja, interpretiraju stanja podataka za određenu klaster entitet.
* **Pohrana slika**: ovaj servis nudi prostora za pohranu i distribucija binarne datoteke iz aplikacije. Ovaj servis nudi datoteka vrste simple raspodijeljeno trgovina u kojoj prenijeli i preuzete iz aplikacije.


## <a name="hosting-subsystem"></a>Hostiranje podsustav
Upravitelj klaster obavještava hostinga podsustav (koja se izvodi na svakom čvora) usluga koje je potrebno upravljati za određeni čvor. Hostinga podsustav zatim upravlja životnim ciklusom aplikaciju na tom čvor. Podržava interakciju s komponentama pouzdanosti i stanja da biste bili sigurni da se replike jesu i dobar.

## <a name="communication-subsystem"></a>Podsustav komunikacije
U ovom podsustav nudi pouzdanog poruka unutar klaster i servis za otkrivanje putem servisa za imenovanje. Servis za imenovanje razrješava nazivi servisa na mjesto u klasteru i korisnicima omogućuje upravljanje nazivi servisa i svojstva. Putem servisa za imenovanje, klijente možete sigurno komunicirati s bilo kojeg čvor u skupini riješiti naziv servisa i dohvaćanje metapodataka servisa. Klijenti za jednostavne imenovanje API-JA, korisnici servisa tkanina razviti servisa i klijenti koje je moguće povezati rješavanja trenutno mrežno mjesto bez obzira na dynamism čvor i ponovno veličine klaster.

## <a name="testability-subsystem"></a>Podsustav testability
Testability je paket sustava Alati posebno dizajnirane za testiranje servise koji se temelji na tkanina servisa. Alati omogućuju razvojni inženjer induce smisleni kvarove i pokrenite test scenariji Nekorištenje i brojne Državama i prijelaze koje će se dogoditi servisa cijeloj njegov vijek sve nadziranim i siguran način za provjeru valjanosti. Testability nudi i mehanizam da biste pokrenuli više testova koji možete iteracija preko različitih moguće pogreške bez gubitka dostupnost. Ovo vam omogućuje testiranje u radnom okruženju.
