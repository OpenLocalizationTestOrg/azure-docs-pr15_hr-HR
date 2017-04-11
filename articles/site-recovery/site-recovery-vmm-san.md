<properties
    pageTitle="Replicirati Hyper-V VMs u VMM oblaka sekundarne web-mjesto s oporavak web-mjesta za Azure pomoću SAN | Microsoft Azure"
    description="U ovom se članku opisuje kako replicirati Hyper-V virtualnim strojevima između dva web-mjesta s Azure pomoću SAN replikacije oporavak za web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-vms-in-a-vmm-cloud-to-a-secondary-site-with-azure-site-recovery-using-san"></a>Replicirati Hyper-V VMs u VMM oblaka sekundarne web-mjesto s Azure pomoću SAN oporavak za web-mjesta

U ovom članku ćete saznati kako implementirati oporavak web-mjesta orkestrirali i automatizaciju SAN replikacije i prebacivanje za Hyper-V virtualnih računala koja se nalazi u VMM centar sustava oblaka sekundarne VMM web-mjesto.

Nakon čitanje u ovom se članku objavljuju komentare ni pitanja pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr). 


## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju tvrtke continuity i Izrada oporavak (BCDR) strategije koji je određuje način aplikacije, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana nedostupnost i čim oporavak normalno funkcionira uvjeta. Vaše BCDR strategije centra oko rješenja koja zadržati poslovnih podataka sigurnih i koje se mogu vratiti i radnih opterećenja neprestano dostupne kada se pojavi Izrada.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarnog web-mjesta da biste zadržali aplikacije i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Oporavak web-mjesta mogu koristiti u broj slučajevi, a možete zaštititi broj radnih opterećenja. Dodatne informacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)

Ovaj članak sadrži upute za postavljanje replikacije Hyper-V VMs iz jednog VMM web-mjesta na drugo pomoću SAN relication. Sadrži pregled programa arhitekture i preduvjeti za implementaciju i upute. Ćete otkrivanje i klasifikaciju SAN prostor za pohranu u VMM, nakon dodjele resursa LUNs, i dodijeliti prostora za pohranu za klastere Hyper-V. Završi tako da testirate prebacivanje da biste provjerili funkcionira sve prema očekivanjima.


## <a name="why-replicate-with-san"></a>Zašto je replicirati s SAN?

Evo što sadrži scenarij:

- Nudi rješenje skalabilni replikacije enterprise koja se automatski tako da oporavak web-mjesta.
- Prednost SAN vodi mogućnostima replikacije koje ste dobili od partnera za pohranu enterprise preko oba fibre kanal i iSCSI prostora za pohranu. U odjeljku našim [partnerima SAN prostora za pohranu](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).
- Upravlja postojeću SAN infrastrukturu za zaštitu ključnih zaštita njihove privatnosti ovise aplikacije u uveden u klastere Hyper-V.
- Pruža podršku za klastere za goste.
- Provjerava dosljednost replikacije preko različite razine aplikacije sa sinkroniziranim replikacije za manje RTO i RPO i asynchronized replikacije za visoke fleksibilnost, ovisno o mogućnostima polja za pohranu.  
- Integrira VMM omogućuje upravljanje SAN u konzoli za VMM i SMI osnovna funkcija Instalacijskog do nedjelje u VMM otkrije postojeće prostora za pohranu.  

## <a name="architecture"></a>Arhitektura

Ovaj scenarij štiti vaše radnih opterećenja tako sigurnosno kopiranje Hyper-V virtualnim strojevima iz jednog lokalnog VMM web-mjesta na drugo pomoću SAN replikacije.

![Arhitektura SAN](./media/site-recovery-vmm-san/architecture.png)

Komponente u ovom scenariju obuhvaćaju sljedeće:

- **Lokalni virtualnim strojevima**– vaše lokalne poslužitelje Hyper-V kojima upravlja VMM privatne oblaka sadrže virtualnim strojevima koji želite zaštititi.
- **VMM lokalne poslužitelje**– možete učiniti jednu ili više VMM poslužitelje s programom primarni web-mjesta na koje želite zaštititi, a zatim na sekundarnog web-mjesta.
- **Prostor za pohranu SAN**– A SAN polja na primarni web-mjesta, a druga u sekundarnog web-mjesta
-  **Oporavak web-mjesta Azure sigurnog**– na sigurnog koordinate i orchestrates replike podataka, prebacivanje i oporavak između vaše lokalne web-mjesta.
- **Davatelj usluga za oporavak web-mjesta za Azure**– davatelja je instalirana na svakom VMM poslužitelju.

