<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima u VMM oblaka sekundarne web-mjesto VMM | Microsoft Azure"
    description="U ovom se članku opisuje kako replicirati Hyper-V VMs u VMM oblaka na sekundarnom VMM web-mjesto s oporavak Azure web-mjesta."
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
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replicirati Hyper-V virtualnim strojevima u VMM oblaka sekundarne VMM web-mjesto

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-vmm.md)
- [Klasični portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell – Voditelj resursa](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md)

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako replicirati Hyper-V virtualnim strojevima na poslužiteljima glavno računalo Hyper-V kojim se upravlja iz oblaka VMM sekundarne VMM web-mjesto koristeći oporavak Azure web-mjesta.

Članak sadrži preduvjeti, pokazuje kako postavljanje zbirke ključeva za oporavak web-mjesta, instalacija davatelja za oporavak Azure web-mjesta na izvoru i ciljani VMM poslužiteljima, registrirali poslužitelji u sigurnog, konfiguriranje postavki zaštite za VMM oblaka i omogućiti zaštitu Hyper-V VMs. Dovršili postupak tako da testirate prebacivanje da biste provjerili funkcionira sve prema očekivanjima.

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="architecture"></a>Arhitektura

Na slici u nastavku prikazuje različite komunikacijski kanali i priključke koristi oporavak Azure web-mjesta za djelovanje ni replikacije

