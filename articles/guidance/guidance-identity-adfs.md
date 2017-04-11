<properties
   pageTitle="Implementacijom ADFS u Azure | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurne hibridnog mreže s autorizacije servis za vanjski pristup servisa Active Directory u Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
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
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Implementacijom Active Directory Federation Services (ADFS) u Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za implementacije sigurno hibridnog mreže koji se prenosi i lokalne mreže Azure, a koji koristi [Active Directory Federation Services (ADFS)] [ active-directory-federation-services] da biste izvršili pridruženim provjere autentičnosti i autorizacije za komponente koji se izvodi u oblaku. U ovom arhitektura proširuje strukturu opisan [Proširivanje servisa Active Directory za Azure][implementing-active-directory].

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. Ovu referenca arhitektura koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

ADFS jer se može pokrenuti lokalni, ali u scenariju hibridnog gdje se nalaze aplikacije u Azure može biti učinkovitije implementaciju ta je funkcija u oblaku. Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Hibridno aplikacije mjesto cilja radnih opterećenja djelomično lokalnog i djelomično u Azure.

- Rješenja koja koriste pridruženim autorizacije da biste otkrili web-aplikacija za partnera tvrtke ili ustanove.

- Sustavima koji podržavaju pristup iz web-preglednika koji se izvodi izvan tvrtke ili ustanove vatrozid.

- Sustavi koji omogućuju korisnicima pristup web-aplikacijama povezivanjem s ovlašteni vanjskih uređaja kao što su udaljenim računalima, bilježnice i drugim mobilnim uređajima. 

