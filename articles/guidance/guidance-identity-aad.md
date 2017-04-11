<properties
   pageTitle="Implementacijom Azure Active Directory | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurne hibridnog mreže pomoću servisa Azure Active Directory."
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Implementacijom Azure Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za integraciju lokalni Active Directory (AD) domene i šuma s Azure Active Directory za davanje provjere autentičnosti utemeljene na oblaku identitet.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. Ovu referenca arhitektura koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Možete koristiti direktorija i identiteta servisa, kao što su oni koje ste dobili od AD Directory Services (AD DS) za provjeru identiteta. Ove identiteta pripadati korisnika, računala, aplikacije i ostali resursi taj obrazac dio granicu sigurnost. Možete hostirati direktorija i identiteta servisa lokalni, ali u scenariju hibridnog gdje se nalaze elementi aplikacije u Azure može biti učinkovitije da biste proširili ta je funkcija u oblak. Takvog smanjiti Latencija uzrokovanih slanja provjeru autentičnosti i lokalne autorizacije zahtjeve iz oblaka natrag u direktorij i identitet servise lokalnog.

Azure nudi dva rješenja za implementaciju identitet i imenik servisa u oblaku:

1. Možete koristiti [Azure Active Directory (AAD)] [ azure-active-directory] da biste stvorili domenom sustava AD u oblak i povezati ga s lokalnog servisa Active Directory domene. AAD omogućuje vam da biste konfigurirali jedinstvenu prijavu (SSO) za korisnike pokretanje aplikacija na kojoj se pristupa putem oblaka. AAD koristi [Azure AD Connect] [ azure-ad-connect] radi lokalnog za replikaciju objekte i identiteta sadrži lokalno spremište u oblak.

    Možete i implementirati AAD bez korištenja lokalnog imenika. U ovom slučaju AAD ponaša se kao primarni izvor sve informacije o identitetu umjesto koji sadrže podatke replicirati iz lokalnog imenika.

    Imajte na umu da je direktorij u oblaku **ne** datotečni nastavak lokalnog imenika. Umjesto toga je kopija koja sadrži objekte i identiteta. Promjene ove stavke na lokalni se kopiraju u oblak, ali promjene u oblak **nisu** je replicirati natrag na lokalne domene.

    >[AZURE.NOTE] Izdanja Azure Active Directory Premium podržava pisanje-stražnjoj strani korisničke lozinke, omogućivanje lokalne korisnike da biste izvršili samostalno u oblaku. To je jedini podaci koje AAD sinkronizira natrag na lokalnog imenika. Dodatne informacije o različitim verzijama AAD i njihovih značajki potražite u članku [Azure Active Directory izdanja][aad-editions].

2. Možete implementirati VM izvodi AD DS kao kontroler domene u Azure, proširivanje postojeću infrastrukturu AD (uključujući AD DS i AD FS) iz vaše lokalne podatkovnog centra. Taj se način je uobičajene scenarije gdje povezani lokalne mreže i Azure virtualne mreže putem VPN-a i/ili ExpressRoute veze. Rješenje podržava i dvosmjerni replikacije omogućujući vam promjene u oblak i informacije o lokalnom gdje god je najprikladnije.

U ovom arhitektura usredotočuje se na rješenje 1. Dodatne informacije o drugi rješenje potražite u članku [Proširivanje servisa Active Directory za Azure][guidance-adds].

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Ponuda SSO za krajnje korisnike radi SaaS i drugim aplikacijama u oblaku, pomoću istih vjerodajnica koje određuju za lokalni aplikacije.

- Objavljivanje lokalnog web-aplikacije putem oblaka za pristup udaljenim korisnicima koji se nalaze u vašoj tvrtki ili ustanovi.

- Implementacijom samouslužne mogućnosti za krajnje korisnike, kao što su ponovno postavljanje lozinke i prenošenjem upravljanje grupe. 

    >[AZURE.NOTE] Te mogućnosti možete upravlja administrator putem pravilnika grupe.

- Situacije u kojima lokalne mreže i Azure virtualne mreže hosting oblaka aplikacije nisu izravno povezani pomoću tunelom VPN-a ili ExpressRoute elektronička.

Imajte na umu da AAD ne nudi sve funkcije AD. Na primjer, AAD trenutno samo rukuje provjere autentičnosti korisnika umjesto provjere autentičnosti za računala. Neka aplikacija i servisa, kao što je SQL Server možda zahtijeva provjeru autentičnosti za računalo u tom slučaju rješenje nije odgovarajuća. Uz to, AAD možda nije prikladna za sustave kojima se komponente migrirali preko granicu na – lokalno/oblaka, a ne samo koja se kopira.