![Topologija E2E](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Prije početka

Provjerite je li te preduvjeti na mjestu:

**Preduvjeti** | **Pojedinosti**
--- | ---
**Azure**| Potreban račun [Sustava Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**VMM** | Potreban vam je barem jedan VMM poslužitelj.<br/><br/>Poslužitelj VMM mora imati najmanje 2012 SP1 centar sustava s najnovijim kumulativnim ažuriranjima.<br/><br/>Ako želite postaviti zaštitu s poslužiteljem jedan VMM, morat ćete najmanje dva oblaka konfiguriran na poslužitelju.<br/><br/>Ako želite implementirati zaštitu s dva poslužitelja VMM svaki poslužitelj morate imati barem jedan oblaka konfiguriran na primarni VMM poslužitelju koji želite zaštititi, a jedan oblaka konfiguriran na sekundarnom VMM poslužitelju koji želite koristiti za zaštitu i oporavak<br/><br/>Sve VMM oblaka, morate imati profil mogućnost Hyper-V postavljen.<br/><br/>Oblak izvora koji želite zaštititi mora sadržavati jednu ili više grupa VMM glavnog računala.<br/><br/>Dodatne informacije o postavljanju oblaka VMM u [vodič: Stvaranje privatne oblaka pomoću sustava centra 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) na blogu Keith Mayer.
**Hyper-V** | Potreban je jedan ili više poslužitelja Hyper-V glavno računalo u grupama glavno računalo VMM primarnih i sekundarnih i jedan ili više virtualnim strojevima na svakom poslužitelju Hyper-V glavnog računala.<br/><br/>Hyper-V poslužitelje glavnog i ciljnog morate imati barem Windows Server 2012 s ulogom Hyper-V i instalirali najnovija ažuriranja.<br/><br/>Bilo koji poslužitelj Hyper-V koja sadrži VMs koji želite zaštititi moraju nalaziti u oblaku za VMM.<br/><br/>Ako koristite Hyper-V klasteru, imajte na umu te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Ćete morati ručno konfigurirati broker klaster. [Dodatne informacije potražite](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) u blogu Aidan Finn.
**Preslikavanje** | Možete konfigurirati preslikavanje da biste bili sigurni da repliciranu virtualnim strojevima optimalnog smještaju se na sekundarnom Hyper-V glavnog računala poslužitelja nakon prebacivanje, a možete povezati s odgovarajućim VM mrežama. Ako ne konfigurirate preslikavanje, replike VMs neće biti povezani s mreže nakon prebacivanje.<br/><br/>Da biste postavili preslikavanje tijekom implementacije, provjerite je li virtualnim strojevima na izvornom Hyper-V poslužitelju glavno računalo povezani s mrežom VMM VM. Tu mrežu može povezati logičke mreže koji je pridružen oblaka. < br /<br/>Oblak cilj na sekundarnom VMM poslužitelja koje koristite za oporavak trebala bi biti odgovarajuće VM mreža konfigurirana, pa ga shodno trebala biti vezana odgovarajuće logičke mrežu koji je pridružen oblaka ciljne.<br/><br/>[Saznajte više](site-recovery-network-mapping.md) o preslikavanje.
**Mapiranje prostora za pohranu** | Prema zadanim postavkama kada replicirati virtualnog računala na poslužitelj Hyper-V izvora na poslužitelju ciljni Hyper-V glavno računalo, repliciranu podaci se pohranjuju na zadano mjesto naznačenu za ciljno glavno računalo Hyper-V u upravitelju Hyper-V. Za veću kontrolu nad pohranjuju repliciranu podataka, možete konfigurirati za pohranu mapiranja<br/><br/> Da biste konfigurirali mapiranja prostora za pohranu, morate postavili klasifikacije prostora za pohranu na izvoru i ciljnog VMM poslužitelje prije nego što počnete implementacije. [Dodatne informacije](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Korak 1: Stvaranje sigurnog za oporavak web-mjesta

1. Prijavite se na [Portal za upravljanje](https://portal.azure.com) poslužitelja VMM želite registrirati.

2. Proširite **Data Services** > **Oporavak usluge** kliknite **Sigurnog oporavak web-mjesta**.

3. Kliknite **Stvori novi** > **brzo stvaranje**.

4. U odjeljak **naziv**unesite neslužbeni naziv da biste odredili na zbirke ključeva.

5. U **području** odaberite regiji u zbirke ključeva. Da biste provjerili podržanih regija potražite u članku geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](http://go.microsoft.com/fwlink/?LinkId=389880).

6. Kliknite **Stvaranje sigurnog**.

    ![Stvaranje zbirke ključeva](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Prijava na traci stanja u sigurnog stvorena. Na sigurnog će biti navedena kao **aktivna** na glavnoj stranici servisa za oporavak.

## <a name="step-2-generate-a-vault-registration-key"></a>Korak 2: Stvaranje sigurnog ključa za registraciju

Generiranje ključa za registraciju u na zbirke ključeva. Nakon preuzimanja davatelja za oporavak Azure web-mjesta i instalirajte ga na poslužitelj VMM, koristit ćete taj ključ da biste registrirali VMM poslužitelja u zbirke ključeva.

1. Na stranici **Servisa oporavak** kliknite sigurnog da biste otvorili stranicu za brzo pokretanje. Brzi početak rada može se otvoriti u bilo kojem trenutku pomoću ikone.

    ![Ikona za brzi početak rada](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Na padajućem popisu odaberite **između dvaju lokalnog VMM web-mjesta**.
3. U **Priprema VMM poslužiteljima**, kliknite **Generiraj Registracija ključne datoteke**. Datoteka s ključem automatski se generira i vrijedi 5 dana nakon generira. Ako ne koristite pristupanja portalu za Azure s poslužitelja VMM morate kopirati tu datoteku na poslužitelj.

    ![Ključa za registraciju](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Korak 3: Instalacija davatelja oporavak Azure web-mjesta

4. Na stranici **Brzi početak rada** na **poslužiteljima Priprema VMM**kliknite **Preuzmite Microsoft Azure web-mjesta oporavak Provider za instalaciju na poslužiteljima VMM** da biste nabavili najnoviju verziju instalacijsku datoteku davatelja usluga.

2. Pokrenite datoteku na s izvorišnim poslužiteljem VMM.

    >[AZURE.NOTE] Ako je VMM implementiran klasteru i instalirate davatelj usluga za prvo ga instalirati na aktivni čvor i dovršetak instalacije da biste registrirali VMM poslužitelja u sigurnog. Zatim instalirajte davatelja usluge na ostale čvorove. Imajte na umu da Ako nadograđujete davatelj morate nadograditi na sve čvorove jer oni trebali biste sve moguće istu verziju davatelja usluga.

3. Instalacijski program ne nekoliko dozvole **Provjerite stara preduvjeti** i zahtjevi za zaustavljanje servisa VMM da biste započeli postavljanje davatelja usluga. Servis VMM će se ponovno pokrenuti automatski kada se dovrši instalacija. Ako ga instalirate na VMM klaster vas će se zatražiti da biste prestali klaster ulogu.

4. U **Programu Microsoft Update** možete odabrati u ima li ažuriranja. Ova postavka omogućena davatelja ažuriranja instalirat će se prema Pravilnik za Microsoft Update.

    ![Microsoftova ažuriranja](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. Mjesto za instalaciju je postavljeno na ** <SystemDrive>\Programske sustava centra 2012 R2\Virtual strojno Manager\bin**. Kliknite gumb Instaliraj da biste pokrenuli instalaciju davatelja usluga.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Nakon instalacije davatelja usluge kliknite **registrirati** da biste registrirali poslužitelja u zbirke ključeva.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
9. U odjeljak **naziv sigurnog**provjerite naziv zbirke ključeva u kojem će biti registriran na poslužitelj. Kliknite *Dalje*.

    ![Registracija poslužitelja](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. U **Internetsku vezu** Navedite način na koji se davatelja usluge na poslužitelju VMM povezuje s Internetom. Odaberite **Poveži s postojeće postavke proxy poslužitelja** za korištenje na zadane postavke internetske veze konfiguriran na poslužitelju.

    ![Postavke internetske](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Ako želite koristiti prilagođenu proxy ga trebali postaviti prije instalacije davatelja usluga. Kada konfigurirate postavke proxyja prilagođene test će se pokrenuti da biste provjerili proxy vezu.
    - Ako koristite prilagođeni proxy ili zadani proxy zahtijeva provjeru autentičnosti morate unijeti detalje proxy poslužitelj, uključujući adresu proxy poslužitelja i priključka.
    - Sljedeći URL-ovi trebali biste moći pristupiti poslužitelju VMM i domaćini Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Dopusti IP adrese opisane u [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i HTTPS protokola (443). Promijenile bijeli popis IP rasponi Azure regije koje namjeravate koristi i koje Zapad SAD-a.
    - Ako koristite prilagođeni proxy VMM RunAs računa (DRAProxyAccount) stvorit će se automatski pomoću vjerodajnica za navedeni proxy poslužitelj. Proxy poslužitelj konfigurirati tako da se ovaj račun možete provjeriti autentičnost uspješno. Postavke računa VMM RunAs moguće je izmijeniti na konzoli VMM. Da biste to učinili, otvoriti radni prostor **Postavke** , proširite **Sigurnost**, kliknite **Pokreni kao računi**, a zatim promijenite lozinku za DRAProxyAccount. Morat ćete ponovno pokrenite servis VMM tako da se ova postavka stupa na snagu.


8. U **Ključa za registraciju**odaberite ključ koji se preuzimaju s oporavak Azure web-mjesta i kopirane na VMM poslužitelj.


10.  Postavka šifriranja koristi samo kada ste replikaciju Hyper-V VMs u VMM oblaka za Azure. Ako ste replikaciju sekundarnu web-mjesto se ne koristi.

11.  U odjeljak **naziv poslužitelja**navedite neslužbeni naziv za prepoznavanje VMM server u zbirke ključeva. U konfiguraciji klaster Navedite naziv uloge VMM klaster.
12.  Odaberite **Sinkroniziraj oblaka metapodataka** želite li da biste sinkronizirali metapodataka za sve oblaka na poslužitelju VMM s na zbirke ključeva. Ova akcija samo se mora dogoditi kada na svakom poslužitelju. Ako ne želite da biste sinkronizirali sve oblaka, ostavite tu postavku poništen, a sinkronizirati svaki oblaka pojedinačno u oblak svojstva konzole za VMM.

13.  Kliknite **Dalje** da biste dovršili postupak. Nakon registracije, metapodataka s poslužitelja VMM se dohvaćaju oporavak Azure web-mjesta. Poslužitelj se prikazuje na **Poslužiteljima VMM** > **poslužitelji** u zbirke ključeva.

    ![Poslužitelja](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Instalacija naredbenog retka

Oporavak davatelja za Azure web-mjesta može biti instaliran i iz naredbenog retka. Ova metoda se može koristiti da biste instalirali davatelja usluge na na poslužitelju CORE za Windows Server 2012 R2.

1. Preuzmite tipku davatelja instalacije datoteke i registraciju u mapu. Na primjer C:\ASR.
2. Zaustavljanje servisa Centar sustava Upravitelj virtualnog računala
3. Izdvajanje davatelja instalacijski program tako da pokrenete te naredbe iz naredbenog retka s **administratorskim** ovlastima:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalacija davatelja tako da pokrenete:

        C:\ASR> setupdr.exe /i

5. Da biste registrirali davatelj tako da pokrenete:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Gdje su parametri:

 - **/Credentials**: obavezna parametar koji određuje mjesto u kojoj se nalazi datoteka s ključem Registracija  
 - **/FriendlyName**: obavezna parametar za naziv poslužitelja Hyper-V glavnog računala koja se pojavljuje na portalu za oporavak Azure web-mjesta.
 - **/EncryptionEnabled**: neobavezan parametar koji ćete morati koristiti samo u VMM za Azure scenarij ako vam je potrebna šifriranje virtualnim strojevima pri na ostale u Azure. Provjerite jesu li naziv datoteke navedite sadrži nastavak **.pfx** .
 - **/proxyAddress**: neobavezan parametar koji određuje adresu proxy poslužitelja.
 - **/proxyport**: neobavezan parametar koji određuje priključak proxy poslužitelj.
 - **/proxyUsername**: neobavezan parametar koji određuje Proxy korisničko ime (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).
 - **/proxyPassword**: neobavezan parametar koji određuje lozinku za provjeru autentičnosti s proxy poslužitelja (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).  

## <a name="step-4-configure-cloud-protection-settings"></a>Korak 4: Konfiguriranje oblaka postavke zaštite

Kada VMM poslužitelji registrirane, možete konfigurirati postavke zaštite oblaka. Ako ste omogućili mogućnost **Sinkroniziraj oblaka podataka pomoću na sigurnog** prilikom instalacije davatelj tako da sva oblaka na poslužitelju VMM pojavit će se kartica **Zaštićeni stavki** u na sigurnog. Ako vam ne mogu se sinkronizirati određene oblaka s oporavak Azure web-mjesta na kartici **Općenito** na stranici Svojstva oblaka na konzoli VMM.

![Objavljeni oblaka](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Na stranici za brzo pokretanje kliknite **Postavljanje protection za VMM oblaka**.
2. Na kartici **VMM oblaka** odaberite oblak koji želite konfigurirati, a zatim idite na karticu **Konfiguracija** .
3. U **ciljnom**, odaberite **VMM**.
4. **Mjesto cilja**, odaberite on-site VMM poslužitelju na kojem se upravlja u oblak koji želite koristiti za oporavak.
4. U **ciljnom oblaka**, odaberite ciljni oblak koji želite koristiti za prebacivanje virtualnih računala u oblaku izvora. Imajte na umu da:

    - Preporučujemo da odaberete Cilj oblaka koja zadovoljava preduvjete za oporavak za virtualnim strojevima ćete zaštita.
    - Tijekom spremanja može pripadati samo jednom oblaka par — kao primarni ili cilj oblaka.

5. U **kopirajte učestalost**, navedite koliko često podataka mora se sinkronizirati 5he izvorišno i odredišno mjesta. Imajte na umu da ova postavka vrijedi samo kada je pokrenuto glavno računalo Hyper-V Windows Server 2012 R2. Za poslužitelji koristi se zadana postavka od pet minuta.
6. U **Dodatne oporavak točke**odredite želite li stvoriti dodatne oporavak točke. Nula zadanu vrijednost pokazuje samo najnovije oporavak točke za primarni virtualnog računala pohranjena na poslužitelju replike glavnog računala. Imajte na umu da Omogućivanje više točaka oporavak potreban dodatan prostor za pohranu za snimke pohranjenima u svakom trenutku oporavak. Prema zadanim postavkama, oporavak se stvaraju točke svaki sat tako da svaka točka oporavak sadrži jedan sat vrijedne podataka. Vrijednost točke za oporavak koje dodijelite za virtualnog računala na konzoli VMM mora biti manja od vrijednosti koje dodijelite konzole za oporavak Azure web-mjesta.
7. U **Učestalost aplikacije dosljedan snimke**, navedite koliko je često potrebno stvoriti aplikaciju dosljedan snimke. Hyper-V koristi dvije vrste snimaka – standardni snimku zaslona koja nudi rastuće snimku cijelu virtualnog računala i aplikaciju dosljedan snimku zaslona koja stvara točke u vrijeme snimku podataka aplikacije unutar virtualnog računala. Aplikacija dosljedan snimke pomoću servisa Kopiraj glasnoću sjena (VSS) aplikacije provjerite jesu li u dosljedan stanju kada se koristi se snimka. Imajte na umu da Ako omogućite aplikacije dosljedan snimke, jer će utjecati na performanse aplikacije koje rade na virtualnim strojevima izvora. Provjerite je li vrijednost postavljate manji broj točaka dodatne oporavak konfigurirate.

    ![Konfiguriranje postavki zaštite](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. U **prijenosa sažimanje podataka**, navedite hoće li se sažeti repliciranu podataka koji se prenose.
9. U **provjeru autentičnosti**, odredite kako će se promet autentičnost između poslužitelja glavno računalo Hyper-V primarni i oporavak. Odaberite HTTPS osim ako imate rad Kerberos okruženje konfigurirano. Oporavak Azure web-mjesta automatski će konfigurirati certifikata za provjeru autentičnosti HTTPS. Potreban je bez ručna konfiguracija. Ako ste odabrali Kerberos, Kerberos karata će se koristiti za međusobna provjeru autentičnosti poslužitelja glavnog računala. Prema zadanim postavkama, priključak 8083 i 8084 (za potvrde) će se otvoriti u vatrozidu za Windows na poslužiteljima Hyper-V glavnog računala. Imajte na umu da ova postavka vrijedi samo za Hyper-V glavnog računala poslužitelja sustavom Windows Server 2012 R2.
10. **Priključak**, izmijenite broj priključka koji je na kojem se glavnog računala izvorišno i odredišno preslušali za replikaciju promet. Ako, na primjer, možda promijeniti postavke ako želite primijeniti ograničavanje za replikaciju promet je propusnost mreže kvalitete Service (QoS). Potvrdite okvir priključak ne koriste neke druge aplikacije i da je otvoren u postavke vatrozida.
11. U **način za replikaciju**odredite kako početne replikacije podataka iz izvora odredišta će se rukovati, prije pokretanja običnog replikacije:

    - **Putem mreže**– kopiranje podataka putem mreže može potrajati i resursa ćete morati usko. Preporučujemo da koristite ovu mogućnost ako oblaka sadrži virtualnim strojevima sa razmjerno malo virtualne tvrdi disk, a primarni web-mjesta s sekundarnog web-mjesta putem veze s širine propusnosti. Možete odrediti da kopiju moraju se odmah pokrenuti ili odaberite neko vrijeme. Ako koristite mrežni replikacije, preporučujemo da ga planirate najfrekventnija.
    - **Izvan mreže**– ovu metodu određuje početne replikacije će se izvršiti pomoću vanjske medijske. Korisno je ako želite da biste izbjegli smanjene performanse na performanse mreže ili geografski udaljenog mjesta. Da biste to učinili odredite mjesto Izvoz na cloud izvora i uvoz mjesto na ciljnom oblaka. Kada omogućite zaštitu virtualnog računala, virtualne na tvrdom disku se kopira mjesto navedeni izvoz. Slanje ciljno web-mjesto i kopirajte je mjesto za uvoz. Sustav kopira uvezene podatke na virtualnim strojevima replike.

12. Odaberite **Izbriši replike virtualnog računala** da biste odredili da se virtualnog računala replike treba izbrisati Ako prestanete zaštita virtualnog računala tako da odaberete mogućnost **Izbriši zaštitu virtualnog računala** na kartici virtualnim strojevima svojstva oblaka. Omogućena, ova postavka kada onemogućite zaštitu virtualnog računala da je uklonjena s oporavak Azure web-mjesta, postavke web-mjesta oporavak virtualnog računala uklanjaju se u VMM konzole i replike se briše.

    ![Konfiguriranje postavki zaštite](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Nakon što spremite postavke posao stvorit će se i moguće nadzirati na kartici **Zadaci** . Svim poslužiteljima Hyper-V glavnog računala u oblaku izvora VMM će se konfigurirati za replikaciju. Na kartici **Konfiguriraj** moguće je izmijeniti postavke oblaka. Ako želite izmijeniti cilj mjesto ili ciljni oblaka ukloniti konfiguraciju oblaka, a konfigurirajte oblaka.

### <a name="prepare-for-offline-initial-replication"></a>Priprema za replikaciju izvan mreže početne

Morat ćete učiniti sljedeće akcije da biste se pripremili za početne replikaciju izvan mreže:

- Na s izvorišnim poslužiteljem ćete Navedite put mjesto s kojeg izvoz podataka se odvija. Dodijelite Potpuna kontrola za NTFS i zajedničko korištenje dozvola za servis VMM na putu izvoz. Na poslužitelju ciljni ćete Navedite put mjesto s kojeg će se pojaviti uvoz podataka. Dodijelite iste dozvole na ovaj put za uvoz.
- Ako se uvoz ili izvoz put se zajednički koristi, dodijelite administratora, Power korisnika, ispis Operator ili Operator poslužitelja članstvo u grupi za račun servisa VMM na udaljenom računalu na kojem zajedničko korištenje nalazi.
- Ako koristite svih računa za pokretanje kao da biste dodali domaćini na putova uvoz i izvoz dodjeljivanje čitanje i dozvole za pisanje račune Pokreni kao u VMM.
- Zajedničko korištenje za uvoz i izvoz treba se nalazi na bilo kojem računalu koji se koristi kao glavno računalo poslužiteljem Hyper-V jer povratna konfiguracije ne podržava Hyper-V.
- U servisu Active Directory na svakom Hyper-V glavnog poslužitelja koja sadrži virtualnim strojevima želite zaštititi, Omogućivanje i konfiguriranje ograničeno delegiranje pouzdanosti udaljenim računalima na kojima putove za uvoz i izvoz nalaze, na sljedeći način:
    1. Na kontrolerom domene otvorite **Active Directory korisnicima i računalima**.
    2. U stablu konzole kliknite **naziv domene** > **računala**.
    3. Desnom tipkom miša kliknite naziv Hyper-V glavnog računala poslužitelja > **Svojstva**.
    4. Na kartici **delegiranje** pritisnite T**Rđa ovo računalo za delegiranje navedeni servisima**.
    5. Kliknite **bilo koji protokol za provjeru autentičnosti**.
    6. Kliknite **Dodaj** > **korisnicima i računalima**.
    7. Upišite naziv računala na kojem je smještena put Izvoz > **u redu**. Na popisu dostupnih usluga, pritisnite i držite tipku CTRL i kliknite **cifs** > **u redu**. Ponovite za naziv računala na kojem je smještena put uvoz. Potrebi ponovite ove korake za dodatne poslužitelje Hyper-V glavnog računala.

## <a name="step-5-configure-network-mapping"></a>Korak 5: Konfiguriranje mapiranja mreže
1. Na stranici za brzo pokretanje kliknite **Mapiranje mrežama**.
2. Odaberite s izvorišnim poslužiteljem VMM iz koje želite mapirati mreža, a zatim ciljnom VMM poslužitelju koji će se mapirati mreže. Prikazuju se popis izvora mreža i svoje mreže povezane cilj. Za mreža koje trenutno nisu mapirani prikazuju se prazna vrijednost.
3. Odaberite mreža u **mreži na izvoru** > **Karta**. Servis otkriva mreža VM na poslužitelju ciljni i prikazuje ih. Kliknite ikonu informacije pored naziva mreže izvorišno i odredišno da biste pogledali podmreže za svaku od mreža.

    ![Konfiguriranje mapiranja mreže](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. U dijaloškom okviru odaberite jednu od VM mreža s poslužitelja VMM cilj.

    ![Odaberite ciljni mrežu](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Kad odaberete Cilj mreže, prikazuju se zaštićene oblaka koje koriste mrežni izvor. Mreža dostupna cilj koji su povezani s oblaka za zaštitu i prikazuju se. Preporučujemo da odaberete Cilj mreže koji je dostupan za sve oblaka koristite za zaštitu. Ili idite na poslužitelju VMM i izmijenite svojstva oblaka da biste dodali logičke mreže odgovara vm mreža za koju želite odabrati.
6. Kliknite kvačicu da biste dovršili postupak mapiranje. Da biste pratili tijek mapiranje pokreće se posao. Ga možete pogledati na kartici **Zadaci** .


## <a name="step-6-configure-storage-mapping"></a>Korak 6: Konfiguriranje mapiranja prostora za pohranu
Prema zadanim postavkama kada replicirati virtualnog računala na poslužitelj Hyper-V izvora na poslužitelju ciljni Hyper-V glavno računalo, repliciranu podaci se pohranjuju na zadano mjesto naznačenu za ciljno glavno računalo Hyper-V u upravitelju Hyper-V. Za veću kontrolu nad pohranjuju repliciranu podatke, možete konfigurirati mapiranja prostora za pohranu na sljedeći način:



1. Definiranje klasifikacije prostora za pohranu na izvorišno i odredišno VMM poslužiteljima. [Dodatne informacije](https://technet.microsoft.com/library/gg610685.aspx). Klasifikacije mora biti dostupni poslužitelji glavno računalo Hyper-V izvorišno i odredišno oblaka. Klasifikacije ne moraju imati istu vrstu prostora za pohranu. Na primjer možete mapirati klasifikacija izvora koji sadrži SMB zajedničko korištenje da biste ciljnu klasifikacija koja sadrži CSVs.
2. Kada su klasifikacije mjestu možete stvoriti mapiranja. Da biste to učinili, na stranici **Brzi Start** > **Mapiranje prostora za pohranu**.
3. Kliknite karticu **za pohranu** > **Mapiranje klasifikacije prostora za pohranu**.
4. Na kartici **Mapiranje klasifikacije prostora za pohranu** odaberite klasifikacije na izvoru i ciljnog VMM poslužiteljima. Spremanje postavki.

    ![Odaberite ciljni mrežu](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Korak 7: Omogući zaštitu virtualnog računala
Nakon poslužitelje, oblaka i mreža ispravno konfigurirani, možete omogućiti zaštitu virtualnih računala u oblaku.

1. Na kartici **virtualnih računala** u oblaku u kojoj se nalazi virtualnog računala, kliknite **Omogući zaštitu** > **Dodaj virtualnih računala**.
2. Na popisu virtualnih računala u oblaku, odaberite onaj koji želite zaštititi.

    ![Omogući zaštitu virtualnog računala](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. Praćenje napretka akciju omogućite zaštitu na kartici **zadataka** , uključujući početne replikacije. Nakon izvođenja posla dovršavanje zaštitu virtualnog računala spremna je za prebacivanje. Kada je omogućena zaštita i virtualnim strojevima su replicirati, ćete moći njihov prikaz u Azure.

    ![Zadatak zaštitu virtualnog računala](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] Možete omogućiti i zaštitu virtualnim strojevima na konzoli VMM. Kliknite **Omogući zaštitu** na alatnoj traci na kartici **Oporavak Azure web-mjesta** u svojstva virtualnog računala.

### <a name="on-board-existing-virtual-machines"></a>Ploči postojeće virtualnim strojevima

Ako imate postojeću virtualnim strojevima u VMM koji su replikaciju s Hyper-V replike morat ćete onboard ih za zaštitu oporavak Azure web-mjesta na sljedeći način:

1. Provjerite imate li primarnih i sekundarnih oblaka. Provjerite je li poslužitelj Hyper-V koji hostira postojeće virtualnog računala nalazi u primarni oblaka i Hyper-V poslužitelj koji hostira virtualnog računala replike nalazi u oblaku sekundarne. Provjerite je li ste konfigurirali postavke zaštite za oblaka. Postavke se moraju podudarati s onima trenutno postavljenima za replike Hyper-V. U suprotnom replikacije virtualnog računala možda neće funkcionirati prema očekivanjima.
2. Zatim omogućiti zaštitu primarni virtualnog računala. Azure oporavak web-mjesta i VMM će provjerite otkrije isti replike glavno računalo i virtualnog računala i oporavak web-mjesta Azure će ponovno iskoristiti i ponovno uspostaviti replikacije pomoću konfigurirali tijekom konfiguriranja oblaka.


## <a name="test-your-deployment"></a>Provjera uvođenja

Da biste testirali implementaciju sustava možete pokrenuti prebacivanje test za jednu virtualnog računala ili stvorite plan oporavak koji se sastoji od više virtualnim strojevima i pokrenite test prebacivanje plana.  Prebacivanje test simulira prebacivanje i oporavak mehanizam Izolirani mreže.

### <a name="create-a-recovery-plan"></a>Stvaranje plana oporavak

1. Na kartici **Tarife za oporavak** kliknite **Stvaranje plana za oporavak**.
2. Unesite naziv za oporavak planiranje i izvorišno i odredišno VMM poslužiteljima. Izvorni poslužitelj mora imati virtualnim strojevima omogućene za prebacivanje i oporavak. Odaberite **Hyper-V tako** da biste vidjeli samo oblaka koja su konfigurirana za replikaciju Hyper-V.

    ![Stvaranje plana za oporavak](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. **Odaberite virtualnog računala**, odaberite replikacije grupe. Sve virtualnim strojevima pridružene grupi replikacije bit će odabran i dodali plan za oporavak. Ove virtualnim strojevima dodaju se u zadanu grupu za oporavak plan – 1 grupe. po potrebi možete dodati dodatne grupe. Imajte na umu da se nakon replikacije virtualnim strojevima počet će skladu redoslijeda grupe plan za oporavak.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Nakon što je oporavak plan je stvoren, pojavljuje se na popisu na kartici **Tarife za oporavak** .

###<a name="run-a-test-failover"></a>Pokrenite test prebacivanje

1. Na kartici **Oporavak tarife** odaberite tarifu, a zatim kliknite **Testiraj prebacivanje**.
2. Na stranici **Potvrda Test prebacivanje** odaberite **ništa**. Imajte na umu da se uz tu se mogućnost omogućeno nije uspio putem replike virtualnim strojevima neće biti povezani s mreže. To će testirajte virtualnog računala ne uspije na uobičajen način, ali testirajte svoje replikacije mrežnom okruženju. Potražite dodatne informacije o tome kako koristiti različite mogućnosti za umrežavanje [pokrenite test prebacivanje](site-recovery-failover.md#run-a-test-failover) .
3. Na glavnom računalu isti kao glavno računalo replike virtualnog računala postoji stvorit će se test virtualnog računala. Ono se dodaje u istom oblak u kojoj se nalazi replike virtualnog računala.

### <a name="run-a-recovery-plan"></a>Provesti plan za oporavak
Nakon replikacije virtualnog računala replike možda neće imati IP adresu koja je isti kao IP adresu primarni virtualnog računala. Virtualnim strojevima će se ažurirati DNS poslužitelju na kojem se koriste kada počnu. Možete dodati i skriptu da biste ažurirali DNS poslužitelj da biste bili sigurni pravovremeno ažuriranja.

#### <a name="script-to-retrieve-the-ip-address"></a>Skripta za dohvaćanje IP adrese
Pokrenite ovaj primjer skripte za dohvaćanje IP adrese.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Skripta za ažuriranje DNS-a
Pokrenite ovaj primjer skripte za ažuriranje DNS-a, pri određivanju IP adresu koju ste vratili pomoću prethodni primjer skripte.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informacije o zaštiti privatnosti za oporavak web-mjesta

Ovo poglavlje sadrži informacije o dodatne izjave o zaštiti privatnosti za servis Microsoft Azure web-mjesta oporavak ("servis"). Izjava o zaštiti privatnosti za Microsoft Azure services potražite u članku [Izjava o zaštiti privatnosti za Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=324899)

**Značajka: Registracija**

- **Funkcija**: registrira poslužitelj sa servisom tako da mogu biti zaštićene virtualnim strojevima
- **Podaci koji se prikupljaju**: nakon registriranja servis prikuplja obrađuje i prenosi informacije o certifikatu upravljanje VMM poslužitelja na kojem je označen za oporavak Izrada pomoću servisa naziv poslužitelja VMM i naziv oblaka virtualnog računala na vašem poslužitelju VMM.
- **Korištenje podataka**:
    - Upravljanje certifikatom – koristi da biste lakše prepoznali i provjere autentičnosti registrirani VMM poslužitelj za pristup servisu. Servis koristi javni ključ dio certifikata radi zaštite tokena koje samo registrirani VMM server može ostvariti pristup. Poslužitelj mora koristiti ovaj token ostvariti pristup značajkama koje su usluge.
    - Naziv poslužitelja VMM – naziv poslužitelja u VMM potrebna je za prepoznavanje i komunicirati s odgovarajućim VMM poslužiteljem na kojem se nalaze oblaka.
    - Nazivi poslužitelja VMM u oblaku – naziv oblaka potreban je prilikom korištenja servisa oblaka uparivanje/unpairing značajka opisan u nastavku. Kada se odlučite za uparivanje oblaka iz centra za primarni podataka s drugog oblaka u centru za oporavak podataka, nazivi oblaka iz centra za oporavak podataka predstavljaju.

- **Odabir**: te podatke je ključni dio postupka registracije servisa jer omogućuje vam i servis za prepoznavanje VMM poslužitelja za koju želite li omogućiti oporavak web-mjesta Azure zaštitu, kao i kao prepoznavanje odgovarajući registrirani VMM poslužitelj. Ako ne želite poslati te podatke na servis, nemojte koristiti taj servis. Ako ste se registrirali na poslužitelju i kasnije želite ga Odjava, to možete učiniti tako da izbrišete podaci o poslužitelju VMM na portalu servisa (što je portal za Azure).

**Značajka: Omogući zaštitu oporavak Azure web-mjesta**

- **Funkcija**: U Azure web-mjesta oporavak davatelj instalirana na poslužitelju VMM je conduit za komunikaciju s uslugom. Davatelj je biblioteka dinamičkih veza (DLL) nalazi u postupku VMM. Nakon instalacije davatelj značajka "Podatkovnim centrom oporavak" dobiva omogućen u administratorskoj konzoli VMM. Bilo koji novi ili postojeći virtualnim strojevima u prikazom oblaka možete omogućiti svojstvo pod nazivom "Podatkovnim centrom oporavak" da biste zaštitili virtualnog računala. Nakon postavljanja tog svojstva davatelj šalje ime i ID virtualnog računala servis. Virtualna zaštitu omogućena je prema Windows Server 2012 ili Windows Server 2012 R2 Hyper-V replikacije tehnologije. Dohvaća podatke virtualnog računala replicirati s jednog Hyper-V glavnog računala na drugo (obično nalazi u podatkovnom centru različite "oporavak").

- **Podaci koji se prikupljaju**: U servis prikuplja, obrađuje i prenosi metapodataka za virtualnog računala, koji obuhvaća naziv, ID, virtualne mreže i naziv oblak koji pripada.

- **Korištenje podataka**: na servis koristi gore navedenih podataka za popunjavanje informacije virtualnog računala portala za servis.

- **Odabir**: to je ključna dio usluge i ne može se isključiti. Ako ne želite da se ovaj podaci koji se šalju servisu, ne omogućite zaštiti oporavak Azure web-mjesta za sve virtualnim računalima. Imajte na umu sve podatke koji su poslane davatelj usluge slanja putem HTTP.

**Značajke: Plan za oporavak**

- **Funkcija**: Ova značajka pomaže vam da biste sastavili djelovanje Osmišljavanje podatkovnog centra "oporavak". Možete definirati redoslijed kojim virtualnim strojevima ili grupe virtualnim strojevima počne na web-mjestu za oporavak. Možete odrediti i sve automatiziranog skripte se izvodi ili sve ručne akcije da bi se otvorila, u trenutku oporavak za svaki virtualnog računala. Prebacivanje (obrađen u sljedećem odjeljku) obično se pokreće na razini oporavak Plan za usklađenih oporavak.

- **Podaci koji se prikupljaju**: U servis prikuplja, obrađuje i prenosi metapodataka za tarifu za oporavak, uključujući metapodataka virtualnog računala i metapodaci sve Automatizacija skripte i ručno akcija bilješke.

- **Korištenje podataka**: metapodataka na prethodno opisan koristi za sastavljanje plan oporavak portalu za servis.

- **Odabir**: to je ključna dio usluge i ne može se isključiti. Ako ne želite da se ovaj podaci koji se šalju servisu, ne Sastavi oporavak tarife u taj servis.

**Značajke: Preslikavanje**

- **Funkcija**: Ova značajka omogućuje vam da biste mapirali podatke o mreži iz centra za primarni podataka u Centar za oporavak podataka. Kada su virtualnim strojevima oporaviti oporavak web-mjesta, ovo mapiranje pomaže u uspostavljanje veza s mrežom za njih.

- **Podaci koji se prikupljaju**: kao dio značajka za mapiranje mreže, servis prikuplja, obrađuje i prenosi metapodataka logičke mreža za svakog web-mjesta (primarni i podatkovnim centrom).

- **Korištenje podataka**: na servis koristi metapodataka za popunjavanje servisni portal gdje možete mapirati podatke o mreži.

- **Odabir**: to je ključna dio usluge i ne može se isključiti. Ako ne želite da se ovaj podaci koji se šalju servisu, nemojte koristiti značajku mapiranje mreže.

**Značajka: Prebacivanje - planiranog, Neplanirano, test**

- **Funkcija**: ta značajka pomaže prebacivanje virtualnog računala centra za jednu VMM upravljanih podataka drugi VMM upravljanih podatkovnog centra. Prebacivanje akcija aktivira se korisnik njihove portala za servis. Mogući razlozi za na prebacivanje su neplanirano događaja (primjerice slučaju prirodnim disaster0; planiranog događaj (primjerice podatkovnog centra za ujednačavanje opterećenja); test prebacivanje (primjerice na oporavak plan Proba).

Davatelj usluga na poslužitelju VMM obavijesti događaja servisa i izvršava akciju za prebacivanje na glavnom računalu Hyper-V putem sučelja VMM. Stvarni prebacivanje virtualnog računala s jednog Hyper-V glavnog računala na drugo (obično izvodi u podatkovnom centru različite "oporavak") upravlja replikacije tehnologija sustava Windows Server 2012 ili Windows Server 2012 R2 Hyper-V. Po dovršetku na prebacivanje davatelja usluge na poslužitelju za VMM podatkovnog centra "oporavak" instalirani šalje informacije uspjeh servisa.

- **Podaci koji se prikupljaju**: U servis koristi gore navedenih podataka za popunjavanje status podataka akcija prebacivanje portala za servis.

- **Korištenje podataka**: na servis koristi gore navedenih podataka na sljedeći način:

    - Upravljanje certifikatom – koristi da biste lakše prepoznali i provjere autentičnosti registrirani VMM poslužitelj za pristup servisu. Servis koristi javni ključ dio certifikata radi zaštite tokena koje samo registrirani VMM server može ostvariti pristup. Poslužitelj mora koristiti ovaj token ostvariti pristup značajkama koje su usluge.
    - Naziv poslužitelja VMM – naziv poslužitelja u VMM potrebna je za prepoznavanje i komunicirati s odgovarajućim VMM poslužiteljem na kojem se nalaze oblaka.
    - Nazivi poslužitelja VMM u oblaku – naziv oblaka potreban je prilikom korištenja servisa oblaka uparivanje/unpairing značajka opisan u nastavku. Kada se odlučite za uparivanje oblaka iz centra za primarni podataka s drugog oblaka u centru za oporavak podataka, nazivi oblaka iz centra za oporavak podataka predstavljaju.

- **Odabir**: to je ključna dio usluge i ne može se isključiti. Ako ne želite da se ovaj podaci koji se šalju servisu, nemojte koristiti taj servis.

## <a name="next-steps"></a>Daljnji koraci

Nakon što prođete prebacivanje test da biste provjerili vaše okruženje funkcionira kako treba, [Dodatne informacije o](site-recovery-failover.md) različitim vrstama failovers.
