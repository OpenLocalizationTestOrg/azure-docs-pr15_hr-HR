<properties 
   pageTitle="Neuspjeh natrag VMware virtualnim strojevima i fizičke poslužitelje s Azure VMware (naslijeđeno) | Microsoft Azure" 
   description="U ovom se članku opisuje kako se neće vratiti VMware virtualnog računala koje je omogućeno replicirati na Azure s oporavak Azure web-mjesta." 
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
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Neuspjeh natrag VMware virtualnim strojevima i fizičke poslužitelje s Azure VMware s oporavak Azure web-mjesta (naslijeđeno)

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-failback-azure-to-vmware.md)
- [Azure klasični Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klasični Portal (naslijeđeno)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md)

## <a name="overview"></a>Pregled

U ovom se članku opisuje način uvoza natrag VMware virtualnim strojevima sa sustavom Windows/Linux fizičke poslužitelje i iz Azure na web-mjesto lokalnog nakon što ste replicirati lokalno mjesto za Azure.

>[AZURE.NOTE] U ovom se članku opisuju naslijeđene scenarij. Upute trebali biste koristiti samo u ovom članku ako replicirati na Azure pomoću [ove naslijeđene upute](site-recovery-vmware-to-azure-classic-legacy.md). Ako postavite replikacije pomoću [poboljšane implementacije](site-recovery-vmware-to-azure-classic-legacy.md) , a zatim slijedite upute u [ovom članku](site-recovery-failback-azure-to-vmware-classic.md) uvoza unatrag. 


## <a name="architecture"></a>Arhitektura

Ovaj dijagram predstavlja scenarij prebacivanje i failback. Plave crte su veze koristi tijekom prebacivanje. Crvene crte su veze koristi tijekom failback. Reci sa strelicama prijeđite putem Interneta.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Prije početka 

- Koje treba nije uspjela putem VMware VMs ili fizičke poslužitelja, a trebao bi biti pokrenut u Azure.
- Imajte na umu da vam samo uspijeva natrag VMware virtualnim strojevima i Windows/Linux fizičke poslužitelja iz Azure na virtualnim računalima sustava VMware na lokalni primarni web-mjestu.  Ako vam se ne uspijeva natrag fizičke stroj, prebacivanje na Azure pretvorit će se u VM Azure i failback za VMware pretvorit će se VMware VM.

Evo kako postaviti failback:

1. **Postavljanje failback komponente**: morat ćete postaviti vContinuum poslužitelja lokalnim i pokažite na poslužitelj za konfiguraciju VM u Azure. Također postavit ćete poslužitelj za postupak kao programa Azure VM slanje podataka natrag u lokalni poslužitelj glavni cilj. Poslužitelj za konfiguraciju rukovati na prebacivanje registrirate poslužitelj za postupak. Instalacija programa lokalnog poslužitelja glavni cilj. Ako vam je potreban Windows server osnovne cilj je postavljen automatski kada instalirate vContinuum. Ako vam je potrebna Linux morat ćete ga ručno postaviti na istom poslužitelju.
2. **Omogućivanje zaštitu i failback**: nakon što ste postavili komponente, u vContinuum morat ćete omogućiti zaštitu za nije uspjela tijekom Azure VMs. Će se pokrenuti provjeru spremnosti na na VMs i pokretanje programa prebacivanje s Azure na web-mjesto lokalnog. Po završetku failback reprotect lokalnog računala da bi je početi replikaciju Azure.



## <a name="step-1-install-vcontinuum-on-premises"></a>Korak 1: Instalacija vContinuum lokalnog

Morat ćete instalirati poslužitelj vContinuum lokalno i pokažite na poslužitelj za konfiguraciju.

