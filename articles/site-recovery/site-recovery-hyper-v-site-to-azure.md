<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima (bez VMM) za Azure oporavak web-mjesta Azure pomoću portala za Azure | Microsoft Azure"
    description="U članku se opisuje kako implementirati Azure oporavak web-mjesta da biste orkestrirali replikacije, prebacivanje i oporavak lokalnog Hyper-V VMs koji ne upravlja VMM za Azure pomoću portala za Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replicirati Hyper-V virtualnim strojevima (bez VMM) za Azure oporavak web-mjesta Azure pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-hyper-v-site-to-azure.md)
- [Klasični Azure](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell – Voditelj resursa](site-recovery-deploy-with-powershell-resource-manager.md)



Dobro došli u oporavak Azure web-mjesta! Koristite ovaj članak ako želite replicirati na lokalni Hyper-V virtualne machines te **ne** upravlja iz oblaka sustava centra virtualnim strojevima Manager (VMM) za Azure. U ovom se članku opisuje kako postaviti replikacije pomoću oporavak Azure web-mjesta na portalu za Azure.

> [AZURE.NOTE] Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije.

> Oporavak Azure web-mjesta podržava oporavak i migracije Hyper-V virtualnim strojevima za Azure. Koraci u ovom članku primjenjuju se jednak način tijekom konfiguriranja replikacije za Azure za oporavak Izrada ili migracije VMs za Azure

Azure oporavak web-mjesta na portalu za Azure nudi brojne nove značajke:

- U na Azure portal, sigurnosno kopiranje Azure i oporavak Azure web-mjesta servisa kombiniraju se u jednu oporavak servisa sigurnog tako da možete postaviti i upravljanje poslovanje i Izrada oporavak (BCDR) s jednog mjesta. Nadzorna ploča za Sjedinjeno komuniciranje omogućuje nadzor i upravljanje operacije na web-mjesta na lokalni Azure javno poslužitelja u oblak i.
- Korisnici s Azure pretplate dodjeli programom oblaka rješenje davatelj (CSP) sada mogu upravljati operacije oporavak web-mjesta na portalu za Azure.
- Oporavak web-mjesta na portalu za Azure možete replicirati strojeva Voditelj resursa za pohranu računima. Oporavak web-mjesta na prebacivanje, stvara VMs utemeljen na Voditelj resursa u Azure.
- Oporavak web-mjesta i dalje podržava replikaciju računi klasični prostora za pohranu i prebacivanje VMs pomoću klasične implementacije modela.


U ovom se članku nakon čitanje objavite povratne informacije pri dnu u odjeljak Komentari Disqus. Zamolite tehnička pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Pregled


Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR će zadržali poslovnih podataka sigurnih i koje se mogu vratiti i radnih opterećenja provjerite jesu li neprestano dostupne kada se pojavi Izrada.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, možete se neće putem sekundarne mjesto da biste zadržali aplikacije i radnih opterećenja dostupna. Povratak na svom primarnom mjestu možete uspjeti kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)

Ovaj članak sadrži sve potrebne informacije za replikaciju lokalnog Hyper-V virtualne machines te **ne** upravlja iz oblaka sustava centra virtualnim strojevima Manager (VMM) za Azure. Uključuje se pregled arhitekture, Planiranje informacije i koraci za implementaciju za konfiguriranje lokalne poslužitelje, Azure, pravila za replikaciju i planiranje kapaciteta. Nakon što ste postavili Infrastruktura možete omogućiti replikaciju na koji želite zaštititi i izvršiti prebacivanje test za provjeru valjanosti za postavljanje međuverzije. Možete i migrirati vaše VMs Azure izvođenjem prvi planiranog prebacivanje, a zatim ispunite migracije.

## <a name="business-advantages"></a>Prednosti tvrtke

- Omogućuje prebacivanje službenoj (Azure) za radnih opterećenja tvrtke i aplikacije koje rade na virtualnim strojevima Hyper-V.
- Sadrži jedan konzole za oporavak servise za jednostavne postavljanje i upravljanje replikacije, prebacivanje i oporavak procesa.
- Omogućuje jednostavno pokrenuti failovers s infrastruktura za lokalni u Azure i nije uspjelo stražnje (Vraćanje) iz Azure lokalnog web-mjesto.
- Tarife za oporavak s više računala, možete konfigurirati tako da se tiered aplikacije radnih opterećenja neće uspjeti putem zajedno.

## <a name="scenario-architecture"></a>Scenarij arhitekture

Ovo su komponente scenarija:

- **Glavno računalo Hyper-V ili klaster**: lokalnog Hyper-V glavnog računala poslužitelja ili klastere. Hyper-V domaćini pokrenut VMs koji želite zaštititi su prikupljene u web-mjesta logičke Hyper-V tijekom implementacije oporavak web-mjesta.
- **Davatelj usluga za oporavak web-mjesta za Azure i agenta servisa za oporavak**: tijekom implementacije instalirate davatelja za oporavak Azure web-mjesta i agent servisa Microsoft Azure oporavak Services na poslužiteljima Hyper-V glavnog računala. Davatelj putem HTTP 443 za replikaciju djelovanje komunicira s oporavak Azure web-mjesta. Agent na poslužitelju za glavno računalo Hyper-V replicira podataka Azure za pohranu putem HTTP 443 prema zadanim postavkama.
- **Azure**: potrebno Azure pretplatu, račun Azure prostora za pohranu podataka iz trgovine replicirati i Azure virtualne mreže tako da se Azure VMs nakon prebacivanje povezani s mrežom.

