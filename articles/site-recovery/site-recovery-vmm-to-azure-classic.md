<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima u VMM oblaka za Azure | Microsoft Azure"
    description="U ovom se članku opisuje kako replicirati Hyper-V virtualnim strojevima na Hyper-V domaćini koja se nalazi u VMM centar sustava oblaka za Azure."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replicirati Hyper-V virtualnim strojevima u VMM oblaka za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-azure.md)
- [PowerShell – Voditelj resursa](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasični Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell – klasični](site-recovery-deploy-with-powershell.md)



Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md).

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako implementirati oporavak web-mjesta za replikaciju Hyper-V virtualnim strojevima na poslužiteljima glavno računalo Hyper-V koje se nalaze u VMM privatne oblaka Azure.

Članak sadrži preduvjeti za scenarij i objašnjava postavljanje zbirke ključeva za oporavak web-mjesta, dobiti davatelja oporavak web-mjesta Azure instalirana na poslužitelju VMM izvora, registraciju poslužitelja u na sigurnog, dodajte račun za Azure prostora za pohranu, instalirajte agent servisa Azure oporavak na poslužiteljima Hyper-V glavno računalo, konfiguriranje postavki zaštite za VMM oblaka koja će se primijeniti na sve zaštićene virtualnim strojevima , a zatim omogućiti zaštitu te virtualnim računalima. Dovršili postupak tako da testirate prebacivanje da biste provjerili funkcionira sve prema očekivanjima.

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="architecture"></a>Arhitektura

