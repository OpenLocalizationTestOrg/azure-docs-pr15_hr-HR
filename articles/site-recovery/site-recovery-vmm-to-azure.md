<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima u VMM oblaka za Azure oporavak web-mjesta pomoću portala za Azure | Microsoft Azure"
    description="U članku se opisuje kako implementirati Azure oporavak web-mjesta da biste orkestrirali replikacije, prebacivanje i oporavak Hyper-V VMs u VMM oblaka za Azure pomoću portala za Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Replicirati Hyper-V virtualnim strojevima u VMM oblaka za Azure oporavak web-mjesta Azure pomoću portala za Azure | Microsoft Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-azure.md)
- [Klasični Azure](site-recovery-vmm-to-azure-classic.md)
- [Voditelj resursa PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasični PowerShell](site-recovery-deploy-with-powershell.md)

Dobro došli u oporavak Azure web-mjesta! Koristite ovaj članak ako želite replicirati lokalnog Hyper-V virtualnim strojevima upravlja se iz oblaka sustava centra virtualnog računala Manager (VMM) za Azure pomoću oporavak Azure web-mjesta na portalu za Azure.

> [AZURE.NOTE]Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model
> ) za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije.


Azure oporavak web-mjesta na portalu za Azure nudi nekoliko novih značajki:

- Na portalu za Azure servisa Azure sigurnosne kopije i oporavak web-mjesta Azure kombiniraju se u jednu oporavak servisa sigurnog, tako da možete postaviti i upravljanje poslovanje i Izrada oporavak (BCDR) s jednog mjesta. Nadzorna ploča za Sjedinjeno komuniciranje omogućuje nadzor i upravljanje operacije na web-mjesta na lokalni Azure javno poslužitelja u oblak i.
- Korisnici s Azure pretplate dodjeli programom oblaka rješenje davatelj (CSP) sada mogu upravljati operacije oporavak web-mjesta na portalu za Azure.
- Oporavak web-mjesta na portalu za Azure možete replicirati strojeva Azure Voditelj resursa za pohranu računima. Oporavak web-mjesta na prebacivanje, stvara VMs utemeljen na Voditelj resursa u Azure.
- Oporavak web-mjesta i dalje podržava replikaciju račune klasični prostora za pohranu. Pri prebacivanje, oporavak web-mjesta stvara VMs pomoću klasične modela.


Kad pročitate članak, pri dnu u komentarima Disqus objavljuju komentare. Zamolite tehnička pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR trebali biste zadržali poslovnih podataka sigurnih i koje se mogu vratiti i provjerite je li radnih opterećenja ostaju stalno dostupne kada se pojavi Izrada.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnim centrom. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarne mjesto da bi aplikacija i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)

Ovaj članak sadrži sve potrebne informacije za replikaciju lokalnog Hyper-V VMs u VMM oblaka za Azure. Uključuje se pregled arhitekture, Planiranje informacije i koraci za implementaciju za konfiguriranje Azure, lokalne poslužitelje, postavke replikacije i planiranje kapaciteta. Nakon što ste postavili Infrastruktura možete omogućiti replikacije na koje želite zaštititi, a zatim potvrdite taj pomoćni funkcionira.


## <a name="business-advantages"></a>Prednosti tvrtke

- Oporavak web-mjesta omogućuje službenoj zaštite radnih opterećenja tvrtke i aplikacije koje rade na VMs Hyper-V.
- Portal za oporavak servisa predstavlja jedno mjesto za postavljanje, upravljanje i praćenje replikacije, prebacivanje i oporavak.
- Možete jednostavno pokrenuti failovers iz infrastruktura za lokalni Azure i failback (Vraćanje) iz Azure Hyper-V glavno računalo poslužiteljima s lokalnim web-mjesta.
- Tarife za oporavak s više računala možete konfigurirati tako da se tiered aplikacije radnih opterećenja neće uspjeti putem zajedno.


## <a name="scenario-architecture"></a>Scenarij arhitekture


Ovo su komponente scenarija:

- **VMM poslužitelja**: lokalnog poslužitelja VMM s jednog ili više oblaka.
- **Glavno računalo Hyper-V ili klaster**: Hyper-V glavnog računala poslužitelja ili klastere upravlja se iz oblaka VMM.
- **Davatelj usluga za oporavak web-mjesta za Azure i agenta servisa za oporavak**: tijekom implementacije instalirati davatelja za oporavak Azure web-mjesta na poslužitelju VMM i agent servisa Microsoft Azure oporavak Services na poslužiteljima Hyper-V glavnog računala. Davatelj usluga na poslužitelju VMM putem HTTP 443 za replikaciju djelovanje komunicira s oporavak web-mjesta. Agent na poslužitelju za glavno računalo Hyper-V replicira podataka Azure za pohranu putem HTTP 443 prema zadanim postavkama.
- **Azure**: potrebno Azure pretplatu, račun Azure prostora za pohranu podataka iz trgovine replicirati i Azure virtualne mreže tako da se Azure VMs nakon prebacivanje povezani s mrežom.