> [AZURE.NOTE] Detaljno objašnjenje funkcioniranja Azure Active Directory, pogledajte na [Microsoft Active Directory objašnjenje][aad-explained].

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističe važne komponente u toj arhitekturi. Dodatne informacije o elementima radno opterećenje u Azure pročitajte [Pokretanje VMs za Arhitektura programa N sloja na Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Zbog jednostavnosti, ovaj dijagram samo prikazuje veze izravno povezani s AAD, a zatim opisuju web preglednika zahtjev preusmjeravanja ili druge protokol povezane prometa koji se mogu pojaviti u sklopu postupka vanjski pristup za provjeru autentičnosti i identitet. Ako, na primjer, korisnik (lokalni ili alat za analizu daljinske) obično pristupa web-aplikacijama putem preglednika, a web-aplikaciji možda proziran preusmjerite web-preglednika za provjeru autentičnosti zahtjeva putem AAD. Nakon provjere autentičnosti zahtjev možete proslijediti vratite se u web-aplikaciji zajedno s informacijama o odgovarajuće identitet.

[! [0]][0]

- **Klijent azure Active Directory**. Ovo je instance komponente AAD stvorenu u vašoj tvrtki ili ustanovi. Ponaša se kao jednostavni imeničkog servisa za oblak aplikacije (ga sadrži objekte koji se kopiraju u lokalnim AD), i njihovi identiteta servisa.

- **Podmreže web razina**. U ovom podmreže sadrži VMs koji implementirati prilagođeni oblaku aplikacije razvio vaša tvrtka ili ustanova te za koje AAD može poslužiti kao programa broker identiteta.

- **Lokalni poslužitelj AD DS**. Tvrtka ili ustanova vjerojatno već ima postojeće imeničkog servisa kao što su AD DS. Sinkronizacija veći broj stavki u AD DS imenika (kao što je korisnika i grupa podataka) s AAD da biste omogućili AAD koristiti te podatke za provjeru identiteta.

- **Poslužitelj za azure AD Connect sinkronizaciju**. Ovo je na računalo lokalnog koja se pokreće sinkronizaciju servisa Azure AD Connect. Instalirajte servis pomoću softvera za Azure AD Connect. Sinkroniziranje servisa Azure AD Connect sinkronizira informacije o (korisnike, grupe, kontakti, itd.) održava u lokalnom sustavu AD za AAD u oblaku. Ako, na primjer, možete Dodjela resursa ili deprovision grupama i korisnicima lokalnog i te promjene prenose AAD. Sinkroniziranje servisa Azure AD Connect prosljeđuje informacije AAD klijenta.

    >[AZURE.NOTE] Sigurnosnih vam razloga korisničke lozinke ne spremaju se izravno u AAD. Umjesto toga AAD sadrži ključa svake lozinke. To je dovoljno da biste provjerili korisničke lozinke. Ako korisnik zahtijeva ponovno postavljanje lozinke, to mora biti izvesti lokalnog poslužitelja i novi raspršivanje šalju AAD. AAD Premium izdanjima uključuju značajke koje je moguće automatizirati zadatak da biste korisnicima omogućili da ponovno postavljanje vlastite lozinki.

## <a name="recommendations"></a>Preporuke

U ovom se odjeljku navedene preporuke za implementaciju AAD na sljedeći način.

- AD Connect
- Sigurnost

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD Connect sinkronizaciju servisa preporuke

Sinkronizacija se bavi Provjera je li informacije o identitetu korisnici u oblaku dosljedno koji se održava lokalno. Svrha sinkronizaciju servisa Azure AD Connect je ostao u ovom. Sljedeće točke sažetak preporuke za sinkroniziranje servisa Azure AD Connect implementacije:

- Prije implementacije sinkronizacije Azure AD Connect određujete preduvjeti za sinkronizaciju vaše tvrtke ili ustanove (što da biste sinkronizirali s kojim se domena i koliko često. U članku [preduvjeti za sinkronizaciju direktorija određivanje] [ aad-sync-requirements] u članku se opisuje točke razmotrite.

- Možete pokrenuti servis sinkronizacije Azure AD Connect pomoću programa VM ili računalu hostira lokalnog. Ovisno o volatility informacije u direktoriju AD, kad je izvršena početne sinkronizacije s AAD opterećenje sinkronizaciju servisa Azure AD Connect je vjerojatno neće biti visoka. Pomoću programa VM omogućuje vam da biste lakše skalirali server (monitor aktivnosti kao što je opisano u odjeljku [pitanja vezana uz nadzor](#monitoring-considerations) da biste odredili hoće li to je potrebno).

- Ako imate više domena lokalne na skup stabala, možete spremiti i sinkronizirati podatke za čitav skup stabala na jednom AAD klijent (to je preporučeni način). Filtriranje podataka za identitete koji se pojavljuju u više od jedne domene tako da svaki identitet prikazuje samo jednom u AAD umjesto duplicirane kao što je to može dovesti do nedosljednosti prilikom sinkronizacije podataka. Taj se način zahtijeva implementacijom više Azure AD Connect sinkronizacijom poslužitelja. Dodatne informacije potražite u članku scenarij više AAD u odjeljku [Topologija pitanja](#topology-considerations) . 

- Pomoću filtriranja možete ograničiti podatke pohranjene u AAD da biste samo one koji je potreban. Tvrtke ili ustanove, na primjer, možda želite da biste pohranili podatke o računima Neaktivni ili -osobno u AAD. Filtriranje može biti grupne, utemeljen na domeni, utemeljen na OU ili utemeljen na atribut, a možete kombinirati i filtre da biste generirali složenija pravila. Nije moguće, na primjer, odaberite da biste sinkronizirali samo one objekte sadrži domene koje sadrže određenu vrijednost iz odabranih atributa. Detaljne informacije potražite u članku [Azure AD Connect sinkronizacije: Konfiguriranje filtriranje][aad-filtering].

- Da biste implementirate visoke dostupnosti za sinkronizaciju servisa AD Connect, pokrenite sekundarne pripremna poslužitelja. Dodatne informacije potražite u odjeljku [Topologija pitanja](#topology-considerations) .

### <a name="security-recommendations"></a>Preporuke o sigurnosti

Sljedeće stavke sažetak primarni sigurnosni preporuke za implementaciju AAD, ovisno o potrebama vaše tvrtke ili ustanove:

- Upravljanje korisničke lozinke.

    Ako koristite premium izdanje sustava AAD, možete omogućiti lozinku pisanje-natrag iz AAD da biste lokalnog imenika prilagođene instalacije Azure AD Connect:

    [! [9]][9]

    Značajka korisnicima omogućuje ponovno postavljanje vlastite lozinki unutar Azure portala, ali samo želite omogućiti nakon pregleda pravila sigurnost lozinke za vaše tvrtke ili ustanove. Ako, na primjer, mogu ograničiti koje korisnici mogu promijeniti svoje lozinke i prilagoditi sučelje za upravljanje lozinku. Dodatne informacije potražite u članku [Upravljanje lozinkama prilagodbu potrebama vaše tvrtke ili ustanove][aad-password-management].

- Održavanje zaštite za lokalni aplikacije koje se može pristupiti vanjsko.

    Koristite Azure AD proxy poslužitelj aplikacije za kontrolirati pristup lokalnog web-aplikacije s vanjskim korisnicima putem AAD. Taj se način štiti aplikacija tako da ne će izravno s Internetom. Samo korisnici koji imaju valjanih se vjerodajnica u imeniku Azure će moći pristupiti aplikacija. Dodatne informacije potražite u članku [Omogućivanje Proxy aplikacije na portalu za Azure][aad-application-proxy].

- Aktivno nadzor AAD za predznaku sumnjivoj aktivnosti.

    Razmislite o korištenju AAD Premium P2 izdanje koja obuhvaća AAD identiteta zaštitu. Zaštita identitet koristi algoritama prilagodljivo strojnog učenja i heuristike za otkrivanje anomalies i rizikom događaja koji se može značiti da je identitet ugrožena. Na primjer, ga može prepoznati potencijalno anomalous aktivnosti kao što su nepravilne prijavu aktivnosti, znak dodataka iz nepoznatih izvora ili IP adrese u sumnjivoj aktivnosti ili znak dodaci s uređaja koje mogu špijunskim. Pomoću ove podatke, zaštitu identiteta generira izvješća i upozorenja koja omogućuje vam da biste istražili tih rizika događaja i poduzeti odgovarajuće olakšava ili ublažiti akcija. Dodatne informacije potražite u članku [Azure Active Directory identiteta zaštita][aad-identity-protection].

    Praćenje sumnjivih i druge vezane uz sigurnost aktivnosti koje su se pojavile unutar sustava možete koristiti i izvješćivanja značajke AAD na portalu za Azure. AAD možete generirati niz sažetak izvješća:

    [! [17]][17]

    Dodatne informacije o korištenju tih izvješća potražite u članku [Azure Active Directory izvješćivanja vodič][aad-reporting-guide].

## <a name="topology-considerations"></a>Razmatranja topologije

Ako su integriranje lokalnog imenika s AAD, konfigurirati Azure AD Connect implementaciju topologije koja najviše odgovara potrebama vaše tvrtke ili ustanove. Topologija s podrškom za Azure AD Connect obuhvaćaju sljedeće:

- **Jedan skup stabala, jedan AAD direktorija**. U ovom topologiji Azure AD Connect koristite da biste sinkronizirali objekte i informacije o identitetu u jednu ili više domena u jednom lokalnog skupa stabala s jednom AAD klijentom. Ovo je zadana topologije pomoću eksplicitnih instalaciju Azure AD Connect.

    [! [1]][1]

    Ne stvorite više poslužitelja Azure AD Connect sinkronizaciju s različitim domenama u isti skup stabala u lokalni istom klijentu AAD osim ako su pokrenuti sinkronizaciju poslužitelj za Azure AD Connect u pripremna način (pogledajte pripremna poslužitelja ispod).

- **Više šuma, jedan AAD direktorija**. Ako imate više od jednog skupa stabala lokalnog, koristite ovaj topologije. Možete konsolidirati podatke o identitetu tako da svaki jedinstveni korisnički predstavlja jednom u direktoriju AAD, čak i ako se isti korisnik postoji u više od jednog skupa stabala. Sve šuma koristi isti sinkronizaciju poslužitelj za Azure AD Connect. Poslužitelj za Azure AD Connect sinkronizaciju ne mora biti dio bilo koje domene, ali mora biti dostupan iz svih šuma:

    [! [2]][2]

    U ovom topologiji ne koristite odvojene Azure AD Connect sinkronizirati poslužitelji za svaki lokalni skupa stabala povezati jedan AAD klijenta. Ako korisnici su prisutne u više od jednog skupa stabala to može rezultirati dupliciranu identiteta informacije u AAD.

- **Više šuma, odvojene topologija**. Taj se način omogućuje vam da biste spojili identiteta informacije iz zasebnom šuma u jednom AAD klijenta. U ovom strategije korisno je ako sastavljate šuma iz različitih tvrtke ili ustanove (nakon spajanja i Nabava, na primjer), a čuva se samo jedan skup stabala identifikacijskih podataka za svakog korisnika:

    Ako se sinkroniziraju GALs u svaki skup stabala, korisnika u jedan skup stabala mogu biti prisutni u drugoj kao kontakt. To se može dogoditi ako, na primjer, vašoj tvrtki ili ustanovi je postavila GALSync Upravitelj identiteta Forefront 2010 ili Microsoft Upravitelj identiteta 2016. U ovom scenariju, možete odrediti da korisnici moraju prepoznaje njihove atribut *pošta* . Možete povezati i identiteti korištenjem atributa *ObjectSID* i *msExchMasterAccountSID* . To je korisno ako imate šuma resursa s onemogućeni računi.

- **Pripremna poslužitelja**. U ovoj konfiguraciji pokrenuti drugog pojavljivanja sinkronizaciju server Azure AD Connect paralelno s prve. Scenariji ovu strukturu podržava kao što su:

    - Visoke dostupnosti.

    - Testiranje i implementacija novu konfiguraciju poslužitelja za Azure AD Connect sinkronizaciju.

    - Uvod u novi poslužitelj i deaktivirati stari konfiguracije. 

    U sljedećim scenarijima drugu pojavu pokreće se u *pripremna načinu rada*. Poslužitelj zapisi uvezeni objekata i sinkronizacije podataka u svoju bazu podataka, ali protok podataka u AAD. Samo kada onemogućite pripremna način ne poslužitelj započnite upisivati podataka AAD i pokreće predstavljanja lozinku pisanje-sprema u lokalnog imenika primjereno:

    [! [4]][4]

    Dodatne informacije potražite u članku [Azure AD Connect sinkronizacije: operativnih zadataka i napomene][aad-connect-sync-operational-tasks].

- **Više AAD direktorija**. Preporučuje se da stvorite jedan AAD direktorij za organizaciju, ali možda postoje situacije u kojima ćete morati particija informacije u zasebnom AAD direktorija. U ovom slučaju, potrebno je provjeriti prikazuje li se sve objekte iz skupa stabala lokalnog samo u jednom AAD direktorija, da biste izbjegli probleme s pisanje natrag za sinkronizaciju i lozinku.

    Možete implementirati scenarij konfiguriranje zasebnom Azure AD Connect sinkronizacija poslužitelji za svaki direktorij AAD i pomoću filtriranja tako da svaki poslužitelj za Azure AD Connect sinkronizaciju pristajete na isključivih skupa objekata: 

    [! [5]][5]

Dodatne informacije o ove topologija potražite u odjeljku [Topologija za Azure AD Connect][aad-topologies].

## <a name="user-sign-in-considerations"></a>Napomene za prijavu korisnika

Prema zadanim postavkama servisa AAD pretpostavlja da korisnici prijaviti unosom istu lozinku da koriste lokalne i sinkronizaciju server Azure AD Connect konfigurira sinkronizaciju lozinke između lokalne domene i AAD. Za mnoge tvrtke ili ustanove, to je prikladno, ali trebali biste postojeća pravila i infrastrukturu vaše tvrtke ili ustanove. Ako, na primjer:

- Pravila sigurnosti vaše tvrtke ili ustanove mogu sprječavaju sinkroniziranje raspršivanja lozinku u oblak.

- Može zahtijevati da korisnici primijetiti objedinjenog SSO (bez dodatnih lozinku Traži) prilikom pristupa resursima oblaka iz domene spajaju strojeva poslovnoj mreži.

- Vaša tvrtka ili ustanova možda već imate ADFS ili nekog drugog davatelja vanjski pristup usluga implementiran. Možete konfigurirati AAD da biste koristili ovu infrastrukture za provjeru autentičnosti implementacija i SSO umjesto korištenja lozinke održava u oblaku.

Dodatne informacije potražite u članku [Azure AD povezivanje korisnika za prijavu mogućnosti][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD razmatranja proxy aplikacije

Koristite Azure AD za pristup lokalnog aplikacije.

- Izložiti vaše lokalne web-aplikacije pomoću poveznika upravljanje proxy komponenta za Azure AD aplikacije proxy aplikacije. Poveznik za proxy aplikacije otvorit će se izlazni mrežom za proxy poslužitelj za Azure AD aplikacije. Korisnici udaljene zahtjeve usmjeruje od AAD putem te veze na web-aplikacije. U ovom mehanizam uklanja potrebno da biste otvorili ulaznog priključaka u vatrozidu za lokalni, smanjite plošni napada koji se prikazuje vaša tvrtka ili ustanova.

Dodatne informacije potražite u članku [Objavljivanje aplikacije koje se koriste proxy Azure AD aplikacije][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Razmatranja sinkronizacije objekta

Zadana konfiguracija Azure AD Connect sinkronizira objekte iz lokalnog imenika AD na temelju skup pravila koji su navedene u članku [Azure AD Connect sinkronizacije: objašnjenje zadanu konfiguraciju][aad-connect-sync-default-rules]. Samo objekte koji zadovoljavaju sinkroniziraju se ovim pravilima, drugima zanemaruju se. Ako, na primjer, objekti korisnik mora imati jedinstveni *sourceAnchor* atribut i atribut *accounEnabled* moraju biti ispunjena. Korisnik objekata koji imaju atribut *sAMAccountName* koji počinju tekst *AAD_* ili *MSOL_* nisu sinkronizirane. Azure AD Connect primjenjuje pravila za nekoliko korisnika, kontakta, grupa, ForeignSecurityPrincipal i računalo objekte. Ako je potrebno izmijeniti zadani skup pravila pomoću uređivaču pravila sinkronizacije instaliran s Azure AD Connect (i navedenih u [Azure AD Connect sinkronizacije: objašnjenje zadanu konfiguraciju][aad-connect-sync-default-rules]).

Možete definirati vlastite filtre da biste ograničili objekata sinkronizirati tako da je domena ili Organizacijska Jedinica. Ili implementirati složenije prilagođene filtriranja, kao što je opisano u [Azure AD Connect sinkronizacije: Konfiguriranje filtriranje][aad-filtering].

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Pomoću kontrola pristupa uvjetno Odbijanje zahtjeva za provjeru autentičnosti iz nepoznatih izvora:

- Pokretanje [Servisa Azure višestruke provjere autentičnosti (MFA)] [ azure-multifactor-authentication] ako korisnik pokuša da biste se povezali s mjesta koje nisu pouzdane (kao što su putem Interneta) umjesto pouzdanih mreže.

- Pomoću platforme vrste uređaja korisnika (iOS, Android i Windows Mobile Windows) da biste odredili pravilnik o pristupu programi i značajke.

- Zabilježite stanje omogućeno/onemogućeno uređaja za korisnika i ugraditi te podatke u access pravila provjere. Ako, na primjer, ako izgubite ili krađe korisničke telefona ga trebali biste spremiti kao onemogućena da biste spriječili koja se koristi za pristupanje.

- Kontrola razinu pristupa za korisnika na temelju članstvo u grupi. Pomoću [pravila za dinamičku članstvo AAD] [ aad-dynamic-membership-rules] da biste pojednostavnili Administracija grupnih. Kratki pregled kako to funkcionira, potražite u članku [Uvod u dinamičku članstva u grupama][aad-dynamic-memberships].

- Pomoću pravila uvjetnog pristup rizika AAD identiteta zaštita dodatno zaštiti na temelju neobično aktivnosti za prijavu ili druge događaje.

Dodatne informacije potražite u članku [Azure Active Directory uvjetno pristup][aad-conditional-access].

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Skalabilnost je adresirana web-mjesto servisa AAD i u okvir za konfiguraciju poslužitelja za sinkronizaciju Azure AD Connect:

- Za servis AAD ne morate konfigurirati sve mogućnosti za implementaciju skalabilnost. Servis AAD podržava skalabilnost na temelju replike. AAD primjenjuje jednu primarnu kopiju, koje ručice za pisanje operacije, a više samo za čitanje sekundarnog replike. AAD proziran preusmjerava koju ste pokušali izvršiti zapisivanja protiv sekundarnog replike pokušaj primarni replike. AAD nudi usmjerenog dosljednost. Sve promjene primarni replike prenose sekundarnog replike. Kao i većina postupaka protiv AAD čitanja umjesto zapisivanje, tu arhitekturu mijenja veličinu dobro.

    Dodatne informacije potražite u članku [Azure AD: Pokaži napredne postavke naš suvišnih zemlj., Visoko dostupna, raspodijeljeno oblaka direktorija][aad-scalability].

- Za sinkronizaciju server Azure AD Connect mora odrediti koliko objekata ćete vjerojatno sinkronizirati iz lokalnog imenika. Ako imate manje od 100 000 objekata, možete koristiti softver za SQL Server Express LocalDB zadani dao Azure AD Connect. Ako imate veći broj objekata, morate instalirati verzija sustava SQL Server i izvršiti prilagođenu instalaciju Azure AD Connect određivanje koje treba koristiti postojeće instance komponente SQL Server.

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Kao i kod opasnosti skalabilnost dostupnost odnose se na servis AAD i konfiguracija Azure AD Connect:

- Servis AAD namijenjen je pružanje visoke dostupnosti. Nema mogućnosti korisnik može konfigurirati dostupnost. Je zemlj distributed i pokreće više podataka centara Podjela svijeta s automatsko prebacivanje. Ako u podatkovnom centru postane dostupan, AAD osigurava direktorija podataka dostupna za instancu programa access u barem dvije dodatne regionally raspršenim podataka.

    >[AZURE.NOTE] SLA dostupnost AAD osnovni i Premium usluge jamstva najmanje 99.9%. Postoji bez SLA za besplatne sloj AAD. Dodatne informacije potražite u članku [SLA za Azure Active Directory][sla-aad].

- Da biste povećali dostupnost poslužitelj za Azure AD Connect sinkronizaciju možete pokrenuti druga instanca u pripremna načinu rada, kao što je opisano u odjeljku [Topologija pitanja](#topology-considerations) . 

    Osim toga, ako se ne koriste instancu sustava SQL Server Express LocalDB koji se isporučuju s Azure AD Connect, zatim razmotrite visoke dostupnosti za SQL Server. Imajte na umu da je rješenje samo visoke dostupnosti podržane SQL Klasteriranje. Azure AD Connect ne podržava rješenja kao što su zrcaljenja i uvijek na.

    Dodatne napomene o zaštiti raspoloživost poslužitelj za Azure AD Connect sinkronizaciju i kako oporaviti nakon neuspješne potražite u članku [Azure AD Connect sinkronizacije: operativnih zadataka i pitanja vezana uz – Izrada oporavak][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Upravljanje značajkama

Postoje dvije aspekti upravljanje AAD:

1. Administriranje AAD u oblaku.

2. Održavanje poslužitelja za sinkronizaciju Azure AD Connect.

AAD nudi sljedeće mogućnosti za upravljanje domenama i direktorija u oblaku:

- [Modul Azure Active Directory PowerShell][aad-powershell]. Ako vam je potrebna skripte uobičajenih Azure AD administrativne zadatke kao što je upravljanje korisnicima, upravljanje domenom i za konfiguriranje jedinstvenu prijavu, koristite ovu modul.

- Plohu upravljanje Azure AD na portalu za Azure. U ovom plohu nudi prikazu interaktivne upravljanje direktorija i omogućuje upravljanje i konfiguriranje većinu aspekata AAD.

    [! [10]][10]

Azure AD Connect instalira sljedeće alate koji koristite da biste zadržali sinkronizaciju servisa Azure AD Connect iz vaše lokalne strojeva:

- Microsoft Azure Active Directory povezivanje konzoli sustava. Ovaj alat omogućuje izmjenu konfiguracije poslužitelj za Azure AD sinkronizaciju, prilagoditi kako sinkronizaciji, omogućiti ili onemogućiti pripremna način i prijelaz korisnika prijavu MOD (možete omogućiti AD FS prijavu pomoću infrastruktura za lokalni).

- Upravitelj sinkronizacije servisa. Pomoću kartice *operacije* u ovom alatu za upravljanje postupka sinkronizacije i otkriti li neke dijelove postupak nije uspjela. Možete pokrenuti sinkronizacijom ručno pomoću alata. 

    [! [12]][12]

    *Poveznika* kartica omogućuje kontrolu veze za domena (lokalni i u oblaku) za koji je pridružen program sinkronizacije:

    [! [13]][13]

-  Uređivač pravila sinkronizacije. Koristite ovaj alat za prilagodbu na način kojim su objekti transformacije kada se kopiraju između lokalnog imenika i AAD u oblaku. Ovaj alat omogućuje vam da navedete atribute i objekte za sinkronizaciju i filtre da biste odredili koje će se instance treba ili nije moguće sinkronizirati.

    Dodatne informacije potražite u odjeljku uređivač pravila za sinkronizaciju u dokumentu [Azure AD Connect sinkronizacije: objašnjenje zadanu konfiguraciju][aad-connect-sync-default-rules].

Na stranici [Azure AD Connect sinkronizacije: najbolje prakse za promjenu zadanu konfiguraciju] [ aad-sync-best-practices] sadrži dodatne informacije i savjete za upravljanje Azure AD Connect.

## <a name="monitoring-considerations"></a>Praćenje pitanja vezana uz

Nadzor stanja provodi pomoću niza agenata instaliran lokalnog programa:

- Azure AD Connect instalira koji se pohranjuju podaci o sinkronizacijom. Praćenje stanja i performanse Azure AD Connect pomoću plohu Azure Active Directory povezivanje stanja na portalu za Azure. Dodatne informacije potražite u članku [Korištenje Azure AD povezivanje stanje sinkronizacije][aad-health].

- Da biste pratili stanju AD DS domene i direktorija iz Azure, instalirajte povežite zdravlje Azure AD za AD DS agent na računalo u domeni lokalnog. Koristite plohu Azure Active Directory povezivanje stanja na portalu za Azure AD DS monitor. Dodatne informacije potražite u članku [Korištenje Azure AD povezivanje stanje s AD DS][aad-health-adds]

- Instalirajte povežite zdravlje Azure AD za AD FS agent za praćenje stanja servisa AD FS se izvode na lokalni i praćenje rada pomoću plohu Azure Active Directory povezivanje stanja na portalu za Azure AD DS. Dodatne informacije potražite u članku [Korištenje Azure AD povezivanje stanja AD fs][aad-health-adfs]

Dodatne informacije o instaliranju agenata AD povežite zdravlje i njihovih preduvjete potražite u članku [Azure AD povežite zdravlje Agent instalacije][aad-agent-installation].

## <a name="sample-solution"></a>Uzorak rješenja

U ovom se odjeljku dokumenti korake za stvaranje rješenja uzorka koje možete koristiti da biste testirali konfiguraciju AAD. Rješenja sastoji se od sljedećih elemenata:

- N sloja web-aplikacije, slijedeći strukturu opisan [Pokrenut VMs za Arhitektura programa N sloja na Azure][implementing-a-multi-tier-architecture-on-Azure].

- Na klijentskom računalu test. Preporučujemo korištenje drugog VM za ovo računalo. Konfiguriranje ovaj VM je nevažan sadržaj, kao imate pristup administrator i može povezati n sloja web-aplikacije.

- Simulirani lokalnog okruženja koja sadrži svoje domene izgrađene pomoću AD DS.

Scenariji u kojima se ilustrira korake u nastavku su:

- Omogućavanje pristup n sloja aplikaciju koja se izvodi u oblaku za vanjske korisnike s AAD omogućuje provjeru autentičnosti lozinke.

- Omogućavanje pristup n sloja aplikaciju koja se izvodi u oblak korisnicima u tvrtki ili ustanovi, koristeći AAD omogućuje provjeru autentičnosti lozinke.

### <a name="prerequisites"></a>Preduvjeti

Koraci koje slijedite pretpostavimo da sljedeće preduvjete:

- Imate postojeću pretplatu Azure u kojem možete stvoriti grupe resursa.

- Instalirate [sučelje naredbenog retka za Azure][azure-cli].

- Ste preuzeli i instalirali najnoviju verziju. U odjeljku [ovdje] [ azure-powershell-download] upute.

- Imate instaliran kopiju [makecert] [ makecert] utility na klijentskom računalu test.

- Imate pristup razvoj računalo na kojem je instaliran je Visual Studio.

### <a name="create-the-n-tier-web-application-architecture"></a>Stvaranje aplikacije arhitektura web n sloja

1. Stvorite mapu pod nazivom `Scripts` koja sadrži podmapu s nazivom `Parameters`.

2. Preuzimanje [Uvođenja ReferenceArchitecture.ps1] [ solution-script] skriptu PowerShell mapu skripti.

3. U mapi parametara stvorite drugi podmape pod nazivom Windows.

4. Preuzmite sljedeće datoteke u mapu parametara/Windows:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Uređivanje datoteke **Uvođenja ReferenceArchitecture.ps**1 u mapi skripte pa promijenite sljedeći redak za određivanje grupa resursa koji treba stvorili ili za držite VM i resursima stvorio skripte:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Uređivanje datoteke u mapi s **parametara/prozora**u nastavku i postavite na `resourceGroup` vrijednosti u na `virtualNetworkSettings` sekcija u svakoj od ove datoteke da biste je isti kao koje ste naveli u **Uvođenja ReferenceArchitecture.ps1** skriptna datoteka.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Otvorite prozor programa Azure PowerShell, premjestite u mapu skripte i pokrenite sljedeću naredbu:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Zamjena **<subscription id>** koristeći Azure pretplate ID.

    Za **<location>**, navedite Azure regiju, kao što su *eastus* ili *westus*.

8. Kada skriptu dovrši, pomoću portala za Azure da biste dobili javnu IP adresu opterećenja web razina (*ra-aad-ntier-web-lb*):

    [! [18]][18]

9. Prijavite se za testiranje klijentskom računalu (ili VM) i provjerite je li pristup web-sloju pomoću programa Internet Explorer da biste pregledali na javnu IP adresu opterećenja web razina. Zadane stranice IIS prikazivati.

### <a name="simulate-configuration-of-a-public-web-site"></a>Kao zamjenu za konfiguraciju javnog web-mjesta

>[AZURE.NOTE] Sljedeće korake kao zamjenu za pridruživanje web razina www.contoso.com naziv DNS izmjenom hosts datoteka na klijentskom računalu test. Uz to, VMs web razina konfigurirane s samopotpisane potvrde i lažne hosting za izdavanje certifikata. Ovo je za testiranje samo i **nemojte koristiti ove tehnike u radnom okruženju**.

1. Na klijentskom računalu test, uredite datoteku **C:\Windows\System32\drivers\etc\hosts** pomoću Blok za pisanje i dodavanje unosa www.contoso.com naziv pridružuje javnu IP adresu web Razina opterećenja:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Provjerite da možete odmah pregledavati www.contoso.com s klijentskog računala test. Isti zadane stranice IIS prikazivati kao prije.

3. Otvorite naredbeni redak kao Administrator i koristiti uslužni makecert do c na klijentskom računalu test
4. Stvori lažne korijenski certifikat za izdavanje certifikata:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Provjerite je li se stvaraju sljedeće datoteke:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Pokrenite sljedeću naredbu da biste instalirali izdavanje test:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Korištenje test ustanova za izdavanje certifikata radi generiranja certifikat za www.contoso.com:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Pokretanje naredbe za **blog** i dodajte certifikati dodatak za račun računala na lokalnom računalu.

7. U na */Certificates (lokalno računalo) / osobno/certifikata/* spremanje, izvoz certifikata www.contoso.com privatni ključ u datoteku pod nazivom www.contoso.com.pfx:

    [! [20]][20]

8. Vratite se na portal za Azure i povezati se sloju upravljanje VM (*ra-aad-ntier-upravljanje dokumentima-vm1*). Zadano korisničko ime je *testuser* lozinkom *AweS0me@PW*:

    [! [21]][21]
    
9. Iz upravljanje sloju VM povezati svaki web razina VMs shodno pomoću korisničkog imena *testuser* i lozinke *AweS0me@PW*, i izvedite sljedeće zadatke. Imajte na umu da se VMs imaju IP adrese privatne 10.0.1.4, 10.0.1.5 i 10.0.1.6:

    >[AZURE.NOTE] Web razina VMs tek privatne IP adrese. Možete se samo povezati s njima putem upravljanja sloju VM.

    1. Kopiranje datoteke *www.contoso.com.pfx* i *MyFakeRootCertificateAuthority.cer* s klijentskog računala test.
    
    2. Otvorite naredbeni redak kao Administrator, premjestite u mapu datoteke koju ste kopirali i izvršite sljedeće naredbe:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Pokretanje u `mmc` naredbu, dodajte certifikati dodatak za račun računala na lokalnom računalu i provjerite da su instalirane sljedeći certifikati:

        - Www.contoso.com \Personal\Certificates\ \Certificates (lokalno računalo)

        - \Certificates (lokalno računalo) \Trusted korijenska ustanova Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Pokrenite konzolu za Upravitelj Internet Information Services (IIS), a zatim otvorite *Sites\Default Web* -mjesto na računalu.

    5. U okno *Akcije* kliknite *povezivanja*i dodajte je https povezivanja putem SSL certifikata www.contoso.com:

        [! [22]][22]

10. Vratite test klijentsko računalo i provjerite je li sada možete pregledavati na https://www.contoso.com.

### <a name="create-an-azure-active-directory-tenant"></a>Stvaranje klijent za Azure Active Directory

1. Pomoću portala za Azure stvaranje klijent za Azure Active Directory kao što su *myaadname*. onmicrosoft.com, pri čemu je *myaadname* koju odaberete naziv direktorija.

2. Dodavanje korisnika s nazivom *administrator* s ulogom GlobalAdmin direktoriju. Zabilježite upravo generirani lozinku.

3. U pregledniku Internet Explorer potražite https://account.activedirectory.windowsazure.com/ i prijavite se kao admin@ *myaadname*. onmicrosoft.com. Promjena lozinke kada se to od vas zatraži.

### <a name="create-and-deploy-a-test-web-application"></a>Stvaranje i implementacija web-aplikaciju za testiranje

1. Pomoću Visual Studio na računalu razvoj stvaranje ASP.NET Web-aplikacije pod nazivom ContosoWebApp1 (pomoću .NET Framework 4.5.2):

    [! [23]][23]

2. Odaberite predložak *MVC* , promijenite provjera autentičnosti na *Službeni telefon i školske račune*, a Navedite naziv AAD klijentu. Ne stvorite aplikacije servisa u oblaku.

    [! [24]][24]

3. Stvaranje i pokretanje aplikacije da biste testirali provjeru autentičnosti. Na stranici za prijavu unesite naziv računa admin@ *myaadname*. onmicrosoft.com, tu lozinku, a zatim kliknite *Prijava*:

    [! [25]][25]

4. Provjerite je li traži dozvolu za prijavu i pročitajte profila AAD, a zatim pokreće izvodi web-aplikacije pomoću identiteta administrator.

5. Zatvorite Internet Explorer i vratite Visual Studio.

6. Otvaranje web.config datoteke i u `<appSettings>` sekcije, promijenite vrijednost ključa *ida: PostLogoutRedirectUri* za *https://www.contoso.com:443 /*. Spremite datoteku.

7. U prozoru *Preglednik rješenja* , desnom tipkom miša kliknite ContosoWebApp1 projekta, kliknite *Objavi*.

8. U prozoru *Web objavljivanje* kliknite *Prilagođeno*. Stvorite prilagođeni profil pod nazivom *ContosoWebApp1*.

9. Na stranici *veze* , postavite *način Objavi* na *Datotečnom sustavu* i navedite mapu pod nazivom *ContosoWebApp*, koja se nalazi na određenom mjestu na vašem računalu razvoj.

10. Na stranici *Postavke* postavljanje *konfiguracije* *izdanje*.

11. Na stranici *Pregled* kliknite *Objavi*.

12. Povezivanje sa svakom web-poslužitelju u nizu (putem upravljanja sloju VM), a zatim izvedite sljedeće zadatke:

    1. Kopirajte *ContosoWebApp* mapu i njezin sadržaj s računala razvoj *C:\inetpub* mapu.

    2. Korištenju konzole za Upravitelj Internet Information Services (IIS), pomaknite se do *Sites\Default Web-mjesto* na računalu.

    3. U oknu *Akcije* kliknite *Temelj postavke*, a *%SystemDrive%\inetpub\ContosoWebApp*promijenite fizički put web-mjesta:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Objavljivanje web-aplikacije test kroz AAD

1. Prijavite se na portal za Azure, a zatim otvorite AAD direktorija.

2. Na kartici *aplikacije* kliknite ContosoWebApp1 aplikacije.

3. Provjerite je li aplikacija uspješnog dodavanja direktoriju, a zatim karticu *KONFIGURACIJA* .

4. Promijeniti *URL za prijavu na* https://www.contoso.com:443 i postavite *URL ADRESE za odgovor* na https://www.contoso.com:443 (isti URL-a).

5. Spremanje konfiguracije.

6. Na klijentskom računalu test dođite do https://www.contoso.com. Provjerite da AAD od vas traži vjerodajnice, a zatim se prijavite.

>[AZURE.NOTE] Ovo je krajnja točka za prvi scenarij: Omogućivanje pristupa n sloja aplikaciju koja se izvodi u oblaku za vanjske korisnike s AAD omogućuje provjeru autentičnosti lozinke.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Stvaranje okruženja Simulirani lokalnog sa servisom Active Directory

Lokalnog okruženja sadrži dvije kontrolera domena za na `contoso.com` domene i dva poslužitelja za hostiranje Azure AD Connect sinkronizaciju servisa. Poslužitelji za hostiranje Azure AD Connect niste domene-član.

1. U Eksploreru za datoteke, vratite se u skripti mapu koja sadrži skripta koja se koristi za stvaranje web-aplikacije N sloja.

2. U mapi parametri dodajte drugu sub mapu pod nazivom `Onpremise`.

3. Preuzimanje datoteke u nastavku parametara/lokalnu verziju mapu:

    - [Dodavanje-dodaje-domene-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Stvaranje-dodaje-skupa stabala-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines adc joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork-dodaje-dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Uređivanje uvođenja ReferenceArchitecture.ps1 datoteke u mapi skripte i promijenite sljedeći redak za određivanje grupa resursa koji treba stvorili ili za držite VM i resursima stvorio skripte:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Uređivanje datoteke u mapi parametara/lokalnu verziju u nastavku i postavite na `resourceGroup` vrijednosti u na `virtualNetworkSettings` sekcija u svakoj od ove datoteke da biste je isti kao koje ste naveli u uvođenja ReferenceArchitecture.ps1 skriptna datoteka.

    - virtualMachines adds.parameters.json

    - virtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork-dodaje-dns.parameters.json

7. Otvorite prozor programa Azure PowerShell, premjestite u mapu skripte i pokrenite sljedeću naredbu:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Zamjena `<subscription id>` koristeći Azure pretplate ID.

    Za `<location>`, navedite Azure regiju, kao što su `eastus` ili `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Instaliranje i konfiguriranje sinkronizacije servisa Azure AD Connect

Konfiguriranje prikazanom u sljedećim koracima sastoji se od dvije instance sinkronizaciju servisa Azure AD Connect. Prvi je aktivan dok drugi je konfiguriran u načinu pripremna omogućuje brz prebacivanje i visoke dostupnosti ne uspijete prvi poslužitelj.

1. Pomoću portala za Azure, idite u grupu resursa držanjem VMs za sinkronizaciju servisa Azure AD Connect (*ra-aad-lokalnu verziju-ru* po zadanom), a povezati *ra-aad-lokalnu verziju-adc-vm1* VM. Prijavite se kao *testuser* lozinkom *AweS0me@PW*.

2. Preuzmite Azure AD Connect iz [ovdje][aad-connect-download].

3. Pokrenite instalacijski program za Azure AD Connect i izvođenje instalacije pomoću mogućnosti *Prilagodi* .

    >[AZURE.NOTE] Ne možete koristiti mogućnost *Ekspresne postavke* jer VM hosting sinkronizaciju servisa Azure AD Connect nije pridruženo za domenu.

4. Na stranici *Instalacija potrebne komponente* neobavezno konfiguracijske postavke ostavite prazan prihvatite zadane mogućnosti.

5. Na stranici *korisničku prijavu* odaberite *Sinkronizaciju lozinke*.

6. Na stranici *Povezivanje s Azure AD* unesite admin@ *myaadname*. onmicrosoft.com, pri čemu je *myaadname* AAD klijentu. Unesite lozinku za administratorski račun.

7. Na stranici *Povezivanje vašeg direktorija* odredite contoso.com za u skup stabala (upišite vrijednost u jer ne prikazuje se na padajućem popisu), CONTOSO\testuser unesite korisničko ime, navedite AweS0me@PW za lozinku, a zatim kliknite *Dodaj direktorija*.

8. Na stranici *konfiguracija za prijavu Azure AD* prihvatite zadane vrijednosti za korisnikovo Glavno ime. Provjerite *Nastavi bez sve provjerene domene*.

    >[AZURE.NOTE] Direktorija contoso.com su navedene kao *Nije provjereno*. Da biste provjerili naziv domene, morate stvoriti TXT zapis za registrar naziva domene. U ovom primjeru lokalne domene nije registriran vanjsko. Dodatne informacije potražite u članku [Dodavanje prilagođenog naziva domene Azure Active Directory][aad-custom-directory].

9. Na stranici *domene i OU filtriranja* , odaberite *sinkronizaciju svih domena i organizacijske jedinice* (zadano).

10. Na stranici *jedinstveno označavanje korisnike* prihvatite zadane vrijednosti.

11. Na stranici *Filtriranje korisnicima i uređajima* odaberite *Sinkroniziraj svim korisnicima i uređajima* (zadano).

12. Na stranici *značajke* odaberite *upisima lozinku*.

13. Na stranici *spremno za konfiguraciju* odaberite *pokretanje postupka za sinkronizaciju nakon dovršetka konfiguracije*, ali ostavite *Omogućivanje načina rada za pripremna* nije odabrana.

14. Provjerite je li da se proces konfiguracije dovršava bez pogrešaka, a zatim zatvorite instalacijski program.

15. Na portalu Azure povezati *ra-aad-lokalnu verziju-adc-vm2* VM. Prijavite se kao *testuser* lozinkom *AweS0me@PW*.

16. Preuzimanje Azure AD Connect, a zatim pokrenite instalacijski program.

17. Počnete slijediti čarobnjak i odgovaranje kao što je opisano u korake od 3 do 12.

18. Na stranici *spremno za konfiguraciju* odaberite *pokretanje postupka za sinkronizaciju kada se dovrši konfiguraciju*i **odabrati i** *Omogućivanje načina rada za pripremna*. Zbog toga sinkronizaciju servisa Azure AD Connect za dohvaćanje pojedinosti o računima i objekata s domenom contoso.com i sprema ih u svoju bazu podataka, ali ga ne prijenos te detalje za vaš klijent AAD.

14. Provjerite je li da se proces konfiguracije dovršava bez pogrešaka, a zatim zatvorite instalacijski program.

### <a name="test-the-aad-configuration"></a>Testiranje AAD konfiguraciju

1. Pomoću portala za Azure, prijeđite u direktoriju AAD, otvorite plohu Azure Active Directory, kliknite *korisnici i grupe*, a zatim *sve korisnike* da biste prikazali popis korisnika i grupa koji se sinkroniziraju s imenikom servisa. Trebali biste vidjeti korisnici sljedećih računa:

    - administrator (admin@ *myaadname*. onmicrosoft.com)

    - Račun servisa sinkronizacije direktorija za lokalni (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - Račun servisa sinkronizacije direktorija za lokalni (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. Na portalu Azure idite u grupu resursa držanjem VMs za AD DS kontrolera domena (*ra-aad-lokalnu verziju-ru* po zadanom) te je povežite *ra-aad-lokalnu verziju-ad-vm1* VM. Prijavite se kao *testuser* lozinkom *AweS0me@PW*.

3. Korištenje konzolu *Active Directory korisnicima i računalima* Dodavanje novog korisnika domene pod nazivom Tibbot Marica s ime za prijavu dianet@contoso.com. Navedite lozinku po izboru. Provjerite je korisnik član lokalne grupe administratora (to je samo da možete se prijaviti lokalno kao taj korisnik kasnije – samo strojeva u domeni su prevođenja).

4. Prijelaz na *ra-aad-lokalnu verziju-adc-vm1* VM, otvorite prozor PowerShell i izvršite sljedeće naredbe:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Naredba Provjerite vraća li *uspješno*.

    >[AZURE.NOTE] Ovaj korak nije potrebna jer je prema zadanim postavkama sinkronizacija zakazano izvođenje intervalima od 30 minuta. Te naredbe prisilno sinkroniziranja odmah.

5. Vratite se na portal za Azure, prijeđite u direktoriju AAD, otvaranje plohu Azure Active Directory, kliknite *korisnici i grupe*, a zatim kliknite *sve korisnike*. Sada trebali biste vidjeti Marica Tibbot (dianet@ *myaadname*. onmicrosoft.com) pojavljuju se na popisu korisnika.

6. U plohu Azure Active Directory kliknite *Aplikacijama*, a zatim kliknite *sve aplikacije*. Kliknite *ContosoWebApp1* aplikacije, a zatim kliknite *korisnici i grupe*. Na alatnoj traci kliknite *Dodaj*. Kliknite *korisnici i grupe*pa odaberite *Marica Tibbot*. Kliknite *Dodijeli*.

7. U pregledniku Internet Explorer dođite do https://account.activedirectory.windowsazure.com web-mjesta. Prijavite se kao dianet@ *myaadname*. onmicrosoft.com pomoću lozinke koje ste prethodno naveli.

    >[AZURE.NOTE] Kliknite ikonu ContosoWebApp na popisu aplikacija. AAD pokušava pronaći web-aplikacija na adresi javno navedene DNS-a za www.contoso.com, koji se razlikuje od adrese vaše web Razina opterećenja.

8. Kliknite karticu *profila* . Treba prikazati detalje o korisniku (Ako ste naveli bilo kada je stavka stvorena).

    >[AZURE.NOTE] Ako kliknete *Promijeni lozinku*, nije dopušteno da biste izvršili taj zadatak, kao što je ovaj postupak mora biti omogućen administrator najprije. Dodatne informacije potražite u članku [Uvod u upravljanje lozinkama][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Omogućivanje lokalne korisnike da biste pokrenuli aplikaciju pomoću provjere autentičnosti putem AAD

1. Vratite se u kontrolerom domene *ra-aad-lokalnu verziju-ad-vm1* VM.

2. Otvorite *Upravitelj DNS* konzolu i dodajte novi zapis glavnog računala za www.contoso.com. Navedite javnu IP adresu opterećenja web razina.

3. Kopirajte datoteku *MyFakeRootCertificateAuthority.cer* s klijentskom računalu test (koju ste stvorili ovaj datoteke u postupku [Simulate konfiguracije javnog web-mjesta](#simulate-configuration-of-a-public-web-site)
    
4. Otvorite naredbeni redak kao Administrator, premjestite u mapu držeći datoteku koju ste upravo kopirali i pokrenite sljedeću naredbu:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. U pregledniku Internet Explorer dođite do https://www.contoso.com. Provjerite je li da se prikaže stranica AAD, prijavu za web-aplikaciju ContosoWebApp1.

6. Prijavite se kao dianet@ *myaadname*. onmicrosoft.com. Aplikacija pokrenite i prijaviti pravilno.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte najbolje postupke za [Proširivanje lokalne domene ZBRAJA Azure][adds-extend-domain]

- Saznajte najbolje postupke za [Stvaranje skupa stabala resursa u ZBRAJA] [ adds-resource-forest] u Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Oblak pomoću servisa Azure Active Directory arhitektura za identitet"
[1]: ./media/guidance-identity-aad/figure2.png "Jedan skup stabala, jedan topologije AAD direktorija"
[2]: ./media/guidance-identity-aad/figure3.png "Više šuma, jedan topologije AAD direktorija"
[4]: ./media/guidance-identity-aad/figure5.png "Pripremna topologije poslužitelja"
[5]: ./media/guidance-identity-aad/figure6.png "Više topologije AAD direktorija"
[6]: ./media/guidance-identity-aad/figure7.png "Odabir prilagođene instalacije Azure AD povezivanje sinkronizacije s instancu sustava SQL Server"
[7]: ./media/guidance-identity-aad/figure8.png "Određuje način SSO za korisničku prijavu"
[8]: ./media/guidance-identity-aad/figure9.png "Određivanje domene i OU mogućnostima filtriranja"
[9]: ./media/guidance-identity-aad/figure10.png "Omogućivanje pisanje natrag za lozinke"
[10]: ./media/guidance-identity-aad/figure11.png "Upravljanje plohu Azure AD na portalu"
[11]: ./media/guidance-identity-aad/figure12.png "Konzola za Azure AD Connect"
[12]: ./media/guidance-identity-aad/figure13.png "Na kartici operacije u Upravitelj servisa sinkronizacije"
[13]: ./media/guidance-identity-aad/figure14.png "Na kartici poveznika u Upravitelj servisa sinkronizacije"
[14]: ./media/guidance-identity-aad/figure15.png "Uređivanje pravila sinkronizacije"
[15]: ./media/guidance-identity-aad/figure16.png "Plohu Azure Active Directory povezivanje stanja na portalu Azure prikaz stanja sinkronizacije"
[16]: ./media/guidance-identity-aad/figure17.png "Plohu Azure Active Directory povezivanje stanja na portalu Azure prikazuje stanje AD DS"
[17]: ./media/guidance-identity-aad/figure18.png "Sigurnost izvješća dostupna na portalu za Azure"
[18]: ./media/guidance-identity-aad/figure19.png "Portal za Azure isticanje javnu IP adresu opterećenja web razina"
[19]: ./media/guidance-identity-aad/figure20.png "Korištenje preglednika Internet Explorer da biste pregledali na javnu IP adresu opterećenja web razina"
[20]: ./media/guidance-identity-aad/figure21.png "Certifikati dodatak prikazuje www.contoso.com certifikata"
[21]: ./media/guidance-identity-aad/figure22.png "Povezivanje s upravljanja sloju VM"
[22]: ./media/guidance-identity-aad/figure23.png "Stvaranje povezivanju HTTPS za zadano web-mjesto"
[23]: ./media/guidance-identity-aad/figure24.png "Stvaranje web-aplikacije ContosoWebApp1"
[24]: ./media/guidance-identity-aad/figure25.png "Postavljanje svojstava za provjeru autentičnosti ContosoWebApp1 web-aplikacije"
[25]: ./media/guidance-identity-aad/figure26.png "Prva prijava u Azure AAD iz web-aplikacije ContosoWebApp1"
[26]: ./media/guidance-identity-aad/figure27.png "Promjena mape za zadano web-mjesto"