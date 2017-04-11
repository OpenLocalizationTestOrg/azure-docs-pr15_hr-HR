<properties 
   pageTitle="Najbolje prakse za StorSimple virtualne polja | Microsoft Azure"
   description="U članku se opisuje najbolje prakse za implementaciju i upravljanje polja virtualne StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Najbolje prakse StorSimple virtualne polja

## <a name="overview"></a>Pregled

Microsoft Azure StorSimple virtualne polja je rješenje integrirani pohrane koja upravlja prostora za pohranu zadataka između lokalnog virtualne uređaju sa sustavom izvodi u hypervisor i za pohranu u oblaku Microsoft Azure. StorSimple virtualne je argument polje alternativu učinkovitiji, učinkovit polja fizičke 8000 niz. Virtualne polja mogu se izvoditi na postojeću infrastrukturu hypervisor podržava iSCSI i protokole SMB te je dobro prikladniji za scenariji za office remote office/grani. Dodatne informacije o StorSimple rješenja, idite na [Microsoft Azure StorSimple pregled](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

U ovom se članku opisuje najbolje prakse implementirana tijekom početne instalacije, implementacije i upravljanja StorSimple virtualne polja. Najbolje prakse navedite provjerene smjernice za upravljanje sustava virtualne polja i postavljanje. U ovom se članku je namijenjena IT administratore koji uvođenje i upravljanje virtualne polja u njihovim podatkovnim centrima.

Preporučujemo da se periodičku pregled najbolje prakse da biste lakše provjerite je li uređaj ne napuštajući usklađenost su promjene na protok podešavanje ili operacija. Trebali biste naiđete na probleme prilikom implementacije najbolje prakse na virtualne polja, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za pomoć.

## <a name="configuration-best-practices"></a>Najbolje prakse za konfiguraciju 

Najbolje prakse pokrivaju smjernice za koje je potrebno slijediti tijekom početne instalacije i implementaciju virtualne polja. Najbolje prakse obuhvaćaju one vezane uz dodjeljivanje virtualnog računala, postavke iz pravilnika grupe, za promjenu veličine, postavljanju za povezivanje s mrežom, konfiguriranje računa za pohranu i stvaranje dionice i količine za virtualne polja. 

### <a name="provisioning"></a>Dodjela resursa 

Virtualna polja StorSimple je virtualnog računala (VM) dodjeli na hypervisor (Hyper-V ili VMware) poslužitelja glavnog računala. Prilikom dodjele resursa virtualnog računala, provjerite je li na glavno računalo moći odvojiti dovoljno resursa. Dodatne informacije, idite na [minimalne uvjete resursa](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) Dodjela polja. 

Implementacija sljedeće najbolje prakse prilikom dodjele resursa virtualne polja:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Vrsta virtualnog računala**   | **Generiranje 2** VM za korištenje sa sustavom Windows Server 2012 ili novijim i *.vhdx* slike. <br></br> **Generiranje 1** VM za korištenje sa sustavom Windows Server 2008 ili novijem i *.vhd* slike.                                                                                                              | Korištenje virtualnog računala verzije 8-11 prilikom korištenja *.vmdk* slike.                                                                      |
| **Vrsta memorije**            | Konfiguriranje kao **statički memorije**. <br></br> Pomoću mogućnosti **dinamične memorije** .            |                                                    |
| **Vrsta podataka na disku**         | Dodjela kao **dinamično proširivanje**.<br></br> **FIXED veličina** traje predugo. <br></br> Pomoću mogućnosti **razlikovanje** .                                                                                                                   | Pomoću mogućnosti **Tanki dodjele resursa** .                                                                                      |
| **Izmjene na disku** | Proširenje ili smanjivanjem nije dopušteno. Pokušaj da biste to učinili rezultira gubitak lokalnih podataka na uređaju.                       | Proširenje ili smanjivanjem nije dopušteno. Pokušaj da biste to učinili rezultira gubitak lokalnih podataka na uređaju. |

### <a name="sizing"></a>Promjena veličine

Kada vaš StorSimple virtualne polja za promjenu veličine, razmotrite sljedeće čimbenike:

- Lokalni rezervacije za jedinicama ili zajedničko korištenje. Približno 12% prostora je rezervirana na lokalni sloju za svaku dodijeljenu tiered glasnoću ili zajedničko korištenje. Otprilike 10% razmaka i rezervirana za lokalno prikvačene glasnoću datotečnom sustavu.
- Snimka indirektni. Otprilike 15% prostora na lokalnom sloju je rezervirana za snimke.
- Treba li vam za vraća. Ako način vraćanja kao nova operacija, promjenu veličine treba računa za prostor potreban za vraćanje. Vraćanje je gotovo da biste zajedničko korištenje ili količine iste veličine ili veća.
- Za sve neočekivane growth, trebali biste dodijeljeno neke međuspremnika.

Na temelju prethodne čimbenika preduvjeti za promjenu veličine moguće predstaviti pomoću sljedeće jednadžbe:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Sljedeći primjeri pokazuju kako veličina virtualne polja prema svojim potrebama.

#### <a name="example-1"></a>Primjer 1:
Virtualna polje, želite omogućiti 

- Dodjela 2 TB tiered glasnoću ili zajednički mrežni resurs.
- Dodjela resursa za 1 TB tiered glasnoću ili zajednički mrežni resurs.
- Dodjela 300 GB lokalno prikvačene jedinice ni zajednički koristiti.


Za prethodni količine ili dionice, Javite nam izračunati potreban prostor na lokalni sloju. 

Prvi put, za svaku tiered glasnoću/zajedničko korištenje lokalne rezervacije bi jednako 12% veličine glasnoću/zajedničko korištenje. Lokalno prikvačene glasnoću/zajedničkog mrežnog resursa lokalne rezervacije je 10% veličine lokalno prikvačene glasnoću/zajedničko korištenje (osim dodijeljenu veličina). U ovom se primjeru potrebno

- Lokalni rezervacija 240 GB (za 2 TB tiered glasnoću/zajedničko korištenje)
- Lokalni rezervacija 120 GB (za 1 TB tiered glasnoću/zajedničko korištenje)
- 330 GB za lokalno prikvačene glasnoću ili zajedničko korištenje (Dodavanje 10% od lokalnog rezervacija 300 GB dodjeli veličina)

Ukupan prostor na dosad potreban na lokalni sloju je: 240 GB + 120 GB + 330 GB = 690 GB.

Drugo, moramo barem koliko prostora na lokalnom sloju kao najveću jedan rezervacije. Dodatni iznos koristi se za slučaj da morate vratiti iz oblaka snimke. U ovom primjeru najveću lokalne rezervacija radi 330 GB (uključujući rezervacije za datotečni sustav), pa želite dodati da na 690 GB: 690 GB + 330 GB = 1020 GB.
Ako obaviti sljedeće dodatne vraća smo možete uvijek oslobodili prostor iz prethodne postupak vraćanja.

Treći, moramo 15% prostora za Ukupno lokalne dosad za pohranu lokalne snimke, tako da je dostupna samo 85% ga. U ovom primjeru koji bi oko 1020 GB = 0.85&ast;dodijeljenu podataka na disku TB. Tako, bio bi disk dodijeljenu podataka (1020&ast;(1/0.85)) = 1200 GB = 1.20 TB ~ 1,25 TB (zaokruživanje na najbliži quartile)

Factoring u neočekivane growth i vraća novi, trebali biste Dodjela oko na lokalni disk od 1,25-1,5 TB.

> [AZURE.NOTE] Preporučujemo i da se na lokalnom disku thinly dodjeli. U ovom preporuke je jer prostora za vraćanje je potrebna samo kada se želite vratiti podatke koji su starije od pet dana. Oporavak razini stavke omogućuje vam da biste vratili podatke za zadnjih pet dana bez dodatnog prostora za vraćanje.

#### <a name="example-2"></a>Primjer 2: 
Virtualna polje, želite omogućiti 

- Dodjela 2 TB tiered glasnoće
- Dodjela 300 GB lokalno prikvačene glasnoće

Na temelju 12% prostora na lokalnom rezervacija za tiered količine/zajedničke i 10% za lokalno prikvačene količine/zajedničke moramo

- Lokalni rezervacije 240 GB (za 2 TB tiered glasnoću/zajedničko korištenje)
- 330 GB za lokalno prikvačene glasnoću ili zajedničko korištenje (Dodavanje 10% od lokalnog rezervacija prostor 300 GB dodjeli)

Ukupno potreban prostor na lokalni sloju je: 240 GB + 330 GB = 570 GB

Minimalna lokalne prostor potreban za vraćanje je 330 GB. 

15% zbroja diska se koristi za pohranu snimke tako da je dostupna samo 0.85. Stoga je veličina diska (900&ast;(1/0.85)) = 1.06 TB ~ 1,25 TB (zaokruživanje na najbliži quartile) 

Factoring u bilo kojem neočekivana rasta, možete je Dodjela 1,25-1,5 TB na lokalni disk.


### <a name="group-policy"></a>Pravilnik grupe

Pravila grupe su infrastruktura za koji vam omogućuje da implementirati određene konfiguracije za korisnike i računala. Postavke pravilnika grupe nalaze se u objekti pravilnika grupe (GPO), koji su povezani sa sljedećim spremnika Active Directory Domain Services (AD DS): web-mjesta, domena ili Organizacijska jedinica (organizacijske jedinice). 

Ako je vaš virtualne polja domene pridružili, GPO se mogu primijeniti na njega. Ove GPO možete instalirati aplikacija kao što je antivirusni softver koji mogu negativno utjecati operacija polja virtualne StorSimple.

Stoga preporučujemo vam:

-   Provjerite je li vaša virtualne polja u vlastitom organizacijsku jedinicu (OU) za Active Directory. 

-   Provjerite je li vaše virtualne polja se primjenjuju bez objekti pravilnika grupe (GPO). Možete blokirati nasljeđivanje da biste bili sigurni da virtualne polja (podređeni čvor) automatski nasljeđuje sve GPO od nadređenog. Dodatne informacije potražite na [nasljeđivanje blok](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Povezivanje s mrežom

Konfiguracija mreže za vaše virtualne polja obavlja putem lokalnog web korisničkog Sučelja. Virtualna mrežno sučelje omogućena hypervisor u kojima je dodijeljena virtualne polja. Koristite stranicu [Postavke mreže](storsimple-ova-deploy3-fs-setup.md) da biste konfigurirali virtualne mreže sučelja IP adrese, podmreže i pristupnik.  Možete konfigurirati i primarnih i sekundarnih DNS poslužitelj, postavke vremena i postavke proxyja neobavezno za svoj uređaj. Većina mrežna konfiguracija je jednokratni postavljanje. Pregled [preduvjeti za umrežavanje StorSimple](storsimple-ova-system-requirements.md#networking-requirements) prije implementacije virtualne polja.

Prilikom implementacije sustava virtualne polja, preporučujemo da slijedite ove najbolje prakse:

-   Provjerite imaju li mrežni u kojem virtualne polja je implementiran uvijek kapaciteta želite odvojiti 5 MB/s propusnosti internetske veze koja (ili više njih). 

    -   Treba li vam propusnosti internetske ovisi o karakteristikama svoje radno opterećenje i stopa promjene podataka.

    -   Promjena podataka koji se može riješiti izravno je proporcionalna internetska propusnost. Kao primjer prilikom poduzimanja sigurnosnu kopiju 5 MB/s propusnosti prikazuje promjene podataka oko 18 GB u osam sati. S četiri puta više propusnost (20 MB/s), možete učiniti promjena četiri puta više podataka (72 GB). 

-   Provjerite je li povezani s Internetom uvijek dostupna. Povremeni gubitak ili nepouzdanih internetske veze za uređaje može dovesti do gubitka programa access s podacima u oblak i može rezultirati nepodržane konfiguracije.

-   Ako planirate implementaciju uređaju kao iSCSI poslužitelj: 
    -   Preporučujemo da onemogućite mogućnost **dohvaćanje IP adrese automatski** (DHCP). 
    -   Konfiguriranje statičke IP adrese. Morate konfigurirati primarne i sekundarne DNS poslužitelj.

    -   Ako koji definira više mreža sučelja na vašem virtualne polja, samo prvi mrežnog sučelja (po zadanom ovo sučelje je **Ethernet**) možete dobiti u oblak. Da biste odredili vrstu promet, možete stvoriti više sučelja virtualne mreže na vaše virtualne polja (konfigurirano kao iSCSI poslužitelj) i povezivanje te sučelja za različite podmreže.

-   Da biste throttle propusnosti oblaka samo (koristi virtualne polja), konfigurirati Reguliranje na usmjerivač ili vatrozid. Ako definirate ograničavanje u vašem hypervisor će throttle sve protokole uključujući iSCSI i SMB umjesto samo oblak propusnosti. 

-   Provjerite te vrijeme sinkronizacije za hypervisors omogućena. Ako pomoću tehnologije Hyper-V, odaberite svoje virtualne polja u upravitelju Hyper-V, idite na **Postavke &gt; servisima za integraciju**, i provjerite je li **Sinkronizacija vremena** potvrđen.

### <a name="storage-accounts"></a>Računi za pohranu

StorSimple virtualne polja mogu se povezati s računom jedan prostora za pohranu. Taj račun za pohranu može biti račun automatski generirani prostora za pohranu, račun u okviru iste pretplate kao usluge, ili s računom za pohranu vezan uz drugu pretplatu. Dodatne informacije potražite u članku Upravljanje [račune za pohranu za vaše virtualne polja](storsimple-ova-manage-storage-accounts.md).

Slijedite ove preporuke za račune za pohranu povezan s vašeg virtualne polja.

-   Pri povezivanju više virtualne polja s računom jedan prostora za pohranu, Faktor u maksimalnog kapaciteta (64 TB) za virtualne polja i maksimalne veličine (500 TB) za račun za pohranu. Time se ograničava broj punoj veličini virtualne polja mogu biti povezane s tim računom za pohranu na otprilike 7.

-   Prilikom stvaranja novog računa za pohranu
    -   Preporučujemo da ga stvorite u području najbliže Office remote office/granu gdje je implementiran na StorSimple virtualne polja da biste minimizirali latencies.

    -   Nose na umu da nije moguće premjestiti na račun za pohranu preko različitih područja. Također možete premještati servisa putem pretplate.

    -   Pomoću računa za pohranu koji implementira zalihosti između u podatkovnim centrima. Zemlj Redundant prostora za pohranu (GRS), Zone suvišnih prostora za pohranu (ZRS) i lokalno suvišnih prostora za pohranu (LRS) podržani su za korištenje s vašeg virtualne polja. Dodatne informacije o različite vrste računa za pohranu, idite na [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Zajedničko korištenje i jedinice

Kada je konfiguriran kao datotečnom poslužitelju i količine kada konfigurirati kao iSCSSI poslužiteljem, za polja vašem StorSimple virtualne Dodjela zajedničko korištenje. Najbolje prakse pri stvaranju dionice i količine vezani uz veličinu i vrstu konfiguriran.

#### <a name="volumeshare-size"></a>Veličina jedinice/zajedničko korištenje

Kada je konfiguriran kao datotečnom poslužitelju i količine kada konfigurirati kao iSCSSI poslužiteljem, na virtualne polje, dodijelite zajedničko korištenje. Najbolje prakse pri stvaranju dionice i jedinicama odnose se na veličinu i vrstu konfiguriran. 

Imajte na umu sljedeće najbolje prakse prilikom dodjele resursa zajedničko korištenje ili količine na uređaju virtualne.

-   Veličina datoteka odnosu dodijeljenu veličinu tiered zajedničko korištenje može utjecati na performanse tiering. Rad s velikih datoteka može rezultirati sporo sloju odgovor. Rad s velikih datoteka, preporučujemo da Najveća datoteka manja od 3% veličine zajedničko korištenje.

-   Maksimalno 16 količine/dionice moguće virtualne polja. Ako lokalno prikvačene jedinicama/dionice može biti između 50 GB 2 TB. Ako tiered, jedinicama/dionice mora biti između 500 GB 20 TB. 

-   Prilikom stvaranja glasnoću faktor potrošnje očekivanih podataka, kao i budućeg rasta. Dok jedinica ne mogu se proširiti kasnije, uvijek možete vratiti veću jedinicu.

-   Nakon što glasnoće, nije moguće smanjiti veličinu jedinice na StorSimple.
   
-   Prilikom pisanja tiered jedinicu na StorSimple, kada podataka glasnoću dosegne određeni prag (odnosu lokalne razmak rezervirana za glasnoću), ograničio je vrijeme UI. Nastavite pisati u ovoj jedinici usporava UI znatno. Iako možete napisati tiered jedinicu izvan njegov dodijeljenu kapacitet (smo aktivno prekinuti korisnika iz pisanje izvan dodijeljenu kapaciteta), vidjet ćete upozorenje gumb za obrube koje ste oversubscribed. Kada se prikaže upozorenje, izuzetne je Ponesite remedial mjere, kao što su brisanje glasnoću ili vraćanje glasnoću veću jedinicu (glasnoću proširenja trenutno nije podržano).

-   Izrada oporavak korištenje slučajeva, kao što je broj dozvoljenu dionice/količine je 16, a maksimalni broj dionice i količine koje se mogu obraditi paralelno i 16, broj dionice/količine nema sa slikom na RPO i RTOs. 

#### <a name="volumeshare-type"></a>Vrsta jedinice/zajedničko korištenje

StorSimple podržava dvije vrste glasnoću/zajedničko korištenje koji se temelji na korištenje: lokalno prikvačeni i tiered. Lokalno prikvačene količine/dionice su thickly dodjeli dok su tiered količine/dionice thinly dodjeli. 

Preporučujemo da implementirati sljedeće najbolje prakse prilikom konfiguriranja količine dionice StorSimple:

-   Odredite vrstu glasnoće na temelju radnih opterećenja koje namjeravate implementacija prije stvaranja jedinicu. Korištenje lokalno prikvačene količine radnih opterećenja koji zahtijevaju lokalne jamstva podataka (čak i tijekom oblaka nedostupnosti) i koji zahtijevaju latencies niskog oblaka. Kada stvorite jedinicu na vašem virtualne polja, ne možete promijeniti vrstu jedinice iz lokalno prikvačena na tiered ili *obratno*. Na primjer, stvorite lokalno prikvačene jedinice pri implementaciji radnih opterećenja SQL ili radnih opterećenja hosting virtualnim strojevima (VMs); Korištenje tiered količine radnih opterećenja zajedničko korištenje datoteka.

-   Potvrdite mogućnost za manje često korištenih arhiviranje podataka o velike datoteke. Poništavanje duplikacije bloka veći od 512 K koristi se kada je omogućena je da biste ubrzali prijenos podataka s oblakom.

#### <a name="volume-format"></a>Oblikovanje glasnoće

Kada stvorite StorSimple jedinicama na poslužitelj za iSCSI, morate pokrenuti postavljanje i oblikovanje na jedinicama. Ovaj postupak se izvodi na glavnom računalu s uređajem StorSimple. Kada postavljanje i oblikovanje količine na glavnom računalu StorSimple, preporučuje se sljedeće najbolje prakse.

-   Brzo oblikovanje izvedete sve StorSimple jedinice.

-   Prilikom oblikovanja jedinice StorSimple, koristite veličina jedinice za dodjelu (Istočna Australija) od 64 KB (Zadana vrijednost je 4 KB). 64 KB Istočna Australija temelji se na broju testiranja gotovo podudarala uobičajenih radnih opterećenja StorSimple i drugih radnih opterećenja.

-   Prilikom korištenja polja virtualne StorSimple konfigurirati kao iSCSSI poslužiteljem, koristite rastegnute jedinice ili dinamičkih diskova kao ove jedinice ili diskova ne podržava StorSimple.

#### <a name="share-access"></a>Zajedničko korištenje programa access

Prilikom stvaranja zajedničke stavke na poslužitelj datoteka virtualne polja, slijedite ove smjernice:

-   Prilikom stvaranja zajedničke mape, dodijeliti grupu korisnika kao administratora za zajedničko korištenje umjesto jednog korisnika.

-   Dozvole za NTFS možete upravljati nakon stvaranja zajedničko korištenje tako da uredite zajedničko korištenje pomoću programa Windows Explorer.

#### <a name="volume-access"></a>Pristup glasnoće

Prilikom konfiguriranja količine iSCSI na vašem StorSimple virtualne polja, važno je za kontrolu pristupa kad god je potrebno. Da biste utvrdili koji poslužitelji glavnog računala mogu pristupiti količine, stvorite i pridružiti zapisa za kontrolu pristupa (ACRs) StorSimple količine.

Prilikom konfiguriranja ACRs za StorSimple količine, koristite sljedeće najbolje prakse:

-   Uvijek da barem jedan ACR pridružiti jedinicu.

-   Definiranje više ACRs samo u okruženju grupirani.

-   Kada dodjeljujete više ACR jedinicu, provjerite je li jedinica ne pojavljuje na način gdje ga možete istovremeno pristupiti više glavnih računala koje nisu grupirani. Ako ste dodijelili većem broju ACRs jedinice, poruku s upozorenjem iskače za da pregledate konfiguraciju.

### <a name="data-security-and-encryption"></a>Sigurnost podataka i šifriranje

Vaše StorSimple virtualne polja ima podataka značajke sigurnosti i šifriranje koji osigurati povjerljivosti i integritet podataka. Prilikom korištenja tih značajki, preporučuje se da slijedite ove praktične savjete: 

-   Definiranje u oblak prostora za pohranu ključa za šifriranje radi generiranja AES 256 šifriranje prije slanja podataka iz vaše virtualne polja u oblak. Ovaj ključ nije potrebna ako vaši podaci šifriran počinje sa. Tipku možete generirati i čuvaju sigurni sustavu upravljanja ključem kao što su [Azure ključa sigurnog](../key-vault/key-vault-whatis.md).

-   Prilikom konfiguriranja računa za pohranu putem servisa za Upravitelj StorSimple, provjerite je li omogućite SSL načina za stvaranje sigurnog kanala za mrežnu komunikaciju između StorSimple uređaja i s oblakom.

-   Obnovi tipki za račune za pohranu (po pristupa servisu Azure prostora za pohranu) povremeno račun za sve promjene da biste pristupili ovisno o promijenjenim popisa administratora.

-   Sažeti i deduplicated prije slanja Azure podataka na vašem virtualne polja. Ne preporučujemo korištenje servisa uloga Poništavanje duplikacije podatke na vašem računalu koje hostira Windows Server.


## <a name="operational-best-practices"></a>Radu najbolje prakse

Radu najbolje prakse su upute koje se moraju biti prikazane tijekom svakodnevno upravljanje ili operacija virtualne polja. Ove savjete prolaženja određene upravljanje zadatke kao što su stvaranje sigurnosne kopije, vraćanje iz sigurnosne kopije skupu izvođenje na prebacivanje, deaktiviranje i brisanje polja, nadzor sustava korištenju i stanju sustava i pokretanje virusa skenira vaše virtualne polja.

### <a name="backups"></a>Sigurnosne kopije

Podatke na vašem virtualne polja sigurnosne kopije u oblak na dva načina, zadani automatski dnevnih sigurnosnu kopiju cijele uređaj početni 22:30 ili putem ručno osvježavati sigurnosnu kopiju. Prema zadanim postavkama, uređaj automatski stvara dnevne snimke oblaka svih podataka residing na njemu. Dodatne informacije potražite u članku sigurnosnu kopiju [vašeg StorSimple virtualne polja](storsimple-ova-backup.md).

Nije moguće promijeniti učestalost i zadržavanja pridružene zadane sigurnosne, ali možete konfigurirati vrijeme na kojoj su dnevni sigurnosne kopije pokrenut svaki dan. Prilikom konfiguriranja vremena početka automatskog sigurnosnog kopiranja, preporučujemo da:

-   Zakazivanje sigurnosne kopije slabog opterećenja Budući sata. Sigurnosno kopiranje početno vrijeme potrebno odgovarali brojne glavno računalo IO.

-   Prilikom planiranja za izvođenje prebacivanje uređaja ili prethodne prozor održavanja da biste zaštitili podatke na vašem virtualne polja, pokrenite ručno osvježavati sigurnosnu kopiju.

### <a name="restore"></a>Vraćanje

Možete vratiti iz sigurnosne kopije postaviti na dva načina: vraćanje drugi glasnoću ili zajednički mrežni resurs ili ponovno razini stavke oporavak (dostupno samo na virtualne polja konfigurirati kao poslužitelj datoteka). Oporavak razini stavke omogućuje vam da biste učinili zrnastog oporavak datoteka i mapa iz oblaka sigurnosnu kopiju dionice na uređaju StorSimple. Dodatne informacije potražite u članku za [Vraćanje iz sigurnosne kopije](storsimple-ova-restore.md).

Prilikom izvršavanja vraćanja imajte sljedeće smjernice na umu:

-   Vaše StorSimple virtualne polja ne podržava vraćanja na istome mjestu. To se može lako no biti postići dva koraka: Provjerite prostora na virtualne polja, a zatim se vratite na drugi glasnoću/zajedničko korištenje.

-   Kada vraćanje iz lokalne opseg, imajte na umu obnavljanja bit će dugo izvodi operacije. Iako glasnoću brzo mogu potjecati online, podatke i dalje biti hydrated u pozadini.

-   Jedinica vrsta ostaje tijekom postupka vraćanja. Tiered glasnoću će se vratiti na neku drugu tiered jedinicu, a zatim lokalno prikvačene glasnoće na drugu lokalno prikvačene glasnoću.

-   Prilikom pokušaja vraćanje jedinice ili zajedničke mape iz sigurnosne kopije postavite, ako Vrati ne uspije, Ciljna jedinica ili zajedničko korištenje i dalje mogu se kreirati na portalu. Nije važno brisanje jedinicu Neiskorišteni cilj ili zajedničko korištenje na portalu za minimiziranje buduće probleme nastale taj element.

### <a name="failover-and-disaster-recovery"></a>Prebacivanje i Izrada oporavak

Prebacivanje s uređaja omogućuje vam za migraciju podataka iz *izvora* uređaj s podatkovnim centrom na drugi uređaj *cilj* koji se nalaze u istim ili različitim geografski. Prebacivanje uređaj je za cijelo uređaj. Tijekom prebacivanje, podataka oblaka za uređaj izvor mijenja vlasništvo koji ciljni uređaj.

Za polja vašem StorSimple virtualne koje možete samo uspjeti putem drugog polja virtualne upravlja iste StorSimple Upravitelj servisa. Prebacivanje na uređaju sa sustavom 8000 niz ili polja upravlja različite servisne StorSimple Manager (od one za uređaj izvora) nije dopušteno. Da biste saznali više o značajkama prebacivanje, idite na [preduvjeti za prebacivanje uređaja](storsimple-ova-failover-dr.md).

Prilikom izvršavanja u Neuspjelo putem za vaše virtualne polja, imajte sljedeće na umu:

-   Za prebacivanje na planirane je preporučena najbolji način da biste preuzeli sve na jedinicama/dionice izvanmrežno prije započinjanja za prebacivanje. Slijedite upute specifične za operacijski sustav da biste snimili na količine i zajedničko korištenje izvan mreže na glavnom računalu prvi i zatim izvršite one izvanmrežno na uređaju virtualne.

-   Na poslužitelju Izrada oporavak datoteka (DR), preporučujemo da se pridružite ciljni uređaj na istom domenu izvor tako da se automatski riješi dozvole za zajedničko korištenje. U ovom izdanju podržana je samo prebacivanje ciljni uređaj u istoj domeni.

-   Kada u DR uspješno završi, uređaj izvora automatski će se izbrisati. Iako uređaj više nije dostupan, virtualnog računala koja vam dodijeljena u sustavu glavno računalo i dalje koristi resurse. Preporučujemo da iz glavnog računala sustava da biste spriječili accruing bilo kakvih troškova brisanje ovog virtualnog računala.

-   Imajte na umu da čak i ako je na prebacivanje to ne uspije, **Podaci se uvijek sigurni u oblaku**. Imajte na umu sljedeće tri scenarija u kojima se prebacivanje nije uspjela:

    -   Pojavila se pogreška u početne faze prebacivanje kao što su kada koji se izvršavaju prije provjere DR. U tom slučaju ciljni uređaj je i dalje koristiti. Možete ponovno prebacivanje na istom uređaju cilj.

    -   Pojavila se pogreška tijekom postupka stvarni prebacivanje. U ovom slučaju ciljni uređaj označen nestabilan. Morate Dodjela resursa i konfiguriranje drugi cilj virtualne polja i koji se koristi za prebacivanje.

    -   Za prebacivanje je dovršena pratiti koji je izbrisan je uređaj izvora, ali ciljni uređaj ima problema i ne može pristupiti podacima. Podaci se i dalje sigurni u oblaku i mogu se jednostavno povući stvaranjem drugi virtualne polja i korištenje ga kao ciljni uređaj za na DR.

### <a name="deactivate"></a>Deaktiviranje

Kada deaktivirate StorSimple polja za virtualne sever veze između uređaja i odgovarajuće StorSimple Upravitelj servisa. Deaktivacija je **trajno** operacija i nije moguće poništiti. Deaktivirana uređaj ne može biti registriran sa servisom StorSimple Upravitelj ponovno. Dodatne informacije potražite [deaktiviranje i brisanje vaše StorSimple virtualne polja](storsimple-deactivate-and-delete-device.md).

Sljedeće najbolje prakse Imajte na umu prilikom deaktivacija sustava virtualne polja:

-   Stvorite snimku oblaka sve podatke prije deaktivacije virtualnog uređaja. Kada deaktivirate virtualne polja, gubi se svi podaci lokalnim uređajem. Izradi oblaka snimke će vam omogućiti oporavak podataka u noviji fazi.

-   Prije nego što deaktivirate polja za virtualne StorSimple, provjerite je li da biste zaustavili ili izbrisali klijenti i domaćini koje ovise o tom uređaju.

-   Ako više ne koristite tako da ga ne ako skupi naknade, izbrišite deaktiviran uređaja.

### <a name="monitoring"></a>Nadzor

Da biste bili sigurni da vaše StorSimple virtualne polja u neprekinuti dobar stanju, morate pratiti polja i provjerite je li primati informacije iz sustava uključujući upozorenja. Da biste pratili stanje virtualne polja, implementirati sljedeće najbolje prakse:

- Konfiguriranje nadzora za praćenje korištenje diska na disku virtualne polja podataka, kao i na disku OS. Ako je pokrenut Hyper-V, kombinacije sustava centra virtualnog računala Manager (SCVMM) i sustava centra operacije Manager (SCOM) možete koristiti za praćenje na virtualizacije domaćini.   

- Konfiguriranje obavijesti e-poštom na vaše virtualne polja slanje upozorenja na određene korištenje razinama.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Indeks pretraživanja i virusa skeniranje aplikacije

Polja za virtualne StorSimple možete automatski tier podatke iz lokalnog sloju oblak Microsoft Azure. Kada se aplikacija kao što su pretraživanje kazala ili skeniranje virusa koristi da biste pregledali podatke spremljene na StorSimple, morate izvršiti da oblaka podaci neće dobiti pristupiti i povlače natrag na lokalni sloju.

Preporučujemo da implementirati sljedeće najbolje prakse prilikom konfiguriranja indeks pretraživanja ili antivirusnog pregleda na vašem virtualne polja:

-   Onemogući sve operacije automatski konfigurirana potpuni pregled.

-   Za tiered količine konfiguriranje indeks pretraživanja ili virusa pregleda aplikacije da biste izvršili rastuća pregleda. To želite pregledati samo na nove podatke vjerojatno residing na lokalni sloju. Tijekom operacije rastuće ne pristupiti podacima koji je tiered u oblak.

-   Provjerite je li ispravne filtri i postavke konfigurirani tako da se skenirana samo svrhu vrste datoteka. Na primjer, slikovne datoteke (JPEG, GIF i TIFF), a zatim inženjerska crteže treba nije moguće skenirati tijekom izvođenje rastuće ili cijeli indeks.

Ako koristite Windows indeksiranje procesa, slijedite ove smjernice:

-   Korištenje komponente za indeksiranje Windows za tiered količine kao što je opoziv velikih količina podataka (TBs) iz oblaka ako indeksu mora se izgraditi često. Ponovna izgradnja indeksa želite dohvatiti sve vrste datoteka za indeksiranje njihov sadržaj.

-   Koristite Windows indeksiranje postupak za lokalno prikvačene količine kao što je to želite pristupiti podacima na lokalni razine da biste sastavili indeksa (oblaka podataka nije moguće pristupiti).

### <a name="byte-range-locking"></a>Zaključavanje raspon bajt

Aplikacije možete zaključati određenog raspona bajtova unutar datoteke. Ako je omogućeno bajt raspon zaključavanja na aplikacije koje pišete vaše StorSimple, zatim tiering ne funkcionira na vašem virtualne polja. Za tiering rad sva područja datoteke u kojima se pristupa mora biti otključane. Zaključavanje raspon bajt nije podržano s tiered količine na vašem virtualne polja.

Preporučena mjere da ćete bajt raspon zaključavanje obuhvaćaju sljedeće:

-   Isključite bajtni raspon zaključavanje logike aplikacije.

-   Korištenje lokalno prikvačene količine (umjesto tiered) za podatke povezane s ovog računala. Lokalno prikvačene jedinicama ne sloju u oblak.

-   Kada pomoću lokalno prikvačene količine s bajt raspon zaključavanje omogućena, jedinica mogu potjecati online prije dovršetka vraćanja. U ove instance, morate pričekati vraćanja bude dovršena.

## <a name="multiple-arrays"></a>Više polja

Više polja virtualne možda će biti potrebno da biste se uvesti na račun za sve veći radni skup podataka koji nije spill premjestite u oblak tako utječu na performanse uređaja. U ove instance je najbolje mjerilo uređajima kao što je radni skup poveća. Potreban je jedan ili više uređaja potrebno dodati u centru za lokalne podatke. Prilikom dodavanja uređaja, nije vam:

-   Podjela trenutni skup podataka.
-   Implementacija novi radnih opterećenja novi uključen.
-   Ako implementirate više virtualne polja, preporučujemo da iz ujednačavanje opterećenja perspektive raspodijelite polja na različitim hypervisor domaćini.

-  Više virtualne polja (kada je konfiguriran kao poslužitelj datoteka ili poslužitelju komponente iSCSI) može uvesti u naziva sustava Distributed datoteke. Detaljne upute, idite na [Distributed datoteka sustava prostor naziva rješenje s vodič za implementaciju hibridnog oblaka za pohranu](https://www.microsoft.com/download/details.aspx?id=45507). Raspodijeljeno replikaciju datoteka sustava trenutno ne preporučuje se za korištenje s virtualne polja. 


## <a name="see-also"></a>Vidi također
Saznajte kako [upravljati virtualne polja vašem StorSimple](storsimple-ova-manager-service-administration.md) putem servisa StorSimple Upravitelj.