![Topologija E2A](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Preduvjeti za Azure

Evo što vam je potrebno u Azure za implementaciju scenarij.

**Preduvjeta** | **Pojedinosti**
--- | ---
**Račun za Azure**| Potreban račun [Sustava Microsoft Azure](http://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Azure prostora za pohranu** | Potreban račun standardne Azure prostora za pohranu za pohranu repliciranu podataka. Možete koristiti LRS ili GRS računa za pohranu. Preporučujemo da GRS tako da se podaci prebacuju ako regionalne nestalo ili ako je primarni regija nije moguće oporaviti. [Dodatne informacije](../storage/storage-redundancy.md). Račun mora biti u području isti kao sigurnog servise za oporavak.<br/><br/>Prostor za pohranu Premium nije podržana.<br/><br/> Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs stvaraju kada dođe do prebacivanje. <br/><br/> [Doznajte](../storage/storage-introduction.md) Azure prostora za pohranu.
**Azure mreže** | Potreban vam je Azure virtualne mreže koji povezuju Azure VMs kada dođe do prebacivanje. Mreža mora biti u području isti kao oporavak servisa sigurnog.

## <a name="on-premises-prerequisites"></a>Lokalni preduvjeti

Evo što vam je potrebno lokalnog

**Preduvjeta** | **Pojedinosti**
--- | ---
**VMM**| Jedan ili više VMM poslužitelji pokrenut na sustavu centar 2012 R2. Svaki poslužitelj VMM trebali biste dobiti jednu ili više oblaka konfiguriran. Tijekom spremanja mora sadržavati:<br/><br/> Jedan ili više VMM glavno računalo grupe.<br/><br/> Jedan ili više Hyper-V glavnog računala poslužitelja ili klastere u svakoj grupi glavnog računala.<br/><br/>[Dodatne informacije](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) o postavljanju VMM oblaka.
**Hyper-V** | Glavno računalo Hyper-V poslužitelje morate imati barem **Windows Server 2012 R2** Hyper-V uloge ili **Microsoft Hyper-V Server 2012 R2** i instalirali najnovija ažuriranja.<br/><br/> Hyper-V poslužitelja smiju sadržavati jedan ili više VMs.<br/><br/> Hyper-V glavnog poslužitelja ili klaster koja obuhvaća VMs želite replicirati njome se upravlja VMM oblaka.<br/><br/>Poslužitelji Hyper-V moraju biti povezani s Internetom izravno ili putem proxy poslužitelj.<br/><br/>Hyper-V poslužitelja mora imati ispravaka navedenih u članku [2961977](https://support.microsoft.com/kb/2961977) instaliran.<br/><br/>Hyper-V glavnog računala poslužitelja za replikacije podataka za Azure potreban pristup Internetu.
**Davatelj usluga i agent** | Tijekom implementacije oporavak web-mjesta za Azure ćete instalirati davatelja za oporavak Azure web-mjesta na poslužitelj VMM i agent za oporavak usluge na Hyper-V domaćini. Davatelj usluga i agent morate se povezati s Azure izravno s Interneta ili putem proxy poslužitelj. Imajte na umu utemeljen na HTTP proxy poslužitelja nije podržana. Proxy poslužitelj na poslužitelju VMM i Hyper-V domaćini dopustite pristup: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Ako imate IP utemeljen na adresi vatrozid pravila na poslužitelju VMM, provjerite da pravila dopustiti komunikaciju za Azure. Morate dopustiti [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i priključak HTTPS (443).<br/><br/>Dopusti rasponi IP adresa za Azure regija pretplate i Zapad SAD-a.<br/><br/>osim toga. proxy poslužitelj na poslužitelju VMM treba pristup https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Preduvjeti za zaštićeni računala


**Preduvjeta** | **Pojedinosti**
--- | ---
**Zaštićeni VMs** | Prije uspjeti putem programa VM, provjerite je li naziv koji je dodijeljen Azure VM uspostavu [Azure preduvjeti](site-recovery-best-practices.md#azure-virtual-machine-requirements). Kada omogućite replikacije za na VM možete izmijeniti naziv. <br/><br/> Kapacitet pojedinačne disk na zaštićenom bi trebalo biti više od 1023 GB. Na VM mogu imati do 16 diskova (dakle do 16 TB).<br/><br/> Zajedničko korištenje na disku goste klastere nisu podržane.<br/><br/> Sjedinjene Extensible firmver sučelja (UEFI) / pokretanje Extensible firmver Interface(EFI) nije podržana.<br/><br/> Ako je izvor VM NIC udruživanje nakon prebacivanje na Azure se pretvara jedan NIC.<br/><br/>Zaštita VMs Linux koristeći statičke IP adrese nisu podržane.

## <a name="prepare-for-deployment"></a>Priprema za implementaciju

Priprema za implementaciju morate:

1. [Postavljanje Azure mreže](#set-up-an-azure-network) u kojem Azure VMs nalazit će se nakon prebacivanje.
2. [Postavljanje računa Azure prostora za pohranu](#set-up-an-azure-storage-account) za repliciranu podataka.
4. [Priprema VMM poslužitelja](#prepare-the-vmm-server) za implementaciju oporavak web-mjesta.
5. [Priprema za preslikavanje](#prepare-for-network-mapping). Postavljanje mreže tako da možete konfigurirati preslikavanje tijekom implementacije oporavak web-mjesta.

### <a name="set-up-an-azure-network"></a>Postavite mrežu Azure

Morate Azure mrežu tako da se Azure VMs stvorene nakon prebacivanje će se povezati s njim.

- Mreža mora biti u području isti kao jedan implementacija sigurnog servise za oporavak.
- Ovisno o modelu resursa koji želite koristiti za nije uspjela tijekom Azure VMs, postavit ćete Azure mreže u [načinu Voditelj resursa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) ili [Klasični](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Preporučujemo da postavite mrežu prije nego što počnete. Ako ih ne morate učiniti tijekom implementacije oporavak web-mjesta.

> [AZURE.NOTE] [Migracija mreže](../resource-group-move-resources.md) preko grupe resursa unutar iste pretplate ili putem pretplate nije podržana za mreže koji se koristi za implementaciju oporavak web-mjesta.


### <a name="set-up-an-azure-storage-account"></a>Postavljanje računa za pohranu za Azure

- Potreban vam je račun standardne Azure prostora za pohranu na čuvanje podataka replicirati na Azure. Račun mora biti u području isti kao sigurnog servise za oporavak.
- Ovisno o modelu resursa koji želite koristiti za nije uspjela tijekom Azure VMs, postavite račun u [načinu Voditelj resursa](../storage/storage-create-storage-account.md) ili [Klasični](../storage/storage-create-storage-account-classic-portal.md).
- Preporučujemo da postavite račun prije nego što počnete. Ako ih ne morate učiniti tijekom implementacije oporavak web-mjesta.

> [AZURE.NOTE] [Migracije računa za pohranu](../resource-group-move-resources.md) po grupama resursa unutar iste pretplate ili uzduž pretplate nije podržana za račune za pohranu koji se koriste za implementaciju oporavak web-mjesta.

### <a name="prepare-the-vmm-server"></a>Priprema VMM poslužitelja

- Provjerite je li poslužitelj VMM uspostavu [preduvjeti](#on-premises-prerequisites).
- Tijekom uvođenja oporavak web-mjesta, možete odrediti sve oblaka na poslužitelju VMM mora biti dostupni na portalu za Azure. Ako želite samo određene oblaka da se pojavi na portalu, možete omogućiti tu postavku na oblaka u administracijskoj konzoli sustava VMM.


### <a name="prepare-for-network-mapping"></a>Priprema za preslikavanje

Morate postaviti preslikavanje tijekom implementacije oporavak web-mjesta. Preslikavanje karte između izvora VMM VM mreže i ciljnu Azure mreže da biste omogućili sljedeće:

- Strojeva koje ne prođu pokazivač na istoj mreži možete povezati s drugoga, čak i ako ih se ne nije uspjela putem na isti način kao i u istu tarifu za oporavak.
- Ako pristupnika mreže postavljena na odredištu Azure mreže, Azure virtualnim strojevima možete povezati s lokalnim virtualnih računala.
- Da biste postavili mrežu mapiranja ovdje je što vam je potrebno da biste pripremili:

    - Provjerite je li VMs na izvornom poslužitelju Hyper-V glavno računalo povezani s mrežom VMM VM. Tu mrežu mora biti povezan s logičke mreže koji je pridružen oblaka.
    - Azure mreža kao što je opisano [iznad](#set-up-an-azure-network)

- [Dodatne informacije](site-recovery-network-mapping.md) o načinu funkcioniranja a preslikavanje.


## <a name="create-a-recovery-services-vault"></a>Stvaranje oporavak servisa sigurnog

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Kliknite **Novi** > **upravljanja** > **servise za oporavak**. Umjesto toga možete kliknuti **Pregledaj** > sefovi**Oporavak usluge** > **Dodaj**.

    ![Nove zbirke ključeva](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. U odjeljku **naziv**, navedite neslužbeni naziv da biste odredili na sigurnog. Ako imate više pretplata, odaberite jedan od njih.
4. [Stvaranje grupe resursa](../resource-group-template-deploy-portal.md)ili odaberite postojeći. Navedite Azure regija. Strojeva će je replicirati na tom području. Da biste provjerili podržanih regija pogledajte geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Ako želite brzo pristupiti na sigurnog na nadzornoj ploči, kliknite **Prikvači na nadzornoj ploči** > **Stvaranje sigurnog**.

    ![Nove zbirke ključeva](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Pojavit će se novi sigurnog na **nadzornoj ploči** > **sve resurse**i na glavnom plohu **sefovi servise za oporavak** .

## <a name="getting-started"></a>Početak rada

Oporavak web-mjesta sadrži prvi koraci sučelje koji olakšava brzo moguće uvesti. Početak rada provjerava preduvjeti web-mjesta i vodi vas kroz implementacije koraka u redoslijedu desnom oporavak web-mjesta.

U prvi koraci odaberite vrstu strojeva želite za replikaciju i mjesto na koje želite replicirati na. Postavljanje lokalne poslužitelje, račune Azure prostora za pohranu i mrežama. Možete stvoriti pravila za replikaciju i izvođenje planiranja kapaciteta. Nakon što ste postavili preduvjete infrastrukture Omogući replikacije za VMs. Pokrenite failovers za određene strojeva ili stvaranje tarife za oporavak uvoza na više računala.

Početak rada započeti tako da odaberete način na koji želite uvesti oporavak web-mjesta. Početak rada tijek promjene malo po vlastitom nahođenju replikacije.



## <a name="step-1-choose-your-protection-goals"></a>Korak 1: Odaberite Zaštita ciljeve

Odaberite što želite replicirati i mjesto na koje želite replicirati na.

1. U plohu **sefovi oporavak servisi** odaberite vaše zbirke ključeva, a zatim **Postavke**.
2. **Početak rada** kliknite **Oporavak web-mjesta** > **Korak 1: Priprema infrastrukture** > **zaštitu cilj**.

    ![Odaberite ciljeva](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. U **cilju zaštite** odaberite **Za Azure**pa odaberite **Da, pomoću značajke Hyper-V**. Odaberite **da** da biste potvrdili VMM koristite da biste upravljali Hyper-V domaćini i vraćanje web-mjesta. Zatim kliknite **u redu**.

    ![Odaberite ciljeva](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Korak 2: Postavljanje okruženja izvora

Instalirajte davatelja za oporavak Azure web-mjesta na poslužitelju VMM i registraciju poslužitelja u na zbirke ključeva. Instalirajte agent servisa Azure oporavak na Hyper-V domaćini.

1. Kliknite **Korak 2: Priprema infrastrukture** > **izvora**.

    ![Postavljanje izvora](./media/site-recovery-vmm-to-azure/set-source1.png)

2. **Priprema izvorne** kliknite **+ VMM** da biste dodali VMM poslužitelja.

    ![Postavljanje izvora](./media/site-recovery-vmm-to-azure/set-source2.png)

3. U provjeri plohu **Dodavanje poslužitelja** koji će se prikazati **VMM centar sustava poslužitelja** u **vrsti poslužitelj** i koje poslužitelj VMM zadovoljava na [preduvjete i preduvjete URL-a](#on-premises-prerequisites).
4. Preuzmite instalacijsku datoteku davatelja za oporavak Azure web-mjesta.
5. Preuzmite ključa za registraciju. Morate to pri pokretanju instalacijskog programa. Ključno je valjan pet dana nakon što ga stvoriti.

    ![Postavljanje izvora](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Instalirajte davatelja za oporavak Azure web-mjesta na poslužitelju VMM.


### <a name="set-up-the-azure-site-recovery-provider"></a>Postavljanje davatelja za oporavak Azure web-mjesta

1.  Davatelj pokrenite datoteku za pokretanje.
2. U **Programu Microsoft Update** možete odabrati u ažuriranja tako da se instalacije ažuriranja davatelja skladu Pravilnik za Microsoft Update.
3. U **instalaciji**Prihvati ili izmjena zadano mjesto za instalaciju davatelja usluge, a zatim kliknite **Instaliraj**.

    ![Mjesto instalacije](./media/site-recovery-vmm-to-azure/provider2.png)

4. Kada se dovrši instalacija kliknite **registrirati** da biste registrirali VMM poslužitelja u zbirke ključeva.
5. Na stranici **Postavki zbirke ključeva** , kliknite **Pregledaj** da biste odabrali datoteku ključa sigurnog. Navedite pretplate oporavak Azure web-mjesta, a zatim naziv zbirke ključeva.

    ![Registracija poslužitelja](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. U **Internetske veze**, navedite kako davatelja usluge na poslužitelju VMM će se povezati s oporavak web-mjesta s Interneta.

    - Ako želite da se davatelj usluga za povezivanje izravno odaberite **Poveži izravno u Azure oporavak web-mjesta bez proxy poslužitelj**.
    - Ako vaše postojeće proxy poslužitelj zahtijeva provjeru autentičnosti ili koji želite koristiti prilagođenu proxy odaberite **Poveži se s oporavak Azure web-mjesta pomoću proxy poslužitelj**.
    - Ako koristite prilagođeni proxy poslužitelj, navedite adresu, priključak i vjerodajnice.
    - Ako koristite proxy poslužitelj treba ste već dopustili URL-ovi opisane u [preduvjeti](#on-premises-prerequisites).
    - Ako koristite prilagođeni proxy VMM RunAs računa (DRAProxyAccount) stvorit će se automatski pomoću vjerodajnica za navedeni proxy poslužitelj. Proxy poslužitelj konfigurirati tako da se ovaj račun možete provjeriti autentičnost uspješno. Postavke računa VMM RunAs moguće je izmijeniti na konzoli VMM. U odjeljku **Postavke**proširite **Sigurnost** > **Pokrenuti kao računi**, a zatim promijenite lozinku za DRAProxyAccount. Morat ćete ponovno pokrenite servis VMM tako da se ova postavka stupa na snagu.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Prihvatite ili izmjena mjesta SSL certifikat koji za šifriranje podataka automatski generira. Taj certifikat koristi se Ako omogućite šifriranje podataka za oblak zaštićen Azure na portalu za oporavak Azure web-mjesta. Zadrži sigurni ovog certifikata. Kada pokrenete u prebacivanje u Azure morat ćete ga dešifrirati, ako je omogućeno šifriranje podataka.


8. U odjeljak **naziv poslužitelja**navedite neslužbeni naziv za prepoznavanje VMM server u zbirke ključeva. U konfiguraciji klaster, navedite naziv uloge VMM klaster.
9. Omogućivanje **sinkronizacije oblaka metapodataka** ako želite sinkronizirati metapodataka za sve oblaka na poslužitelju VMM s na sigurnog. Ova akcija samo se mora dogoditi kada na svakom poslužitelju. Ako ne želite da biste sinkronizirali sve oblaka, ostavite tu postavku poništen, a sinkronizirati svaki oblaka pojedinačno u oblak svojstva konzole za VMM. Kliknite **Registracija** da biste dovršili postupak.

    ![Registracija poslužitelja](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Registracija pokreće. Po završetku Registracija poslužitelja prikazuje se na stranici **Postavke** > plohu**poslužitelji** u na zbirke ključeva.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Instalacija naredbenog retka za davatelja za oporavak Azure web-mjesta

Oporavak davatelja za Azure web-mjesta mogu instalirati iz naredbenog retka. Ova metoda se može koristiti da biste instalirali davatelja usluge na poslužitelju Core za Windows Server 2012 R2.

1. Preuzmite tipku davatelja instalacije datoteke i registraciju u mapu. Na primjer, C:\ASR.
2. S dodatnim naredbeni redak, pokrenite te naredbe da biste izdvojili instalacijski program za davatelja usluga:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Pokrenite sljedeću naredbu da biste instalirali komponente:

            C:\ASR> setupdr.exe /i

4. Izvedite te naredbe da biste registrirali poslužitelja u sigurnog:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Pri čemu je:

- **/Credentials**: obavezna parametar koji određuje gdje se nalazi datoteka s ključem Registracija.  
- **/FriendlyName**: obavezna parametar za naziv poslužitelja Hyper-V glavnog računala koja se pojavljuje na portalu za oporavak Azure web-mjesta.
- - **/EncryptionEnabled**: neobavezan parametar kada ste replikaciju Hyper-V VMs u VMM oblaka za Azure. Navedite želite li šifriranje virtualnim strojevima u Azure (pri šifriranje rest). Provjerite imaju li naziv datoteke s nastavkom **.pfx** . Šifriranje je po zadanom isključena.
- **/proxyAddress**: neobavezan parametar koji određuje adresu proxy poslužitelja.
- **/proxyport**: neobavezan parametar koji određuje priključak proxy poslužitelj.
- **/proxyUsername**: neobavezan parametar koji određuje proxy korisničko ime (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).
- **/proxyPassword**: neobavezan parametar koji određuje lozinku da biste je izvršila provjera autentičnosti proxy poslužitelja (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Kliknite pločicu agent servisa Azure oporavak domaćini Hyper-V

1. Nakon što ste postavili davatelja usluge, morate preuzeti instalacijsku datoteku za agent servisa Azure oporavak. Pokrenite instalacijski program na svakom Hyper-V poslužitelju u oblaku VMM.

    ![Web-mjesta Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Na stranici **Provjera preduvjeti** , kliknite **Dalje**. Bilo koji nedostaju preduvjeti instalirat će se automatski.

    ![Agenta servisa za oporavak preduvjeti](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Na stranici **Postavke instalacije** prihvatite ili promijenite mjesto instalacije i predmemoriju. Možete konfigurirati predmemoriju na pogon koji ima najmanje 5 GB prostora za pohranu dostupna, no preporučujemo da pogon predmemorije s 600 GB slobodnog prostora. Zatim kliknite **Instaliraj**.
4. Nakon dovršetka instalacije, kliknite **Zatvori** da biste završili.

    ![Registrirajte se OŽU Agent](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Instalacija naredbenog retka za agenta servisa za oporavak Azure web-mjesta

Agenta za servise sustava Microsoft Azure oporavak možete instalirati iz naredbenog retka pomoću sljedeće naredbe:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Postavljanje pristup Internetu proxy poslužitelja za oporavak web-mjesta iz domaćini Hyper-V

Agent za oporavak Services pokrenut na domaćini Hyper-V potreban je pristup Internetu za Azure za replikaciju VM. Ako ste pristup Internetu putem proxy poslužitelj, postaviti na sljedeći način:

1. Otvorite Microsoft Azure sigurnosne kopije BLOG dodatak na glavnom računalu Hyper-V. Prema zadanim postavkama prečac za Microsoft Azure Backup dostupna na radnoj površini ili u c:\Programske datoteke\Microsoft Azure oporavak usluge Agent\bin\wabadmin.
2. U dodatak kliknite **Promijeni svojstva**.
3. Na kartici **Konfiguracija proxy poslužitelja** navedite informacija proxy poslužitelja.

    ![Registrirajte se OŽU Agent](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Provjerite je li agenta može pristupiti URL-ovi opisane u [preduvjeti](#on-premises-prerequisites).


## <a name="step-3-set-up-the-target-environment"></a>Korak 3: Postavljanje okruženja cilj

Navedite račun za Azure prostora za pohranu će se koristiti za replikaciju i Azure mrežu na kojoj Azure VMs će se povezati nakon prebacivanje.

1.  Kliknite **Priprema infrastrukture** > **Ciljna** i odaberite Azure pretplatu koju želite koristiti.
2.  Određivanje implementacije model koji želite koristiti za VMs nakon prebacivanje.
3.  Oporavak web-mjesta provjere imate li jednu ili više računa kompatibilne Azure prostora za pohranu i mreže.

    ![Prostor za pohranu](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Ako niste stvorili račun za pohranu i želite da biste je stvorili pomoću upravitelja resursa, kliknite **+ račun za pohranu** potrebno tom retku.  Na **račun za pohranu za stvaranje** plohu Navedite naziv računa, vrsta, pretplate i mjesto. Račun mora biti na istom mjestu kao sigurnog servise za oporavak.

    ![Prostor za pohranu](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Imajte na umu da:

    - Ako želite stvoriti račun za pohranu pomoću klasične modela, to učiniti na portalu za Azure. [uči više](../storage/storage-create-storage-account-classic-portal.md)
    - Ako koristite račun za pohranu premium repliciranu podataka, morate postaviti račun dodatnog prostora za pohranu standardne za spremanje zapisnika replikacije koji obuhvaćaju daljnje promjene lokalnih podataka.

4.  Ako niste stvorili Azure mreže i želite da biste je stvorili pomoću upravitelja resursa kliknite **+ mreže** da biste učinili tom retku. Na **Stvaranje virtualne mreže** plohu Navedite naziv mreže, raspon adresu, podmreže detalje, pretplate i mjesto. Mreža mora biti na istom mjestu kao sigurnog servise za oporavak.

    ![Mreža](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Ako želite stvoriti mreže pomoću klasične modela to ćete učiniti na portalu za Azure. [Dodatne informacije](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfiguriranje mapiranja mreže

- [Čitanje](#prepare-for-network-mapping) Kratak pregled koje preslikavanje ne. [Pročitajte ovaj](site-recovery-network-mapping.md) za detaljnije objašnjenje.
- Provjerite je li virtualnim strojevima na poslužitelju VMM povezani s mrežom VM i da ste stvorili barem jedan Azure virtualne mreže. Više mreža VM može mapirati jedne mreže Azure.

Konfiguriranje mapiranja na sljedeći način:

1. U **postavkama** > **Infrastruktura za oporavak web-mjesta** > **mreže mapiranja** > **Mreže mapiranja**, kliknite ikonu **+ mapiranja mreže** .

    ![Preslikavanje](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Na **Dodaj preslikavanje** odaberite izvorni VMM poslužitelj i **Azure** kao cilj.
3. Provjerite je li pretplate i model implementacije nakon prebacivanje.
4. U odjeljku **mreža izvor**odaberite mrežni VM lokalnog izvora koje želite mapirati na popisu koji je povezan s poslužiteljem VMM.
5. U odjeljku **mreža ciljne**odaberite Azure mreže koju replike Azure VMs nalazit će se kada su ste stvorili. Zatim kliknite **u redu**.

    ![Preslikavanje](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Evo što se događa kada započne preslikavanje:

- Postojeći VMs na mreži VM izvor povezani s mrežom cilj kada započne mapiranje. Novi VMs povezani s mrežom VM izvora su povezani s mrežom mapiranih Azure kada dođe do replikacije.
- Ako mijenjate postojeće preslikavanje, replike virtualnim strojevima će se povezati pomoću nove postavke.
- Ako mreža cilj ima više podmreže i jednu od tih podmreže ima isti naziv kao podmreže na kojem se nalazi virtualnog računala izvora, zatim virtualnog računala replike će se povezati s tom podmreže ciljne nakon prebacivanje.
- Ako nema cilj podmreže pod nazivom koji se podudaraju, virtualnog računala će se povezati s prvom podmreže u mreži.



## <a name="step-4-set-up-replication-settings"></a>Korak 4: Postavljanje postavki replikacije


1. Da biste stvorili novog pravilnika replikacije, kliknite **Priprema infrastrukture** > **Replikacije postavke** > **+ Stvori i povezati**.

    ![Mreža](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. **Stvaranje i pridruži pravila**, navedite naziv pravila.
3. U **kopirajte učestalost**, odredite koliko često replicirati delta podataka nakon početnog replikacije (svakih 30 sekundi, 5 ili 15 minuta).
4. U **oporavak pokažite zadržavanja**, navedite u satima koliko prozoru zadržavanja će biti za svaku točku za oporavak. Zaštićeni strojeva se može oporaviti postavite unutar prozora.
6. U **aplikaciju dosljedan učestalost snimke**, unesite koliko se često (1 do 12 sati) oporavak točke koja sadrži aplikaciju dosljedan snimke će biti stvoren. Hyper-V koristi dvije vrste snimaka – standardni snimku zaslona koja nudi rastuće snimku cijelu virtualnog računala i aplikaciju dosljedan snimku zaslona koja stvara točke u vrijeme snimku podataka aplikacije unutar virtualnog računala. Aplikacija dosljedan snimke pomoću servisa Kopiraj glasnoću sjena (VSS) aplikacije provjerite jesu li u dosljedan stanju kada se koristi se snimka. Imajte na umu da Ako omogućite aplikacije dosljedan snimke, jer će utjecati na performanse aplikacije koje rade na virtualnim strojevima izvora. Provjerite je li vrijednost postavljate manji broj točaka dodatne oporavak konfigurirate.
3. U **vrijeme početka replikacije početni**pokazuju kada želite pokrenuti početne replikacije. Na replikacije pojavljuje se iznad internetska propusnost stoga želite zakazati izvan vaše zauzet sati.
5. **Šifriraj podatke pohranjene na Azure**navedite želite li da biste šifrirali na ostale podatke u Azure prostora za pohranu. Zatim kliknite **u redu**.

    ![Pravila replikacije](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Prilikom stvaranja novog pravila ga automatski povezao s VMM oblaka. Kliknite **u redu**. Možete pridružiti dodatne VMM oblaka (i VMs u njima) ovo pravilo za replikaciju u **postavkama** > **replikacije** > naziv pravila > **Pridružiti VMM oblaka**.

    ![Pravila replikacije](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Korak 5: Planiranje kapaciteta

Sad kad ste na basic infrastrukture postavljanje koje možete razmislite o planiranje kapaciteta i utvrđivanje trebate li dodatne resurse.

Oporavak web-mjesta omogućuje kapaciteta planer za dodjelu prave resurse za vaše okruženje izvora, komponente oporavak web-mjesta, mreže i prostora za pohranu. Planer možete pokrenuti u brzom načinu rada za Procjena Prosječan broj VMs, diskova i prostora za pohranu na temelju ili u detaljnom načinu kojim ćete slika na razini radno opterećenje za unos. Prije nego što počnete morat ćete:

- Prikupite informacije o okruženju sustava replikacije, uključujući VMs diskova po VMs i prostor za pohranu po disk.
- Procjenu dnevnih stopa promjena (churn) morat ćete repliciranu podataka. [Planer kapaciteta za Hyper-V replike](https://www.microsoft.com/download/details.aspx?id=39057) možete koristiti da biste mogli učiniti ovo.

1.  Kliknite **Preuzimanje** Preuzmite alat, a zatim ga pokrenuti. [U članku čitanje](site-recovery-capacity-planner.md) koja se isporučuje se uz alat.
2.  Kada završite odaberite **da** **ste pokrenuli planer kapaciteta**?

    ![Planiranje kapaciteta](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Razmatranja propusnosti mreže

Možete koristiti alat planer kapaciteta za izračun propusnosti vam je potrebna za replikaciju (Početna replikacije i zatim delta). Omogućuje kontrolu količine korištenja propusnosti za replikaciju imate nekoliko mogućnosti:

- **Throttle propusnosti**: Hyper-V prometa koji umnaža sekundarnog web-mjesta prolazi kroz određene značajke Hyper-V glavno računalo. Možete throttle propusnosti na poslužitelju glavnog računala.
- **Dotjerati propusnosti**: možete utjecati propusnost za replikaciju pomoću nekoliko ključevima registra.

#### <a name="throttle-bandwidth"></a>Ograničenja propusnosti

1. Otvorite Microsoft Azure sigurnosne kopije BLOG dodatak na poslužitelju Hyper-V glavnog računala. Prema zadanim postavkama prečac za Microsoft Azure Backup dostupna na radnoj površini ili u c:\Programske datoteke\Microsoft Azure oporavak usluge Agent\bin\wabadmin.
2. U dodatak kliknite **Promijeni svojstva**.
3. Na kartici **Throttling** odaberite **Omogućivanje korištenja propusnosti internetske ograničavanje za sigurnosne kopije operacije**, postavite ograničenja za posao i koje nisu posla sati. Valjani raspon ćete od 512 kb/s 102 MB/s sekundi.

    ![Ograničenja propusnosti](./media/site-recovery-vmm-to-azure/throttle2.png)

[Postavljanje OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet možete koristiti i da biste postavili ograničavanje. Slijedi primjer:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Postavljanje OBMachineSetting NoThrottle** označava da nema ograničavanje potreban je.


#### <a name="influence-network-bandwidth"></a>Utjecati propusnost mreže

Vrijednosti u registru **UploadThreadsPerVM** određuje broj niti koji se koriste za prijenos podataka (početne ili delta replikacije) na disku. Veću vrijednost povećava propusnost mreže koji se koristi za replikaciju. Vrijednosti u registru **DownloadThreadsPerVM** određuje broj niti koje se koristi za prijenos podataka tijekom failback.

1. U registru pronađite **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - Promijenite vrijednost **UploadThreadsPerVM** (ili, ako ne postoji, stvorite tipku) da biste niti kontrole koje se koriste za replikaciju disk.
    - Promijenite vrijednost **DownloadThreadsPerVM** (ili, ako ne postoji, stvorite tipku) da biste niti kontrole koje se koriste za failback promet s Azure.
2. Zadana vrijednost je 4. U odjeljku "overprovisioned" mreža tih ključeva registra treba promijeniti iz zadane vrijednosti. Maksimalna je 32. Praćenje promet za optimiziranje vrijednost.

## <a name="step-6-enable-replication"></a>Korak 6: Omogući replikacije

Sada omogućiti replikaciju na sljedeći način:

1. Kliknite **Korak 2: replicirati aplikacije** > **izvora**. Kada omogućite replikacije prvi put kliknete **+ replicirati** u sigurnog da biste omogućili replikacije za dodatnih računala.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. U plohu **izvora** > odaberite poslužitelj VMM poslužitelja u oblak i u kojem se nalaze domaćini Hyper-V. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. U **ciljnoj** odaberite pretplatu, model implementacije objavu prebacivanje i račun za pohranu koji koristite za repliciranu podatke.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Odaberite račun za pohranu koji želite koristiti. Ako želite da se pomoću računa za pohranu za različite od onih koje imate koje možete [stvoriti](#set-up-an-azure-storage-account). Da biste stvorili u prostor za pohranu račun pomoću modela resursima kliknite **Stvori novi**. Ako želite stvoriti račun za pohranu pomoću klasične modela učinite te [na portalu za Azure](../storage/storage-create-storage-account-classic-portal.md). Zatim kliknite **u redu**.
5. Odaberite Azure mreža i podmreže na koji će Azure VMs povezivanje kada ih se spun nakon prebacivanje. Odaberite **Konfiguriraj sada za odabrani strojeva** Primjena postavki mreže na svim računalima odaberete za zaštitu. Odaberite **Konfiguriraj kasnije** da biste odabrali Azure mreže svako računalo. Ako želite koristiti drugog mrežnog od onih imate koje možete [stvoriti](#set-up-an-azure-network). Da biste stvorili koristeći model upravljanja resursima kliknite **Stvori novi**. Ako želite stvoriti mreže pomoću klasične modela ćete učiniti te [na portalu za Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Ako je primjenjivo, odaberite podmreži. Zatim kliknite **u redu**.
6. Na **virtualnim računalima sustava** > **Odaberite virtualnim strojevima** kliknite i odaberite svakom računalu koje želite replicirati. Možete odabrati samo računala za koje je moguće omogućiti replikacije. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. U **svojstvima** > **Konfiguriranje svojstava**odaberite operacijski sustav za odabrani VMs i OS disk. Zatim kliknite **u redu**. Naknadno možete postaviti dodatna svojstva.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. U odjeljku **Postavke replikacije** > **Konfiguracija postavki replikacije**, odaberite replikacije pravilo koje želite primijeniti za zaštićeni VMs. Zatim kliknite **u redu**. Možete izmijeniti pravilo replikacije u **postavkama** > **replikacije pravila** > naziv pravila > **Uređivanje postavki**. Promjene primijenite koriste se za računala koja se već replikaciju i novi strojeva.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Možete pratiti napredak posao **Omogućite zaštite** u **postavkama** > **Poslovi** > **Poslovi oporavak web-mjesta**. Nakon što je posao **Dovršavanje zaštitu** pokreće na računalu spremna je za prebacivanje.

### <a name="view-and-manage-vm-properties"></a>Prikaz i upravljanje svojstvima VM

Preporučujemo da provjerite svojstva izvora računala. Imajte na umu da Azure VM naziv mora biti u skladu s [zahtjevima za Azure virtualnog računala](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Kliknite **Postavke** > **Zaštićene stavke** > **Replicirati stavke** >, a zatim odaberite računalo da biste vidjeli pojedinosti.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. U **svojstvima** možete pogledati informacija replikacije i prebacivanje na VM.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. U **računalnim i mrežne** > **Svojstva za izračun** možete odrediti Azure VM veličinu ime i cilj. Promijenite naziv u skladu s [zahtjevima za Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) po potrebi. Pogledajte i izmijenili informacije o mreži ciljne, podmreže i IP adresu koja vam je dodijeljen Azure VM. Imajte na umu sljedeće:

    - Možete postaviti IP adresu cilja. Ako ne unesete adresu nije uspio putem računala koristit će DHCP. Ako postavite adresu koja nije dostupna na prebacivanje na prebacivanje neće uspjeti. Iste ciljni IP adresa može se koristiti za testiranje prebacivanje ako je dostupan u mreži prebacivanje test adresu.
    - Broj mrežnih prilagodnika diktirali je veličinom koju navedete za virtualnog računala cilj na sljedeći način:

        - Ako je broj mrežnih prilagodnika na računalu izvor manje od ili jednak broju prilagodnika dopušteno Odredišna veličina računalu, zatim cilj će imati isti broj prilagodnika kao izvor.
        - Ako je broj prilagodnika za virtualnog računala izvora premašuje broj dopušteno Odredišna veličina, a zatim se koristi Maksimalna veličina cilj.
        - Ako, primjerice stroj izvora sadrži dvije mrežnih prilagodnika i odredišna veličina računalo podržava četiri, ciljnom računalu će imati dva prilagodnika. Ako na računalu izvor ima dva prilagodnika, ali veličinu podržani cilj podržava samo jedan ciljnom računalu neće imati samo jedan prilagodnika.     
        - Ako na VM ima više mrežnih prilagodnika oni će sve se povezati s istom mrežom.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  U **diskova** možete vidjeti operacijski sustav i diskova podataka na VM koji je replicirati.



## <a name="step-7-test-your-deployment"></a>Korak 7: Testiranje implementaciju sustava

Da biste testirali implementacijskih možete pokrenuti test prebacivanje za jednu virtualnog računala ili oporavak tarifu koja sadrži jedan ili više virtualnim računalima.


### <a name="prepare-for-failover"></a>Priprema za prebacivanje

- Da biste pokrenuli test prebacivanje preporučujemo stvorite novu Azure mrežu koja sadrži Izolirani u mreži Azure proizvodnje (to je zadano ponašanje prilikom stvaranja nove mreže u Azure). [Dodatne informacije](site-recovery-failover.md#run-a-test-failover) o pokretanju test failovers.
- Da biste ostvarili najbolje performanse kada ne preko Azure, instalirajte Agent za Azure na zaštićenom računalu. Omogućuje pokretanje brže i pomaže s otklanjanjem poteškoća. Instalirajte agent za [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) ili [Linux](https://github.com/Azure/WALinuxAgent) .
- U potpunosti ispitati implementaciju sustava potrebno je infrastrukture za repliciranu strojno funkcionirati prema očekivanjima. Ako želite testirati servisa Active Directory i DNS možete stvoriti virtualnog računala kao kontroler domene s DNS-a i replicirati to Azure pomoću oporavak Azure web-mjesta. Pročitajte više u [test prebacivanje zahtjevi za Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Ako želite pokrenuti neplanirano prebacivanje umjesto test prebacivanje Imajte na umu sljedeće:

    - Ako je to moguće isključite primarni strojeva prije pokretanja neplanirano prebacivanje. Na taj način nemate izvora i replike računala izvode u isto vrijeme.
    - Kada pokrenete neplanirano prebacivanje prestane replikacije podataka iz primarnog strojeva tako da sve delta podataka neće prenijeti nakon neplanirano prebacivanje započinje. Nadalje ako pokrenete neplanirano prebacivanje na tarifu za oporavak funkcionirat će dok se ne dovrši, čak i ako dođe do pogreške.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Priprema za povezivanje s Azure VMs nakon prebacivanje

Ako se želite povezati VMs Azure pomoću RDP nakon prebacivanje, obavezno učinite sljedeće:

**Na računalu na lokalni prije prebacivanja u slučaju pogreške**:

- Za pristup putem Interneta omogućiti RDP, provjerite je li TCP i UDP pravila dodaju za na **javno**i provjerite je li RDP dopušten u **Vatrozid za Windows** -> **dopuštene aplikacije i značajke** za sve profile.
- Za pristup putem web-mjesto veze Omogućivanje RDP na računalu i provjerite je li RDP dopušten u **Vatrozid za Windows** -> **dopuštene aplikacije i značajke** za **domenu** i **privatne** mreže.
- Instalirajte [agent za Azure VM](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) na lokalno računalo.
- Provjerite da je operacijski sustav SAN pravilo postavljeno na OnlineAll. [uči više]( https://support.microsoft.com/kb/3031135)
- Isključite servis IPSec prije nego što pokrenete u prebacivanje.

**Na stranici Azure VM nakon prebacivanje**:

- Dodajte javno krajnja točka za RDP protocol (priključak 3389), a zatim navedite vjerodajnice za prijavu.
- Provjerite je li još nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.
- Pokušajte se povezati. Ako se ne možete povezati, provjerite je li u VM traje. Savjete za otklanjanje poteškoća u ovom [članku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Ako želite da biste pristupili programa Azure VM izvodi Linux nakon prebacivanje pomoću ljuske za sigurnu klijenta (ssh), učinite sljedeće:

**Na računalu na lokalni prije prebacivanja u slučaju pogreške**:

- Provjerite je li servis za sigurnu ljuske Azure VM postavljen na automatsko pokretanje na pokretanje sustava.
- Provjerite da pravila vatrozida omogućivanje programa SSH veze na njega.

**Na stranici Azure VM nakon prebacivanje**:

- Na mreži sigurnosne grupe pravila na nije uspio nad VM i Azure podmreže s kojim je povezan morati Dopusti dolazne veze s priključkom SSH.
- Javni krajnjoj točki treba kreirati da dopušta dolazne veze na priključak SSH (TCP priključak 22 po zadanom).
- Ako na VM se pristupa putem veze za VPN-a (usmjeravanje Express ili web-mjesta za web-mjesta VPN) klijent može koristiti povezati izravno na VM putem SSH.


### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

Da biste pokrenuli test Nemoj prebacivanje na sljedeći način:

1. Neuspjeh putem jednog VM u **postavkama** > **Replicirati stavki**, kliknite na VM > **+ Prebacivanje Test**.
2. Neuspjeh putem tarifu za oporavak, u odjeljku **Postavke** > **Oporavak tarife**, desnom tipkom miša kliknite željenu tarifu > **Prebacivanje Test**. Da biste stvorili na oporavak planiranje [, slijedite ove upute](site-recovery-create-recovery-plans.md).

3. **Prebacivanje Test** odaberite Azure mreže kojoj Azure VMs povezivanje nakon što se događa prebacivanje.
4. Kliknite **u redu** da biste započeli s pomoćnim. Možete pratiti napredak tako da kliknete na VM da biste otvorili svojstva ili na poslu **Test prebacivanje** u **postavkama** > **poslove oporavak web-mjesta**.
5. Kada u prebacivanje dosegne fazu **testiranje dovrši** , učinite sljedeće:

    1. Prikaz virtualnog računala replike Azure portalu. Provjerite je li uspješno pokrene virtualnog računala.
    2. Ako ste postavljeno na virtualnim strojevima pristup iz lokalne mreže možete pokrenuti udaljenu radnu površinu vezu virtualnog računala.
    3. Kliknite **dovrši test** da ga završite.
    4. Kliknite **bilješke** da biste snimili i spremili sve opažanja povezan s pomoćnim test.
    5. Kliknite **test prebacivanje je dovršena**. Čišćenje okruženje za testiranje automatski isključujte i brisanje test virtualnog računala.
    6. U ovoj fazi elemente ni VMs automatski stvorio oporavak web-mjesta tijekom prebacivanje test se briše. Sve dodatne elemente koji ste stvorili za testiranje prebacivanje neće se izbrisati.

    > [AZURE.NOTE] Ako testiranje prebacivanje više od dva tjedna nastavlja se dovrši po prisilno.

6. Nakon dovršetka na prebacivanje i trebali da biste vidjeli replike Azure računalu prikazuju se na portalu za Azure > **virtualnih računala**. Provjerite je li u VM na odgovarajuću veličinu, koji ima povezani s odgovarajućim mrežom i je li pokrenut.
7. Ako ste [spremni za veze nakon prebacivanje](#prepare-to-connect-to-Azure-VMs-after-failover) trebali povezati Azure VM.


## <a name="monitor-your-deployment"></a>Praćenje implementaciju sustava

Evo kako možete pratiti konfiguracijske postavke, status i stanja za implementaciju sustava oporavak web-mjesta:

1. Kliknite naziv sigurnog pristupa na nadzornoj ploči **Essentials** . U ovoj nadzornoj ploči možete zadacima, replikacije status, tarife za oporavak, dijagnostiku poslužitelja i događaje oporavak web-mjesta.  Možete prilagoditi Essentials da bi se prikazala pločice i rasporedima koje su najkorisniji za vas, uključujući stanje druge sefovi oporavak web-mjesta i sigurnosno kopiranje.

    ![Osnove](./media/site-recovery-vmm-to-azure/essentials.png)

2. Na pločici **stanja** možete pratiti web-mjesta (VMM ili konfiguracija) poslužitelji koji se pojavio problem i događaje upućuju oporavak web-mjesta u zadnja 24 sata.
3. Možete upravljati i praćenje replikacije u **Replicirati stavki**, **Oporavak tarife**, i pločice **Poslovi oporavak web-mjesta** . Možete dubinski zadacima u **postavkama** -> **poslove** -> **Poslove oporavak web-mjesta**.


## <a name="next-steps"></a>Daljnji koraci

Nakon uvođenja postavljen i radi, [Dodatne informacije](site-recovery-failover.md) o različitim vrstama prebacivanje.
