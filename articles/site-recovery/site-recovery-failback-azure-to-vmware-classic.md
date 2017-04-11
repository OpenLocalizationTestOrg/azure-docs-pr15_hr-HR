<properties 
   pageTitle="Neuspjeh natrag VMware virtualnim strojevima i fizičke poslužitelja web-mjesto lokalnog | Microsoft Azure"
   description="Informirajte se o neuspjeh natrag na lokalno mjesto nakon prebacivanje VMware VMs i fizičke poslužitelji za Azure." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Nije uspjelo natrag VMware virtualnim strojevima i fizičke poslužitelji za lokalno mjesto

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-failback-azure-to-vmware.md)
- [Azure klasični Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klasični Portal (naslijeđeno)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



Članci opisuje način uvoza natrag Azure virtualnim strojevima iz Azure lokalnog web-mjesto. Kada budete spremni vratiti pogrešku uvoza VMware virtualnim strojevima sa sustavom ili Windows/Linux fizičke poslužitelja nakon što ih ste nije uspjela tijekom s lokalnim web-mjesta Azure pomoću ovaj [Praktični vodič](site-recovery-vmware-to-azure-classic.md), slijedite upute u ovom članku.



## <a name="overview"></a>Pregled

Ovaj dijagram prikazuje arhitektura failback za taj scenarij.

Pomoću ovog arhitektura kada poslužitelj za postupak je lokalno i koristite je ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Pomoću ovog arhitektura kada poslužitelj za postupak je na Azure, a imate VPN ili vezu s ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Da biste vidjeli popis svih priključci i na failback architechture dijagram odnose na slici u nastavku

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Evo kako funkcionira failback:

- Kada je pokazivač ste uspjelo Azure, ne vratite na web-mjesto lokalnog u nekoliko faza:
    - **Faza 1**: reprotect Azure VMs tako da pokrenu replikaciju natrag na VMware VMs radi s lokalnim web-mjesta. Omogućivanje reprotection prelazi na VM u grupi zaštita failback koja je stvorena automatski kada ste napravili grupi zaštita prebacivanje. Ako ste dodali prebacivanje CORBIS tarifu za oporavak zatim failback zaštitu grupi automatski dodan na željenu tarifu.  Tijekom reprotect odrediti planirati uvoza natrag.
    - **Faza 2**: kada su vaše Azure VMs replikaciju na web-mjesto lokalnog, pokrenete u Neuspjelo uvoza natrag od Azure.
    - **Faza 3**: nakon podataka nije uspjela natrag, reprotect VMs lokalnog koja se nije uspjela natrag na tako da pokrenu replikaciju Azure.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Failback izvorne ili zamjenski mjesto

Ako nije uspjelo putem VMware VM možete uspjeti natrag na isti izvor VM ako i dalje postoji lokalni. U ovom scenariju samo delta promjene će biti nije uspjelo natrag. Imajte na umu da:

- Neuspješne putem fizičke poslužitelje pa failback je uvijek novi VM VMware.
    - Prije nego što ponovno ne uspijeva fizičke strojno Imajte na umu da
    - Fizička računalo zaštićeno će vratiti kao virtualnog računala kada nije uspjela tijekom natrag od Azure VMware
    - Provjerite je li otkrijete barem jedan cilj matrica sever uz potrebne domaćini ESX/ESXi na koje ćete morati failback.
- Ako ne vratite izvornu VM sljedeće potreban je:
    - Ako na VM upravlja vCenter poslužitelja host ESX matrica ciljne moraju imati pristup VMs spremištu podataka.
    - Ako na VM uključen je ESX glavno računalo, no ne upravlja vCenter tvrdog diska na VM mora biti u na datastore pristupiti tako da na MT glavnog računala.
    - Ako vaš VM uključen je ESX glavno računalo, a ne koristite vCenter pa biste trebali izvršiti otkrivanje ESX mnoštvom na MT prije nego što ste reprotect. To vrijedi ako ste previše neuspješnih natrag fizičke poslužiteljima.
    - (Ako postoji VM lokalnog) je i mogućnost da biste izbrisali prije na failback. Zatim failback će stvoriti novi VM na glavnom računalu isti kao glavno računalo za ESX glavni cilj.
    
- Kada failback zamjenski mjesto podataka će oporaviti istom spremištu podataka i ESX glavnog računala isti kao koja koristi lokalni poslužitelj glavni cilj. 


## <a name="prerequisites"></a>Preduvjeti

- Morat ćete okruženju VMware da bi se neće uspjeti sigurnosno VMware VMs i fizičke poslužiteljima. Neuspjeh fizičke poslužitelj nije podržana.
- Da biste vratili uspjeti treba ste stvorili Azure mreže pri početku postavljanje protection. Failback mora VPN-a ili ExpressRoute veze s mrežom Azure u kojem se nalaze na web-mjesto lokalnog Azure VMs.
- Ako na VMs želite da se neće vratiti upravlja morat ćete provjerite je li poslužitelj za vCenter imate potrebne dozvole za otkrivanje VMs na poslužiteljima vCenter. [Dodatne informacije potražite u](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Ako su prisutne u VM snimke zatim reprotection neće uspjeti. Možete izbrisati snimki ili na diskova. 
- Prije nego što ponovno uspjeti morat ćete stvoriti broj komponente:
    - **Stvaranje poslužitelj za postupak u Azure**. Ovo je programa Azure VM koje ćete morati stvoriti i zadržati pokrenut tijekom failback. Na računalu možete izbrisati nakon failback.
    - **Stvaranje osnovne cilj poslužitelja**: osnovne odredišni poslužitelj šalje i prima failback podatke. Stvorite lokalni poslužitelj za upravljanje sadrži osnovne odredišni poslužitelj koji se instalira po zadanom. Međutim, ovisno o količinu nije uspjelo natrag promet možda potrebnih za stvaranje zasebnom osnovne cilj poslužitelj za failback.
    - Ako želite stvoriti poslužitelj za osnovne cilj koji se izvodi na Linux, morat ćete postaviti Linux VM prije instalacije osnovne odredišni poslužitelj prema uputama u nastavku.
- Poslužitelj za konfiguraciju je potrebna lokalnog kada radite s failback. Tijekom failback, virtualnog računala mora se nalaziti u konfiguraciji poslužitelja baze podataka, ne uspijeva koji ne failback biti uspješno. Dakle provjerite je li snimljenih običnog zakazano sigurnosno kopiranje poslužitelja. U slučaju Izrada, morat ćete vratiti s istom IP adresom tako da se failback će funkcionirati.

## <a name="set-up-the-process-server-in-azure"></a>Postavljanje postupak server Azure

Morate instalirati poslužitelj za postupak u Azure tako da se Azure VMs možete poslati podatke natrag na lokalni poslužitelj glavni cilj. 

1.  Na portalu za oporavak web-mjesta > **Konfiguraciju poslužitelja** odaberite da biste dodali novi poslužitelj za postupak.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Navedite naziv poslužitelja za proces, a zatim unesite ime i lozinku koje će se koristiti za povezivanje s VM Azure kao administrator. U **Konfiguraciji poslužitelja** odaberite lokalni poslužitelj za upravljanje i navedite Azure mrežu u koju želite uvesti na poslužitelj za postupak. Trebali biste mreže u kojem se nalaze Azure VMs. Navedite jedinstveni IP adresu iz odaberite podmreže i počnite implementacije.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Zadatak za implementaciju poslužitelja proces se pokrenuti

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Kada poslužitelj za postupak je uveden u Azure prijavite se na ga pomoću vjerodajnica koje ste naveli. Pokrenut će se prvi put se prijavite u dijaloškom okviru poslužitelja postupak. Upišite IP adresu poslužitelja za upravljanje lokalnim i njegov pristupni izraz. Ostavite je zadana postavka priključak 443. Možete ostaviti zadani 9443 priključak za replikaciju podataka osim ako ste posebno promijenili tu postavku prilikom postavljanja lokalni poslužitelj za upravljanje. 

    >[AZURE.NOTE] Poslužitelj neće biti vidljiv u odjeljku **Svojstva VM**. Vidljiv je samo na kartici **poslužitelji** u kojem ga je registrirana poslužitelju za upravljanje. Može potrajati o 10 do 15 minuta za poslužitelj za postupak da se pojavi.


## <a name="set-up-the-master-target-server-on-premises"></a>Postavljanje na glavni cilj informacije o lokalnom poslužitelju

Glavni odredišni poslužitelj prima failback podataka. Glavni cilj server automatski se instalira na poslužitelju za upravljanje lokalnim, no ako vam se ne uspijeva natrag velike količine podataka morat ćete postaviti poslužitelj za osnovne cilj. To na sljedeći način:

>[AZURE.NOTE] Ako želite instalirati poslužitelj osnovne cilj na Linux, slijedite upute iz sljedećeg postupka.

1. Ako instalirate osnovne ciljnom poslužitelju u sustavu Windows, otvorite stranicu brzi početak rada s VM na koji ste instalacije osnovne odredišni poslužitelj i preuzmite instalacijsku datoteku za korištenje čarobnjaka za postavljanje Sjedinjeno komuniciranje Azure web-mjesta oporavak.
2. Pokrenite instalacijski program, a **prije nego što počnete** odaberite **Dodaj dodatne postupak poslužitelji za promjenu veličine out implementacije**.
3. Dovršite čarobnjak na isti način kao i kada niste [postavili poslužitelju za upravljanje](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Na stranici **Detalji o poslužitelju za konfiguraciju** navedite IP adresu poslužitelja osnovne ciljne i pristupni izraz da biste pristupili na VM.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Postavljanje Linux VM kao glavni odredišni poslužitelj
Da biste postavili poslužitelju za upravljanje izvodi osnovne ciljnom poslužitelju kao Linux VM morat ćete instalirati na Cent) 6.6 S minimalnim operacijskom sustavu, dohvatiti SCSI ID-a za svaki SCSI tvrdi disk, instaliranje neke dodatne paketa i Primjena neke prilagođene promjene.

#### <a name="install-centos-66"></a>Instalacija CentOS 6.6

1.  Instalirajte CentOS 6.6 minimalnog operacijski sustav na poslužitelju za upravljanje VM. Imajte na ISO na DVD pogon, a zatim pokretanje sustava. Preskakanje testiranje medijskih sadržaja, pri jezik odaberite NAM engleski, odaberite **Osnovne uređaji za pohranu**, provjerite da tvrdi disk ne sadrži važne podatke i kliknite **da**, Odbaci sve podatke. Unesite naziv poslužitelja za upravljanje i odaberite poslužitelj mrežnog prilagodnika.  U dijaloškom okviru **Uređivanje sustava** odaberite** Automatsko povezivanje** i statičke IP adrese, mreže i postavke DNS-a. Određivanje vremenske zone i korijenski lozinku da biste pristupili poslužitelju za upravljanje. 
2.  Kad vas vrstu instalacije želite **Stvoriti prilagođeni izgled** kao odaberite particije. Nakon što kliknete **Dalje** odaberite **oslobađanje** i kliknite Stvori. Stvaranje **/**, **/var/crash** i **/ kućni particije** s **Vrsta FS:** **ext4**. Stvaranje partion Zamijeni kao **Vrsta FS: zamjena**.
3.  Ako postojećih uređaja nalaze pojavit će se poruka upozorenja. Kliknite **Oblikovanje** da biste oblikovali pogon s postavkama particije. Kliknite da biste primijenili promjene particija **Pisanje promjena na disk** .
4.  Odaberite **Instaliraj pokretanje loader** > **sljedeće** da biste na korijenskom instalirati učitavanje pokretanja.
5.  Nakon dovršetka instalacije kliknite **ponovno pokrenuti računalo**.


#### <a name="retrieve-the-scsi-ids"></a>Dohvaćanje SCSI ID-a

1. Nakon instalacije dohvatiti SCSI ID-a za svaki SCSI tvrdi disk u na VM. Da biste izvršili ovaj isključi poslužitelju za upravljanje VM, u svojstvima VM u VMware desnom tipkom miša kliknite stavku VM > **Uređivanje postavki** > **Mogućnosti**.
2. Odaberite **Napredno** > **Općenito stavku** i kliknite **Parametri konfiguracije**. Ta mogućnost bit će de-active kada je pokrenut na računalu. Da biste ga aktivirali mora biti zatvoren na računalu.
3. Ako redak **disk. EnableUUID** postoji Provjerite je li vrijednost postavljena na **True** (velika i mala slova). Ako već nije možete otkazati i testirati naredbu SCSI unutar goste operacijski sustav nakon što ste je pokrenuli. 
4.  Ako u retku ne postojeći kliknite **Dodaj redak** – i dodajte ga s vrijednost **True** . Nemojte koristiti dvostruke navodnike.

#### <a name="install-additional-packages"></a>Instaliranje dodatne paketa

Morat ćete preuzeti i instalirati neki dodatnih paketa. 

1.  Provjerite je li poslužitelj osnovne cilj povezan s Internetom.
2.  Pokrenite sljedeću naredbu da biste preuzeli i instalirali 15 paketa u spremištu CentOS: **# yum instalacija – y xfsprogs perl lsscsi rsync wget kexec-alata**.
3.  Ako izvor strojeva ste zaštita koristite Linux završava Reiser ili XFS datotečnog sustava korijenske ili pokretanjem uređaja, a zatim treba preuzmete i instalirate dodatne paketa na sljedeći način:

    - # <a name="cd-usrlocal"></a>/usr/local CD-a
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>RPM – ivh kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm reiserfs-uslužni-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>RPM – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Primjena prilagođenog promjena

Učinite sljedeće da biste primijenili prilagođeni promjene nakon što ste dovršili korake nakon instalacije i instalirali pakete:

1.  Kopirajte RHEL 6 64 Sjedinjeno komuniciranje agenta binarni na VM. Pokrenite sljedeću naredbu da biste untar u binarni: **ciljni – zxvf <file name> **
2.  Pokrenite sljedeću naredbu za dodjelu dozvola: **# chmod 755./ApplyCustomChanges.sh**
3.  Pokrenuti skriptu: **#./ApplyCustomChanges.sh**. Pokrećite samo skriptu jedanput. Ponovno pokrenete poslužiteljem nakon pokretanja skripte uspješno.


## <a name="run-the-failback"></a>Pokretanje sustava failback

### <a name="reprotect-the-azure-vms"></a>Reprotect Azure VMs

1.  Na portalu za oporavak web-mjesta > kartica **strojeva** odaberite VM koji se nije uspjela tijekom, a zatim kliknite **ponovno zaštiti**.
2.  U **Glavni poslužitelja** i **Poslužitelj za postupak** odaberite lokalnog poslužitelja osnovne cilj i poslužitelj za Azure VM postupak.
3.  Odaberite račun koji ste konfigurirali za povezivanje s VM.
4.  Odaberite verziju failback grupe za zaštitu. Za primjer ako je na VM zaštićen u PG1, zatim morat ćete odabrati PG1_Failback.
5.  Ako želite vratiti na drugo mjesto, odaberite pogon zadržavanja i datastore konfiguriran za osnovnu odredišni poslužitelj. Kada ne vratite na web-mjesto lokalnog VMware VMs u planu zaštitu failback će koristiti isti spremištu podataka kao glavni odredišni poslužitelj. Ako želite oporaviti replike Azure VM jednaka lokalnog VM, a zatim VM lokalnog trebalo bi biti u istom spremištu podataka kao glavni odredišni poslužitelj. Ako nema VM lokalnog tijekom reprotection stvorit će se novi.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Kada kliknete **u redu** da biste započeli reprotection posao započinje za replikaciju u VM iz Azure lokalno mjesto. Na kartici **Zadaci** možete pratiti napredak.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Pokrenite je prebacivanje na lokalno mjesto

Nakon reprotection na VM se premješta u failback verziju njegove grupe zaštitu i automatski se dodaju plan oporavak koji se koristi za prebacivanje na Azure ako postoji.

1.  Na stranici **Oporavak tarife** odaberite tarifu oporavak koja sadrži grupe failback i kliknite **Prebacivanje** > **Neplanirano prebacivanje**.
2.  U **Potvrdi prebacivanje** provjerite je li prebacivanje smjera (Azure), a zatim odaberite oporavak točke koji želite koristiti za prebacivanje (najnoviji). Ako je omogućeno **Više VM** kada konfigurira svojstva replikacije možete oporaviti najnoviju aplikaciju ili rušenje dosljedan oporavak točke. Kliknite kvačicu da biste započeli s pomoćnim.
3.  Tijekom prebacivanje oporavak web-mjesta će se isključiti Azure VMs. Kada potvrdite taj failback dovrši očekivani koje možete možete provjeriti da Azure VMs imaju isključen prema očekivanjima.

### <a name="reprotect-the--on-premises-site"></a>Reprotect lokalno mjesto

Nakon dovršetka failback podataka će se vratiti na lokalno mjesto, ali neće biti zaštićene. Da biste pokrenuli replikaciju Azure ponovno učinite sljedeće:

1.  Na portalu za oporavak web-mjesta > odaberite karticu **strojeva** VMs koji nije uspjela sigurnosno, a zatim **ponovno zaštitite**. 
2.  Kada potvrdite da replikacija za Azure je funkcioniraju kako želite, u Azure možete izbrisati Azure VMs (trenutno nije pokrenut) koje su nije uspjela natrag.


### <a name="common-issues-in-failback"></a>Uobičajeni problemi u failback

1. Ako izvođenje otkrivanje vCenter korisnik samo za čitanje i zaštiti virtualnim strojevima, ga uspješnog i prebacivanje funkcionira. U vrijeme Reprotect ga neće uspjeti jer se datastores ne može otkriti. Kao na simptoma nećete vidjeti datastores naveden tijekom ponovno zaštitu. Da biste riješili taj problem, možete ažurirati vjerodajnica vCenter s odgovarajućim računa s dozvolama i ponovite zadatak. [Saznajte više](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Kada failback Linux VM i pokretanje lokalno, vidjet ćete da deinstalirate mreže Upravitelj paketa na računalu. To je zato kada je na VM oporaviti u Azure, uklanja se paket Upravitelj mreže.
3. Kada je VM je konfiguriran pomoću statičke IP adrese, a nije uspio putem Azure, IP adresa je nabaviti putem DHCP. Kada neće uspjeti putem natrag da biste lokalno, na VM i dalje koristiti DHCP za dohvaćanje IP adrese. Morat ćete ručno prijava u računalo i postavljanje IP adresu natrag u statični adresu po potrebi.
4. Ako koristite ESXi 5,5 besplatne edition ili vSphere 6 Hypervisor besplatne edition, prebacivanje bi uspjela, ali failback neće uspjeti. Će ned za nadogradnju na ili licenca za procjenu da biste omogućili failback.

## <a name="failing-back-with-expressroute"></a>Neispravni natrag s ExpressRoute

Možete se neće vratiti putem veze za VPN-a ili Azure ExpressRoute. Ako želite koristiti ExpressRoute bilješke na sljedeći način:

- ExpressRoute trebali postaviti na Azure virtualne mreže na izvorne strojeva Neuspjelo pokazivač, a zatim u koje Azure VMs nalaze se nakon što se pojavljuje u prebacivanje.
- Podatke je replicirati na račun Azure prostora za pohranu na javno krajnjoj točki. Trebali biste postaviti javno peering u ExpressRoute s ciljnom podatkovnog centra za replikaciju oporavak web-mjesta da biste koristili ExpressRoute.



