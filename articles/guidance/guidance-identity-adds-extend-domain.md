<properties
   pageTitle="Referenca Azure arhitektura - IaaS: Proširivanje servisa Active Directory za Azure | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurne hibridnog mreže s autorizacije servisa Active Directory u Azure."
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
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Proširivanje usluge Active Directory (ZBRAJA) za Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za proširivanje okruženje za Active Directory (AD) za Azure za pružanje usluga raspodijeljeno provjere autentičnosti putem [Servisa Active Directory Domain Services (AD DS)][active-directory-domain-services]. Tu arhitekturu proširuje opisani u člancima [implementacijom arhitekturu mreže sigurne hibridnog u Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementaciju arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. Ovu referenca arhitektura koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

AD DS koju koristite za provjeru identiteta. Ove identiteta pripadati korisnika, računala, aplikacije i ostali resursi taj obrazac dio sigurnosne domene. Možete hostirati u AD DS lokalni, ali u scenariju hibridnog gdje se nalaze elementi aplikacije u Azure može biti učinkovitiji za replikaciju ta je funkcija i spremištu servisa Active Directory u oblak. Takvog smanjiti Latencija uzrokovanih slanja provjeru autentičnosti i zahtjevi za lokalni autorizacije iz oblaka natrag u AD DS radi lokalnog. 

Za hostiranje imeničkim servisima u Azure na dva načina:

1. Možete koristiti [Azure Active Directory (AAD)] [ azure-active-directory] da biste stvorili novu domenu AD u oblak i povezati ga s lokalnog servisa Active Directory domene. Postavljanje [Azure AD Connect] [ azure-ad-connect] na-promises za replikaciju identiteta sadrži u spremištu lokalne u oblak. Imajte na umu da je direktorij u oblaku **ne** datotečni nastavak lokalnog sustava, umjesto toga je kopija koja sadrži iste identiteta. Promjene ove identiteta lokalne kopirat će se u oblak, ali promjene izvršene u oblak **neće** je replicirati natrag na lokalne domene. Ako, na primjer, lozinki mora biti izvesti lokalnog i koristiti Azure AD Connect da biste kopirali promjena u oblak. Osim toga, imajte na umu da se istoj instanci AAD može povezati s više instanci AD DS; AAD će sadržavati identiteta svaki spremištu AD na koji je povezan.

    AAD je korisno za situacije u kojima lokalne mreže i Azure virtualne mreže hostiranje u oblaku resursima nisu izravno povezani pomoću tunelom VPN-a ili ExpressRoute sklopovske. Iako je rješenje jednostavne, možda nije prikladna za sustave gdje komponente migrirali preko granicu na – lokalno/oblaku kao što je to može biti obavezno ponovno konfiguriranje od AAD. Osim toga, AAD samo rukuje provjere autentičnosti korisnika umjesto provjere autentičnosti za računala. Neka aplikacija i servisa, kao što je SQL Server možda zahtijeva provjeru autentičnosti za računalo u tom slučaju rješenje nije odgovarajuća.

2. Možete implementirati VM izvodi AD DS kao kontroler domene u Azure, proširivanje postojeću infrastrukturu AD iz vaše lokalne podatkovnog centra. Taj se način je uobičajene scenarije gdje povezani lokalne mreže i Azure virtualne mreže putem VPN-a i/ili ExpressRoute veze. Rješenje podržava i dvosmjerni replikacije omogućujući vam promjene u oblak i informacije o lokalnom gdje god je najprikladnije. Ovisno o sigurnosne preduvjete instalacije AD u oblaku mogu biti:
    - dio istu domenu kao taj čuvanju lokalnog
    - novu domenu unutar zajednički skup stabala
    - zasebnom skupa stabala

<!-- we might want to add info on how to choose from the three options above -->

U ovom arhitektura usredotočuje se na rješenje 2, koristeći istu domenu AD DS Azure i lokalnih.

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Hibridno aplikacije mjesto cilja radnih opterećenja djelomično lokalnog i djelomično u Azure.

- Aplikacija i servisa koji obavljaju provjera autentičnosti pomoću servisa Active Directory.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističe važne komponente u toj arhitekturi (*kliknite da biste povećali*). Dodatne informacije o elementima zasivljen izlaz čitati [implementacijom arhitekturu mreže sigurne hibridnog u Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementaciju arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Lokalne mreže.** Lokalne mreže obuhvaća lokalne poslužitelje AD koje možete izvršiti provjeru autentičnosti i ovlaštenja komponente koje se nalaze informacije o lokalnom.

- **AD poslužitelji.** To su kontrolera domena implementacijom directory services (AD DS) izvodi kao VMs u oblaku. Sljedećim poslužiteljima omogućuje provjeru autentičnosti komponenti koji se izvodi u Azure virtualne mreže.

- **Active Directory podmreže.** Poslužitelji AD DS nalaze se u zasebnom podmreže. Pravila NSG zaštita poslužitelja za AD DS i pružaju Vatrozid za promet iz nepoznatih izvora.

- **Sinkronizacija azure pristupnika i AD.**. Pristupnik za Azure nudi veze između lokalne mreže i Azure VNet. To može biti [VPN vezu] [ azure-vpn-gateway] ili [Azure ExpressRoute][azure-expressroute]. Sve zahtjeve za sinkronizaciju između AD poslužitelja u oblak i lokalne proći kroz pristupnika. Korisnički definirane usmjerava (UDRs) ručicu za usmjeravanje za sinkronizaciju promet koji prosljeđuje izravno AD poslužitelja u oblak i proći kroz na postojeće mreže virtualne aparata (NVAs) u ovom scenariju.

    Dodatne informacije o konfiguriranju UDRs i u NVAs potražite u članku [implementacijom arhitekturu mreže sigurne hibridnog u Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Preporuke

U ovom se odjeljku navedene preporuke za implementaciju AD DS u Azure, koji prekriva:

- Preporuke za VM.

- Preporuke za povezivanje s mrežom.

- Preporuke o sigurnosti. 

- Preporuke Active Directory web-mjesta.

- Active Directory FSMO položaj preporuke.

>[AZURE.NOTE] Dokument [smjernice za implementaciju sustava Windows Server Active Directory virtualnim računalima sustava na Azure] [ ad-azure-guidelines] sadrži detaljnije informacije o mnoge od tih točaka.

### <a name="vm-recommendations"></a>Preporuke VM

Stvorite VMs s dovoljno resursa za rukovanje očekivani količinu promet. Koristite veličina strojeva hosting AD DS lokalno kao početnu točku. Praćenje Upotreba resursa; možete promijeniti veličinu na VMs i skaliranje ako su prevelika. Dodatne informacije o AD DS kontrolera domena za promjenu veličine potražite u članku [Planiranje kapaciteta za Active Directory Domain Services][capacity-planning-for-adds].

Stvaranje diska zasebne virtualne podatke za spremanje baze podataka, zapisnika i SYSVOL za AD. Spremanje stavke na istom disku kao operacijski sustav. Imajte na umu da po zadanom diskova podataka koje su priložene na VM koristiti putem pisanja predmemoriju. Međutim, ovaj oblik predmemorije može biti u sukobu s potrebama AD DS. Zbog toga postavite postavka *Preferirani predmemorije glavnog računala* na disku podataka na *ništa*. Dodatne informacije potražite u članku [položaj Windows Server AD DS baze podataka i SYSVOL][adds-data-disks].

Implementacija najmanje dva VMs pokretanje AD DS kao kontrolera domena Azure virtualne mreže [dostupnost](#Availability-considerations) razloga.

### <a name="networking-recommendations"></a>Preporuke za povezivanje s mrežom

Konfiguriranje mrežno sučelje za svaku VMs hosting AD DS pomoću statičke IP adrese privatno. Tu konfiguraciju bolje podržava DNS-a na svim AD VMs. Dodatne informacije potražite [u]članku postavljanje statičke privatne IP adrese na portalu za Azure[set-a-static-ip-address].

> [AZURE.NOTE] Dajte AD DS VMs javnu IP adrese. U odjeljku [Sigurnosna pitanja vezana uz] [ security-considerations] više pojedinosti.

Morate biti sigurni da promet možete tijeka slobodno između AD poslužitelja u oblak i lokalne:

- Dodati pravila za NSG podmreže AD koje dopušta promet lokalnim. Detaljne informacije o priključke koje koriste AD DS potražite u članku [servisa Active Directory i Active Directory domene servisa priključke][ad-ds-ports].

- Provjerite je li tablica UDR usmjeravanje prometa NVAs koji se koristi u ovom scenariju AD DS. Za vlastite implementacije, ovisno o tome što NVAs koristite, želite preusmjeriti te promet.

### <a name="security-recommendations"></a>Preporuke o sigurnosti

AD DS poslužitelje rukovati provjeru autentičnosti i zbog toga su vrlo osjetljive stavke. Onemogućite Izravni izlaganje poslužitelja za AD DS s Internetom. Postavite AD DS poslužitelji u zasebnom podmreže, s vlastitom vatrozid. Provjerite je li potrebno koristiti AD DS za provjeru autentičnosti i autorizacije i poslužitelji synchronzing priključke ostati otvoren, ali zatvorite sve druge priključke. Dodatne informacije potražite u članku [Active Directory i Active Directory domene servisa priključke][ad-ds-ports]. [Komponente rješenja] [ solution-components] odjeljak u nastavku ovog dokumenta prikazuje se NSG pravila koje koristi uzorak rješenja da biste otvorili priključke.

Pomoću pravila NSG da biste stvorili jednostavan vatrozid. Ako tražite detaljnije zaštitu možete implementirati na opseg dodatne sigurnosti oko poslužitelje pomoću para podmreže i NVAs, kao što je opisano dokument [implementacijom arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

Šifriranje disk koji se nalaze u AD DS bazu podataka pomoću BitLocker ili Azure šifriranje.

### <a name="active-directory-site-recommendations"></a>Preporuke Active Directory web-mjesta

Možete koristiti web-mjesta u AD DS za grupu zajedno domene kontrolera koji su povezani brzo veza. Kontrolera domena na istom mjestu AD DS replicirati njihove promjene direktorija automatski, a malo konfiguracija je potrebno učiniti s replikacijom.

Da BISTE odredili promet replikacije između Azure i vaše lokalne podatkovnog centra, preporučuje se da biste dodali zasebnom mjesta AD DS predstavlja prostor adresa koji se koristi Azure. Možete konfigurirati web-mjesta vezu između lokalnih AD DS web-mjesta i učinkovitije upravljanje replikacije web-mjesta.

Da biste primijenili drugi objekti pravilnika grupe (GPO) da biste se pridružili računala u Azure, a da biste iskoristili aplikacije koje su "web-mjesta umu", kao što je Upravitelj konfiguracije centar sustava možete koristiti i odvojenosti web-mjesta.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO položaj preporuke

Prilagodljivo jedne osnovne operacije (FSMO) poslužitelji su kontrolera specijalizirane domena, reposnsible dosljednost podataka preko različite postavke u AD DS navedena u nastavku.

- **Matrica shemu**. To je skup stabala razini uloga koja održava strukturu sheme koristi AD DS.

- **Imenovanje matrica domena**. To je skup stabala razini uloga koja upravlja informacije o nazivima domena unutar u skup stabala.

- **Primarni kontroler domene (PDC)**. Ovo je domena razini uloga. PDC-om upravlja lozinku ažuriranja i lockouts računa. To je konzultirali tako da druge skupa kada servisne zahtjeve za provjeru autentičnosti lozinke nepodudaran. PDC-om upravlja pravilnik grupe ažuriranja, i cilj Kontroler za naslijeđene aplikacije i neke administrator alate pomoću kojih se izvršiti neke snimanje operacije.

- **Matrica relativni ID (RID)**. RIDs koriste se za pomoć identificirati samo objekte unutar direktorij. Ovaj poslužitelj je zadužen za generiranje domene razini skupa RIDs za korištenje po poslužiteljima AD unutar domene.

- **Infrastruktura uloge**. Objekt u jednu domenu možete referencirati objekta u drugu domenu. Ta uloga domene razini je odgovoran za ažuriranje SID i Razlikovni naziv u referenci objekta domenama na neki objekt. Imajte na umu da je poslužitelj implementacijom ta uloga i ne može poslužiti kao globalni katalog (GC) poslužitelja.

Dodatne informacije potražite u članku [Active Directory FSMO uloge u sustavu Windows][AD-FSMO-roles-in-windows].

U ovom scenariju preporučujemo izbjegli uvođenja FSMO uloge kontrolera domena u Azure. 

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Stvaranje raspoloživost postaviti za AD poslužitelje. Provjeriti postoje li barem dva poslužitelja u skupu. AD poslužitelja u oblak mora biti kontrolera domena unutar iste domene. To će omogućiti automatsko replikacije između poslužitelja.

Uzmite u obzir određivanje poslužitelje kao [čekanja operacije matrice] [ standby-operations-masters] u slučaju prekida se veza s poslužiteljem ulozi FSMO uloge.

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Ako su sve administratorske zadatke domene vjerojatno izvršiti pomoću skupa lokalnog, razmotrite mogućnost prevođenja u oblaku samo za čitanje. Samo za čitanje Kontroler samo održava podskup vjerodajnice korisnika (dovoljno da biste izvršili provjeru autentičnosti lokalno), a mogu konfigurirati predmemorije informacije samo za određene korisnike. Stoga samo za čitanje Kontroler ugrožena, samo informacije za korisnike predmemorirane utjecat će, umjesto detalje o svakoj račun na domeni. Dodatne informacije potražite u članku [Kontrolera domene samo za čitanje][read-only-dc].

Da biste smanjili slabe pojedinačne korisničke račune i uskratiti pokušaji break-in, slijedite Najbolji postupci za postavljanje i održavanje korisničke lozinke u AD. Dodatne informacije potražite u članku [Najbolje prakse za nametanje lozinki][best_practices_ad_password_policy]. Osim toga, pripazite da kojih ste grupa za dodjelu korisnika. Na primjer, provjerite svakodnevne korisnici članovi grupe Administratori tvrtke, grupi sheme administratori i grupe Domain Admins.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

AD je automatski podesiva za kontrolera domena koje su dio iste domene. Zahtjevi za raspodijeliti su sve kontrolera unutar domene. Možete dodati druge kontroler domene pa ga automatski će se sinkronizirati s domenom. Konfiguriranje zasebnom opterećenja da biste usmjerili promet kontrolera unutar domene. Osiguravanje svih kontrolera dovoljno memorije i resursa za pohranu za rukovanje domene baze podataka. najbolje Učini sve kontroler domene VMs iste veličine.

## <a name="management-considerations"></a>Upravljanje značajkama

Kopiranje datoteke VHD kontrolera domene umjesto izvođenje redovito sigurnosno kopiranje jer vraćanju može uzrokovati nedosljednosti u stanju između kontrolera domena.

Isključivanje i ponovno pokretanje VM koji se izvodi ulogu kontroler domene u Azure unutar operacijski sustav goste umjesto korištenja mogućnosti Isključi na portalu za Azure. Pomoću portala za Azure isključiti na VM uzrokuje VM da biste se deallocated. Ova akcija vraća VM GenerationID koji je neželjenog za kontroler domene. Kada je ponovno VM GenerationID, invocationID u spremištu servisa Active Directory i ponovno postavljanje, skup RID se odbacuju i SYSVOL označen kao koje nisu mjerodavne.

## <a name="monitoring-considerations"></a>Praćenje pitanja vezana uz

Nemogućnost nadzora i održavanja mreže AD poslužitelja može uzrokovati probleme kao što su:

- **Neuspjela prijava**. Neuspjela se može dogoditi cijeloj domene ili skup stabala ne uspijete pouzdanost razlučivosti odnos ili naziv ili globalni katalog server ne može odrediti univerzalno članstvo u grupi.

- **Lockout računa**. Račune korisnika i servisa može postati zaključan ako je primarni kontroler domene dostupan ili replikacije ne uspije između nekoliko kontrolera domena.

- **Pogreška kontroler domene**. Ako je pogon datotekom Ntds.dit ponestane prostora na disku, kontrolerom domene možete prestati raditi.

- **Pogreška programa**. Aplikacijama koje su ključna za svoje tvrtke, kao što je Microsoft Exchange ili neku drugu aplikaciju za e-pošte, možete uspjeti ako adresu adresara upita u direktoriju neće uspjeti.

- **Podaci koje nisu usklađene direktorija**. Ne uspijete replikacije dulje vrijeme, dvostruke objekte baze podataka možete stvoriti u direktoriju i možda će biti potrebno proširenom Dijagnostika i vrijeme da biste uklonili.

- **Pogreška pri stvaranju računa**. Kontroler domene ne može stvoriti račune za korisnika ili računalo ako je exhausts njegov opskrbu relativni ID-ova i glavno računalo za RID nije dostupan.

- **Sigurnosna pravila pogreška**. Ako SYSVOL zajedničku mapu pravilno replicirati, objekti pravilnika grupe i sigurnosna pravila nije ispravno primjenjuju se na klijente.

Pažljivo praćenje AD poslužitelja, a Budite spremni brzo poduzmite akciju ispravljanja. Stvaranje kontrolnog popisa od praćenje zadataka koje je potrebno izvršiti odgovarajuća interval. Na primjer sljedeće ključnih zadataka nije rasporeda svakodnevno:

- Pregled i rješavanje upozorenja dojavi kontrolera domena 

- Provjerite je li svih kontrolera mogu komunicirati i radi li da replikacija.

- Provjerite je li ostaje li SYSVOL zajedničke.

- Nedostajući problema u sinkronizaciji vremena.

- Provjerite ima li prostora na disku na fizičke pogonima koristi AD.

Ostali redovno zadaci manje nije moguće izvesti često. Na primjer, nije moguće pregledajte odnose pouzdanosti i tjednih potražiti zastario ili neispravne trusts i provjerite da su svih kontrolera ostalo mjesečno pomoću isti servisnim paketima i zakrpa hitni popravak.

Dodatne informacije potražite u članku [Praćenje servisa Active Directory][monitoring_ad]. Instalirajte alate kao što je [Microsoftova centra za sustave] [ microsoft_systems_center] na poslužitelju za nadzor (pogledajte [arhitekturu dijagram][architecture]) da biste lakše izvršiti sljedeće zadatke. 

## <a name="solution-components"></a>Komponente rješenja

Rješenje naveli za tu arhitekturu stvara sigurno hibridnog mreže kao što je opisano dokumenata [implementacijom arhitekturu mreže sigurne hibridnog u Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementaciju arhitekturu sigurne hibridnog mreže s Internetom u Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], ali dodatak od sljedećih stavki:

- Grupe sustava Azure resursa pod nazivom *basename*- dns-ru, pri čemu je *basename* prefiks koji navedete kada implementacija rješenja.

- Dva Azure VMs pod nazivom *basename*-ad1-vm i *basename*-ad2-vm stvorene u grupi *basename*- dns ru resursa. Ove VMs su konfigurirana kao AD poslužitelje s imeničkim servisima i DNS instalacije i konfiguracije.

- Na NSG pod nazivom *basename*-ad-nsg u grupi *basename*- ntwk ru Azure resursa. Ovu grupu resursa je dio infrastrukture koji čine sigurne hibridnog mreže, ali novi NSG je dodatak koji definira ulaznog sigurnosna pravila za poslužitelje AD kao što je prikazano u sljedećoj tablici:


    Prioritet|Ime|Izvor|Odredište|Servis|Akcija|
    --------|----|------|-----------|-------|------|
    170|vnet port53|10.0.0.0/16|Sve|Custom(any/53)|Dopusti|
    180|vnet port88|10.0.0.0/16|Sve|Custom(any/88)|Dopusti|
    190|vnet port135|10.0.0.0/16|Sve|Custom(any/135)|Dopusti|
    200|vnet-na-port137 9|10.0.0.0/16|Sve|Custom(any/137-139)|Dopusti|
    210|vnet port389|10.0.0.0/16|Sve|Custom(any/389)|Dopusti|
    220|vnet port464|10.0.0.0/16|Sve|Custom(any/464)|Dopusti|
    230|vnet-na-rpc dinamičkih|10.0.0.0/16|Sve|Custom(any/49152-65535)|Dopusti|
    240|onprem ad za port53|192.168.0.0/24|Sve|Custom(any/53)|Dopusti|
    250|onprem ad za port88|192.168.0.0/24|Sve|Custom(any/88)|Dopusti|
    260|onprem ad za port135|192.168.0.0/24|Sve|Custom(any/135)|Dopusti|
    270|onprem ad za port389|192.168.0.0/24|Sve|Custom(any/389)|Dopusti|
    280|onprem ad za port464|192.168.0.0/24|Sve|Custom(any/464)|Dopusti|
    290|Upravljanje dokumentima-rdp-Dopusti|10.0.0.128/25|Sve|Custom(any/3389)|Dopusti|
    300|pristupnik Dopusti|10.0.255.224/27|Sve|Custom(any/any)|Dopusti|
    310|Samostalno Dopusti|10.0.255.192/27|Sve|Custom(any/any)|Dopusti|
    320|vnet-Uskrati|Sve|Sve|Custom(any/any)|Dopusti|

    AD DS koristi priključke 53, 89, 135, 389 i 464 da biste prihvatili dolazni replikacije i promet za provjeru autentičnosti. U ovoj tablici kontrolerom domene lokalnog se prostor 192.168.0.0/24 adresa (adresnog prostora mogu se razlikovati - ove informacije navedete kao parametar predloške uvesti rješenje.

    Na NSG definira i sljedeće izlaznog sigurnosna pravila koja se Omogući sinkronizaciju i autorizacije promet da teče lokalne mreže:

    Prioritet|Ime|Izvor|Odredište|Servis|Akcija|
    --------|----|------|-----------|-------|------|
    100|Izlaz port53|Sve|192.168.0.0/24|Custom(any/53)|Dopusti|
    110|Izlaz port88|Sve|192.168.0.0/24|Custom(any/88)|Dopusti|
    120|Izlaz port135|Sve|192.168.0.0/24|Custom(any/135)|Dopusti|
    130|Izlaz port389|Sve|192.168.0.0/24|Custom(any/389)|Dopusti|
    140|Izlaz port445|Sve|192.168.0.0/24|Custom(any/445)|Dopusti|
    150|Izlaz port464|Sve|192.168.0.0/24|Custom(any/464)|Dopusti|
    160|Izlaz rpc dinamičkih|Sve|192.168.0.0/24|Custom(any/49152-65535)|Dopusti|

Skripta koji ste dobili uz rješenje i izvedite sljedeće zadatke:

- Dodaje *basename*-ad1-vm i *basename*-ad2-poslužiteljima vm kontrolera domena na domenu. Konzola za *Active Directory korisnicima i računalima* u kontrolerom domene lokalnog možete pogledati sljedećim poslužiteljima:

![[1]][1]

- Stvara novu podmreže (10.0.0.0/16) AD-mjesta pod nazivom Azure-VNet-Ad-web-mjesta s domenom. Ovo web-mjesto sadrži *basename*-ad1-vm i *basename*-ad2-vm poslužiteljima. 

- Dodaje IP među web-mjesta prijenosa postavke konfiguriranje intervala ponavljanja između lokalnog web-mjesta i kontrolera domena u oblaku. Postavke za podmreže, web-mjesta i postavke prijenosa na konzoli *Active Directory web-mjesta i poslužitelja* za kontrolerom domene lokalnog možete vidjeti:

![[2]][2]

## <a name="deployment"></a>Uvođenje

Uzorak rješenja sastoji se od sljedećih prerequsites:

- Već ste konfigurirali lokalne domene, a da ste konfigurirali DNS i instalirani usmjeravanje i daljinski pristup servisima za podršku VPN povezati pristupnika Azure VPN-a.


- Instalirali najnoviju verziju EŽA Azure. [Slijedite ove upute detalje][cli-install].

- Ako ste implementacije rješenja sustava Windows, morate instalirati alat koji omogućuje ljuske tulumu, kao što su [Radna površina GitHub][github-desktop].

>[AZURE.NOTE] Ako nemate pristup postojeće lokalne domene, možete stvoriti okruženju Probno korištenje [onpremdeploy.sh] [ onpremdeploy] tulum skripte. Ova skripta stvara mreža i VM u oblak koji simulira vrlo osnovni lokalne instalacije. Uređivanje ovu skriptu prije pokretanja i postaviti sljedeće varijable definiran na početku datoteku:
>
> - **BASE_NAME**. Korisnički definirane prefiks grupa resursa i VM stvorio skriptu. Stavka mora biti **dulji od 5 znakova** u suprotnom skripta će pokušati generiranje VM s nazivom koji nisu valjani i neće uspjeti.
> 
> - **PRETPLATE**. Svoj ID Azure pretplate. U ovom suscription stvorit će se u grupu resursa.
> 
> - **Mjesto**. Azure mjesto na kojem želite stvoriti grupu resursa, kao što su *eastus* ili *westus*.
> 
> - **ADMIN_USER_NAME**. Naziv će se koristiti za administratorski račun u na VM.
> 
> - **ADMIN_PASSWORD**. Lozinka za administratorski račun.

Poduzmite sljedeće korake da biste sastavili uzorak rješenja:

1. Preuzimanje i uređivanje [azuredeploy.sh] [ azuredeploy] skripte i postavljanje sljedećih parametara na početku datoteku:

    - **BASE_NAME**. Korisnički definirane prefiks grupe resursa i VMs stvorio skriptu. Što prije, ta stavka mora biti **dulji od 5 znakova**.

    - **PRETPLATE**. Svoj ID Azure pretplate.

    - **OS_TYPE**. Operacijski sustav (*Windows* ili *Linux*) da biste koristili za web, tvrtke i podataka pristupiti VMs sloju. Imajte na umu da AD poslužiteljima stvorio skriptu pokrenuti Windows Server 2012, bez obzira na tu postavku.

    - **Naziv_domene**. Naziv lokalne domene. Imajte na umu da ako koristite okruženja stvorena pomoću skripte onpremdeploy.sh, to mora biti *contoso.com*.

    - **Mjesto**. Azure mjesto na kojem želite stvoriti grupe resursa.

    - **ADMIN_USER_NAME**. Naziv će se koristiti za administratorske račune u različitim VMs.

    - **ADMIN_PASSWORD**. Lozinka za administratorski račun.

    - **ON_PREMISES_PUBLIC_IP**. Javnu IP adresu lokalnog računala VPN-a.

    - **ON_PREMISES_ADDRESS_SPACE**. Prostor interne adrese lokalne mreže. Ako koristite okruženja stvorena pomoću skripte onpremdeploy.sh, to mora biti 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. IPSec zajednički ključ za uspostavljanje VPN veza između lokalne mreže i Azure VPN pristupnika.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. IP adresu DNS poslužitelja lokalnog. Ako koristite okruženja stvorena pomoću skripte onpremdeploy.sh, to mora biti 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Prefiks adresu lokalne podmreže. Ako koristite okruženja stvorena pomoću skripte onpremdeploy.sh, to mora biti 192.168.0.0/24.

    >[AZURE.NOTE] Da BISTE spremili resurse i vrijeme, skripte stvorili razine pristupa web-mjesto tvrtke ili u okvir za podatke. Ako tražite ove stavke, možete uklonite odjeljak azuredeploy.sh skripte:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Otvorite upit ljuske tulumu i Premjesti u mapu koja sadrži skripte azuredeploy.sh.

3. Prijavite se na račun za Azure. U ljusci tulumu unesite sljedeću naredbu:

    ```
    azure login
    ```

    Slijedite upute za povezivanje s Azure.

4. Pokrenite naredbu `./azuredeploy.sh`, te pričekajte dok skripta stvara mrežne infrastrukture.

5. Naredbenom *Provjerite je li u VNet je stvoren*pomoću portala za Azure da biste provjerili grupu resursa pod nazivom *basename*- ntwk-ru je stvoren, a da sadrži stavke sličan je prikazano na sljedećoj slici:

    ![[3]][3]

    >[AZURE.NOTE] U prikazanim primjerima *basename* postavljeno je *oblak* prilikom pokretanja skripte.

    Kliknite *basename*- vnet VNet, kliknite *podmreže*i provjerite je li podmreže prikazano u nastavku su stvorena:

    ![[4]][4]

6. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se stvaraju web razina i opterećenja.

7. Naredbenom *Provjerite je li da Web razina ispravno stvorena*pomoću portala za Azure da biste provjerili grupu resursa pod nazivom *basename*web-sloju-ru je stvoren, a da sadrži stavke sličan je prikazano u nastavku:

    ![[5]][5]

8. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se stvaraju se u NVAs.

9. Naredbenom *Provjerite je li da se NVA ispravno stvorena*pomoću portala za Azure da biste provjerili da grupu resursa pod nazivom *basename*- upravljanje dokumentima-ru stvorena sljedeće sadržaje:

    ![[6]][6]

10. Kada se zatraži u prozoru ljuske tulumu tipku i pričekajte dok se stvara u jumpbox.

11. Naredbenom *Provjerite je li da se jumpbox ispravno stvorena*pomoću portala za Azure da biste provjerili da sljedeće stavke su dodani u grupu *basename*- upravljanje dokumentima ru resursa:

    - Skup dostupnost naziva *basename*- jb-kao.

    - VM koji se naziva - jb vm *basename*.

    - Mrežno sučelje naziva *basename*- jb-nic.

12. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se stvaraju se pristupnik za Azure VPN i veze. Imajte na umu da taj korak može potrajati do 30 minuta.

13. Naredbenom *Provjerite je li VPN pristupnik ispravno stvorena*pomoću portala za Azure da biste provjerili da sljedeće stavke su dodani u grupu *basename*- ntwk ru resursa:

    - Pristupnik za lokalnu mrežu naziva na – lokalno-lgw.
    
    - Pristupnik virtualne mreže naziva *basename*- vpngw.

    - Povezivanje pristupnika pod nazivom *basename*- vnet-vpnconn. Imajte na umu da stanje ove veze možda *nije povezano* Ako još niste konfigurirali lokalnog kraj veze; će to adresa kasnije.

14. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se stvaraju se u VMs i ostale resurse za na DMZ.

15. Naredbenom *Provjerite je li da se DMZ ispravno stvorena*pomoću portala za Azure da biste provjerili da grupu resursa pod nazivom *basename*-dmz-ru stvorena sljedeće sadržaje:

    ![[7]][7]

16. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku. Sljedeće upute prikazivati:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Prijavite se na računalo lokalnog koja se povezuje s Azure pristupnika i konfigurirati vezu pravilno. Dodati statične usmjerava na lokalni pristupni uređaj koji usmjerava requestsfor raspon adresa 10.0.0.0/16 do pristupnik na VNet. Upute za taj postupak razlikuju se ovisno o kako se povezujete. U odjeljku [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ implementing-a-hybrid-network-architecture-with-vpn] dodatne informacije.

    Imajte na umu da možete pronaći javnu IP adresu pristupnika Azure VPN pomoću portala za Azure da biste pregledali pristupnika - vpngw *basename*u grupi *basename*- ntwk ru resursa:

    ![[8]][8]

    Možete odrediti hoće li veza uspostavljena je ispravno tako da pogledate stanja veze - vnet vpnconn *basename*. Mora biti postavljena na *povezan*.

    Da biste testirali veze, otvarati udaljene radne površine jumpbox (10.0.0.254) s računala koja se nalazi u lokalnu mrežu.

17. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku. Na sljedeći upit, *pritisnite bilo koju tipku da biste ažurirali VNet postavku za VNet na lokalni DNS-a*, pritisnite tipku i pričekajte da se vrijednosti koje ste naveli kao parametar **ON_PREMISES_DNS_SERVER_ADDRESS** u skripti azuredeploy.sh će se ažurirati postavke DNS-a za na VNet.

18. Kada se zatraži, *Provjerite je li poslužitelj postavke DNS-a na na VNet je ažurirana*, pomoću portala za Azure pregledajte postavke *DNS poslužitelji* u *basename*- vnet VNet u grupi *basename*- ntwk ru resursa. Mora biti postavljena na *Prilagođeni DNS*, a *primarnom DNS poslužitelju* mora biti adresu DNS poslužitelja lokalnog:

    ![[9]][9]

19. Naredbenom *koju tipku da biste stvorili grupu resursa za poslužitelje AD* u prozoru ljuske tulumu tipku i pričekajte dok se stvara grupu resursa za čuvanje poslužitelja za AD u oblaku.

20. Naredbenom *koju tipku da biste stvorili VMs za poslužitelje AD* u prozoru ljuske tulumu tipku i pričekajte da VMs da biste stvorili i dodali u grupu resursa.

21. Kada se pojavi *bilo koju tipku da biste se pridružili VMs da biste nad lokalnom domenom* , idite na portal za Azure i provjerite je li grupu pod nazivom *basename*- dns-ru je stvorena i ona sadrži dvije VMS (*basename*-ad1-vm i *basename*-ad2-vm):

    ![[10]][10]

22. U grupi resursa – ntwk ru *basename*potvrdite da je NSG stvorili naziva *basename*-ad-nsg. Pregledajte ulazni i izlazni sigurnosna pravila za ovaj NSG. Oni moraju podudarati s onima navedene u tablicama u [komponenti rješenja] [ solution-components] sekciju.

23. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se u VMs dodaju se lokalne domene.

24. Pri na *idite na lokalni AD server da biste potvrdili da računala dodane su domena* zatraži, povezati s vašim lokalnim računalom i koristiti konzolu *Active Directory Users i računala* da biste provjerili da oba VMs dodane su domene:

    ![[11]][11]

25. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se web-mjesta za replikaciju AD se stvara u domeni.

26. Pri na *idite na lokalni poslužitelj AD da biste provjerili replikacije web-mjesto je stvorena* pitanje, koristite konzolu *Active Directory web-mjesta i servise* za da biste provjerili je li web-mjesta replikacije pod nazivom *Azure Vnet-Ad-web -* uspješno je stvorena, zajedno s vezom IP prijenosa među web-mjesta pod nazivom *AzureToOnpremLink*i podmreže koja se odnosi na VNet:

    ![[12]][12]

27. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku i pričekajte da se skripta instalira imeničkim servisima i DNS- a na svim AD VMs.

28. Kada se pojavi zaslon za *prijavu rješenje svaki poslužitelj za Azure AD da biste potvrdili da imeničkim servisima konfiguriran uspješno* , otvorite udaljene radne površine programa na lokalnom računalu jumpbox (*basename*- jb vm), a zatim otvorite drugi udaljene radne površine iz na jumpbox prvi poslužitelj servisa Active Directory (*basename*-ad1-vm). Prijavite se pomoću **Naziv_domene**, **ADMIN_USER_NAME**i **ADMIN_PASSWORD** koji ste naveli u skripti azuredeploy.sh. Pomoću upravitelja poslužitelja, provjerite da uloge AD DS-a i DNS- a i dodane. Ponovite taj postupak za drugi poslužitelj servisa Active Directory (*basename*-ad2-vm).

29. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku. Kada se pojavi upit *koju tipku da biste postavili Azure VNet DNS postavke tako da pokazuje na DNS u Azure* , pritisnite tipku i Omogući skriptu da biste ažurirali postavke DNS-a za na VNet.

30. Kada na upit *provjerite DNS VNet postavka je ažuriran je referenca Azure VM DNS poslužitelje* pojavit će se pomoću Azure portala potvrdite postavka *DNS poslužitelji* u *basename*- vnet VNet u grupi *basename*- ntwk ru resursa. DNS poslužitelji primarnih i sekundarnih sada trebalo pozivati dva VMs AD:

    ![[13]][13]

31. Iznova na svakoj od AD VMs prije nastavka. Ovaj korak nije potrebno da biste bili sigurni da svaki korisnik obraditi točne postavke DNS-a iz Azure. Pričekajte da su oba VMs pokrenuti prije nastavka.

32. Kada se zatraži u prozoru ljuske tulumu, pritisnite tipku. U sljedeći redak *koju tipku da biste primijenili pristupnika UDR u podmreži pristupnika (to je premještena)*, pritisnite tipku i Dopusti skriptu da biste osvježili pristupnika UDR.

33. Provjerite je li uspješno Završi skriptu.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte najbolje postupke za [Stvaranje skupa stabala resursa u ZBRAJA] [ adds-resource-forest] u Azure.

- Saznajte najbolje postupke za [Stvaranje infrastruktura za ADFS] [ adfs] u Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Sigurne hibridnog arhitektura mreže s servisom Active Directory"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Konzolu Active Directory korisnicima i računalima stavku dva Azure VMs kao poslužitelje"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Konzolu Active Directory web-mjesta i usluge prikazuje replikacije postavke web-mjesta u oblaku"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Sadržaj grupe basename ntwk ru resursa"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Podmreže u VNet basename vnet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Stavke u sloju web"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "NVAs u grupi basename, upravljanje dokumentima i ru resursa"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Resursa u grupi basename dmz ru resursa"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Pronalaženje javnu IP adresu pristupnika Azure VPN-a"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "Postavke DNS poslužitelja za na * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "Na * basename *-dns-ru resursa grupu koja sadrži AD server VMs"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Konzolu Active Directory korisnicima i računalima zapis poslužitelja AD VMs kao članove popisa za domene"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Konzolu Active Directory web-mjesta i usluge prikazuje replikacije web-mjesta za poslužitelje Azure AD"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "Postavke DNS poslužitelja za VNet basename vnet upućuju na poslužitelje AD u oblaku"