Dodatne informacije o načinu funkcioniranja a ADFS potražite u članku [Pregled Services vanjski pristup Active Directory][active-directory-federation-services-overview]. Uz to, u članku [ADFS implementacije u Azure] [ adfs-intro] sadrži detaljne detaljne Uvod u implementacije ADFS u Azure.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističe važne komponente u toj arhitekturi (*kliknite da biste povećali*). Dodatne informacije o bilo kojeg elementa koji se ne odnose na ADFS čitati [implementacijom arhitekturu mreže sigurne hibridnog u Azure][implementing-a-secure-hybrid-network-architecture], [implementacijom arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], i [implementaciju arhitekturu sigurne hibridnog mreže s identiteti Azure Active Directory][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Ovaj dijagram prikazuje sljedeće slučajeve korištenja:
>
>- Aplikacija kod koji se izvodi unutar tvrtke ili ustanove partnera pristupa web-aplikacije nalazi unutar vaše VNet Azure.
>
>- Vanjski, registrirani korisnika (s vjerodajnice spremljene u toj ZBRAJA) pristupa web-aplikacije nalazi unutar vaše VNet Azure.
>
>- Korisnik povezati svoje VNet pomoću ovlašteni uređaja i pokretanjem nalazi unutar vaše Azure VNet web-aplikacije.
>
>Uz to, tu arhitekturu usredotočuje se na pasivni vanjski pristup, gdje se vanjskim poslužiteljima odlučite kako i kada za provjeru autentičnosti korisnika. Korisnik se očekuje možete unijeti podatke za prijavu prilikom pokretanja aplikacije sustava pokrenut. Ovo je mehanizam najčešće koriste web-preglednici i obuhvaća protokol koji preusmjerava web-pregledniku na web-mjesto koje korisnik može unijeti vjerodajnice. ADFS podržava i aktivni vanjski pristup što aplikaciju preuzima odgovornost za unošenju podataka za vjerodajnice bez daljnje interakcije s korisnikom, no taj slučaj je izvan opsega tu arhitekturu.

- **ZBRAJA podmreže.** ZBRAJA poslužitelje nalazi se unutar vlastite podmreže. Pravila NSG zaštititi poslužitelje ZBRAJA i pružaju Vatrozid za promet iz nepoznatih izvora.

- **DODAJE poslužiteljima.** To su kontrolera domena izvodi kao VMs u oblaku. Sljedećim poslužiteljima omogućuje provjeru autentičnosti lokalne identiteta unutar domene.

- **ADFS podmreže.** Poslužitelji ADFS može se nalaziti unutar vlastite podmreže s pravilima NSG ulozi vatrozid.

- **ADFS poslužitelji.** Poslužitelji za ADFS daju pridruženim autorizacije i provjeru autentičnosti. U toj arhitekturi mogu izvršiti sljedeće zadatke:

    - Mogu primati sigurnosnih tokena koje sadrže zahtjevima načinio partnera vanjskom poslužitelju ime korisnika partnera. ADFS možete tokena provjerite jesu li valjan prije prosljeđivanje zahtjevima za web-aplikacije koji se izvodi u Azure. Tvrtke web-aplikacije (u Azure) možete koristiti te zahtjevima da biste autorizirali zahtjeva. U ovom scenariju tvrtke web-aplikacija je *potrebe za oslanjanjem proizvođača*pa je odgovornosti vanjskom poslužitelju partnera za izdavanje zahtjeve koji su razumjele tvrtke web-aplikaciji. Vanjskim poslužiteljima partnera se nazivaju i *partnerima račun* jer slanja zahtjeva za pristup ime čija je autentičnost provjerena računi u tvrtki ili ustanovi partnera. ADFS poslužitelji se nazivaju *resursa partnere* jer omogućuju pristup resursima (web-aplikacije, u ovom slučaju).

    - Oni mogu provjere autentičnosti (putem [Servis Registracija uređaja Active Directory]i ZBRAJA[ADDRS]) i autorizirali zahtjevi za s vanjskim korisnicima koji se izvodi web-preglednik ili uređaj koji treba pristup tvrtke web-aplikacija. 

    ADFS poslužitelji konfigurirana kao farma, kojoj se pristupa putem Azure opterećenja. Da biste poboljšali dostupnosti i skalabilnost pomaže u ovu strukturu. Osim toga, imajte na umu da ADFS poslužitelji ne pojavljuje se izravno s Internetom, radije internetski promet filtriran putem ADFS web-aplikacije proxy poslužiteljima i na DMZ.

- **Podmreže proxy poslužitelj za ADFS.** Proxy poslužitelji ADFS se mogu nalaziti unutar vlastite podmreže NSG pravila koja omogućuje zaštitu. Poslužitelji u ovom podmreže su izložen putem Interneta kroz skup aparata virtualne mreže koji omogućuju vatrozid između Azure virtualne mreže i s Internetom.

- **ADFS web-aplikacije (WAP) proxy poslužiteljima.** Ta računala poslužiti kao ADFS poslužitelji za dolaznih zahtjeva iz tvrtke ili ustanove partnera i vanjskih uređaja. Poslužitelji WAP funkcioniraju kao filtar shielding poslužitelji ADFS iz izravan pristup s Interneta javno. Kao i s poslužitelja ADFS implementacija u WAP poslužiteljima u farmi s opterećenja vam veća dostupnost i skalabilnost od implementacija zbirke samostalni poslužitelja.

    >[AZURE.NOTE] Detaljne informacije o instaliranju WAP poslužiteljima potražite u članku [Instaliranje i konfiguriranje web-aplikacije Proxy poslužitelj][install_and_configure_the_web_application_proxy_server]

- **Partner tvrtke ili ustanove.** Ovo je primjer partnera organizacije, koji se izvodi web-aplikacije koje se zatraži pristup web-aplikaciju koja se izvodi u Azure. Vanjskom poslužitelju u tvrtki ili ustanovi partnera potvrđuje zahtjeva za lokalno i šalje sigurnosnih tokena koje sadrže zahtjevima za ADFS koji se izvodi u Azure. ADFS u Azure Provjeri valjanost sigurnosnih tokena, a ako su valjane možete proslijediti zahtjevima za web-aplikaciju u Azure da biste autorizirali ih.

    >[AZURE.NOTE] Možete konfigurirati i VPN tunelom pomoću pristupnika Azure pružanje izravan pristup za ADFS pouzdanih partnerima. Zahtjeva primljenih od ovih partnera se proći kroz WAP poslužitelja.

## <a name="recommendations"></a>Preporuke

U ovom se odjeljku navedene preporuke za implementaciju ADFS u Azure, prekrivajući:

- Preporuke za VM.

- Preporuke za povezivanje s mrežom.

- Dostupnost preporuke.

- Preporuke o sigurnosti.

- Preporuke za instalaciju ADFS.

- Preporuke za ADFS pouzdanim.

### <a name="vm-recommendations"></a>Preporuke VM

Stvorite VMs s dovoljno resursa za rukovanje očekivani količinu promet. Koristite veličinu postojeće strojeva hosting ADFS lokalno kao početnu točku. Praćenje Upotreba resursa. Možete promijeniti veličinu na VMs i skaliranje ako su prevelika.

Slijedite preporuke popisu [izvodi Windows VM na Azure][vm-recommendations].

### <a name="networking-recommendations"></a>Preporuke za povezivanje s mrežom

Konfiguriranje mrežno sučelje za svaku VMs hosting ADFS i WAP poslužitelji statičke IP adrese privatno.

Dajte ADFS VMs javnu IP adrese. Dodatne informacije potražite u članku [Sigurnosna pitanja vezana uz][security-considerations].

Postavljanje IP adresu DNS poslužitelja Preferirani i sekundarnih za sučelje mreže za svaku ADFS i WAP VM referentni VMs DODAJE (koji moraju biti pokrenut DNS-a). Ovaj korak nije potrebno da biste omogućili svaki VM da biste se uključili u domeni.

### <a name="availability-recommendations"></a>Dostupnost preporuke

Stvorite farma sustava ADFS s najmanje dva poslužitelja da biste povećali dostupnost usluge ADFS.

Koristite za pohranu za različite račune za svaki VM ADFS u farmi. Taj se način omogućuje da biste bili sigurni da pogreška u jednom prostora za pohranu računa ne i njegovo cijelu farmu ne može pristupiti.

Stvaranje skupova zasebnom Azure dostupnosti za ADFS i WAP VMs. Provjeriti postoje li barem dva VMs u svakom skupu. Svaki skup dostupnost morate imati barem dvije ažuriranje domene i dvije kvara domene.

Konfiguriranje balancers opterećenja za ADFS VMs i WAP VMs na sljedeći način:

- Korištenje Azure opterećenja radi olakšavanja vanjskog pristupa WAP VMs i interna opterećenja za raspodjelu opterećenja svim ADFS poslužiteljima u farmi ADFS.

- Samo prenesite promet na priključak 443 (HTTPS) ADFS-WAP poslužiteljima.

- Dajte raspoređivača opterećenja statičke IP adrese.

- Stvaranje stanja probni pomoću HTTPS protokola TCP, a ne. Možete pomoću naredbe ping priključak 443 da biste provjerili funkcionira li poslužitelj za ADFS.

    >[AZURE.NOTE] ADFS poslužitelji koristiti protokol oznaka poslužitelja naziva (SNI) tako da pokušaju isprobati pomoću krajnje HTTPS iz neuspjeha raspoređivača opterećenja.

- Dodavanje zapisa DNS- *A* domene za ADFS opterećenja. 

    Navedite IP adresu raspoređivača opterećenja i dajte mu naziv domene (primjerice adfs.contoso.com). To je naziv koji klijenata i poslužitelja WAP pristup ADFS poslužitelja.

### <a name="security-recommendations"></a>Preporuke o sigurnosti

Onemogućite Izravni izlaganje poslužitelja ADFS s Internetom. ADFS poslužitelji su domene pridruženo računala koje imaju potpunu autorizacije da biste dodijelili sigurnosnih tokena. Ako poslužitelj za ADFS ugrožena, zlonamjerni korisnik možete izdati tokeni puni pristup za sve web-aplikacije i poslužitelje koji su zaštićeni po ADFS. Ako sustav mora rukovati zahtjevima vanjski korisnici ne moraju nužno biti povezivanja s web-mjesta pouzdanih partnera, koristiti poslužitelje WAP rukovati tih zahtjeva. Dodatne informacije potražite u članku [gdje želite smjestiti vanjskim Proxy poslužiteljem][where-to-place-an-fs-proxy].

Mjesto ADFS i WAP poslužitelja u zasebnom podmreže vlastite vatrozidima. Pomoću pravila NSG da biste definirali pravila vatrozida. Ako tražite detaljnije zaštitu možete implementirati na opseg dodatne sigurnosti oko poslužitelje pomoću para podmreže i NVAs, kao što je opisano dokument [implementacijom arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]. Imajte na umu da sve vatrozide dopustite promet na priključak 443 (HTTPS).

Ograničiti pristup Izravni Prijava na poslužitelje za ADFS i WAP. Samo DevOps osoblje trebali biste moći povezati.

Ne uključuj poslužitelje WAP domene.

### <a name="adfs-installation-recommendations"></a>Preporuke za instalaciju ADFS

U članku [Implementacija poslužitelja za vanjski pristup] [ Deploying_a_federation_server_farm] sadrži detaljne upute za instaliranje i konfiguriranje ADFS. Prije konfiguriranja prvi poslužitelj za ADFS u farmi izvedite sljedeće zadatke:

1. Nabavite javnom pouzdanom certifikat za provjeru autentičnosti poslužitelja. *Predmetni naziv* mora sadržavati naziv koji klijenti pristup servisu vanjski pristup. To može biti naziv DNS-a registrirali za opterećenja, na primjer, *adfs.contoso.com* (izbjegavanje imena zamjenskih znakova kao što su **. contoso.com*, sigurnosnih vam razloga). Korištenje istog certifikata na sve VMs poslužitelj za ADFS. Možete kupiti iz pouzdana ustanova za izdavanje certifikata, no ako vaša tvrtka ili ustanova koristi Active Directory Services certifikata Stvorite vlastitu. 

    *Alternativni naziv predmeta* u DRS koristi da biste omogućili pristup s vanjskim uređaja. To mora biti obrazac *enterpriseregistration.contoso.com*.

    Dodatne informacije potražite u članku [Nabavljanje i konfiguriranje SSL certifikata za ADFS][adfs_certificates].

2. Na kontrolerom domene generirati novi ključ korijenski ključ servisa za raspodjelu. Postavite učinkovitih time se trenutno vrijeme minus 10 sati (tu konfiguraciju smanjuje odgode koji se pojavljuju u raspodjela i sinkroniziranje tipke preko domene). Ovaj korak nije potrebno podržava stvaranje računa servisa za grupu koji se koriste za pokretanje servisa ADFS. Sljedeću naredbu komponente Powershell prikazuje primjer kako to učiniti:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Svaki poslužitelj za ADFS VM dodati domenu.

>[AZURE.NOTE] Da biste instalirali ADFS, kontrolerom domene radi PDC emulator FSMO uloge domene mora biti pokrenut i može pristupiti iz ADFS VMs.

### <a name="adfs-trust-recommendations"></a>Preporuke za ADFS pouzdanosti

Uspostavljanje vanjski pristup pouzdanosti između ADFS instalacije i vanjske poslužitelje organizacije partnera. Konfiguriranje sve zahtjevima filtriranja i mapiranje obavezno. Ovaj postupak zahtijeva:

- DevOps drugog osoblja **u tvrtki ili ustanovi svaki partnera** da biste dodali relying pouzdanost proizvođača za web-aplikacije dostupne putem poslužitelja za ADFS.

- DevOps drugog osoblja **u tvrtki ili ustanovi** da biste konfigurirali zahtjevima davatelja Smatraj pouzdanim da biste omogućili poslužitelja ADFS pouzdanosti zahtjevima koje će vam pružiti partner tvrtke ili ustanove.

- DevOps drugog osoblja **u tvrtki ili ustanovi** da biste konfigurirali ADFS za prosljeđivanje pritužbe na vaše tvrtke ili ustanove web-aplikacije.

    Dodatne informacije potražite u članku [Uspostavljanje pouzdanost za vanjski pristup][establishing-federation-trust].

Objavljivanje web-aplikacije vaše tvrtke ili ustanove i učinite ih dostupnima za vanjski partneri pomoću prethodna kroz WAP poslužitelja. Dodatne informacije potražite u članku [Objavljivanje aplikacije pomoću ADFS Preauthentication][publish_applications_using_AD_FS_preauthentication]

Imajte na umu da ADFS podržava tokena transformacije i traži. Ta značajka ne nudi Azure Active Directory. Uz ADFS, prilikom postavljanja odnosima pouzdanosti možete učiniti sljedeće:

- Konfiguriranje pretvorbe zahtjeva za pravila za provjeru autentičnosti. Na primjer, možete mapirati sigurnosne grupe iz predstavljanje koristi tvrtka ili ustanova nije Microsoftova partnera za nešto što možete te ZBRAJA autorizirali u tvrtki ili ustanovi.

- Pretvaranje zahtjevima iz jednog oblika u drugi. Na primjer, možete mapirati iz SAML 2.0 SAML 1.1 ako aplikacija podržava samo SAML 1.1 zahtjevima. 

## <a name="availability-considerations"></a>Razmatranja dostupnosti

SQL Server ili Windows interne baze podataka (RID) možete koristiti za spremanje informacija o konfiguraciji ADFS. RID nudi osnovni zalihosti. Promjene se pisati izravno samo jedna baza podataka za ADFS u skupini ADFS dok još poslužitelji istaknuti replikacije zadržavanje pomoću svoje baze podataka u tijeku. Korištenje sustava SQL Server možete unijeti zalihosti cijelog baze podataka i visoke dostupnosti pomoću prebacivanje Klasteriranje ili zrcaljenje.

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

ADFS koristi HTTPS protokola, pa provjerite da NSG pravila za podmreže koja sadrži web razina VMs dopustite HTTPS zahtjeva. Te zahtjeve možete potječu iz lokalne mreže podmreže koja sadrži web razina, sloju tvrtke, razina podataka, privatni DMZ, javno DMZ i podmreže koja sadrži ADFS poslužiteljima.

Razmislite o korištenju postavljanje mreže virtualne aparata zapisnike detaljne informacije o promet traversing ruba virtualne mreže za nadzor svrhe.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Imajte na umu sljedeće, sažeta iz dokumenta [planirate implementaciju sustava ADFS][plan-your-adfs-deployment], dati početnu točku za promjenu veličine ADFS farme:

- Ako imate manje od 1000 korisnika, stvoriti poseban poslužitelj za ADFS, ali umjesto toga instalirati ADFS na svim poslužiteljima ZBRAJA u oblak (Provjerite jeste li povezani s najmanje dva poslužitelja ZBRAJA, da biste zadržali dostupnost). Stvorite jedan WAP poslužitelj.

- Ako imate između 1000 i 15000 korisnika, stvorite dva namjenski ADFS i dva namjenski WAP poslužitelja.

- Ako imate između 15000 i 60000 korisnika, stvorite između tri i pet namjenski ADFS, i najmanje dva namjenski WAP poslužitelja.

Te iznose pretpostavlja da koristite dva VMs quad core (standardni D4_v2 ili bolje) za hostiranje poslužitelji Azure.

Imajte na umu da ako koristite Windows Internal Database radi pohrane podataka konfiguracije ADFS ste ograničeni na osam ADFS poslužiteljima u farmi. Ako predviđate koje je potrebno više pomoću SQL Server. Dodatne informacije potražite u članku [U ulogu bazi podataka za konfiguraciju ADFS][adfs-configuration-database].

## <a name="management-considerations"></a>Upravljanje značajkama

Osoblje DevOps moraju se pripremite za izvođenje sljedećih zadataka:

- Upravljanje vanjskim poslužiteljima (Upravljanje farme ADFS, upravljanje pravilnik o pouzdanosti na poslužiteljima vanjski pristup i upravljanje certifikate koji se koriste u vanjski pristup servisima).

- Upravljanje WAP poslužitelja (Upravljanje farme WAP, upravljanje certifikatima WAP).

- Upravljanje web-aplikacijama (Konfiguriranje potrebe za oslanjanjem stranama, metoda provjere autentičnosti i zahtjevima mapiranja).

- Sigurnosno kopiranje ADFS komponente.

## <a name="monitoring-considerations"></a>Praćenje pitanja vezana uz

[Paket za upravljanje Microsoft sustava centra za Active Directory Federation servisa 2012 R2] [ oms-adfs-pack] sadrži određene proaktivne i ponovno aktiviranje nadzor uvođenja ADFS za vanjskom poslužitelju. Paket za upravljanje nadzire:

- Događaji u ADFS servis je zapisa u zapisnicima događaja ADFS.

- Performanse podaci koji se prikupljanje mjerača performansi ADFS. 

- Stanje ADFS sustav i web-aplikacije (potrebe za oslanjanjem stranama,) i daje upozorenja za kritičnih problema i upozorenja.

## <a name="solution-components"></a>Komponente rješenja

Skripta za rješenja uzorka, [Uvođenje ReferenceArchitecture.ps1][solution-script], dostupna da možete koristiti implementirati arhitektura koji se nalazi iza preporuke opisane u ovom članku. Ova skripta koristi Voditelj resursa Azure predložaka. Predlošci su dostupni kao skup temeljne sastavne blokove, od kojih svaka izvodi određenu akciju kao što je VNet stvaranju ili konfiguriranju programa NSG. Svrha skriptu je orkestrirali uvođenje predloška.

Predlošci su parametrizirane, s parametrima sadrži zasebne datoteke JSON Možete izmijeniti parametara u te datoteke da biste konfigurirali implementacije u skladu s vlastitim potrebama. Nije potrebno izmijeniti predložaka sami. Imajte na umu da ne možete promijeniti sheme objekata u datotekama parametar.

Prilikom uređivanja predložaka, stvaranje objekata koji slijede konvencije imenovanja opisane u [Preporučuje konvencije imenovanja za resurse Azure][naming-conventions].

Uzorak rješenja stvara i konfigurira okruženje u oblaku comprising ZBRAJA podmreže i poslužiteljima ADFS podmreže i poslužitelje, podmreže proxy poslužitelj za ADFS i poslužitelji DMZ, web razina, sloju tvrtke i podataka programa access sloju komponente, VPN pristupnika i upravljanje sloju. Ogledno rješenje obuhvaća i neobavezna konfiguracije za stvaranje Simulirani lokalnog okruženja.

U sljedećim se odjeljcima opisuju elemenata na lokalni i konfiguracija u oblaku.

### <a name="on-premises-components"></a>Lokalni komponente

>[AZURE.NOTE] Te komponente nisu glavni fokusa arhitektura opisani u ovom dokumentu i dostupna je samo da bi se dobilo priliku da biste testirali okruženje oblaka sigurno, umjesto stvarnih radnog okruženja. U ovom se odjeljku zbog toga samo navedene ključne parametar datoteke. Promijenite postavke kao što je IP adresa ili veličine na VMs, ali preporučuje se da biste napustili mnoge druge parametara nepromijenjena.

U ovom okruženje sastoji se od skupa stabala u AD za domenu pod nazivom contoso.com. Domene sadrži dva poslužitelja ZBRAJA s IP adresama 192.168.0.4 i 192.168.0.5. Ove dva poslužitelja i pokrenuti DNS servis. Lokalni administratorski račun na oba VMs zove `testuser` lozinkom `AweS0me@PW`. Uz to, konfiguraciju postavlja pristupnik VPN-a za povezivanje s VNet u oblaku. Konfiguracija možete izmijeniti tako da uredite sljedeće JSON datoteke koja se nalazi u [**parametara/lokalnu verziju**] [ on-premises-folder] mape:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Tu datoteku definira prostor adrese mreže za lokalnog okruženja.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Datoteka sadrži konfiguraciju za lokalni VMs ZBRAJA usluge hostiranja. Po zadanom se stvaraju dva *Standardna DS3 v2* VMs.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** i ** [connection.parameters.json][on-premises-connection-parameters]**. Te se datoteke držite postavke za VPN vezu s pristupnikom Azure VPN-a u oblaku, uključujući zajednički ključ koja će se koristiti za zaštitu promet traversing pristupnik.

Preostali datoteke u mapi sadrže informacije o konfiguraciji koristi za stvaranje nad lokalnom domenom pomoću ovog infrastrukture. Pomoću njih možete instalirati ZBRAJA postavljanje DNS-a, stvaranje skupa na stabala i konfiguriranje web-mjesta replikacije za u skup stabala.

### <a name="cloud-components"></a>Oblak komponente

Te komponente obrasca core tu arhitekturu. [**Parametri/azure**] [ azure-folder] mapa sadrži sljedeće datoteke parametar za konfiguriranje sljedeće komponente:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Tu datoteku definira strukturu na VNet na VMs i druge komponente u oblaku. Uključuje postavke, kao što su ime, adresu prostor, podmreže i adrese neki DNS poslužitelji obavezno. Imajte na umu adrese DNS prikazano u ovom primjeru sadrže referencu IP adrese lokalni DNS poslužitelji i zadani Azure DNS poslužitelj. Izmjena te adrese referentni postavki DNS-a ako ne koristite lokalnog okruženja uzorak:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Datoteka konfigurira VMs radi ZBRAJA u oblak. Konfiguracija sastoji se od dva VMs. Promijenite administratorskog korisničkog imena i lozinke u na `virtualMachineSettings` odjeljak, a po želji možete promijeniti veličinu VM u skladu s potrebama domene:

    Dodatne informacije potražite u članku [Proširivanje servisa Active Directory za Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [Dodavanje-dodaje-domene-controller.parameters.json][add-adds-domain-controller-parameters]**. Datoteka sadrži postavke za stvaranje CONTOSO domene koje se protežu na poslužiteljima ZBRAJA. Koristi prilagođeni proširenja uspostaviti domene i dodavanje poslužitelje ZBRAJA. Dok ne stvorite Dodatne poslužitelje ZBRAJA (u tom slučaju morate ih dodati u na `vms` polja), mijenjati njihova imena zadanu ili želite stvoriti domene pod drugim nazivom nije potrebno da biste izmijenili tu datoteku.

- ** [loadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Datoteka sadrži dva skupa parametara konfiguracije. Na `virtualMachineSettings` odjeljak definira VMs koji hostira ADFS servis u oblaku. Prema zadanim postavkama skripta stvara dvije od tih VMs unutar istog dostupnosti skupa:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    Na `loadBalancerSettings` odjeljak pruža opis raspoređivača opterećenja o tim VMs. Raspoređivača opterećenja prosljeđuje prometa koji se pojavljuje na priključak 443 (HTTPS) jedan ili nekog drugog programa u VMs:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [adfs-farme-domene-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Datoteka sadrži postavke koje se koriste za dodavanje poslužitelje ADFS domene tvrtke CONTOSO. Samo ćete morati izmijeniti ove datoteke ako ste stvorili dodatne poslužitelje ADFS (ažuriranje u `vms` polja u ovom slučaju), ili su promijenjene naziv domene.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs farme first.parameters.json][adfs-farm-first-parameters]**, a ** [adfs farme rest.parameters.json][adfs-farm-rest-parameters]**. Skripta za stvaranje farmi poslužitelja ADFS koristi postavke u te datoteke. 

    *Gmsa.parameters.json* datoteka sadrži postavke za račun servisa za grupu upravlja servis za ADFS koristi. Ako želite promijeniti naziv računa ili domene možete izmijeniti tu datoteku.

    *Adfs farme first.parameters.json* datoteka sadrži podatke koji su potrebni za stvaranje farmi poslužitelja za ADFS i dodavanje prvi poslužitelj. Ako ste promijenili domene ili naziv računa servisa za grupu upravlja, trebali biste ažurirati tu datoteku.

    Da biste dodali preostale ADFS poslužitelje na farmi poslužitelja se koristi datoteka *adfs farme rest.parameters.json* . Ponovno, ako ste promijenili domene ili naziv računa servisa za grupu upravlja, trebali biste ažurirati tu datoteku. Ažuriranje na `vms` polja ako ste stvorili dodatne ADFS poslužiteljima.

- ** [loadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Datoteka je slična strukture i sadržaja *loadBalancer adfs.parameters.json* datoteku. S podacima za stvaranje ADFS proxy poslužitelji i opterećenja.

- ** [adfsproxy farme first.parameters.json][adfsproxy-farm-first-parameters]**, a ** [adfsproxy farme rest.parameters.json][adfsproxy-farm-rest-parameters]**. Skripta koristi postavke u te datoteke da biste stvorili ADFS proxy poslužitelja. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Datoteka sadrži postavke koje se koriste za stvaranje pristupnika Azure VPN-a u oblaku koja se koristi za povezivanje s lokalne mreže. Izmijenite u `sharedKey` vrijednosti u na `connectionsSettings` sekciju u skladu lokalnog uređaja za VPN-a. Dodatne informacije potražite u članku [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** i ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Te se datoteke konfiguriranje ulaznog (javnih) i izlazni (privatna) strane VMs koje čine DMZ, zaštita poslužitelja u oblak. Dodatne informacije o tih elemenata i njihove konfiguracije potražite u članku [implementacijom DMZ između Azure i Interneta][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters json][loadBalancer-biz-parameters]**, a ** [loadBalancer data.parameters json][loadBalancer-data-parameters]**. Te datoteke parametara sadrže specifikacije VM za pristup razine web, tvrtke i podataka i konfigurirajte učitavanja balancers za svaki sloju. To su VMs glavno računalo web-aplikacije i baza podataka, a izvođenje radnih opterećenja business za tvrtku ili ustanovu. Veličina i broj VMs u svakom sloju možete izmijeniti u skladu s potrebama.

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Datoteka sadrži konfiguraciju za skočni okvir/upravljanje VMs. Moguće je samo da bi se dobio prijave i administrativni pristup VMs web, tvrtke i razine podataka iz okvira Skok. Prema zadanim postavkama, skripta stvara jednu *Standard_DS1_v2* VM, ali možete izmijeniti ove datoteke da biste stvorili veće ili dodatne VMs ako je radno opterećenje upravljanje vjerojatno će biti vrlo.

## <a name="solution-deployment"></a>Uvođenje rješenja

Rješenje podrazumijeva sljedeće preduvjete:

- Imate postojeću pretplatu Azure u kojem možete stvoriti grupe resursa.

- Ste preuzeli i instalirali najnoviju verziju programa Azure Powershell. U odjeljku [ovdje] [ azure-powershell-download] upute.

Da biste pokrenuli skriptu koja uvodi rješenja:

1. Premještanje praktičan mapu na lokalnom računalu i stvorite sljedeće podmape:

    - Skripte

    - Skripte/parametara

    - Skripte/parametara/lokalnu verziju

    - Parametri/skripte/Azure

2. Preuzimanje [Uvođenja ReferenceArchitecture.ps1] [ solution-script] datoteku u mapu skripti

3. Preuzimanje sadržaja [parametara/lokalnu verziju] [ on-premises-folder] mape u mapu skripte/parametara/lokalnu verziju:

4. Preuzimanje sadržaja [parametara/azure] [ azure-folder] mape u mapu parametara/skripte/Azure.

5. Uređivanje uvođenja ReferenceArchitecture.ps1 datoteke u mapi skripte i promijenite sljedeće retke za određivanje grupa resursa koje treba stvorili ili za držite resursi stvorio skripte:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Uređivanje datoteke parametar u skripti/parametara/lokalnu verziju i skripte/parametara/Azure mape. Ažurirajte reference grupu resursa u te datoteke tako da odgovara nazive grupa resursa dodijeljene varijable u datoteci uvođenja ReferenceArchitecture.ps1. Sljedeća tablica pokazuje koje datoteke parametar referencu koju grupu resursa. Imajte na umu grupe *ra-adfs-radno opterećenje-ru*, *ra-adfs-sigurnost-ru*, *ra-adfs-dodaje-ru*, *ra-adfs-adfs-ru*i *ra-adfs-proxy-ru* koriste se samo u skriptu PowerShell, a ne pojavljuju se u parametru datoteke.

  	|Grupa resursa|Parametar datoteke|
  	|--------------|--------------|
  	|Ra-adfs-lokalnu verziju-ru|parameters\onpremise\connection.PARAMETERS.JSON<br /> parameters\onpremise\virtualMachines-adds.PARAMETERS.JSON<br />parameters\onpremise\virtualNetwork-adds-DNS.PARAMETERS.JSON<br />parameters\onpremise\virtualNetwork.PARAMETERS.JSON<br />parameters\onpremise\virtualNetworkGateway.PARAMETERS.JSON<br />parameters\azure\virtualNetworkGateway.PARAMETERS.JSON
  	|Ra-adfs-mreže-ru|parameters\onpremise\connection.PARAMETERS.JSON<br />parameters\azure\dmz-Private.PARAMETERS.JSON<br />parameters\azure\dmz-public.PARAMETERS.JSON<br />parameters\azure\loadBalancer-ADFS.PARAMETERS.JSON<br />parameters\azure\loadBalancer-adfsproxy.PARAMETERS.JSON<br />parameters\azure\loadBalancer-Biz.PARAMETERS.JSON<br />parameters\azure\loadBalancer-Data.PARAMETERS.JSON<br />parameters\azure\loadBalancer-web.PARAMETERS.JSON<br />parameters\azure\virtualMachines-adds.PARAMETERS.JSON<br />parameters\azure\virtualMachines-mgmt.PARAMETERS.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.PARAMETERS.JSON<br />parameters\azure\virtualNetwork.PARAMETERS.JSON<br />parameters\azure\virtualNetworkGateway.PARAMETERS.JSON (*dvije pojave*)

    Uz to, postavljanje konfiguracije za na lokalni i cloud komponente, kao što je opisano u odjeljku [komponente rješenja] [komponente rješenja].

7. Otvorite prozor programa Azure PowerShell, premjestite u mapu skripte i pokrenite sljedeću naredbu:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Zamjena `<subscription id>` koristeći Azure pretplate ID.

    Za `<location>`, navedite Azure regiju, kao što su `eastus` ili `westus`.

    Na `<mode>` parametar možete koristiti neku od sljedećih vrijednosti:

    - `Onpremise`, radi stvaranja Simulirani lokalnog okruženja.

    - `Infrastructure`, da biste stvorili infrastrukture VNet i skočni okvir u oblaku.

    - `CreateVpn`, omogućuje stvaranje pristupnika Azure virtualne mreže i povežite ga s lokalne mreže.

    - `AzureADDS`, za sastavljanje VMs ulozi ZBRAJA poslužitelje Implementacija servisa Active Directory te VMs i stvaranje domene u oblaku.

    - `AdfsVm`Da biste stvorili ADFS VMs i pridružiti domene u oblaku.

    - `ProxyVm`omogućuje stvaranje proxy poslužitelj za ADFS VMs i pridružiti domene u oblaku.

    - `Prepare`, koji se izvodi iznad zadatke. **To je preporučena mogućnost ako su stvaranje posve novog implementacije i nemate postojeću infrastrukturu za lokalni.**

    >[AZURE.NOTE] Možete i pokrenuti skriptu s na `<mode>` parametar `Workload` da biste stvorili web tvrtke i podataka sloju VMs i mreže. Ovaj će instalacijski program nije dio na `Prepare` načinu rada.

    Ako koristite na `Prepare` mogućnost skriptu traje nekoliko sati, a ne završi s porukom *Priprema Završi. Instalirajte certifikata na sve ADFS i proxy VMs.*

8.  Ponovno pokrenite skočni okvir (*ra-adfs-upravljanje dokumentima-vm1* u grupi *ra-adfs-sigurnost-ru* ) da biste omogućili postavke DNS-a na snagu.

9.  [Nabavite SSL certifikat za ADFS] [ adfs_certificates] i instalacija ovog certifikata na ADFS VMs. Imajte na umu da se mogu povezati s VMs ADFS putem okvira Skok. IP adrese su *10.0.5.4* i *10.0.5.5*. Korisničko ime zadani je *contoso\testuser* lozinkom *AweSome@PW*.

    >[AZURE.NOTE] Komentari u skripti uvođenja ReferenceArchitecture.ps1 sada pružaju detaljne upute za stvaranje samopotpisane testni certifikat i izdavanje pomoću na `makecert` naredbe. Međutim, koristite potvrde generira makecert u radnom okruženju.

10. Pokrenite sljedeću naredbu komponente Powershell za konfiguraciju farme poslužitelja ADFS:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. U okviru Skok pronađite *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* testirajte instalaciju ADFS (možda ćete primiti upozorenje o certifikatu, koje možete zanemariti za ovaj test). Stranica za prijavu u Tvrtka Contoso provjerite prikazuje li se. Prijavite se kao *contoso\testuser* lozinkom *AweS0me@PW*.

12. Instalirati SSL certifikat na proxy poslužitelj za ADFS VMs. IP adrese su *10.0.6.4* i *10.0.6.5*.

13. Pokrenite sljedeću naredbu komponente Powershell da biste konfigurirali prvi proxy poslužitelj za ADFS:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Slijedite upute prikazana skripta za testiranje instalacije prvi proxy poslužitelj za ADFS.

15. Pokrenite sljedeću naredbu komponente Powershell da biste konfigurirali drugi prvi ADFS proxy poslužitelja:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Slijedite upute prikazuje skriptu da biste testirali dovršili konfiguriranje proxy ADFS.

## <a name="next-steps"></a>Daljnji koraci

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Sigurne hibridnog arhitektura mreže s servisom Active Directory"