## <a name="before-you-start"></a>Prije početka

Provjerite je li te preduvjeti na mjestu:

**Preduvjeti** | **Pojedinosti** 
--- | ---
**Azure**| Potreban vam je račun sustava [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta. 
**VMM** | Potreban vam je barem jedan VMM poslužitelj implementiran kao samostalni fizički ili virtualni poslužitelj ili virtualne klaster. <br/><br/>Poslužitelj VMM trebao bi biti pokrenut sustava centra 2012 R2 s najnovijim kumulativnim ažuriranjima.<br/><br/>Potreban vam je barem jedan oblaka konfiguriran na primarni VMM poslužitelju koji želite zaštititi i jedan oblaka konfiguriran na sekundarnom VMM poslužitelju koji želite koristiti za zaštitu i oporavak<br/><br/>Oblak izvora koji želite zaštititi mora sadržavati jednu ili više grupa VMM glavnog računala.<br/><br/>Sve VMM oblaka, morate imati profil Hyper-V kapaciteta postavljen.<br/><br/>Dodatne informacije o postavljanju oblaka VMM u [konfiguraciji oblaka tkanina VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [vodič: Stvaranje privatne oblaka pomoću sustava centra 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).
**Hyper-V** | Potreban vam je jedan ili više Hyper-V klaster primarnih i sekundarnih web-mjesta i jedan ili više VMs na klasteru Hyper-V izvora. VMM grupe glavnog računala na mjestima primarnih i sekundarnih trebali biste dobiti jednu ili više Hyper-V klastere u svakoj grupi.<br/><br/>Hyper-V poslužitelje glavnog i ciljnog morate imati barem Windows Server 2012 s ulogom Hyper-V i instalirali najnovija ažuriranja.<br/><br/>Bilo koji poslužitelj Hyper-V koja sadrži VMs koji želite zaštititi moraju nalaziti u oblaku za VMM.<br/><br/>Ako koristite Hyper-V u bilješci klaster te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Morat ćete ručno konfiguriranje broker klaster. [Dodatne informacije potražite](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) u blogu Aidan Finn.
**Prostor za pohranu SAN** | Korištenje SAN replikacije mogli ponoviti goste grupirani virtualnim strojevima sa iSCSI ili prostora za pohranu za kanal fibre ili pomoću zajedničkog virtualne tvrdom disku (vhdx).<br/><br/>Potreban vam je SAN dvama postavljanje, jednu primarnu web-mjestu, a druga u sekundarne.<br/><br/>Mrežne infrastrukture trebali postaviti između poljima. Peering ni replikacije mora biti konfigurirana. Licence za replikaciju trebali postaviti skladu s potrebama polja za pohranu.<br/><br/>Povezivanje s mrežom trebaju imati postavljene između glavnog računala poslužitelja Hyper-V i polja za pohranu bi domaćini možete komunicirati s LUNs prostora za pohranu pomoću ISCSI ili Fibre kanala.<br/><br/> Provjerite popis [podržana polja za pohranu](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/>SMI osnovna funkcija Instalacijskog S davatelje usluga koje ste dobili od proizvođača polja za pohranu potrebno instalirati, a polja SAN se upravlja davatelj usluge. Postavljanje davatelja usluge u skladu s njihovu dokumentaciju.<br/><br/>Provjerite je li usluga SMI osnovna funkcija Instalacijskog S polja na poslužitelju koji poslužitelj VMM možete pristupiti putem mreže IP adresa ili FQDN.<br/><br/>Svaki SAN polja trebali biste dobiti jednu ili više prostora za pohranu grupe dostupan za upotrebu ovog implementacije. Poslužitelj VMM na web-mjestu primarni ćete morati upravljanje primarnog polja i sekundarni poslužitelj VMM će upravljati sekundarne polja.<br/><br/>Poslužitelj VMM na web-mjestu primarni treba upravljanje primarnog polja i sekundarni poslužitelj VMM treba upravljanje sekundarne polja.
**Preslikavanje** | Možete konfigurirati preslikavanje da biste bili sigurni da repliciranu virtualnim strojevima optimalnog smještaju se na sekundarnom Hyper-V glavnog računala poslužitelja nakon prebacivanje, a možete povezati s odgovarajućim VM mrežama. Ako ne konfigurirate mreže mapiranja replike VMs neće biti povezani s mreže nakon prebacivanje.<br/><br/>Da biste postavili mrežu mapiranja tijekom implementaciju provjerite virtualnim strojevima na izvornom Hyper-V poslužitelju glavno računalo povezani s mrežom VMM VM. Tu mrežu može povezati logičke mreže koji je pridružen oblaka. < br /<br/>Oblak cilj na sekundarnom VMM poslužitelja koje koristite za oporavak trebala bi biti odgovarajuće VM mreža konfigurirana, pa ga shodno trebala biti vezana odgovarajuće logičke mrežu koji je pridružen oblaka ciljne.<br/><br/>[Saznajte više](site-recovery-network-mapping.md) o preslikavanje.


## <a name="step-1-prepare-the-vmm-infrastructure"></a>Korak 1: Priprema infrastrukture VMM

Da biste pripremili infrastruktura za VMM morate:

1. Provjerite je li postavili VMM oblaka
2. Integracija i klasifikaciju SAN prostor za pohranu u VMM
3. Stvaranje LUNs i dodijeliti prostora za pohranu
4. Stvaranje grupa replikacije
5. Postavljanje mreže za VM

### <a name="ensure-vmm-clouds-are-set-up"></a>Provjerite je li postavili VMM oblaka

Oporavak web-mjesta orchestrates zaštitu virtualnih računala koja se nalazi na poslužiteljima glavno računalo Hyper-V VMM oblaka. Morat ćete da biste bili sigurni da te oblaka pravilno postavljena prije nego što počnete implementacije oporavak web-mjesta. Dodatne informacije u [stvaranju privatne oblaka](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) na blogu Keith Mayer.

### <a name="integrate-and-classify-san-storage-in-vmm"></a>Integracija i klasifikaciju SAN prostor za pohranu u VMM

Dodavanje programa classify SANs na konzoli VMM:

1. U radnom prostoru **tkanina** kliknite **prostora za pohranu**. Kliknite **Polazno** > **Dodavanje resursa** > **Uređaji za pohranu** da biste pokrenuli čarobnjak za dodavanje prostora za pohranu uređaja.
2. Na stranici **Odabir vrste davatelj usluga za pohranu** , odaberite **SAN i NAS otkrivanja i upravlja davatelj SMI osnovna funkcija Instalacijskog S**.

    ![Vrsta davatelja usluga](./media/site-recovery-vmm-san/provider-type.png)

3. Na stranici **Navedite protokol i adresu davatelja usluga za pohranu SMI osnovna funkcija Instalacijskog S** odaberite **CIMXML SMI osnovna funkcija Instalacijskog S** , a zatim odredite postavke za povezivanje s davatelja usluga.
4. **Davatelj IP adresa ili FQDN** i **TCP/IP priključak**, odredite postavke za povezivanje s davatelja usluga. Možete koristiti SSL veze za SMI osnovna funkcija Instalacijskog S CIMXML samo.

    ![Povezivanje davatelja usluga](./media/site-recovery-vmm-san/connect-settings.png)

5. U **pokreće kao račun** odredite VMM Pokreni kao račun koji mogu pristupiti davatelja usluga ili stvorite novi račun.
6. Na stranici **Prikupite informacije** VMM pokušava automatsko otkrivanje i uvoz podataka uređaj za pohranu. Da biste pokušali ponovno otkrivanje, kliknite **Pregled davatelja**. Ako se postupak otkrivanja potvrdi, polja otkriven prostora za pohranu, grupe za pohranu, proizvođač, model i kapacitet navedene su na stranici.

    ![Uvod u prostor za pohranu](./media/site-recovery-vmm-san/discover.png)

7. **Odaberite grupe za pohranu za smještanje u odjeljku Upravljanje i dodjeljivanje klasifikacija**, odaberite grupe za pohranu VMM će upravljati i dodijeliti ih klasifikacija. Informacije o LUN će biti uvezeni iz grupe za pohranu. Stvaranje LUNs na temelju aplikacije morate zaštita njihove kapacitetom i obavezu što je potrebno za replikaciju zajedno.

    ![Klasifikaciju prostora za pohranu](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Stvaranje LUNs i dodijeliti prostora za pohranu

1. Kada je prostor za pohranu SAN je integriran u VMM možete stvoriti (nakon dodjele resursa) logičke jedinice (LUNs):

    - [Odabir načina za stvaranje logičke jedinice u VMM](https://technet.microsoft.com/library/gg610624.aspx)
    - [Dodjela resursa za pohranu logičke jedinice u VMM](https://technet.microsoft.com/library/gg696973.aspx)

    >[AZURE.NOTE] Nakon što ste replikacije Omogući za stroj bi trebalo dodate VHDs za njega LUNs koje se ne nalaze u grupi replikacije oporavak web-mjesta. Ako vidite neće se provjeravati po oporavak web-mjesta.

2. Zatim dodijeliti kapacitet pohrane glavno računalo klaster Hyper-V tako da se VMM možete implementirati virtualnog računala podataka Dodjela resursa za pohranu:

    - Prije nego što dodjeljivanje spremište klaster morate dodijeliti grupi VMM glavnog računala na kojem se nalazi klaster. Pogledajte [kako dodijeliti logičke jedinice za pohranu u grupu glavnog računala](https://technet.microsoft.com/library/gg610686.aspx) te [kako dodijeliti grupe za pohranu u grupu glavnog računala](https://technet.microsoft.com/library/gg610635.aspx). </a>.
    - Zatim dodijeliti kapacitet pohrane klaster kao što je opisano [kako konfigurirati za pohranu na glavno računalo klaster Hyper-V u VMM](https://technet.microsoft.com/library/gg610692.aspx). </a>.

### <a name="create-replication-groups"></a>Stvaranje grupa replikacije

Stvaranje grupe replikacije koja obuhvaća sve LUNs koje morate replicirati zajedno.

1. Na konzoli VMM otvorite karticu **Replikacije grupe** svojstava polja za pohranu, a zatim kliknite **Novo**.
2. Zatim stvorite grupu replikacije.

    ![Grupa SAN replikacije](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Postavljanje mreže

Ako želite da biste konfigurirali mapiranja za mrežu, učinite sljedeće:

1. [Doznajte](site-recovery-network-mapping.md) preslikavanje.
2. Priprema VM mreža u VMM:

    - [Postavljanje mreže za logičke](https://technet.microsoft.com/library/jj721568.aspx).
    - [Postavljanje mreže za VM](https://technet.microsoft.com/library/jj721575.aspx).

## <a name="step-2-create-a-vault"></a>Korak 2: Stvaranje na zbirke ključeva


1. Prijavite se na [Portal za upravljanje](https://portal.azure.com) poslužitelja VMM želite registrirati.

2. Proširite **Data Services** > **Oporavak usluge** kliknite **Sigurnog oporavak web-mjesta**.

3. Kliknite **Stvori novi** > **brzo stvaranje**.

4. U odjeljak **naziv**unesite neslužbeni naziv da biste odredili na zbirke ključeva.

5. U **području** odaberite regiji u zbirke ključeva. Da biste provjerili podržanih regija potražite u članku geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/).

6. Kliknite **Stvaranje sigurnog**.

    ![Nove zbirke ključeva](./media/site-recovery-vmm-san/create-vault.png)

Provjerite traku stanja da biste potvrdili na sigurnog uspješno je stvorena. Na sigurnog će biti navedena kao **aktivna** na glavnoj stranici **Servisa za oporavak** .


### <a name="register-the-vmm-servers"></a>Registrirajte se VMM poslužitelja

1. Otvorite stranicu za brzi početak rada sa stranice **Usluge za oporavak** . Brzi početak rada može se otvoriti u bilo kojem trenutku pomoću ikone.

    ![Ikona za brzi početak rada](./media/site-recovery-vmm-san/quick-start-icon.png)

2. Na padajućem popisu odaberite **između Hyper-V lokalnog web-mjesta pomoću replikacije polja**.

    ![Ključa za registraciju](./media/site-recovery-vmm-san/select-san.png)


3. Na **poslužiteljima Priprema VMM**, preuzmite najnoviju verziju instalacijsku datoteku za oporavak davatelja za Azure web-mjesta.
4. Pokrenite datoteku na s izvorišnim poslužiteljem VMM. Ako je VMM implementiran klasteru i instalirate davatelj usluga za prvo ga instalirati na aktivni čvor i dovršetak instalacije da biste registrirali VMM poslužitelja u sigurnog. Zatim instalirajte davatelja usluge na ostale čvorove. Imajte na umu da Ako nadograđujete davatelj morat ćete nadograditi na sve čvorove jer oni trebali biste sve moguće istu verziju davatelja usluga.
5. Instalacijski program ne nekoliko dozvole **Provjerite stara preduvjeti** i zahtjevi za zaustavljanje servisa VMM da biste započeli postavljanje davatelja usluga. Servis VMM će se ponovno pokrenuti automatski kada se dovrši instalacija. Ako ga instalirate na VMM klaster vas će se zatražiti da biste prestali klaster ulogu.
6. U **Programu Microsoft Update** možete odabrati u ima li ažuriranja. Ova postavka omogućena davatelja ažuriranja instalirat će se prema Pravilnik za Microsoft Update.

    ![Microsoftova ažuriranja](./media/site-recovery-vmm-san/ms-update.png)

7. Mjesto za instalaciju je postavljeno na ** <SystemDrive>\Programske sustava centra 2012 R2\Virtual strojno Manager\bin**. Kliknite gumb Instaliraj da biste pokrenuli instalaciju davatelja usluga.

    ![InstallLocation](./media/site-recovery-vmm-san/install-location.png)

8. Nakon instaliranja davatelja usluge kliknite 'registrirati gumb da biste registrirali poslužitelja u zbirke ključeva.

    ![InstallComplete](./media/site-recovery-vmm-san/install-complete.png)

9. U **Internetsku vezu** Navedite način na koji se davatelja usluge na poslužitelju VMM povezuje s Internetom. Odaberite **koristite zadane postavke proxy poslužitelja za sustav** da biste koristili na zadane postavke internetske veze konfiguriran na poslužitelju.

    ![Postavke internetske](./media/site-recovery-vmm-san/proxy-details.png)

    - Ako želite koristiti prilagođenu proxy ga trebali postaviti prije instalacije davatelja usluga. Kada konfigurirate postavke proxyja prilagođene test će se pokrenuti da biste provjerili proxy vezu.
    - Ako koristite prilagođeni proxy ili zadani proxy zahtijeva provjeru autentičnosti morat ćete unijeti detalje proxy poslužitelj, uključujući adresu proxy poslužitelja i priključka.
    - Sljedeći URL-ovi trebali biste moći pristupiti poslužitelju VMM i domaćini Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Dopusti IP adrese opisane u [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i HTTPS protokola (443). Promijenile bijeli popis IP rasponi Azure regije koje namjeravate koristi i koje Zapad SAD-a.
    - Ako koristite prilagođeni proxy VMM RunAs računa (DRAProxyAccount) stvorit će se automatski pomoću vjerodajnica za navedeni proxy poslužitelj. Proxy poslužitelj konfigurirati tako da se ovaj račun možete provjeriti autentičnost uspješno. Postavke računa VMM RunAs moguće je izmijeniti na konzoli VMM. Da biste to učinili, otvoriti radni prostor postavke, proširite sigurnost, kliknite Pokreni kao računi, a zatim promijenite lozinku za DRAProxyAccount. Morat ćete ponovno pokrenite servis VMM tako da se ova postavka stupa na snagu.

10. U **Ključa za registraciju**odaberite preuzimaju s oporavak Azure web-mjesta i kopirane na VMM poslužitelj.
11. U odjeljak **naziv sigurnog**provjerite naziv zbirke ključeva u kojem će biti registriran na poslužitelj. 

    ![Registracija poslužitelja](./media/site-recovery-vmm-san/vault-creds.png)

12. Postavka šifriranja se koriste isključivo radi VMM za Azure scenariju, ako ste VMM VMM samo korisniku, a zatim možete zanemariti ovaj zaslon.

    ![Registracija poslužitelja](./media/site-recovery-vmm-san/encrypt.png)

13. U odjeljak **naziv poslužitelja**navedite neslužbeni naziv za prepoznavanje VMM server u zbirke ključeva. U konfiguraciji klaster Navedite naziv uloge VMM klaster.
14. U **Početni oblaka metapodataka sinkronizaciju** navedite neslužbeni naziv poslužitelja na kojem će se pojavljuju u na sigurnog i odaberite želite li sinkronizirati metapodataka za sve oblaka na poslužitelju VMM s na sigurnog. Ova akcija samo se mora dogoditi kada na svakom poslužitelju. Ako ne želite da biste sinkronizirali sve oblaka, ostavite tu postavku poništen, a sinkronizirati svaki oblaka pojedinačno u oblak svojstva konzole za VMM.

    ![Registracija poslužitelja](./media/site-recovery-vmm-san/friendly-name.png)

15. Kliknite **Dalje** da biste dovršili postupak. Nakon registracije, metapodataka s poslužitelja VMM se dohvaćaju oporavak Azure web-mjesta. Na kartici *Poslužitelji VMM* na stranici **poslužitelja** u na sigurnog prikazat će se na poslužitelj.

### <a name="command-line-installation"></a>Instalacija naredbenog retka

Oporavak davatelja za Azure web-mjesta mogu se instalirati i pomoću sljedećih naredbenog retka. Ova metoda se može koristiti da biste instalirali davatelja usluge na na poslužitelju CORE za Windows Server 2012 R2.

1. Preuzmite instalacijsku datoteku davatelja i ključa za registraciju u mapu izgovorite C:\ASR
2. Zaustavljanje servisa Centar sustava Upravitelj virtualnog računala
3. Izdvajanje davatelja instalacijski program tako da pokrenete sljedeći iz naredbenog retka s **administratorskim** ovlastima:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalacija davatelja ponovnim pokretanjem sljedeće:

        C:\ASR> setupdr.exe /i

5. Da biste registrirali davatelj ponovnim pokretanjem sljedeće:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Gdje su parametri:

 - **/Credentials** : obavezna parametar koji određuje mjesto u kojoj se nalazi datoteka s ključem Registracija  
 - **/FriendlyName** : obavezna parametar za naziv poslužitelja Hyper-V glavnog računala koja se pojavljuje na portalu za oporavak Azure web-mjesta.
 - **/EncryptionEnabled** : neobavezan parametar koji ćete morati koristiti samo u VMM za Azure scenarij ako vam je potrebna šifriranje virtualnim strojevima pri na ostale u Azure. Provjerite jesu li naziv datoteke navedite sadrži nastavak **.pfx** .
 - **/proxyAddress** : neobavezan parametar koji određuje adresu proxy poslužitelja.
 - **/proxyport** : neobavezan parametar koji određuje priključak proxy poslužitelj.
 - **/proxyUsername** : neobavezan parametar koji određuje Proxy korisničko ime (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).
 - **/proxyPassword** : neobavezan parametar koji određuje lozinku za provjeru autentičnosti s proxy poslužitelja (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).


## <a name="step-3-map-storage-arrays-and-pools"></a>Korak 3: Mapiranje polja za pohranu i grupe

Mapiranje polja da biste odredili koji skup sekundarne prostora za pohranu prima replikacije podatke iz primarnog grupe aplikacija. Prostor za pohranu mora mapirati prije no što konfigurirate oblaka zaštitu jer se informacije o mapiranju koristi kada omogućite zaštitu za replikaciju grupe.

Prije početka provjerite oblaka pojavljuju se u na zbirke ključeva. Oblaka otkrije tako da odaberete da biste sinkronizirali sve oblaka kada instalirate davatelja usluga ili tako da odaberete da biste sinkronizirali određene oblak na kartici **Općenito** svojstva oblaka na konzoli VMM. Zatim mapiranje polja za pohranu na sljedeći način:

1. Kliknite **Resursi** > **poslužitelja za pohranu** > **karte izvora i ciljnog polja**.
    ![Registracija poslužitelja](./media/site-recovery-vmm-san/storage-map.png)
2. Odabir polja za pohranu na web-mjestu primarnog i mapirajte ih polja za pohranu na sekundarnog web-mjesta.
3.  Mapirajte izvorišno i odredišno spremište grupe u poljima. Da biste to učinili, u **Prostor za pohranu grupe** odaberite izvorišno i odredišno spremište skup mapirati.

    ![Registracija poslužitelja](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-cloud-protection-settings"></a>Korak 4: Konfiguriranje oblaka postavke zaštite

Kada VMM poslužitelji registrirane, možete konfigurirati postavke zaštite oblaka. Mogućnost **Sinkroniziraj oblaka podataka pomoću na sigurnog** omogućen prilikom instalacije davatelj tako da sva oblaka na poslužitelju VMM pojavit će se na karticu <b>Zaštićeni stavke</b> u na sigurnog.

![Objavljeni oblaka](./media/site-recovery-vmm-san/clouds-list.png)

1. Na stranici za brzo pokretanje kliknite **Postavljanje protection za VMM oblaka**.
2. Na kartici **Zaštićeni stavki** odaberite oblak koji želite konfigurirati, a zatim idite na karticu **Konfiguracija** . Imajte na umu da:
3. U **ciljnom**, odaberite **VMM**.
4. **Mjesto cilja**, odaberite on-site VMM poslužitelju na kojem se upravlja u oblak koji želite koristiti za oporavak.
5. U **ciljnom oblaka**, odaberite ciljni oblak koji želite koristiti za prebacivanje virtualnih računala u oblaku izvora. Imajte na umu da:
    - Preporučujemo da odaberete Cilj oblaka koja zadovoljava preduvjete za oporavak za virtualnim strojevima ćete zaštita.
    - Tijekom spremanja može pripadati samo jednom oblaka par — kao primarni ili cilj oblaka.
6. Oporavak Azure web-mjesta potvrđuje da oblaka imati pristup SAN replikacije koji podržavaju pohranu i da su polja za pohranu peered. Prikazuju se kolege koji sudjeluju polja.
7. Ako je provjera uspješna, u **vrsti replikacije**, odaberite **SAN**.

Nakon što spremite postavke posao stvorit će se i moguće nadzirati na kartici **Zadaci** . Na kartici **Konfiguriraj** moguće je izmijeniti postavke oblaka. Ako želite izmijeniti cilj mjesto ili ciljni oblaka ukloniti konfiguraciju oblaka, a konfigurirajte oblaka.

## <a name="step-5-enable-network-mapping"></a>Korak 5: Omogućivanje preslikavanje

1. Na stranici za brzo pokretanje kliknite **Mapiranje mrežama**.
2. Odaberite s izvorišnim poslužiteljem VMM iz koje želite mapirati mreža, a zatim ciljnom VMM poslužitelju koji će se mapirati mreže. Prikazuju se popis izvora mreža i svoje mreže povezane cilj. Za mreža koje trenutno nisu mapirani prikazuju se prazna vrijednost. Kliknite ikonu informacije pored naziva mreže izvorišno i odredišno da biste pogledali podmreže za svaku od mreža.
3. Odaberite mreža u **mreži na izvoru**, a potom kliknite **Karta**. Servis otkriva mreža VM na poslužitelju ciljni i prikazuje ih.

    ![Arhitektura SAN](./media/site-recovery-vmm-san/network-map1.png)

4. U dijaloškom okviru koji se prikazuje, odaberite jednu od VM mreža s poslužitelja VMM cilj.

    ![Arhitektura SAN](./media/site-recovery-vmm-san/network-map2.png)

5. Kad odaberete Cilj mreže, prikazuju se zaštićene oblaka koje koriste mrežni izvor. Mreža dostupna cilj koji su povezani s oblaka za zaštitu i prikazuju se. Preporučujemo da odaberete Cilj mreže koji je dostupan za sve oblaka koristite za zaštitu.
6.  Kliknite kvačicu da biste dovršili postupak mapiranje. Da biste pratili tijek mapiranje pokreće se posao. Ga možete pogledati na kartici **Zadaci** .


## <a name="step-6-enable-replication-for-replication-groups"></a>Korak 6: Omogućiti replikaciju za replikaciju grupe

Prije omogućivanja mogućnosti zaštite virtualnim strojevima morat ćete omogućiti replikaciju za pohranu replikacije grupe.

1. Na portalu za oporavak Azure web-mjesta na stranici Svojstva primarni oblaka otvorite karticu **virtualnih računala** . Kliknite **Dodaj grupu replikacije**.
2. Odaberite jednu ili više grupa replikacije VMM koje su vezane uz oblaka, provjerite je li izvorišno i odredišno polja i odredite učestalost ponavljanja.

Po dovršetku ovog postupka Azure oporavak web-mjesta, zajedno s VMM SMI osnovna funkcija Instalacijskog S davateljima Dodjela ciljnog web-mjesta za pohranu LUNs i omogućiti replikaciju prostora za pohranu. Ako grupi replikacije već replicirati, oporavak web-mjesta Azure ponovo koristi postojećim odnosom replikacije i ažurirati podatke na oporavak Azure web-mjesta.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Korak 7: Omogući zaštitu za virtualnim strojevima

Kada je prostor za pohranu je replikaciju grupe, omogućiti zaštitu virtualnim strojevima na konzoli VMM pomoću bilo koju od sljedećih načina:

- **Novi virtualnog računala**– u VMM konzole prilikom stvaranja novog virtualnog računala Omogući oporavak web-mjesta Azure zaštitu i virtualnog računala pridruživanje grupi replikacije.
Uz tu se mogućnost VMM koristi Inteligentna položaj optimalnog smještanje virtualnog računala prostora za pohranu na LUNs grupi replikacije. Oporavak Azure web-mjesta orchestrates stvaranje virtualnog računala sjene na sekundarnog web-mjesta, a dodjeljuje kapaciteta tako da se replike virtualnih računala može se pokrenuti nakon prebacivanje.
- **Postojeći virtualnog računala**– ako virtualnog računala već implementiran u VMM, možete omogućiti zaštitu oporavak Azure web-mjesta i provesti migraciju prostora za pohranu u grupu replikacije. Nakon dovršetka VMM i oporavak web-mjesta Azure otkriti nove virtualnog računala i započnite upravljati u oporavak Azure web-mjesta za zaštitu. Sjene virtualnog računala je stvoreno sekundarnog web-mjesta i dodijeliti kapaciteta tako da se replike virtualnog računala može se pokrenuti nakon prebacivanje.

    ![Omogući zaštitu](./media/site-recovery-vmm-san/enable-protect.png)

Nakon virtualnim strojevima omogućena za zaštitu prikažu konzole za oporavak Azure web-mjesta. Možete pregledavati svojstva virtualnog računala, praćenje stanja i neće uspjeti putem replikacije grupe koje sadrže više virtualnih računala. Imajte na umu da u SAN replikacije sve virtualnim strojevima pridružene grupi replikacije ne smije putem zajedno. To je zato prebacivanje najprije pojavljuje sloju prostora za pohranu. Važno je da pravilno grupiranje grupe replikacije i mjesto samo pridružene virtualnim strojevima zajedno.

>[AZURE.NOTE] Nakon što ste replikacije Omogući za stroj bi trebalo dodate VHDs za njega LUNs koje se ne nalaze u grupi replikacije oporavak web-mjesta. Ako vidite neće se provjeravati po oporavak web-mjesta.

Možete pratiti napredak akciju omogućite zaštitu na kartici **zadataka** , uključujući početne replikacije. Nakon izvođenja posla dovršavanje zaštitu virtualnog računala spremna je za prebacivanje.

![Zadatak zaštitu virtualnog računala](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-the-deployment"></a>Korak 8: Testiranje implementaciju

Testirajte implementaciju sustava da biste bili sigurni virtualnim strojevima i podataka neće uspjeti preko prema očekivanjima. Da biste to učinili tako da odaberete grupe replikacije stvorit ćete plan za oporavak. Zatim pokrenite test prebacivanje u planu.

1. Na kartici **Tarife za oporavak** kliknite **Stvaranje plana za oporavak**.
2. Unesite naziv za oporavak planiranje i izvorišno i odredišno VMM poslužiteljima. Izvorni poslužitelj mora imati virtualnim strojevima omogućene za prebacivanje i oporavak. Odaberite **SAN** da biste vidjeli samo oblaka koja su konfigurirana za replikaciju SAN.
3.
    ![Stvaranje plana za oporavak](./media/site-recovery-vmm-san/r-plan.png)

4. **Odaberite virtualnog računala**, odaberite replikacije grupe. Sve virtualnim strojevima pridružene grupi replikacije bit će odabran i dodali plan za oporavak. Ove virtualnim strojevima dodaju se u zadanu grupu za oporavak plan – 1 grupe. po potrebi možete dodati dodatne grupe. Imajte na umu da se nakon replikacije virtualnim strojevima će se pokrenuti skladu redoslijeda grupe plan za oporavak.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmm-san/r-plan-vm.png)
5. Nakon što je oporavak plan je stvoren, pojavljuje se na popisu na kartici **Tarife za oporavak** .
6. Na kartici **Oporavak tarife** odaberite tarifu, a zatim kliknite **Testiraj prebacivanje**.
7. Na stranici **Potvrda Test prebacivanje** odaberite **ništa**. Imajte na umu da se uz tu se mogućnost omogućeno nije uspio putem replike virtualnim strojevima neće biti povezani s mreže. To će testirajte virtualnog računala ne uspije na uobičajen način, ali testirajte svoje replikacije mrežnom okruženju. Potražite dodatne informacije o tome kako koristiti različite mogućnosti za umrežavanje [pokrenite test prebacivanje](site-recovery-failover.md#run-a-test-failover) .


    ![Odaberite test mreže](./media/site-recovery-vmm-san/test-fail1.png)

8. Na glavnom računalu isti kao glavno računalo replike virtualnog računala postoji stvorit će se test virtualnog računala. Nije se dodaju u oblak u kojoj se nalazi replike virtualnog računala.
9. Nakon replikacije virtualnog računala replike će imati IP adresu koja nije isto što i IP adrese primarni virtualnog računala. Ako ste izdavanja adrese iz DHCP pa će se ažurirati automatski. Ako ne koristite DHCP i želite da biste provjerili adrese jednaki morat ćete pokrenuti nekoliko skripti.
10. Pokrenite ovaj primjer skripte za dohvaćanje IP adrese.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

11. Pokrenite ovaj primjer skripte za ažuriranje DNS-a, pri određivanju IP adresu koju ste vratili pomoću prethodni primjer skripte.

        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Daljnji koraci

Nakon što prođete prebacivanje test da biste provjerili vaše okruženje funkcionira kako treba, [Dodatne informacije o](site-recovery-failover.md) različitim vrstama failovers.