![Arhitektura Hyper-V web-mjesta](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Preduvjeti za Azure

Evo što morate u Azure implementacija scenarij.

**Preduvjeta** | **Pojedinosti**
--- | ---
**Račun za Azure**| Potreban vam je račun sustava [Microsoft Azure](http://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Azure prostora za pohranu** | Potreban vam je račun za standardne prostora za pohranu. Možete koristiti LRS ili GRS računa za pohranu. Preporučujemo da GRS tako da se podaci prebacuju ako regionalne nestalo ili ako je primarni regija nije moguće oporaviti. [Dodatne informacije](../storage/storage-redundancy.md). Račun mora biti u području isti kao sigurnog servise za oporavak.<br/><br/> Prostor za pohranu Premium nije podržana.<br/><br/> Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs stvaraju kada dođe do prebacivanje.<br/><br/> [Doznajte](../storage/storage-introduction.md) Azure prostora za pohranu.
**Azure mreže** | Morat ćete Azure virtualne mreže koji Azure VMs će se povezati s kada dođe do prebacivanje. U području isti kao sigurnog oporavak Services mora biti Azure virtualne mreže.

## <a name="on-premises-prerequisites"></a>Lokalni preduvjeti

Evo što sve trebate lokalnog.

**Preduvjeta** | **Pojedinosti**
--- | ---
**Hyper-V** | Jedan ili više lokalne poslužitelje s programom **Windows Server 2012 R2** najnovija ažuriranja i Hyper-V uloge omogućeno ili **Microsoft Hyper-V Server 2012 R2**.<br/><br/>Hyper-V poslužitelja smiju sadržavati virtualnih računala.<br/><br/>Poslužitelji Hyper-V moraju biti povezani s Internetom izravno ili putem proxy poslužitelj.<br/><br/>Hyper-V poslužitelja mora imati rješenja koji se spominju u [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") instaliran.
**Davatelj usluga i agent** | Tijekom implementacije oporavak web-mjesta Azure ćete instalirati davatelja za oporavak Azure web-mjesta. Instalacija davatelja i instalirajte agenta servisa za oporavak Azure na svakom Hyper-V izvodi virtualnim strojevima koji želite zaštititi. Svi Hyper-V poslužitelji u zbirke ključeva za oporavak web-mjesta moraju imati iste verzije davatelja i agent.<br/><br/>Davatelj ćete se povezati oporavak Azure web-mjesta s Interneta. Promet se može poslati izravno ili putem proxy poslužitelj. Imajte na umu HTTPS temelji proxy nije podržano. Proxy poslužitelj dopustite pristup: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Ako imate IP utemeljen na adresi vatrozid pravila na poslužitelju, potvrdite da pravila dopustiti komunikaciju za Azure. Morat ćete dopustiti [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i priključak HTTPS (443).<br/><br/>Dopusti rasponi IP adresa za Azure regija pretplate i Zapad SAD-a.

## <a name="protected-machine-prerequisites"></a>Preduvjeti za zaštićeni računala


**Preduvjeta** | **Pojedinosti**
--- | ---
**Zaštićeni VMs** | Prije nego što se neće uspjeti putem programa VM morat ćete da biste bili sigurni da se naziv koji će se dodijeliti Azure VM usklađenost sa [Azure preduvjeti](site-recovery-best-practices.md#azure-virtual-machine-requirements). Kada omogućite replikacije za na VM možete izmijeniti naziv.<br/><br/> Kapacitet pojedinačne disk na zaštićenom bi trebalo biti više od 1023 GB. Na VM mogu imati do 16 diskova (dakle do 16 TB).<br/><br/> Zajedničko korištenje na disku goste klastere nisu podržane.<br/><br/> Ako je izvor VM NIC udruživanje nakon prebacivanje na Azure se pretvara jedan NIC.<br/><br/>Zaštita VMs Linux koristeći statičke IP adrese nisu podržane.

## <a name="prepare-for-deployment"></a>Priprema za implementaciju

Priprema za implementaciju morat ćete:

1. [Postavljanje Azure mreže](#set-up-an-azure-network) u kojem Azure VMs nalazit će se kada ih se stvorene nakon prebacivanje.
2. [Postavljanje računa Azure prostora za pohranu](#set-up-an-azure-storage-account) za repliciranu podataka.
3. [Priprema Hyper-V domaćini](#prepare-the-hyper-v-hosts) da biste bili sigurni mogu pristupiti URL-ove potreban.

### <a name="set-up-an-azure-network"></a>Postavite mrežu Azure

Postavite Azure mrežu. Potreban vam je to tako da se Azure VMs stvorene nakon prebacivanje su povezani s mrežom.

- U području isti kao onom u kojem ćete implementacija oporavak servisa sigurnog mora biti mreže.
- Ovisno o modelu resursa koji želite koristiti za nije uspjela tijekom Azure VMs, postavit ćete Azure mreže u [načinu Voditelj resursa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) ili [Klasični](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Preporučujemo da postavite mrežu prije nego što počnete. Ako to ne morat ćete učiniti tijekom implementacije oporavak web-mjesta.

> [AZURE.NOTE] [Migracija mreže](../resource-group-move-resources.md) preko grupe resursa unutar iste pretplate ili putem pretplate nije podržana za mreže koji se koristi za implementaciju oporavak web-mjesta.

### <a name="set-up-an-azure-storage-account"></a>Postavljanje računa za pohranu za Azure

- Potreban vam je račun standardne Azure prostora za pohranu na čuvanje podataka replicirati na Azure.
- Ovisno o resursa model koji želite koristiti za nije uspjela tijekom Azure VMs, a u [načinu Voditelj resursa](../storage/storage-create-storage-account.md) ili [Klasični](../storage/storage-create-storage-account-classic-portal.md)ćete postaviti račun.
- Preporučujemo da postavljanje računa za pohranu prije nego što počnete. Ako to ne morat ćete učiniti tijekom implementacije oporavak web-mjesta. Računi moraju biti u istom području kao sigurnog servise za oporavak.

> [AZURE.NOTE] [Migracije računa za pohranu](../resource-group-move-resources.md) po grupama resursa unutar iste pretplate ili uzduž pretplate nije podržana za račune za pohranu koji se koriste za implementaciju oporavak web-mjesta.

### <a name="prepare-the-hyper-v-hosts"></a>Priprema domaćini Hyper-V

- Provjerite je li domaćini Hyper-V pridržavate [preduvjeti](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Stvaranje oporavak servisa sigurnog

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Kliknite **Novi** > **upravljanja** > **sigurnosno kopiranje i vraćanje web-mjesta (OMS)**. Umjesto toga možete kliknuti **Pregledaj** > sefovi**Oporavak usluge** > **Dodaj**.

    ![Nove zbirke ključeva](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. U odjeljku **naziv** navedite neslužbeni naziv da biste odredili na zbirke ključeva. Ako imate više pretplata, odaberite jedan od njih.
4. [Stvori novu grupu resursa](../resource-group-template-deploy-portal.md) ili odaberite postojeći pa navedite Azure regiju. Strojeva će je replicirati na tom području. Da biste provjerili podržanih regija pogledajte geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Ako želite brzo pristupiti na sigurnog na nadzornoj ploči kliknite **Prikvači na nadzornu ploču** , a zatim kliknite **Stvori sigurnog**.

    ![Nove zbirke ključeva](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Nove zbirke ključeva pojavit će se na **nadzornoj ploči** > **sve resurse**i na glavnom plohu **sefovi servise za oporavak** .

## <a name="getting-started"></a>Početak rada

Oporavak web-mjesta sadrži prvi koraci sučelje koji olakšava brzo moguće uvesti. Početak rada provjerava preduvjeti web-mjesta i vodi vas kroz implementacije koraka u redoslijedu desnom oporavak web-mjesta.

U prvi koraci odaberite vrstu strojeva želite za replikaciju i mjesto na koje želite replicirati na. Postavljanje lokalne poslužitelje, račune Azure prostora za pohranu i mrežama. Možete stvoriti pravila za replikaciju i izvođenje planiranja kapaciteta. Nakon što ste postavili preduvjete infrastrukture Omogući replikacije za VMs. Pokrenite failovers za određene strojeva ili stvaranje tarife za oporavak uvoza na više računala.

Početak rada započeti tako da odaberete način na koji želite uvesti oporavak web-mjesta. Početak rada tijek promjene malo po vlastitom nahođenju replikacije.



## <a name="step-1-choose-your-protection-goals"></a>Korak 1: Odaberite Zaštita ciljeve

Odaberite što želite replicirati i mjesto na koje želite replicirati na.

1. U plohu **sefovi oporavak servisi** odaberite vaše zbirke ključeva, a zatim **Postavke**.
2. U odjeljku **Postavke** > **Početak rada** kliknite **Oporavak web-mjesta** > **Korak 1: Priprema infrastrukture** > **zaštitu cilj**.

    ![Odaberite ciljeva](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. U **cilju zaštite** odaberite **Za Azure**pa odaberite **Da, pomoću značajke Hyper-V**. Odaberite **bez** da biste potvrdili ne koristite VMM. Zatim kliknite **u redu**.

    ![Odaberite ciljeva](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Korak 2: Postavljanje okruženja izvora

Postavite Hyper-V web-mjesto, kliknite pločicu davatelja za oporavak Azure web-mjesta i servisa Azure oporavak agent Hyper-V domaćini i registrirati hosts u sigurnog.


1. Kliknite **Korak 2: Priprema infrastrukture** > **izvora**. Da biste dodali novo web-mjesto Hyper-V kao spremnik za Hyper-V domaćini ili klastere, kliknite **+ Hyper-V web-mjesta**.

    ![Postavljanje izvora](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. U **web-mjesta stvoriti Hyper-V** plohu Navedite naziv web-mjesta. Zatim kliknite **u redu**. Odaberite web-mjesta koji ste upravo stvorili.

    ![Postavljanje izvora](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Kliknite **+ Server Hyper-V tako** da biste dodali na poslužitelju web-mjesto.
4. **Dodavanje**Server > **Vrsta poslužitelja** provjerite prikazuje se **Hyper-V poslužitelja** . Provjerite je li poslužitelj Hyper-V koju želite dodati uspostavu [preduvjeti](#on-premises-prerequisites) i moći pristupiti određenih URL-ova.
4. Preuzmite instalacijsku datoteku davatelja za oporavak Azure web-mjesta. Ćete pokrenuti tu datoteku da biste instalirali davatelj i agent za oporavak usluge na svakom računalu koje hostira Hyper-V.
5. Preuzmite ključa za registraciju. Potreban vam je to pri pokretanju instalacijskog programa. Ključno je valjan 5 dana nakon ga stvoriti.

    ![Postavljanje izvora](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Pokrenite davatelj instalacijsku datoteku na svakom računalu koje hostira dodali Hyper-V web-mjesta. Ako ga instalirate na Hyper-V klaster, pokrenite instalacijski program na svakom čvor klaster. Instalacija i registracija svaki čvorove Hyper-V klaster osigurava virtualnim strojevima ostaju zaštićena čak i ako ih migrirati preko čvorove.

### <a name="install-the-provider-and-agent"></a>Instalacija davatelja i agent

1. Davatelj pokrenite datoteku za pokretanje.
2. U **Programu Microsoft Update** možete odabrati u ažuriranja tako da se instalacije ažuriranja davatelja skladu Pravilnik za Microsoft Update.
3. U **instalaciji** Prihvati ili izmjena zadano mjesto za instalaciju davatelja usluge, a zatim kliknite **Instaliraj**.
4. Na stranici **Postavki zbirke ključeva** , kliknite **Pregledaj** da biste odabrali datoteku sigurnog ključa koji ste preuzeli. Navedite pretplate oporavak Azure web-mjesta, naziv sigurnog i Hyper-V web-mjesta na kojem pripada Hyper-V poslužitelja.

    ![Registracija poslužitelja](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. u **Postavke proxyja** odredite kako davatelja koji će se instalirati na poslužitelju će se povezati s oporavak Azure web-mjesta s Interneta.

- Ako želite da se davatelj usluga za povezivanje izravno odaberite **Poveži izravno bez proxy poslužitelj**.
- Ako se želite povezati s proxy poslužitelja koja je trenutno postavljena na poslužitelju odaberite **Poveži s postojeće postavke proxyja**.
- Ako vaše postojeće proxy poslužitelj zahtijeva provjeru autentičnosti ili koji želite koristiti prilagođenu proxy poslužitelja za odabir veze davatelja **povezivanje za postavke proxyja prilagođene**.
- Ako koristite prilagođeni proxy morat ćete navesti adresu, priključak i vjerodajnice
- Ako koristite proxy stvaranjem sigurni što je opisano u [preduvjeti](#on-premises-prerequisites) URL-ovi dopušteno kroz nju.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. kada se instalacija završi kliknite **registrirati** da biste registrirali poslužitelja u zbirke ključeva.

![Mjesto instalacije](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. završetku Registracija metapodataka iz Hyper-V poslužitelja dohvaćenih oporavak Azure web-mjesta i prikazat će se na poslužitelju na stranici **Postavke** > **Infrastruktura za oporavak web-mjesta** > plohu**Hyper-V domaćini** .


### <a name="command-line-installation"></a>Instalacija naredbenog retka

Davatelj usluga za oporavak web-mjesta za Azure i agent može biti instaliran i pomoću sljedećih naredbenog retka. Ova metoda se može koristiti da biste instalirali davatelja usluge na na poslužitelju Core za Windows Server 2012 R2.

1. Preuzmite tipku davatelja instalacije datoteke i registraciju u mapu. Na primjer C:\ASR.
2. S dodatnim naredbeni redak, pokrenite te naredbe da biste izdvojili instalacijski program za davatelja usluga:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Pokrenite sljedeću naredbu da biste instalirali komponente:

            C:\ASR> setupdr.exe /i

4. Pokrenite te naredbe da biste registrirali poslužitelja u sigurnog: CD c:\Programske datoteke\Microsoft Azure web-mjesta oporavak Provider\
            C:\Programske datoteke\Microsoft Azure web-mjesta oporavak davatelja\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /vjerodajnice<path of the credentials file>

<br/>
Pri čemu je:

- **/Credentials** : obavezna parametar koji određuje mjesto u kojoj se nalazi datoteka s ključem Registracija  
- **/FriendlyName** : obavezna parametar za naziv poslužitelja Hyper-V glavnog računala koja se pojavljuje na portalu za oporavak Azure web-mjesta.
- **/proxyAddress** : neobavezan parametar koji određuje adresu proxy poslužitelja.
- **/proxyport** : neobavezan parametar koji određuje priključak proxy poslužitelj.
- **/proxyUsername** : neobavezan parametar koji određuje Proxy korisničko ime (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).
- **/proxyPassword** : neobavezan parametar koji određuje lozinku za provjeru autentičnosti s proxy poslužitelja (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).


## <a name="step-3-set-up-the-target-environment"></a>Korak 3: Postavljanje okruženja cilj

Navedite račun za Azure prostora za pohranu će se koristiti za replikaciju i Azure mrežu na kojoj Azure VMs će se povezati nakon prebacivanje.

1.  Kliknite **Priprema infrastrukture** > **Ciljna** i odaberite Azure pretplatu koju želite koristiti.
2.  Određivanje implementacije model koji želite koristiti za VMs nakon prebacivanje.
3.  Oporavak web-mjesta provjere imate li jednu ili više računa kompatibilne Azure prostora za pohranu i mreže.

    ![Prostor za pohranu](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Ako niste stvorili račun za pohranu i želite da biste je stvorili pomoću upravitelja resursa kliknite **+ prostor za pohranu** računa da biste učinili tom retku. Na **račun za pohranu za stvaranje** plohu Navedite naziv računa, vrsta, pretplate i mjesto. Račun mora biti na istom mjestu kao sigurnog servise za oporavak.

    ![Prostor za pohranu](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Ako želite stvoriti račun za pohranu pomoću klasične modela ćete učiniti te [na portalu za Azure](../storage/storage-create-storage-account-classic-portal.md).

5.  Ako niste stvorili Azure mreže i želite da biste je stvorili pomoću upravitelja resursa kliknite **+ mreže** da biste učinili tom retku. Na **Stvaranje virtualne mreže** plohu Navedite naziv mreže, raspon adresu, podmreže detalje, pretplate i mjesto. Mreža mora biti na istom mjestu kao sigurnog servise za oporavak.

    ![Mreža](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Ako želite stvoriti mreže pomoću klasične modela ćete učiniti te [na portalu za Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Korak 4: Postavljanje postavki replikacije

1. Da biste stvorili novi replikacije pravila kliknite **Priprema infrastrukture** > **Replikacije postavke** > **+ Stvori i povezati**.

    ![Mreža](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. **Stvaranje** i pridruži pravila unesite naziv pravila.
3. U **kopirajte učestalost** odredite koliko često replicirati delta podataka nakon početnog replikacije (svakih 30 sekundi, 5 ili 15 minuta).
4. U **oporavak pokažite zadržavanja**, navedite u satima koliko prozoru zadržavanja će biti za svaku točku za oporavak. Zaštićeni strojeva se može oporaviti postavite unutar prozora.
6. U **aplikaciju dosljedan snimke učestalost** unesite koliko se često (1 do 12 sati) oporavak točke koja sadrži aplikaciju dosljedan snimke će biti stvoren. Hyper-V koristi dvije vrste snimaka – standardni snimku zaslona koja nudi rastuće snimku cijelu virtualnog računala i aplikaciju dosljedan snimku zaslona koja stvara točke u vrijeme snimku podataka aplikacije unutar virtualnog računala. Aplikacija dosljedan snimke pomoću servisa Kopiraj glasnoću sjena (VSS) aplikacije provjerite jesu li u dosljedan stanju kada se koristi se snimka. Imajte na umu da Ako omogućite aplikacije dosljedan snimke, jer će utjecati na performanse aplikacije koje rade na virtualnim strojevima izvora. Provjerite je li vrijednost postavljate manji broj točaka dodatne oporavak konfigurirate.
3. U **vrijeme početka replikacije početni** odredite kada želite pokrenuti početne replikacije. Na replikacije pojavljuje se iznad internetska propusnost stoga želite zakazati izvan vaše zauzet sati. Zatim kliknite **u redu**.

    ![Pravila replikacije](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Prilikom stvaranja novog pravilnika ga automatski povezao s Hyper-V web-mjesta. Kliknite **u redu**. Web-mjestu tehnologije Hyper-V (i VMs u njoj) možete se povezati s više pravila za replikaciju u **postavkama** > **replikacije** > naziv pravila > **Pridružiti Hyper-V web-mjesta**.

## <a name="step-5-capacity-planning"></a>Korak 5: Planiranje kapaciteta

Sad kad ste na basic infrastrukture postavljanje koje možete razmislite o planiranje kapaciteta i utvrđivanje trebate li dodatne resurse.

Oporavak web-mjesta omogućuje kapaciteta planer za dodjelu prave resurse za vaše okruženje izvora, komponente oporavak web-mjesta, mreže i prostora za pohranu. Planer možete pokrenuti u brzom načinu rada za Procjena Prosječan broj VMs, diskova i prostora za pohranu na temelju ili u detaljnom načinu kojim ćete slika na razini radno opterećenje za unos. Prije nego što počnete morat ćete:

- Prikupite informacije o okruženju sustava replikacije, uključujući VMs diskova po VMs i prostor za pohranu po disk.
- Procjenu dnevnih stopa promjena (churn) morat ćete repliciranu podataka. [Planer kapaciteta za Hyper-V replike](https://www.microsoft.com/download/details.aspx?id=39057) možete koristiti da biste mogli učiniti ovo.

1.  Kliknite **Preuzimanje** Preuzmite alat, a zatim ga pokrenuti. [U članku čitanje](site-recovery-capacity-planner.md) koja se isporučuje se uz alat.
2.  Kada završite odaberite **da** **ste pokrenuli planer kapaciteta**?

    ![Planiranje kapaciteta](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Razmatranja propusnosti mreže

Možete koristiti alat planer kapaciteta za izračun propusnosti vam je potrebna za replikaciju (Početna replikacije i zatim delta). Omogućuje kontrolu količine korištenja propusnosti za replikaciju imate nekoliko mogućnosti:

- **Throttle propusnosti**: Hyper-V prometa koji umnaža Azure prolazi kroz određene značajke Hyper-V glavno računalo. Možete throttle propusnosti na poslužitelju glavnog računala.
- **Dotjerati propusnosti**: možete utjecati propusnost za replikaciju pomoću nekoliko ključevima registra.

#### <a name="throttle-bandwidth"></a>Ograničenja propusnosti

1. Otvorite Microsoft Azure sigurnosne kopije BLOG dodatak na poslužitelju Hyper-V glavnog računala. Prema zadanim postavkama prečac za Microsoft Azure Backup dostupna na radnoj površini ili u c:\Programske datoteke\Microsoft Azure oporavak usluge Agent\bin\wabadmin.
2. U dodatak kliknite **Promijeni svojstva**.
3. Na kartici **Throttling** odaberite **Omogućivanje korištenja propusnosti internetske ograničavanje za sigurnosne kopije operacije**, postavite ograničenja za posao i koje nisu posla sati. Valjani raspon ćete od 512 kb/s 102 MB/s sekundi.

    ![Ograničenja propusnosti](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

[Postavljanje OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet možete koristiti i da biste postavili ograničavanje. Slijedi primjer:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Postavljanje OBMachineSetting NoThrottle** označava da nema ograničavanje potreban je.


#### <a name="influence-network-bandwidth"></a>Utjecati propusnost mreže

1. U registru pronađite **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Da biste utjecati propusnosti promet na replicira disk, promijenite vrijednost **UploadThreadsPerVM**, ili stvoriti ključ ako ne postoji.
    - Da biste utjecati propusnosti za failback promet s Azure, izmijenite vrijednost **DownloadThreadsPerVM**.
2. Zadana vrijednost je 4. U odjeljku "overprovisioned" mreža tih ključeva registra treba promijeniti iz zadane vrijednosti. Maksimalna je 32. Praćenje promet za optimiziranje vrijednost.

## <a name="step-6-enable-replication"></a>Korak 6: Omogući replikacije

Sada omogućiti replikaciju na sljedeći način:

1. Kliknite **Korak 2: replicirati aplikacije** > **izvora**. Kada omogućite replikacije prvo ćete kliknite **+ replicirati** u sigurnog da biste omogućili replikacije za dodatnih računala.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. U plohu **izvora** > odaberite Hyper-V web-mjesta. Zatim kliknite **u redu**.
3. U **ciljnoj** odaberite pretplatu sigurnog i prebacivanje modela koji želite koristiti u Azure (klasični ili resursa za upravljanje) nakon prebacivanje.
4. Odaberite račun za pohranu koji želite koristiti. Ako želite da se pomoću računa za pohranu za različite od onih koje imate koje možete [stvoriti](#set-up-an-azure-storage-account). Da biste stvorili u prostor za pohranu račun pomoću modela resursima kliknite **Stvori novi**. Ako želite stvoriti račun za pohranu pomoću klasične modela ćete učiniti te [na portalu za Azure](../storage/storage-create-storage-account-classic-portal.md). Zatim kliknite **u redu**.
5.  Odaberite Azure mreža i podmreže na koji će Azure VMs povezivanje kada ih se spun nakon prebacivanje. Odaberite **Konfiguriraj sada za odabrani strojeva** Primjena postavki mreže na svim računalima odaberete za zaštitu. Odaberite **Konfiguriraj kasnije** da biste odabrali Azure mreže svako računalo. Ako želite koristiti drugog mrežnog od onih imate koje možete [stvoriti](#set-up-an-azure-network). Da biste stvorili koristeći model upravljanja resursima kliknite **Stvori novi**. Ako želite stvoriti mreže pomoću klasične modela ćete učiniti te [na portalu za Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Ako je primjenjivo, odaberite podmreži. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. Na **virtualnim računalima sustava** > **Odaberite virtualnim strojevima** kliknite i odaberite svakom računalu koje želite replicirati. Možete odabrati samo računala za koje je moguće omogućiti replikacije. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. U **svojstvima** > **Konfiguriranje svojstava**odaberite operacijski sustav za odabrani VMs i OS disk. Provjerite naziv Azure VM (ime ciljnog) uspostavu [preduvjeti Azure virtualnog računala](site-recovery-best-practices.md#azure-virtual-machine-requirements) i izmjena po potrebi. Zatim kliknite **u redu**. Naknadno možete postaviti dodatna svojstva.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. U odjeljku **Postavke replikacije** > **Konfiguracija postavki replikacije**, odaberite replikacije pravilo koje želite primijeniti za zaštićeni VMs. Zatim kliknite **u redu**. Možete izmijeniti pravilo replikacije u **postavkama** > **replikacije pravila** > naziv pravila > **Uređivanje postavki**. Promjene primijenite će se koristiti za računala koja se već replikaciju i novi strojeva.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Možete pratiti napredak posao **Omogućite zaštite** u **postavkama** > **Poslovi** > **Poslovi oporavak web-mjesta**. Nakon što je posao **Dovršavanje zaštitu** pokreće na računalu spremna je za prebacivanje.

### <a name="view-and-manage-vm-properties"></a>Prikaz i upravljanje svojstvima VM

Preporučujemo da provjerite svojstva izvora računala.

1. Kliknite **Postavke** > **Zaštićene stavke** > **Replicirati stavke** >, a zatim odaberite na računalu.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. U **svojstvima** možete pogledati informacija replikacije i prebacivanje na VM.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. U **računalnim i mrežne** > **Svojstva za izračun** možete odrediti Azure VM veličinu ime i cilj. Promijenite naziv u skladu s zahtjevima za Azure po potrebi. Pogledajte i izmijenili informacije o mreži ciljne, podmreže i IP adresu koja će se dodijeliti Azure VM. Imajte na umu sljedeće:

    - Možete postaviti IP adresu cilja. Ako ne unesete adresu nije uspio putem računala koristit će DHCP. Ako postavite adresu koja nije dostupna na prebacivanje na prebacivanje neće uspjeti. Iste ciljni IP adresa može se koristiti za testiranje prebacivanje ako je dostupan u mreži prebacivanje test adresu.
    - Broj mrežnih prilagodnika diktirali je veličinom koju navedete za virtualnog računala cilj na sljedeći način:

        - Ako je broj mrežnih prilagodnika na računalu izvor manje od ili jednak broju prilagodnika dopušteno Odredišna veličina računalu, zatim cilj će imati isti broj prilagodnika kao izvor.
        - Ako broj prilagodnika za virtualnog računala izvora premašuje broj dopušten za Odredišna veličina, a zatim Maksimalna veličina ciljne će se koristiti.
        - Ako, primjerice stroj izvora sadrži dvije mrežnih prilagodnika i odredišna veličina računalo podržava četiri, ciljnom računalu će imati dva prilagodnika. Ako na računalu izvor ima dva prilagodnika, ali veličinu podržani cilj podržava samo jedan ciljnom računalu neće imati samo jedan prilagodnika.     
        - Ako na VM ima više mrežnih prilagodnika oni će sve se povezati s istom mrežom.

    ![Omogućivanje replikacije](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  U **diskova** možete vidjeti operacijski sustav i diskova podataka na VM koji je replicirati.


## <a name="step-7-test-the-deployment"></a>Korak 7: Testiranje implementaciju

Da biste testirali implementacijskih možete pokrenuti test prebacivanje za jednu virtualnog računala ili oporavak tarifu koja sadrži jedan ili više virtualnim računalima.


### <a name="prepare-for-test-failover"></a>Priprema za prebacivanje test

- Da biste pokrenuli test prebacivanje preporučujemo stvorite novu Azure mrežu koja sadrži Izolirani u mreži Azure proizvodnje (to je zadano ponašanje prilikom stvaranja nove mreže u Azure). [Dodatne informacije](site-recovery-failover.md#run-a-test-failover) o pokretanju test failovers.
- Da biste ostvarili najbolje performanse kada ne preko Azure, instalirajte Agent za Azure na zaštićenom računalu. Omogućuje pokretanje brže i pomaže s otklanjanjem poteškoća. Instalirajte agent za [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) ili [Linux](https://github.com/Azure/WALinuxAgent) .
- U potpunosti ispitati implementaciju sustava morat ćete do infrastrukture za repliciranu strojno funkcionirati prema očekivanjima. Ako želite testirati servisa Active Directory i DNS možete stvoriti virtualnog računala kao kontroler domene s DNS-a i replicirati to Azure pomoću oporavak Azure web-mjesta. Pročitajte više u [test prebacivanje zahtjevi za Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
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

- Dodajte javnu IP adresu NIC pridružene VM Azure dopustili RDP.
- Provjerite je li još nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.
- Pokušajte se povezati. Ako se ne možete povezati provjerite je li na VM traje. Savjete za otklanjanje poteškoća u ovom [članku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Ako želite da biste pristupili programa Azure VM izvodi Linux nakon prebacivanje pomoću ljuske za sigurnu klijenta (ssh), učinite sljedeće:

**Na računalu na lokalni prije prebacivanja u slučaju pogreške**:

- Provjerite je li servis za sigurnu ljuske Azure VM postavljen na automatsko pokretanje na pokretanje sustava.
- Provjerite da pravila vatrozida omogućivanje programa SSH veze na njega.

**Na stranici Azure VM nakon prebacivanje**:

- Na mreži sigurnosne grupe pravila na nije uspio nad VM i Azure podmreže s kojim je povezan morati Dopusti dolazne veze s priključkom SSH.
- Javni krajnjoj točki treba kreirati da dopušta dolazne veze na priključak SSH (TCP priključak 22 po zadanom).
- Ako na VM se pristupa putem veze za VPN-a (usmjeravanje Express ili web-mjesto VPN) klijent može koristiti povezati izravno na VM putem SSH.

### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

Da biste pokrenuli test Nemoj prebacivanje na sljedeći način:

1. Neuspjeh putem jednog VM u **postavkama** > **Replicirati stavki**, kliknite na VM > **+ Prebacivanje Test**.

    ![Prebacivanje test](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Neuspjeh putem tarifu za oporavak, u odjeljku **Postavke** > **Oporavak tarife**, desnom tipkom miša kliknite željenu tarifu > **Prebacivanje Test**. Da biste stvorili na oporavak planiranje [, slijedite ove upute](site-recovery-create-recovery-plans.md).

3. **Prebacivanje Test** odaberite Azure mrežu na kojoj će se povezati Azure VMs nakon što se događa prebacivanje.

    ![Prebacivanje test](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Kliknite **u redu** da biste započeli s pomoćnim. Možete pratiti napredak tako da kliknete na VM da biste otvorili njegov properies ili na poslu **Test prebacivanje** u **postavkama** > **poslove oporavak web-mjesta**.
5. Kada u prebacivanje dosegne faza **testiranje dovrši** , učinite sljedeće:
    1. Prikaz virtualnog računala replike Azure portalu. Provjerite je li uspješno pokrene virtualnog računala.
    2. Ako ste postavljenih najviše pristup virtualnim strojevima iz lokalne mreže možete pokrenuti udaljenu radnu površinu vezu virtualnog računala.
    3. Kliknite **dovrši test** da ga završite.
    4. Kliknite **bilješke** da biste snimili i spremili sve opažanja povezan s pomoćnim test.
    5. Kliknite **test prebacivanje je dovršena**. Čišćenje okruženje za testiranje automatski isključujte i brisanje test virtualnog računala.
    6. U ovoj fazi elemente ni VMs automatski stvorio oporavak web-mjesta tijekom prebacivanje test se briše. Sve dodatne elemente koji ste stvorili za testiranje prebacivanje neće se izbrisati.

    > [AZURE.NOTE] Ako testiranje prebacivanje više od dva tjedna nastavlja se dovrši po prisilno.

6. Nakon dovršetka na prebacivanje i trebali da biste vidjeli replike Azure računalu prikazuju se na portalu za Azure > **virtualnih računala**. Provjerite je li u VM na odgovarajuću veličinu, koji ima povezani s odgovarajućim mrežom i je li pokrenut.
7. Ako ste [spremni za veze nakon prebacivanje](#prepare-to-connect-to-Azure-VMs-after-failover) trebali povezati Azure VM.


## <a name="failover"></a>Prebacivanje

Po dovršetku početne replikacije za vašeg računala, možete pozvati failovers kao potreba. Oporavak web-mjesta podržava različite vrste failovers - Test prebacivanje, prebacivanje Planirano i Neplanirana prebacivanje.
[Dodatne informacije](site-recovery-failover.md) o različitim vrstama failovers i detaljne opise kada i kako izvesti svaki od njih.

> [AZURE.NOTE] Ako je namjeru migriranja virtualnim strojevima u Azure, preporučujemo da za migriranje virtualnim strojevima u Azure koristite [Planirano prebacivanje operacija](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) . Kada migriranim aplikacija je provjeriti valjanost Azure pomoću testa prebacivanje, da biste dovršili migracije virtualnih računala, slijedite korake spomenute u odjeljku [Potpuni migracije](#Complete-migration-of-your-virtual-machines-to-Azure) . Nije potrebno izvršiti potvrdi ili Izbriši. Dovršavanje migracije dovršava migracije, uklanja zaštitu za virtualnog računala i zaustavlja naplata oporavak Azure web-mjesta za računalo.


###<a name="run-an-unplanned-failover"></a>Pokretanje neplanirano prebacivanje

To treba odabrati kada primarni web-mjesta Pretvori ne može pristupiti zbog incident neočekivane, kao što je power prekida ili virusa napada. Ovaj postupak opisuje kako pokrenuti na 'neplanirano prebacivanje' za oporavak plan. Umjesto toga možete pokrenuti prebacivanje za jednu virtualnog računala na kartici virtualnim računalima. Prije nego što počnete, provjerite je li sve virtualnim strojevima želite neće uspjeti putem dovršili početne replikacije.

1. Odaberite **oporavak tarife > recoveryplan_name**.
2. Na tarifu plohu oporavak kliknite **Neplanirano prebacivanje**.
3. Na stranici **neplanirano prebacivanje** odaberite izvorišno i odredišno mjesta.
4. Odaberite **Isključi virtualnim strojevima i sinkronizacija najnovije podatke** da biste odredili da biste isključili zaštićeni virtualnim strojevima i sinkronizirati podatke da bi se najnoviju verziju podataka će se nije uspjela putem trebao oporavak web-mjesta.
5. Nakon prebacivanje, virtualnim strojevima se potvrdi na čekanju stanje.  Kliknite **Potvrdi** za primjenu na prebacivanje.

[uči više](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Dovršavanje migracije virtualnim strojevima za Azure


>[AZURE.NOTE] Sljedeći koraci primjenjuju samo ako premještate virtualnim strojevima Azure

1. Izvođenje planirane prebacivanje kao spomenuta [ovdje](site-recovery-failover.md)
2. U **Postavke > replicirati stavke**, desnom tipkom miša kliknite virtualnog računala i odaberite **Dovrši migracije**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Kliknite **u redu** da biste dovršili migracije. Možete pratiti napredak pritiskom na VM da biste otvorili svojstva ili pomoću zadatak dovršen migracije u **Postavke > poslove oporavak web-mjesta**.


## <a name="monitor-your-deployment"></a>Praćenje implementaciju sustava

Evo kako možete pratiti konfiguracijske postavke, status i stanja za implementaciju sustava oporavak web-mjesta:

1. Kliknite naziv sigurnog pristupa na nadzornoj ploči **Essentials** . U ovoj nadzornoj ploči možete poslovi oporavak web-mjesta, replikacije status, tarife za oporavak, dijagnostiku poslužitelja i događaje.  Možete prilagoditi Essentials da bi se prikazala pločice i rasporedima koje su najkorisniji za vas, uključujući stanje druge sefovi oporavak web-mjesta i sigurnosno kopiranje.

    ![Osnove](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. Na pločici **stanja** možete nadzirati poslužiteljima web-mjesta koji dolazi do problema i događaje upućuju oporavak web-mjesta u zadnja 24 sata.
3. Možete upravljati i praćenje replikacije u **Replicirati stavki**, **Oporavak tarife**, i pločice **Poslovi oporavak web-mjesta** . Možete dubinski zadacima u **postavkama** -> **poslove** -> **Poslove oporavak web-mjesta**.