![Arhitektura](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Oporavak davatelja za Azure web-mjesta na koji je instaliran na VMM tijekom implementacije oporavak web-mjesta i poslužitelja VMM registriran u sigurnog oporavak web-mjesta. Davatelj komunicira s oporavak web-mjesta za rukovanje replikacije djelovanje.
- Agent servisa Azure oporavak je instalirana na poslužiteljima glavno računalo Hyper-V tijekom implementacije oporavak web-mjesta. Obrađuje podataka replikacije Azure za pohranu.


## <a name="azure-prerequisites"></a>Preduvjeti za Azure

Evo što sve trebate u Azure.

**Preduvjeta** | **Pojedinosti**
--- | ---
**Račun za Azure**| Potreban vam je račun sustava [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Azure prostora za pohranu** | Morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka. Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs su spun se kada se pojavi prebacivanje. <br/><br/>Potreban [račun standardni zemlj suvišnih prostora za pohranu](../storage/storage-redundancy.md#geo-redundant-storage). Račun mora na istom području kao servis oporavak web-mjesta i pridružiti iste pretplate. Imajte na umu da replikacija s računima za pohranu premium trenutno nije podržan, a ne bi trebalo koristiti.<br/><br/>[Doznajte](../storage/storage-introduction.md) Azure prostora za pohranu.
**Azure mreže** | Morat ćete Azure virtualne mreže koji Azure VMs će se povezati s kada dođe do prebacivanje. Azure virtualne mreže mora biti u istom području kao sigurnog oporavak web-mjesta.

## <a name="on-premises-prerequisites"></a>Lokalni preduvjeti

Evo što sve trebate lokalnog.

**Preduvjeta** | **Pojedinosti**
--- | ---
**VMM** | Potreban vam je barem jedan VMM poslužitelj implementiran kao samostalni fizički ili virtualni poslužitelj ili virtualne klaster. <br/><br/>Poslužitelj VMM trebao bi biti pokrenut sustava centra 2012 R2 s najnovijim kumulativnim ažuriranjima.<br/><br/>Potreban vam je barem jedan oblaka konfiguriran na poslužitelju VMM.<br/><br/>Oblak izvora koji želite zaštititi mora sadržavati jednu ili više grupa VMM glavnog računala.<br/><br/>Dodatne informacije o postavljanju oblaka VMM u [vodič: Stvaranje privatne oblaka pomoću sustava centra 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) na blogu Keith Mayer.
**Hyper-V** | Potreban vam je Hyper-V glavnog računala poslužitelja ili klastere u oblaku VMM. Poslužitelj za glavno računalo mora imati i jedan ili više VMs. <br/><br/>Poslužitelj Hyper-V mora biti pokrenut na najmanje **Windows Server 2012 R2** sa Hyper-V ulogu ili **Microsoft Hyper-V Server 2012 R2** i instalirali najnovija ažuriranja.<br/><br/>Bilo koji poslužitelj Hyper-V koja sadrži VMs koji želite zaštititi moraju nalaziti u oblaku za VMM.<br/><br/>Ako koristite Hyper-V u bilješci klaster te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Morat ćete ručno konfiguriranje broker klaster. [Dodatne informacije potražite](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) u blogu Aidan Finn.
**Zaštićeni računalima** | VMs koji želite zaštititi moraju biti usklađene sa [Azure preduvjeti](site-recovery-best-practices.md#azure-virtual-machine-requirements).


## <a name="network-mapping-prerequisites"></a>Preduvjeti za mapiranje mreže
Kada zaštitite virtualnim strojevima u Azure mreže karte mapiranja između VM mreže na izvornom poslužitelju VMM i ciljnu Azure mreže da biste omogućili sljedeće:

- Koje prebacivanje na istoj mreži možete se povezati s drugoga, bez obzira koju tarifu za oporavak su na svim računalima.
- Ako mrežni pristupnik je postavljena na odredištu Azure mreže, virtualnim strojevima možete povezati s drugim lokalnog virtualnim računalima sustava.
- Ako ne konfigurirate mreže mapiranje samo virtualnim strojevima koje ne prođu putem u istu tarifu oporavak će se moći povezati međusobno nakon prebacivanje na Azure.

Ako želite uvesti preslikavanje morat ćete sljedeće:

- Virtualnim strojevima koji želite zaštititi na izvornom poslužitelju VMM moraju biti povezani s mrežom VM. Tu mrežu mora biti povezan s logičke mreže koji je pridružen oblaka.
- Programa Azure mreža repliciranu virtualnim strojevima na koji možete povezati nakon prebacivanje. Ovu mrežu odabrat ćete trenutku prebacivanje. Mreža mora biti u području isti kao pretplate oporavak Azure web-mjesta.

Priprema za mrežni mapiranja na sljedeći način:

1. [Doznajte](site-recovery-network-mapping.md) mapiranje mreže.
2. Priprema VM mreža u VMM:

    - [Postavljanje mreže za logičke](https://technet.microsoft.com/library/jj721568.aspx).
    - [Postavljanje mreže za VM](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Korak 1: Stvaranje sigurnog za oporavak web-mjesta

1. Prijavite se na [Portal za upravljanje](https://portal.azure.com) poslužitelja VMM želite registrirati.
2. Kliknite **podatkovne usluge** > **oporavak Services** > **sigurnog oporavak web-mjesta**.
3. Kliknite **Stvori novi** > **brzo stvaranje**.
4. U odjeljak **naziv**unesite neslužbeni naziv da biste odredili na zbirke ključeva.
5. U **regiji**, odaberite regiji u zbirke ključeva. Da biste provjerili podržanih regija potražite u članku geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kliknite **Stvaranje sigurnog**.

    ![Nove zbirke ključeva](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Provjerite traku stanja da biste potvrdili na sigurnog uspješno je stvorena. Na sigurnog će biti navedena kao **aktivna** na glavnoj stranici servisa za oporavak.

## <a name="step-2-generate-a-vault-registration-key"></a>Korak 2: Stvaranje sigurnog ključa za registraciju

Generiranje ključa za registraciju u na zbirke ključeva. Nakon preuzimanja davatelja za oporavak Azure web-mjesta i instalirajte ga na poslužitelj VMM, koristit ćete taj ključ da biste registrirali VMM poslužitelja u zbirke ključeva.

1. Na stranici **Servisa oporavak** kliknite sigurnog da biste otvorili stranicu za brzo pokretanje. Brzi početak rada može se otvoriti u bilo kojem trenutku pomoću ikone.

    ![Ikona za brzi početak rada](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Na padajućem popisu odaberite **primjećuje lokalnog VMM web-mjestu sustava Microsoft Azure**.
3. U **Priprema VMM poslužiteljima**, kliknite **Generiraj ključa za registraciju** datoteku. Datoteka s ključem automatski se generira i vrijedi 5 dana nakon generira. Ako ne koristite pristupanja portalu za Azure s poslužitelja VMM morat ćete da biste kopirali ovoj datoteci na poslužitelj.

    ![Ključa za registraciju](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Korak 3: Instalacija davatelja oporavak Azure web-mjesta

1. U **Brzi Start** > **Priprema VMM poslužitelji**kliknite **Preuzmite Microsoft Azure web-mjesta oporavak Provider za instalaciju na poslužiteljima VMM** da biste nabavili najnoviju verziju instalacijsku datoteku davatelja usluga.
2. Pokrenite datoteku na s izvorišnim poslužiteljem VMM.

    >[AZURE.NOTE] Ako je VMM implementiran klasteru i instalirate davatelj usluga za prvo ga instalirati na aktivni čvor i dovršetak instalacije da biste registrirali VMM poslužitelja u sigurnog. Zatim instalirajte davatelja usluge na ostale čvorove. Imajte na umu da Ako nadograđujete davatelj morat ćete nadograditi na sve čvorove jer oni trebali biste sve moguće istu verziju davatelja usluga.

3. Instalacijski program provjerava prerequirements i zahtijeva dozvolu za zaustavljanje servisa VMM da biste započeli postavljanje davatelja usluga. Servis VMM će se ponovno pokrenuti automatski kada se dovrši instalacija. Ako ga instalirate na VMM klaster vas će se zatražiti da biste prestali klaster ulogu.

4. U **Programu Microsoft Update** možete odabrati u ima li ažuriranja. Ova postavka omogućena davatelja ažuriranja instalirat će se prema Pravilnik za Microsoft Update.

    ![Microsoftova ažuriranja](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  Mjesto za instalaciju za davatelja postavljeno na ** <SystemDrive>\Programske sustava centra 2012 R2\Virtual strojno Manager\bin**. Kliknite **Instaliraj**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Nakon instalacije davatelja usluge kliknite **registrirati** da biste registrirali poslužitelja u zbirke ključeva.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. U odjeljak **naziv sigurnog**provjerite naziv zbirke ključeva u kojem će biti registriran na poslužitelj. Kliknite *Dalje*.

    ![Registracija poslužitelja](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. U **Internetsku vezu** Navedite način na koji se davatelja usluge na poslužitelju VMM povezuje s Internetom. Odaberite **Poveži s postojeće postavke proxy poslužitelja** za korištenje na zadane postavke internetske veze konfiguriran na poslužitelju.

    ![Postavke internetske](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Ako želite koristiti prilagođenu proxy ga trebali postaviti prije instalacije davatelja usluga. Kada konfigurirate postavke proxyja prilagođene test će se pokrenuti da biste provjerili proxy vezu.
    - Ako koristite prilagođeni proxy ili zadani proxy zahtijeva provjeru autentičnosti morat ćete unijeti detalje proxy poslužitelj, uključujući adresu proxy poslužitelja i priključka.
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

13.  Kliknite **Dalje** da biste dovršili postupak. Nakon registracije, metapodataka s poslužitelja VMM se dohvaćaju oporavak Azure web-mjesta. Na kartici **Poslužitelji VMM** na stranici **poslužitelja** u na sigurnog prikazat će se na poslužitelj.

    ![Lastpage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Nakon registracije, metapodataka s poslužitelja VMM se dohvaćaju oporavak Azure web-mjesta. Na kartici **Poslužitelji VMM** na stranici **poslužitelja** u na sigurnog prikazat će se na poslužitelj.

### <a name="command-line-installation"></a>Instalacija naredbenog retka

Oporavak davatelja za Azure web-mjesta mogu se instalirati i pomoću sljedećih naredbenog retka. Ova metoda se može koristiti da biste instalirali davatelja usluge na na poslužitelju Core za Windows Server 2012 R2.

1. Preuzmite tipku davatelja instalacije datoteke i registraciju u mapu. Na primjer: C:\ASR.
2. Zaustavljanje servisa upravitelja virtualnog računala centar sustava
3. S dodatnim naredbeni redak izdvajanje installer davatelja pomoću ove naredbe:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalacija davatelja na sljedeći način:

        C:\ASR> setupdr.exe /i

5. Davatelj registrirali na sljedeći način:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Gdje Parametri se na sljedeći način:

 - **/Credentials** : obavezna parametar koji određuje mjesto u kojoj se nalazi datoteka s ključem Registracija  
 - **/FriendlyName** : obavezna parametar za naziv poslužitelja Hyper-V glavnog računala koja se pojavljuje na portalu za oporavak Azure web-mjesta.
 - **/EncryptionEnabled** : neobavezan parametar odredite želite li šifriranje virtualnim strojevima u Azure (pri šifriranje rest). Naziv datoteke mora imati nastavak **.pfx** .
 - **/proxyAddress** : neobavezan parametar koji određuje adresu proxy poslužitelja.
 - **/proxyport** : neobavezan parametar koji određuje priključak proxy poslužitelj.
 - **/proxyUsername** : neobavezan parametar koji određuje proxy korisničko ime.
 - **/proxyPassword** : neobavezan parametar koji određuje proxy lozinku.  


## <a name="step-4-create-an-azure-storage-account"></a>Korak 4: Stvorite račun za Azure prostora za pohranu

1. Ako nemate račun za Azure prostora za pohranu kliknite **Dodaj račun za pohranu Azure** da biste stvorili račun.
2. Stvaranje poslovnog subjekta s zemlj replikacijom omogućena. Morate u području isti kao servisa Azure oporavak web-mjesta, a biti pridruženi iste pretplate.

    ![Račun za pohranu](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migracije računa za pohranu](../resource-group-move-resources.md) po grupama resursa unutar iste pretplate ili uzduž pretplate nije podržana za račune za pohranu koji se koriste za implementaciju oporavak web-mjesta.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Korak 5: Instalacija agenta servisa za Azure oporavak

Instalirajte agent servisa Azure oporavak na svakom Hyper-V glavnog poslužitelja u oblak VMM.

1. Kliknite **Brzi Start** > **Preuzimanje Azure web-mjesta servisa agenta za oporavak i instalacija na domaćini** da biste nabavili najnoviju verziju agent instalacijsku datoteku.

    ![Instalacija agenta servisa za oporavak](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Pokrenite instalacijsku datoteku na svakom poslužitelju Hyper-V glavnog računala.
3. Kliknite **Dalje**na stranici **Provjera preduvjeti** . Bilo koji nedostaju preduvjeti instalirat će se automatski.

    ![Agenta servisa za oporavak preduvjeti](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Na stranici **Postavke instalacije** navedite mjesto na koje želite instalirati agenta, a zatim odaberite mjesto predmemorije u kojem će se instalirati metapodatke sigurnosne kopije. Zatim kliknite **Instaliraj**.
5. Kada se instalacija završi kliknite **Zatvori** da biste dovršili Čarobnjak.

    ![Registrirajte se OŽU Agent](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Instalacija naredbenog retka

Možete instalirati i agenta za servise sustava Microsoft Azure oporavak iz naredbenog retka ta naredba:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Korak 6: Konfiguriranje oblaka postavke zaštite

Kada je registrirana VMM poslužitelj, možete konfigurirati postavke zaštite oblaka. Mogućnost **Sinkroniziraj oblaka podataka pomoću na sigurnog** omogućen prilikom instalacije davatelj tako da sva oblaka na poslužitelju VMM pojavit će se na karticu <b>Zaštićeni stavke</b> u na sigurnog.

![Objavljeni oblaka](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Na stranici za brzo pokretanje kliknite **Postavljanje protection za VMM oblaka**.
2. Na kartici **Zaštićeni stavke** kliknite oblak koji želite konfigurirati, a zatim idite na karticu **Konfiguracija** .
3. U **ciljnoj** odaberite **Azure**.
4. U **Prostor za pohranu računa** odaberite račun Azure prostora za pohranu koji koristite za replikaciju.
5. Postavite **šifriranje pohranjene podatke** na **Isključeno**. Tom se postavkom određuje se da šifrirane podataka replicirati između lokalnog web-mjesta i Azure.
6. U **kopirajte učestalost** ostavite je zadana postavka. Ta vrijednost određuje koliko se često podataka mora se sinkronizirati izvorišno i odredišno mjesta.
7. U **Zadrži oporavak točke za**ostavite je zadana postavka. Sa zadanom vrijednosti nula, samo najnovije oporavak točke za primarni virtualnog računala pohranjuju se na poslužitelju replike glavnog računala.
8. U **Učestalost aplikacije dosljedan snimke**, ostavite je zadana postavka. Ta vrijednost određuje koliko je često potrebno stvoriti snimke. Snimke pomoću servisa Kopiraj glasnoću sjena (VSS) da bi bili aplikacije u dosljedan stanju kada se koristi se snimka.  Ako postavite vrijednost, obavezno je manji od broj točaka dodatne oporavak konfigurirate.
9. U **vrijeme početka replikacije**, navedite kada bi trebao počinjati početne replikacije podataka za Azure. Koristit će se vremenska zona na poslužitelju Hyper-V glavnog računala. Preporučujemo da planirate početne replikacije najfrekventnija.

    ![Postavke za replikaciju oblaka](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Nakon što spremite postavke posao stvorit će se i moguće nadzirati na kartici **Zadaci** . Svim poslužiteljima Hyper-V glavnog računala u oblaku izvora VMM će se konfigurirati za replikaciju.

Nakon spremanja, oblaka moguće izmijeniti postavke na kartici **Konfiguriraj** . Da biste izmijenili odredišnog mjesta ili račun za pohranu za ciljnu morat ćete ukloniti konfiguraciju oblaka i konfigurirajte oblaka. Imajte na umu da ako promijenite račun za pohranu promjena se primjenjuje samo za virtualnim strojevima omogućene za zaštitu kada je izmijenjena račun za pohranu. Postojeće virtualnim strojevima ne migrira se na novi račun za pohranu.

## <a name="step-7-configure-network-mapping"></a>Korak 7: Konfiguriranje mapiranja mreže
Prije nego što počnete preslikavanje provjerite je li virtualnim strojevima na izvornom poslužitelju VMM povezani s mrežom VM. Osim toga stvoriti jednu ili više mreža Azure virtualne. Imajte na umu da se više mreža VM mogu mapirati u jedne mreže Azure.

1. Na stranici za brzo pokretanje kliknite **Mapiranje mrežama**.
2. Na kartici **mreža** u **izvornom mjestu**, odaberite s izvorišnim poslužiteljem VMM. **Ciljno mjesto** odaberite Azure.
3. Na **izvor** mreža prikazat će se popis mreža VM povezan s poslužiteljem VMM. Na **ciljnom** mreža prikazat će se Azure mreža povezan s pretplatom.
4. Odabir mreže VM izvora, a zatim kliknite **Karta**.
5. Na stranici **odaberite ciljni mrežu** , odaberite ciljnu Azure mreže koju želite koristiti.
6. Kliknite kvačicu da biste dovršili postupak mapiranje.

    ![Postavke za replikaciju oblaka](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Nakon što spremite postavke posao pokreće za praćenje napretka mapiranje i moguće nadzirati na kartici zadaci. Sve postojeće replike virtualnim strojevima koji odgovaraju na mrežu VM izvora će se povezati s ciljnu Azure mrežama. Novi virtualnim strojevima koji su povezani s mrežom VM izvora će biti povezani s mapirani Azure mrežni nakon replikacije. Ako mijenjate postojeći mapiranje pomoću nove mreže, replike virtualnim strojevima će se povezati pomoću nove postavke.

Imajte na umu da ako mreža cilj ima više podmreže, a zatim neku od tih podmreže ima isti naziv kao podmreže na kojem nalazi virtualnog računala izvora, a zatim virtualnog računala replike će se povezati s tom podmreže ciljne nakon prebacivanje. Ako nema cilj podmreže pod nazivom koji se podudaraju, virtualnog računala će se povezati s prvom podmreže u mreži.

> [AZURE.NOTE] [Migracija mreže](../resource-group-move-resources.md) preko grupe resursa unutar iste pretplate ili putem pretplate nije podržana za mreže koji se koristi za implementaciju oporavak web-mjesta.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Korak 8: Omogućavanje zaštitu virtualnim strojevima

Nakon poslužitelje, oblaka i mreža ispravno konfigurirani, možete omogućiti zaštitu virtualnih računala u oblaku. Imajte na umu sljedeće:

- Virtualnim strojevima mora zadovoljavati [Azure uvjete](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Da biste omogućili zaštitu operacijski sustav i operacijski sustav morate postaviti na disku svojstva virtualnog računala. Kada stvorite virtualnog računala u VMM pomoću predloška virtualnog računala možete postaviti svojstvo. Možete postaviti i tih svojstava za postojeće virtualnim strojevima na karticama **Općenito** i **Hardverske konfiguracije** svojstva virtualnog računala. Ako ne postavite tih svojstava u VMM ćete moći ih konfigurirali na portalu za oporavak Azure web-mjesta.

    ![Stvaranje virtualnog računala](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Izmijenite svojstva virtualnog računala](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Da biste omogućili zaštitu, na kartici **virtualnih računala** u oblaku u kojoj se nalazi, virtualnog računala kliknite **Omogući zaštitu** > **Dodaj virtualnih računala**.
2. Na popisu virtualnih računala u oblaku, odaberite onaj koji želite zaštititi.

    ![Omogući zaštitu virtualnog računala](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Praćenje tijeka **Omogućiti zaštitu** akcije na kartici **zadataka** , uključujući početne replikacije. Nakon izvođenja posla **Dovršavanje zaštitu** virtualnog računala spremna je za prebacivanje. Kada je omogućena zaštita i virtualnim strojevima su replicirati, ćete moći njihov prikaz u Azure.


    ![Zadatak zaštitu virtualnog računala](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Provjerite svojstva virtualnog računala i izmijenite prema potrebi.

    ![Provjerite je li virtualnim strojevima](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Na kartici **Konfiguriraj** svojstva virtualnog računala mogu mijenjati sljedeća svojstva mreže.





- **Broj mrežnih prilagodnika na ciljnom računalu virtualne** – broj mrežnih prilagodnika diktirali je veličinom navedete ciljne virtualnog računala. Potvrdite [Specifikacija veličina virtualnog računala](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) za broj prilagodnika podržava veličinu virtualnog računala. Kada mijenjati veličinu virtualnog računala i spremili postavke, broj mrežnog prilagodnika će se promijeniti kada otvorite stranicu za **Konfiguriranje** sljedeći put. Broj mrežnih prilagodnika cilj virtualnih računala je najmanji broj mrežnih prilagodnika na izvor virtualnog računala i najveći broj mrežnih prilagodnika podržava veličina virtualnog računala odabran, na sljedeći način:

    - Ako je broj mrežnih prilagodnika na računalu izvor manje od ili jednak broju prilagodnika dopušteno Odredišna veličina računalu, zatim cilj će imati isti broj prilagodnika kao izvor.
    - Ako broj prilagodnika za virtualnog računala izvora premašuje broj dopušten za Odredišna veličina, a zatim Maksimalna veličina ciljne će se koristiti.
    - Na primjer, ako izvor strojno ima dvije mrežnih prilagodnika, a odredišna veličina računalo podržava četiri, ciljnom računalu neće imati dva prilagodnika. Ako na računalu izvor ima dva prilagodnika, ali veličinu podržani cilj podržava samo jedan ciljnom računalu neće imati samo jedan prilagodnika.    

- **Mreža virtualnog računala cilj** - mrežu na kojoj virtualnog računala povezuje s ovisi o preslikavanje mreže izvor virtualnog računala. Ako virtualnog računala izvor sadrži više od jednog mrežnog prilagodnika i izvor mreža mapiraju se različite mreža ciljnom, zatim morat ćete odabrati neki od mreža ciljne.
- **Podmreže svaki mrežnog prilagodnika** - za svaku mrežnog prilagodnika možete odabrati podmreže na koji nije uspio putem virtualnog računala želite povezati.
- **Cilj IP adresa** – ako mrežnog prilagodnika izvor virtualnog računala je konfiguriran za korištenje statičke IP adrese, a zatim možete unijeti IP adresa za ciljnu virtualnog računala. Pomoću ove značajke zadržati IP adresu izvora virtualnog računala nakon na prebacivanje. Ako je ne IP adrese pa dostupna IP adresa daje mrežnog prilagodnika trenutku prebacivanje. Ako IP adresu cilja nije naveden, ali se već koristi drugi virtualnog računala koji se izvodi u Azure zatim prebacivanje neće uspjeti.  

    ![Izmijenite svojstva za mrežu](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Linux virtualnim strojevima sa statičke IP adrese nisu podržane.

## <a name="test-the-deployment"></a>Provjera uvođenja

Da biste testirali implementaciju sustava možete pokrenuti prebacivanje test za jednu virtualnog računala ili stvorite plan oporavak koji se sastoji od više virtualnim strojevima i pokrenite test prebacivanje plana.  

Prebacivanje test simulira prebacivanje i oporavak mehanizam Izolirani mreže. Imajte na umu da:

- Ako se želite povezati virtualnog računala u Azure putem udaljene radne površine kada se prebacivanje omogućiti udaljene radne površine na virtualnog računala prije pokretanja prebacivanje test.
- Nakon prebacivanje koristit ćete javnu IP adresa za povezivanje s virtualnog računala u Azure putem udaljene radne površine. Ako želite da biste to učinili, provjerite je li još nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.

>[AZURE.NOTE] Da biste ostvarili najbolje performanse kada to učinite na prebacivanje Azure, provjerite je li Provjerite jeste li instalirali Agent za Azure u zaštićenom računala. To pomaže u pokretanje brže i pomaže u Dijagnostika u slučaju problema. Agent za Linux mogu biti pronađene [ovdje](https://github.com/Azure/WALinuxAgent) - i Windows agent nalazi se [ovdje](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Stvaranje plana oporavak

1. Na kartici **Tarife za oporavak** dodajte nove tarife. Navedite naziv **VMM** u odjeljku **Vrsta izvora**i s izvorišnim poslužiteljem VMM u **izvor**, u ciljne će se Azure.

    ![Stvaranje plana za oporavak](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Na stranici **Odaberite virtualnim strojevima** odaberite virtualnih računala da biste dodali plan za oporavak. Ove virtualnim strojevima dodaju se u zadanu grupu za oporavak plan – 1 grupe. Maksimalno 100 virtualnim strojevima u jednom oporavak plan testirate.

- Ako želite provjeriti svojstva virtualnog računala prije nego ih dodate na željenu tarifu, kliknite virtualnog računala na stranici Svojstva oblaka u koje it je nalazi. Na konzoli VMM možete konfigurirati i svojstva virtualnog računala.
- Sve virtualnim strojevima koje su prikazane su omogućene za zaštitu. Popis obuhvaća oba virtualnim strojevima koji imaju omogućeno je dovršena zaštitu i početne replikacije i one koje su omogućene za zaštitu s velikim početnim replikacije na čekanju. Samo virtualnim strojevima s početnog replikacijom dovršiti uspijeva putem kao dio plan za oporavak.

    ![Stvaranje plana za oporavak](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Nakon oporavka planiranje stvaranja ga pojavit će se na kartici **Tarife za oporavak** . [Automatizacija Azure runbooks](site-recovery-runbook-automation.md) možete dodati i oporavak plan za automatizaciju akcija tijekom prebacivanje.

### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

Pokrenite test prebacivanje u Azure na dva načina.

- **Testiranje prebacivanje bez Azure mreže**– ovu vrstu prebacivanje test provjerava da virtualnog računala isporučuje pravilno u Azure. Svaka mreža za Azure neće biti povezani virtualnog računala nakon prebacivanje.
- **Testiranje prebacivanje sa Azure mrežom**– ovu vrstu provjere prebacivanje okruženje za cijelu replikacije dolazi do prema očekivanjima i koja se nije uspjela tijekom virtualnim strojevima će se povezati s cilj Azure mreže. Za rukovanje podmreže za testiranje prebacivanje podmreže virtualnog računala test će biti shvatili ovisno o podmreže replike virtualnog računala. Ovo je različite običnog replikacije kad podmreže replike virtualnog računala temelji se na podmreži izvor virtualnog računala.

Ako želite pokrenuti prebacivanje test za virtualnog računala bez navođenja programa Azure cilj mreža ne morate ništa Priprema omogućen za zaštitu Azure. Da biste pokrenuli test prebacivanje s ciljnu Azure mreži morat ćete da biste stvorili novu Azure mrežu koja sadrži Izolirani u mreži Azure proizvodnje (zadano ponašanje prilikom stvaranja nove mreže u Azure). Pogledajte kako [pokrenite test prebacivanje](site-recovery-failover.md#run-a-test-failover) više pojedinosti.


Također morat ćete postaviti infrastrukture za repliciranu virtualnog računala funkcionirati prema očekivanjima. Virtualnog računala s kontroler domena i DNS- a, na primjer, moguće je replicirati na Azure pomoću oporavak Azure web-mjesta i moguće stvoriti u mreži Probno korištenje testiranje prebacivanje. Pogledajte [testiranje prebacivanje pitanja vezana uz za active directory](site-recovery-active-directory.md#considerations-for-test-failover) odjeljak dodatne informacije.

Da biste pokrenuli učinite za prebacivanje test na sljedeći način:

1. Na kartici **Oporavak tarife** odaberite tarifu, a zatim kliknite **Testiraj prebacivanje**.
2. Na stranici **Potvrda Test prebacivanje** odaberite **ništa** ili određenu Azure mrežu.  Napomena Ako odaberete nema prebacivanje testa Provjera točan replicirati virtualnog računala na Azure ali ne provjerava mrežna konfiguracija replikacije.

    ![Nema mreže](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Ako je šifriranje podataka omogućen za oblak, **Ključa za šifriranje** odaberite certifikat izdan tijekom instalacije davatelja usluge na poslužitelju VMM kada je uključena mogućnost da biste omogućili šifriranje podataka za tijekom spremanja.
4. Na kartici **Zadaci** možete pratiti napredak prebacivanje. I trebali biste moći vidjeti replike test virtualnog računala na portalu za Azure. Ako ste postavljeno na virtualnim strojevima pristup iz lokalne mreže možete pokrenuti udaljenu radnu površinu vezu virtualnog računala.
5. Kada u prebacivanje dosegne faza **testiranje dovrši** , kliknite **Dovrši testirati** da biste dovršili postupak prebacivanje test. Možete se Dubinska analiza prema dolje na kartici **zadatak** da biste pratili tijek prebacivanje i status, a da biste izvršili radnje koje su vam potrebne.
6. Nakon prebacivanje ćete je moći vidjeti replike test virtualnog računala na portalu za Azure. Ako ste postavljeno na virtualnim strojevima pristup iz lokalne mreže možete pokrenuti udaljenu radnu površinu vezu virtualnog računala. Učinite sljedeće:

    1. Provjerite je li uspješno pokretanje virtualnih računala.
    2. Ako se želite povezati virtualnog računala u Azure putem udaljene radne površine kada se prebacivanje omogućiti udaljene radne površine na virtualnog računala prije pokretanja prebacivanje test. Ćete morati dodati krajnje RDP virtualnog računala. Omogućuje korištenje [Azure Automatizacija Runbooks](site-recovery-runbook-automation.md) da to učinite.
    3. Nakon prebacivanje ako koristite javno IP adresu za povezivanje s virtualnog računala u Azure putem udaljene radne površine, provjerite je li nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.

7.  Po dovršetku testiranja učinite sljedeće:
    - Kliknite **test prebacivanje je dovršena**. Čišćenje okruženje za testiranje automatski isključujte i brisanje virtualnim strojevima test.
    - Kliknite **bilješke** da biste snimili i spremili sve opažanja povezan s pomoćnim test.

>

## <a name="next-steps"></a>Daljnji koraci

Informacije o [postavljanju tarife za oporavak](site-recovery-create-recovery-plans.md) i [Prebacivanje](site-recovery-failover.md).