1.  [Preuzmite vContinuum](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Zatim ga preuzmite [Ažuriranje vContinuum](http://go.microsoft.com/fwlink/?LinkID=533813) verziju.
3. Instalirajte najnoviju verziju vContinuum. Na stranici **dobrodošlice** kliknite **Dalje**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Na prvoj stranici čarobnjaka navedite IP adresu poslužitelja CX i priključak poslužitelja CX. Odaberite **koristi HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Pronađite poslužitelj za konfiguraciju IP adresu na kartici **nadzorne ploče** poslužitelja za konfiguraciju VM u Azure.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Poslužitelj za konfiguraciju pronaći HTTPS javno priključak na kartici **krajnje točke** poslužitelja za konfiguraciju VM Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Na stranici **Detalji o pristupni izraz CS** navedite pristupni izraz koji ste zabilježili dolje kada registrirate poslužitelj za konfiguraciju. Ako se ne sjećate je prijavite **C:\\programske datoteke (x86)\\InMage sustavi\\privatne\\connection.passphrase** na poslužitelj za konfiguraciju VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  Na stranici **Odabir mjesta odredište** navedite mjesto na koje želite instalirati vContinuum server, a zatim kliknite **Instaliraj**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Kada se instalacija dovrši, možete pokrenuti vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Korak 2: Instalirajte poslužitelj za postupak u Azure 

Morate instalirati poslužitelj za postupak u Azure tako da se VMs u Azure možete poslati podatke natrag na lokalni poslužitelj glavni cilj. 

1.  Na stranici za **Konfiguraciju poslužitelja** u Azure odaberite da biste dodali novi poslužitelj za postupak.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Navedite poslužitelj za postupak ime i ime i lozinku za povezivanje s virtualnog računala kao administrator. Odaberite poslužitelj za konfiguraciju na koji želite da biste registrirali poslužitelj za postupak. To mora biti isti poslužitelj koristite da biste zaštitili i neće funkcionirati na virtualnim računalima.
3.  Navedite Azure mrežu u koju želite uvesti na poslužitelj za postupak. Mora biti u istoj mreži kao poslužitelj za konfiguraciju. Navedite jedinstveni IP podmreže za odabranu adresu i počnite implementacije.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Zadatak se pokreće za implementaciju poslužitelja postupak.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Kada poslužitelj za postupak je uveden u Azure možete prijaviti na poslužitelj pomoću vjerodajnica koje ste naveli. Registrirajte se poslužitelj za postupak na isti način kao i registrirati lokalni poslužitelj za postupak. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] Poslužitelji registrirana tijekom failback neće biti vidljivi u odjeljku Svojstva VM u oporavak web-mjesta. Samo su vidljive na kartici **poslužitelji** poslužitelja za konfiguraciju na koji ste registrirana. To može potrajati oko 10 do 15 minuta dok ih poslužitelj za postupak pojavljuje se na kartici.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Korak 3: Instalacija informacije o lokalnom poslužitelju za osnovne cilj

Ovisno o operacijskom sustavu virtualnim strojevima izvora, morate instalirati na Linux ili Windows server osnovne cilj lokalno.

### <a name="deploy-a-windows-master-target-server"></a>Implementacija Windows server osnovne cilj

Glavni cilj Windows već vezanoj instalaciji s postavljanjem vContinuum. Kada instalirate na vContinuum, glavnog poslužitelja je implementiran na istom računalu i registriran za poslužitelj za konfiguraciju.

1.  Da biste započeli implementaciju, stvorite prazan lokalnog na glavnom ESX na koji želite da biste vratili na VMs iz Azure na računalu.

2.  Provjeriti postoje li barem dva diskova priložiti u VM – se koristi za operacijski sustav i ostale služi za pogon zadržavanja.

3.  Instalirajte operacijski sustav.

4.  Instalirajte na vContinuum na poslužitelju. Time se i dovršava instalaciju osnovne odredišni poslužitelj.

### <a name="deploy-a-linux-master-target-server"></a>Implementacija poslužitelja za osnovne cilj Linux

1.  Da biste započeli implementaciju, stvorite prazan lokalnog na glavnom ESX na koji želite da biste vratili na VMs iz Azure na računalu.

2.  Provjeriti postoje li barem dva diskova priložiti u VM – se koristi za operacijski sustav i ostale služi za pogon zadržavanja.

3.  Instalirajte operacijski sustav Linux. Glavni ciljni sustav Linux trebali biste koristiti LVM za korijenske ili zadržavanja prostori za pohranu. Poslužitelj za osnovne cilj Linux konfiguriran da biste izbjegli LVM particije/diskova otkrivanje prema zadanim postavkama.
4.  Particije koje možete stvoriti:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Izvršavanje u ispod koraci instalacije prije pokretanja instalacije glavni cilj.


#### <a name="post-os-installation-steps"></a>Objava OS korake za instalaciju

Da biste dobili SCSI ID's za svaku SCSI tvrdi disk u Linux virtualnog računala, omogućiti parametar "disk. EnableUUID = TRUE "na sljedeći način:

1. Isključivanje virtualnog računala.
2. Desnom tipkom miša kliknite stavku VM u lijevom oknu > **Uređivanje postavki**.
3. Kliknite karticu **Mogućnosti** . Odaberite **Dodatno\>Općenito stavke** > **Konfiguracije parametara**. Mogućnost **Konfiguracija parametara** dostupna je samo kada se računalo isključi.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Provjerava je li redak s **disk. EnableUUID** postoji. Ako ga ne, a postavljen na **False** postavite ga na **True** (nije velika i mala slova). Ako postoji i postavljen na true, kliknite **Odustani** i testiranje naredbu SCSI unutar operacijski sustav goste nakon što ste je pokrenuli prema gore. Ako ne postoji kliknite **Dodaj redak**.
5. Dodajte disk. EnableUUID u stupcu **naziv** . Postavite vrijednost kao TRUE. Nemoj dodati gornje vrijednosti uz dvostruke navodnike.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Preuzmite i instalirajte dodatnu paketa

Napomena: Provjerite je li sustav ima mogućnost povezivanja s Internetom prije nego što preuzmete i instalirate dodatne paketa.

\#yum instalacija -y xfsprogs perl lsscsi rsync wget kexec-alata

Ta naredba preuzimanja te 15 pakete CentOS 6.6 spremištu i instalira:

BC 1.06.95 1.el6.x86\_64. rpm

busybox 1.15.1 20.el6.x86\_64. rpm

elfutils-libs-0.158-3.2.el6.x86\_64. rpm

kexec – Alati – 2.0.0-280.el6.x86\_64. rpm

lsscsi 0,23 2.el6.x86\_64. rpm

lzo 2.03 3.1.el6\_5.1.x86\_64. rpm

perl 5.10.1 136.el6\_6.1.x86\_64. rpm

perl-modul-Uključiv-3.90-136.el6\_6.1.x86\_64. rpm

perl – Pod-Escapes-1.04-136.el6\_6.1.x86\_64. rpm
    
perl – Pod – jednostavno-3.13-136.el6\_6.1.x86\_64. rpm

perl-libs-5.10.1-136.el6\_6.1.x86\_64. rpm

perl-verzije-0.77-136.el6\_6.1.x86\_64. rpm

rsync 3.0.6 12.el6.x86\_64. rpm

snappy-1.1.0-1.el6.x86\_64. rpm

wget 1.12 5.el6\_6.1.x86\_64. rpm

Napomena: Ako na računalu izvor koristi Reiser ili XFS datotečnom sustavu korijenski ili pokretanjem uređaja, zatim sljedeće paketa treba preuzeti i instalirati Linux osnovne ciljnom prije zaštitu.

\#/usr/local CD-a

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#RPM - ivh kmod-reiserfs-0.0-1.el6.elrepo.x86\_64. rpm reiserfs-uslužni-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#RPM - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Primjena promjena prilagođena konfiguracija

Prije primjene tih promjena obavezno dovršite prethodne sekcije, a zatim slijedite ove korake:


1. Kopirajte RHEL 6 64 Sjedinjeno komuniciranje agenta binarni novostvorenu OS.

2. Pokrenite sljedeću naredbu da biste untar u binarni: **ciljni - zxvf \<naziv datoteke\> **

3. Pokrenite sljedeću naredbu za dodjelu dozvola: \# **chmod 755./ApplyCustomChanges.sh**

4. Pokrenuti skriptu: ** \# ./ApplyCustomChanges.sh**. Pokrenite skriptu samo jednom na poslužitelju. Ponovno pokrenite poslužiteljem nakon pokretanja skripte.



### <a name="install-the-linux-server"></a>Instalacija Linux poslužitelja


1. [Preuzmite](http://go.microsoft.com/fwlink/?LinkID=529757) instalacijsku datoteku.
2. Kopirajte datoteku da biste ciljnu Linux matrica virtualnog računala pomoću programa sftp klijentski program po izboru. Također možete prijavite se na Linux osnovne cilj virtualnog računala i koristiti wget za preuzimanje instalacijskog paketa navedene veze
3. Zapisnik premjestite koristi za virtualnog računala za Linux osnovne cilj poslužitelj programa ssh klijenta po izboru
4. Ako ste povezani s mrežom Azure na kojem je implementiran poslužitelj za osnovne cilj Linux putem veze za VPN-a pomoću internu IP adresu poslužitelja koje možete pronaći na kartici **nadzorne ploče** za virtualnog računala i priključak 22 za povezivanje s ciljnom osnovne Linux poslužitelja pomoću ljuske za sigurnu.
5. Ako se povezujete s poslužiteljem osnovne cilj Linux putem javne internetske veze pomoću poslužitelja osnovne cilj Linux javno virtualna IP adresa (na kartici **nadzorne ploče** virtualnih računala), a javno krajnja točka za stvara ssh prijava Linux servder.
6. Izdvajanje datoteka iz gzipped Linux osnovne cilj poslužitelja installer tar arhiva tako da pokrenete: *"ciljni – xvzf Microsoft ASR\_UA\_8.2.0.0\_RHEL6 64\"* iz imenika koja sadrži datoteku instalacijski program.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Ako dobivenih installer datoteke na neki drugi direktorij promjena Directory radi koji sadržaj na tar arhiviranje su izdvajati. Iz ovog puta imenika pokrenuti "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Kada se to od vas zatraži da biste odabrali temeljna funkcija odaberite **2 (matrica cilj)**. Druge mogućnosti interaktivne instalacije ostaviti na njihove zadane vrijednosti.
9. Pričekajte za instalaciju i nastavili i sučelje Config glavnog računala da se pojavi. Konfiguracija Hosta utility za osnovne sarget Linux poslužitelj je uslužni program naredbenog retka. Ne promijenite veličinu na ssh klijentski program prozora. Pomoću tipki sa strelicama odaberite željenu mogućnost **globalnih** i pritisnite ENTER na tipkovnici.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. U polje **IP** unesite interne IP adresu poslužitelja za konfiguraciju (koje mogu pronaći na kartici **nadzorne ploče** poslužitelja za konfiguraciju VM) i pritisnite ENTER. U **priključak** unesite **22** i pritisnite ENTER. 
11.  Ostavite **HTTPS koristi** kao **da**, a zatim pritisnite ENTER.
12.  Unesite pristupni izraz koja je stvorena na poslužitelj za konfiguraciju. Ako koristite PUTTY klijenta s računala za Windows da biste ssh da biste Linux virtualnog računala možete koristiti Shift + Insert za lijepljenje sadržaja međuspremnika. Kopiranje pristupni izraz u lokalnom međuspremnik pomoću Ctrl-C i koristiti Shift + Insert da biste ga zalijepili. Pritisnite ENTER.
13.  Pomoću tipke strelica desno da biste došli do zatvorite, a zatim pritisnite ENTER. Pričekajte za instalaciju je potrebno da biste dovršili.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Ako zbog nekog razloga uspio da biste registrirali poslužitelj za osnovne cilj Linux na poslužitelj za konfiguraciju To možete učiniti tako da ponovno tako da pokrenete glavno računalo za konfiguraciju iz /usr/local/ASR/Vx/bin/hostconfigcli (najprije morate da biste postavili dozvole za pristup tom imeniku tako da pokrenete chmod kao super korisnik).


#### <a name="validate-master-target-registration"></a>Provjerite valjanost Registracija osnovne cilj

Možete provjeriti da osnovne odredišni poslužitelj je registriran uspješno na poslužitelj za konfiguraciju u sigurnog oporavak Azure web-mjesta > **Poslužitelj za konfiguraciju** > **Detalji o poslužitelju**.

>[AZURE.NOTE] Nakon registracije osnovne odredišni poslužitelj, ako vam se prikaže konfiguracijske pogreške koje virtualnog računala možda je izbrisana iz Azure ili krajnje točke nije ispravno konfigurirano, to je jer iako Azure dndpoints konfiguraciju osnovne cilj otkrio kada osnovne cilj je uveden u Azure, to nije odnosi na lokalni osnovne ciljne informacije o lokalnom poslužitelju. To ne utječe na failback, a možete zanemariti te pogreške. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Korak 4: Zaštita virtualnim strojevima na lokalno mjesto

Morate štititi VMs web-mjesto lokalnog prije ponovno uspjeti.

### <a name="before-you-begin"></a>Prije početka

Kada je VM je uspjelo putem Azure, dodaje vrlo temp pogon za datoteke. To je dodatni pogon nužan obično ne tako da vaše nije uspjelo putem VM jer je možda već imate pogon Namjenska stranica datoteke. Prije nego počnete obrnutim zaštitu virtualnim strojevima, morate biti sigurni da pogonu je izvanmrežno tako da ga se ne mogu zaštititi. To na sljedeći način:

1.  Otvorite Upravljanje računalom i odaberite upravljanje pohranom tako da ga popisa diskova na Internetu i povezan s računalom.
2.  Odaberite privremene disk koji je povezan s računalom, a zatim odaberite da bi se prikazala je izvan mreže. 

### <a name="protect-the-vms"></a>Zaštita na VMs

1. Na portalu Azure Provjera stanja virtualnog računala da biste bili sigurni da ste uspjela iznad.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Pokrenite vContinuum na vašem računalu.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Kliknite **Novi zaštitu** i odaberite vrstu sustava operacije u 

4.  U novom prozoru koji se otvori odaberite **vrstu OS** > **Dohvaćanje detalja** za VMs želite ponovno uspjeti. U **primarni poslužitelj pojedinosti**Prepoznajte i odaberite virtualnim strojevima koji želite zaštititi. VMs navedene su u odjeljku naziv glavnog računala na vCenter koji su na prije prebacivanja u slučaju pogreške.
5.  Kada odaberete virtualnog računala da biste zaštitili (i ga već nije putem Azure) skočni prozor nudi dvije stavke za virtualnog računala. To je jer je poslužitelj za konfiguraciju otkrije dvije instance programa virtualnim strojevima registrirana na njega. Morate ukloniti unosa za lokalni VM tako da možete zaštititi točan VM. Da biste odredili točan Azure VM unos ovdje, možete je prijaviti Azure VM i prijeći na C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. U drscout.conf datoteke odredite ID glavnog računala U dijaloškom okviru vContinuum Zadrži stavke za koje pronaći ID glavnog računala na na VM. Izbrišite sve druge stavke. Da biste odabrali ispravnu VM može se odnositi na IP adrese. Na IP adresa raspon lokalnog bit će VM lokalnog.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Na poslužitelju vCenter zaustavite virtualnog računala. Možete i izbrisati s VMs lokalne.
7. Zatim odredite na koji želite zaštititi u VMs lokalnog poslužitelja MT. Da biste to učinili, povezati s poslužiteljem vCenter na koji želite ponovno uvoza.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Odaberite osnovne ciljnom poslužitelju koji se temelji na glavnom računalu na koje želite oporaviti na VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Navedite replikacije mogućnost za svaku virtualnih računala. Da biste to učinili morate odabrati oporavak strani spremištu podataka koji se VMs moguće oporaviti. U tablici navedene različite mogućnosti dostupne su vam trebaju za svaki VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Mogućnost** | **Mogućnost preporučuje se vrijednost**
    ---|---
    IP adresa procesa poslužitelja | Odaberite poslužitelj za postupak implementiran u Azure
    Veličina zadržavanja u MB| 
    Vrijednost zadržavanja | 1
    Dana sati | Dana
    Interval dosljednosti | 1
    Odaberite ciljni datastore | Spremištu podataka dostupni na web-mjestu za oporavak. Spremište podataka treba ima li dovoljno prostora, a biti dostupni na ESX glavno računalo na kojem želite da biste vratili virtualnog računala.


10. Konfiguriranje svojstava koja virtualnog računala će dobiti nakon prebacivanje na lokalno mjesto. Svojstva su navedene u tablici u nastavku.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Svojstvo** | **Pojedinosti**
    ---|---
    Mrežna konfiguracija| Za svaki mrežnog prilagodnika otkriven, odaberite ga, a zatim kliknite **Promjena** da biste konfigurirali failback IP adresa za virtualnog računala. 
    Hardverska konfiguracija| Odredite na procesora i memorije u VM. Postavke primjenjuje se na sve VMs želite zaštititi. Da biste odredili odgovarajuće vrijednosti za opterećenje procesora i memorije, odnose se na željenu veličinu IAAS VMs uloga i potražite u članku broj jezgri i memorije dodijeljeni.
    Zaslonsko ime | Nakon failback na lokalni možete preimenovati virtualnim strojevima onako kako će se pojavljuju u vCenter zalihama. Zadani naziv je naziv glavnog računala na računalo virtualnog računala. Da biste utvrdili naziv VM, možete se referirati na popis VM u grupi zaštita.
    Konfiguriranje NAT | U nastavku detalja

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Konfiguriranje postavki NAT

1. Da biste omogućili zaštitu virtualnim strojevima, dva kanala komunikacije morate uspostaviti. Prvi kanal je između virtualnog računala i poslužitelja postupak. Ovaj kanal prikuplja podatke iz sustava VM i šalje ga na poslužitelj za postupak koji šalje podatke osnovne odredišni poslužitelj. Ako poslužitelj za postupak i virtualnog računala da biste biti zaštićene na istom Azure virtualne mreže pa ne morate koristiti NAT postavke. U suprotnom odredite NAT postavke. Prikaz javnu IP adresu poslužitelja postupak Azure. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Drugi kanal je između poslužitelj za postupak i osnovne odredišni poslužitelj. Mogućnost korištenja NAT ili ne ovisi o korištenje VPN temelji veze ili komunikaciju putem Interneta. Nemojte odabrati NAt ako koristite VPN, ali samo ako koristite povezani s Internetom.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Ako još niste izbrisane virtualnim strojevima lokalnog kao što je navedeno a spremištu podataka ne uspijeva natrag da biste i dalje sadrži stare VMDK zatim morat ćete Pobrinite se da failback VM dobiva stvorene u novom mjestu. Da biste to učinili odaberite **Dodatne** postavke, a zatim odredite zamjenske mape da biste vratili u **Postavkama naziv mape**. Ostavite druge mogućnosti uz zadane postavke. Primjena postavki naziva mape na svim poslužiteljima.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Pokrenite provjeru spremnosti da biste bili sigurni da su virtualnim strojevima spremna za biti zaštićene natrag na lokalni. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Pričekajte da se završi. Ako je uspješno na sve VMs možete navesti naziv plan za zaštitu. Zatim kliknite **zaštiti** da biste započeli.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Možete pratiti napredak u vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. Nakon koraka **Aktiviranje zaštite planirate** Završi možete nadzirati VM zaštitu na portalu za oporavak web-mjesta.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Možete vidjeti status točno klikom na u VM te praćenje tijeku.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Priprema failback plan

Možete pripremiti plan failback pomoću vContinuum tako da se aplikacija se nije natrag na lokalno mjesto u bilo kojem trenutku. Planova oporavak vrlo su slične tarife za oporavak u oporavak web-mjesta.

1.  Pokretanje vContinuum, a zatim odaberite **Upravljanje tarife** > **oporaviti.** Vidjet ćete popis sve tarife kojim uvoza putem VMs. Koristite iste tarife za oporavak.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Odaberite tarifu za zaštitu i sve VMs koju želite oporaviti u njoj. Kad odaberete svaki VM vidjet ćete više detalja uključujući ESX poslužitelju ciljni i izvorni VM disk. Kliknite **Dalje** da biste započeli Čarobnjak za oporavak i odaberite VMs koju želite oporaviti.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Možete oporaviti utemeljene na više mogućnosti, no preporučujemo korištenje **Najnovije oznaka** i odaberite **Primijeni za sve VMs** da biste bili sigurni da će koristiti najnovije podatke iz virtualnog računala.
4. Pokretanje sustava **Provjera spremnosti.** To će provjeriti ako desno parametre konfigurirani tako da Omogući oporavak VM. Ako su sve provjere uspješan, kliknite **Dalje** . Ako niste u zapisniku i rješavanje pogrešaka.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  **Konfiguriranje VM** Pobrinite se da oporavak postavke pravilno postavljene. Postavke VM možete promijeniti ako je potrebno.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Pregledajte popis virtualnim strojevima koji će ih moći vratiti, a zatim odredite redosljed oporavak. Imajte na umu da su VMs navedene pomoću naziv glavnog računala. Ponekad je teško mapiranje glavno računalo virtualnog računala.
Da biste mapirali imena, idite na virtualnim strojevima **nadzorne ploče** Azure i provjerite naziv glavnog računala VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Navedite naziv plan, a zatim odaberite **oporavak kasnije**. Preporučujemo da biste oporavili kasnije nakon početnog zaštitu možda neće biti potpuni. 
11. Kliknite da biste spremili plana ili pokrenuti oporavak ako ste odabrali da biste oporavili sada i ne kasnije **oporaviti** . Možete provjeriti status oporavak da biste vidjeli ako je spremljen u planu.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Oporavak virtualnim strojevima

Kada je stvorena tarifu, možete oporaviti virtualnih računala. Prije no što potvrdite virtualnim strojevima dovršite sinkronizacije. Ako replikacije status prikazana vrijednost u redu dovršetka zaštitu i ispunjeni RPO praga. Možete provjeriti stanje u VM svojstva.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Isključite Azure virtualnim strojevima prije nego što započnete oporavka. Na taj način postoji bez podjele početnog i korisnici samo pristupati jednu kopiju aplikacije. 


1.  Pokretanje spremljenog plan. U vContinuum odaberite **Monitor** tarifama sustava. To navedene sve tarife na koje ste pokrenuta.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Odaberite tarifu u **oporavak** , a zatim kliknite **Start**. Možete nadzirati oporavak. Nakon VMs pretvoren u možete povezati s njima u vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Zaštita za Azure ponovno nakon failback

Nakon dovršetka failback vjerojatno ćete htjeti ponovno zaštitite virtualnih računala. To na sljedeći način:

1.  Provjerite rade na virtualnim strojevima lokalne i jesu li aplikacija dostupna.
2.  Na portalu za oporavak Azure web-mjesta odaberite virtualnim strojevima i njihovo brisanje. Odaberite da biste onemogućili zaštitu virtualnih računala. Taj se način u VMs više su zaštićeni.
3.  U Azure izbrišite nije uspio putem Azure virtualnim strojevima
4.  Izbrišite stari virtualnog računala na vSpehere. To su VMs koje ste prethodno nije uspjela tijekom Azure.
5.  Na portalu za oporavak web-mjesta zaštiti VMs koja se nedavno nije uspjela iznad. Kada ih se zaštićeni možete ih dodati na tarifu za oporavak.
 
## <a name="next-steps"></a>Daljnji koraci



- [Doznajte](site-recovery-vmware-to-azure-classic.md) replikaciju VMware virtualnim strojevima i fizičke poslužitelje Azure pomoću poboljšane implementacije.

 
