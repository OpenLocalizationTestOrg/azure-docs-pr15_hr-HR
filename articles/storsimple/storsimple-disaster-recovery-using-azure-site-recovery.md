<properties 
   pageTitle="Automatiziranje DR za zajedničke datoteke na StorSimple pomoću oporavak web-mjesta Azure | Microsoft Azure"
   description="U članku se opisuje korake i najbolje prakse pri stvaranju rješenja za oporavak Izrada za zajedničke datoteke hostirane na StorSimple prostora za pohranu."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatski oporavak Izrada rješenja pomoću oporavak Azure web-mjesta za zajedničke datoteke hostirane na StorSimple

## <a name="overview"></a>Pregled

Microsoft Azure StorSimple je rješenje hibridnog oblaka prostora za pohranu koji se odnosi complexities nestrukturirane podatke koji se često povezane sa zajedničkim datotekama. StorSimple služi za pohranu u oblaku kao datotečni nastavak lokalnog rješenja i automatski razine podataka putem lokalnog i prostor za pohranu za pohranu u oblaku. Zaštita podataka integrirani lokalno i brze snimke u oblaku, nema potrebe za sprawling infrastruktura za pohranu.

[Oporavak Azure web-mjesta](../site-recovery/site-recovery-overview.md) je servis za Azure sustavom koji omogućuje Izrada mogućnosti za oporavak (DR) po orchestrating replikacije, prebacivanje i oporavak virtualnih računala. Oporavak Azure web-mjesta podržava broj tehnologije za replikaciju dosljedno replicirati, zaštitu i jednostavno neće funkcionirati na virtualnim računalima i aplikacijama privatno/javno ili glavnom računalu oblaka.

Korištenje oporavak Azure web-mjesta, replikacije virtualnog računala i StorSimple oblaka snimke mogućnosti, možete zaštititi okruženju poslužitelja za cijeli dokument. U slučaju prekidu, jednim klikom možete koristiti da bi se prikazala vaše zajedničke datoteke na internetu u Azure u samo nekoliko minuta.

Ovaj dokument detaljno objašnjava kako možete stvoriti Izrada oporavak rješenje za vaše zajedničke datoteke hostirane na StorSimple prostora za pohranu i izvođenje planiranog, neplanirano i testirati failovers koristi plan za oporavak jednim klikom. Zapravo, prikazuje se kako možete izmijeniti Plan za oporavak u oporavak Azure web-mjesta zbirke ključeva da biste omogućili StorSimple failovers tijekom Izrada scenarija. Osim toga, opisuju podržani konfiguracije i preduvjeti. Ovaj dokument pretpostavlja da ste upoznati s osnovama oporavak Azure web-mjesta i StorSimple arhitekturi.

## <a name="supported-azure-site-recovery-deployment-options"></a>Podržani mogućnosti implementacije oporavak Azure web-mjesta

Korisnici možete implementacija poslužitelja datoteku kao fizičke poslužitelja ili virtualnim strojevima (VMs) izvodi na Hyper-V ili VMware, a zatim stvorite zajedničke datoteke iz jedinicama carved više StorSimple mjesta za pohranu. Oporavak Azure web-mjesta možete zaštititi fizičkih i virtualne implementacijama ili sekundarni web-mjestu ili Azure. Ovaj dokument pokriva pojedinosti DR rješenja s Azure s oporavak web-mjesta za datoteke poslužitelja VM hostirane na Hyper-V i zajedničke datoteke na StorSimple prostora za pohranu. Drugi slučajevi u kojima se datotečnom poslužitelju VM nalazi na web-mjesto VMware VM ili u okvir za fizičke računala može se implementirati na sličan način.

## <a name="prerequisites"></a>Preduvjeti

Implementacije rješenja za oporavak jednim klikom Izrada koja ih koriste oporavak Azure web-mjesta za zajedničke datoteke hostirane na StorSimple prostora za pohranu sadrži sljedeće preduvjete:

-   Lokalni poslužitelj sustava Windows Server 2012 R2 datoteka VM hostirane na Hyper-V ili VMware ili fizičke stroj

-   StorSimple prostora za pohranu uređaju lokalnog registriran Upravitelj Azure StorSimple

-   Potražite oblaka za StorSimple stvorene u upravitelju Azure StorSimple (to može biti zadržane u stanju isključi prema dolje)

-   Zajedničke datoteke hostirane na količine konfigurirali StorSimple uređaj za pohranu

-   [Oporavak azure web-mjesta servisa sigurnog](../site-recovery/site-recovery-vmm-to-vmm.md) stvorene u pretplatu na Microsoft Azure

K tome, ako je Azure web-mjesta za oporavak, pokrenite [Alat za procjenu za pripremu Azure virtualnog računala](http://azure.microsoft.com/downloads/vm-readiness-assessment/) na VMs da biste bili sigurni da su kompatibilne sa servisa Azure VMs i oporavak Azure web-mjesta.

Da biste izbjegli Latencija problema (što može uzrokovati veće troškove), svakako stvorite StorSimple oblaka uređaj, automatizacija računa i pohranu račune na istom području.

## <a name="enable-dr-for-storsimple-file-shares"></a>Omogućivanje DR za StorSimple zajedničke datoteke  

Svaku komponentu lokalnog okruženja mora biti zaštićene da biste omogućili dovršeno replikacije i oporavak. U ovom se odjeljku opisuje kako:

-   Postavljanje servisa Active Directory i DNS- a replikacije (nije obavezno)

-   Korištenje oporavak Azure web-mjesta da biste omogućili zaštitu datotečnom poslužitelju VM

-   Omogući zaštitu StorSimple jedinica

-   Konfiguriranje mreže

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Postavljanje servisa Active Directory i DNS- a replikacije (nije obavezno)

Ako želite zaštititi računala izvode servisa Active Directory i DNS- a tako da bi bili dostupni na web-mjestu DR morate izričito Zaštitno (tako da nakon prekida putem s provjerom autentičnosti su dostupni poslužitelji datoteka). Postoje dvije preporučene mogućnosti temelju složenost kupca lokalnog okruženja.

#### <a name="option-1"></a>Mogućnost 1

Ako je korisnik odabrao je mali broj aplikacije, kontroler jedne domene za cijeli lokalnog web-mjesta i će neuspješnih preko cijelog web-mjesta, a zatim preporučujemo korištenje replikacije oporavak Azure web-mjesta za replikaciju strojno kontroler domene na sekundarnom web-mjesto (to je moguće primijeniti na web-mjesto i web-mjesta za Azure).

#### <a name="option-2"></a>Mogućnost 2

Ako je korisnik odabrao sadrži veliki broj aplikacije, radi skupa stabala u servisu Active Directory i će neuspješnih preko nekoliko aplikacije po, a zatim preporučujemo postavljanja programa kontroler dodatnu domenu na web-mjestu DR (ili u sekundarnog web-mjesta ili u Azure).

Pogledajte [Automated DR rješenja za Active Directory i DNS-a pomoću oporavak web-mjesta za Azure](../site-recovery/site-recovery-active-directory.md) upute kada dostupnost kontroler domene na web-mjestu DR. Za ostatak ovog dokumenta, ne možemo će da kontroler domene je dostupna u web-mjesta DR.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Korištenje oporavak Azure web-mjesta da biste omogućili zaštitu datotečnom poslužitelju VM

Ovaj korak zahtijeva pripremite lokalnog okruženja za poslužitelj datoteka, stvaranje i Priprema za oporavak web-mjesta Azure sigurnog i omogućivanje zaštite datoteka na VM.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Da biste pripremili lokalnog okruženja za poslužitelj datoteka

1.  Postavite **kontrola korisničkih računa** na **obavijesti**. Ovo je obavezan da bi mogli koristiti Azure Automatizacija skripte za povezivanje ciljeve iSCSI nakon prekida ispočetka tako oporavak Azure web-mjesta.

    1.  Pritisnite tipke Windows + Q i potražiti **kontrolu korisničkih RAČUNA**.

    2.  Odabir **kontrola korisničkih računa za promjenu postavki**.

    3.  Povucite traku prema dolje prema **Obavijesti**.

    4.  Kliknite **u redu** , a zatim odaberite **da** kada se to od vas zatraži.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Instalirajte VM Agent za svaku od datotečnom poslužitelju VMs. Ovo je obavezan tako da možete pokrenuti Azure Automatizacija skripte na nije uspio putem VMs.

    1.  Da biste [preuzeli agenta](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Otvorite Windows PowerShell u načinu Administrator (Pokreni kao Administrator), a zatim unesite sljedeću naredbu da biste došli do mjesta za preuzimanje:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Naziv datoteke može se promijeniti ovisno o verziji.

1.  Kliknite **Dalje**.

2.  Prihvatite **Uvjete ugovor** , a zatim kliknite **Dalje**.

3.  Kliknite **Završi**.


1.  Stvaranje zajedničke datoteke pomoću količine carved više StorSimple mjesta za pohranu. Dodatne informacije potražite u članku [Korištenje upravitelja StorSimple servisa za upravljanje količine](storsimple-manage-volumes.md).

    1.  Na VMs vaše lokalne pritisnite tipke Windows + Q i potražiti **iSCSI**.

    2.  Odaberite **pokretač iSCSI**.

    3.  Odaberite karticu **Konfiguracija** i kopirajte pokretač naziv.

    4.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com/).

    5.  Odaberite karticu **StorSimple** , a zatim odaberite servis upravitelja StorSimple koja sadrži fizički uređaj.

    6.  Stvaranje container(s) glasnoće, a zatim stvorite volume(s). (Ove jedinice su share(s) datoteke na datotečnom poslužitelju VMs). Kopirajte naziv pokretač i dati odgovarajući naziv za pristup zapisima kontrolu prilikom stvaranja jedinice.

    7.  Odaberite karticu **Konfiguracija** i bilješke dolje IP adresa uređaja.

    8.  Ponovno otvorite **pokretač iSCSI** VMs vaše lokalne i unesite na IP u odjeljku brzo povezivanje. Kliknite mogućnost **Brzi povezivanje** (uređaj sad trebao biti povezan).

    9.  Otvorite Portal za upravljanje Azure i odaberite karticu **količine i uređaje** . Kliknite **automatski Konfiguriraj**. Jedinicu koju ste upravo stvorili prikazivati.

    10. Na portalu, odaberite karticu **uređaji** , a zatim odaberite **stvorili novu datoteku virtualne.** (Ovo virtualnog uređaja koristit će se ako se pojavi na prebacivanje). U ovom virtualnog uređaja zadržavaju se u izvanmrežni rad da biste izbjegli dodatnih troškova. Izvanmrežni virtualnog uređaja, prijeđite na odjeljak **virtualnim strojevima** portala i zatvorite ga.

    11. Vratite se na lokalni VMs i otvorite Disk Management (pritisnite tipku s logotipom sustava Windows + X i odaberite **Upravljanje Disk**).

    12. Uočit ćete neke dodatne diskova (ovisno o broju količine koje ste stvorili). Desnom tipkom miša kliknite prvoga, odaberite **Pokretanje Disk**i odaberite **u redu**. Desnom tipkom miša kliknite sekcija **Unallocated** , odaberite **Nova jednostavna jedinica**, dodijelite joj slovo i čarobnjaka.

    13. Ponovite korak l za sve diskova. Sada možete vidjeti sve diskova na **Ovom Računalu** u programu Windows Explorer.

    14. Stvaranje zajedničke datoteke te količine pomoću uloga datoteka i servise za pohranu.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Da biste stvorili i pripremiti je sigurnog oporavak Azure web-mjesta

Pogledajte [dokumentaciju za oporavak Azure web-mjesta](../site-recovery/site-recovery-hyper-v-site-to-azure.md) za početak rada s oporavak Azure web-mjesta prije zaštite datotečnom poslužitelju VM.

#### <a name="to-enable-protection"></a>Da biste omogućili zaštitu

1.  Prekid veze iSCSI target(s) s lokalnim VMs koji želite zaštititi pomoću oporavak Azure web-mjesta:

    1.  Pritisnite tipke Windows + Q i traženje **iSCSI**.

    2.  Odaberite **Postavljanje iSCSI pokretač**.

    3.  Prekid veze uređaja StorSimple koje prethodno povezali. Osim toga, možete se prebacivati isključivanje datotečnom poslužitelju za nekoliko minuta prilikom omogućivanja zaštitu.

    > [AZURE.NOTE] Time će zajedničke datoteke da biste privremeno nedostupan

1.  [Omogući zaštitu virtualnog računala](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) poslužitelja datoteka VM s portala za oporavak Azure web-mjesta.

2.  Kada početne sinkronizacije započne, možete povežete s ciljem ponovno. Otvorite pokretač iSCSI, odaberite uređaj StorSimple i kliknite **Poveži**.

3.  Kada sinkronizacije dovršetka i status u VM je **zaštićenog**, odaberite na VM, odaberite karticu **Konfiguracija** i ažuriranje mreže na VM sukladno tome (to je mreža za koju nije uspio putem VM(s) će biti dio). Ako mreža ne prikazuje se, to znači sinkroniziranje i dalje će.

### <a name="enable-protection-of-storsimple-volumes"></a>Omogući zaštitu StorSimple jedinica

Ako niste odabrali mogućnost **Omogući zadane sigurnosne kopije za ovu jedinicu** za jedinice StorSimple, idite na **Sigurnosne kopije pravila** u servisu StorSimple Manager, a stvaranje odgovarajuću sigurnosne kopije pravila za sve jedinice. Preporučujemo da postavite učestalost sigurnosnih kopija na oporavak točke cilj (RPO) koje želite da biste vidjeli za aplikaciju.

### <a name="configure-the-network"></a>Konfiguriranje mreže

Za datoteke poslužitelja VM konfigurirati postavke mreže u oporavak Azure web-mjesta tako da se mreža VM su priložene odgovarajuće mrežne DR nakon prebacivanje.

Možete odabrati na VM **Oblaka VMM** ili **Zaštita grupe** da biste konfigurirali postavke mreže kao što je prikazano na sljedećoj slici.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Stvaranje plana oporavak

Možete stvoriti plan za oporavak u ASR da biste automatizirali proces prebacivanje zajedničke datoteke. Ako se pojavi na prekidu, možete premjestiti i zajedničke datoteke prema gore za nekoliko minuta samo jednim klikom. Da biste omogućili ovu Automatizacija, trebat ćete račun za Azure automatizaciju.

#### <a name="to-create-the-account"></a>Da biste stvorili račun

1.  Idite na portalu za Azure klasični i prijeđite na odjeljak **automatizaciju** .

1.  Stvaranje novog računa automatizaciju. Imajte na istom zemlj. / područje u kojem su stvoreni StorSimple oblaka potražite i račune za pohranu.

2.  Kliknite **Novo** &gt; **Aplikacije servisa** &gt; **Automatizacija** &gt; **Runbook** &gt; **Iz galerije** da biste uvezli potrebna runbooks račun za automatizaciju.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Dodajte sljedeće runbooks iz okna **Izrada oporavak** u galeriji:

    -   Neuspjeh iznad spremnika StorSimple glasnoće

    -   Čišćenje StorSimple jedinica nakon Test prebacivanje (TFO)

    -   Postavljanje količine na uređaju StorSimple nakon prebacivanje

    -   Pokretanje StorSimple virtualne uređaj

    -   Deinstalacija datotečni nastavak prilagođene skripte u Azure VM

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Objavite sve skripte tako da odaberete u runbook na računu za automatizaciju i **Autor** kartice. Nakon ovaj korak kartici **Runbooks** pojavit će se na sljedeći način:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Na računu za automatizaciju idite na karticu **Resursi** , kliknite **Dodaj postavku** &gt; **Dodavanje vjerodajnica**i dodavanje Azure vjerodajnice – naziv resursa AzureCredential.

    Pomoću vjerodajnica za Windows PowerShell. Trebali biste vjerodajnica koja sadrži pomoću ID-a korisničko ime i lozinku s pristupom Azure pretplate i višestruke provjere autentičnosti koje su onemogućena. Ovo je potreban za provjeru autentičnosti ime korisnika tijekom na failovers i da bi se prikazala količine poslužitelj datoteka na web-mjestu DR.

1.  U račun za automatizaciju odaberite karticu **Resursi** , a zatim kliknite **Dodaj postavka** &gt; **varijabla Dodaj** i dodajte sljedeće varijable. Možete odabrati da biste šifrirali te resursi. Ove varijable su oporavak plan – određene. Ako vaš oporavak planiranje (koje ćete stvoriti u sljedećem koraku) TestPlan je naziv, a zatim varijabli mora biti TestPlan StorSimRegKey, TestPlan AzureSubscriptionName i tako dalje.

    -   *RecoveryPlanName* **-StorSimRegKey**: tipku registraciju za servis StorSimple Manager.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: naziv Azure pretplate.

    -   *RecoveryPlanName* **-ResourceName**: naziv resursa StorSimple koja ima StorSimple uređaja.

    -   *RecoveryPlanName* **-DeviceName**: uređaj koji ima nije uspjela iznad.

    -   *RecoveryPlanName* **-TargetDeviceName**: U StorSimple oblaka uređaj na kojem spremnike ćete nije uspjela iznad.

    -   *RecoveryPlanName* **-VolumeContainers**: odvojenih zarezom niz glasnoću spremnika izlaganje na uređaju koje je potrebno nije uspjela tijekom; na primjer, volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: naziv servisa ciljni uređaj (to je moguće pronaći u odjeljku **virtualnog računala** : naziv usluge je isti kao naziv DNS-a).

    -   *RecoveryPlanName* **-StorageAccountName**: naziv računa za pohranu u kojem će se spremiti skripte (koji je pokrenuti nije uspio putem VM). To može biti bilo koje prostora za pohranu računa koji ima malo prostora za pohranu skriptu privremeno.

    -   *RecoveryPlanName* **-StorageAccountKey**: tipkovni prečac za gornje račun za pohranu.

    -   *RecoveryPlanName* **-ScriptContainer**: naziv spremnika u kojem će se spremiti skriptu u oblaku. Ako ne postoji spremnik, će biti stvorena.

    -   *RecoveryPlanName* **-VMGUIDS**: nakon zaštite u VM, oporavak web-mjesta Azure dodjeljuje svaki VM jedinstveni ID koji daje detalje o nije uspio putem VM. Da biste nabavili u VMGUID, odaberite karticu **Servisi za oporavak** , a zatim kliknite **Stavku zaštićena** &gt; **Zaštitu grupe** &gt; **strojeva** &gt; **Svojstva**. Ako imate više VMs, dodajte GUID-ovi kao niz odvojenih zarezom.

    -   *RecoveryPlanName* **-AutomationAccountName** – naziv računa za automatizaciju u kojem ste dodali u runbooks i imovinu.

    Ako, na primjer, ako je naziv tarifa za oporavak fileServerpredayRP, zatim na kartici **Resursi** prikazivati na sljedeći način nakon što dodate sva sredstva.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Prijeđite na odjeljak **Oporavak servise** , a zatim odaberite sigurnog oporavak Azure web-mjesta koje ste prethodno stvorili.

2.  Odaberite karticu **Oporavak tarife** i stvorite novu tarifu oporavak na sljedeći način:

    na.  Navedite naziv, a zatim odaberite odgovarajuće **Zaštitu grupe**.

    b.  Na raspolaganju u VMs zaštitu grupu koju želite uvrstiti u planu oporavak.

    c.  Nakon oporavka stvara se tarifa, odaberite ga da biste otvorili prikaz prilagodbe plan za oporavak.

    d.  Odaberite **isključivanje sve grupe**, kliknite **skripta**pa odaberite **Dodaj primarni skripti prije zatvaranja svih grupa**.

    e.  Odaberite račun za automatizaciju (u koju ste dodali u runbooks), a zatim odaberite runbook **neće uspjeti nad-StorSimple-glasnoću – spremnika** .

    f.  Kliknite **grupe 1: pokretanje**, odaberite **virtualnim strojevima**i dodavanje VMs koji će biti zaštićene u planu za oporavak.

    g.  Kliknite **grupe 1: pokretanje**, odaberite **skripte**i dodajte sljedeće skripte redoslijedom kao **grupu 1 nakon** korake.

    - Početak-StorSimple-virtualne-potražite runbook
    - Neuspjeh runbook nad-StorSimple-glasnoću – spremnika
    - Runbook postavljanja količine nakon prebacivanje
    - Deinstalacija Prilagođeno-skripte-proširenja runbook

1.  Dodajte ručni akciju nakon 4 skripti komponente iznad u istom **grupe 1: nakon korake** sekciju. Ova je akcija točke na kojoj možete provjeriti sve radi li ispravno. Ova akcija nije potrebno moguće dodavati samo kao dio test prebacivanje (samo pa odaberite **Testiranje prebacivanje** potvrdni okvir).

2.  Nakon ručne akcije dodajte skriptu čišćenje koristeći isti postupak koji ste koristili za druge runbooks. Spremite plan za oporavak.

    > [AZURE.NOTE] Kada se pokrene test prebacivanje, potrebno je provjeriti sve u koraku ručno akciju jer StorSimple količine koja je imala je klonirana na uređaju ciljne izbrisat će se kao dio sustava čišćenje nakon dovršetka ručnog akciju.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Izvođenje testiranja prebacivanje

Potražite u vodiču pomoćnom [Rješenja Active Directory DR](../site-recovery/site-recovery-active-directory.md) pitanja vezana uz određenu sa servisom Active Directory tijekom prebacivanje test. Postavljanje lokalnog je uopće disturbed kada dođe do prebacivanje test. StorSimple količine koje su priložene VM lokalnog klonirana su u oblak potražite StorSimple na Azure. VM svrhe test se ne unese prema gore u Azure i kloniranu količine su priložene na VM.

#### <a name="to-perform-the-test-failover"></a>Da biste izvršili prebacivanje test

1.  Azure klasični portalu odaberite sigurnog za oporavak vašeg web-mjesta.

1.  Kliknite Stvori za poslužitelj datoteka VM plan oporavak.

2.  Kliknite **Testiraj prebacivanje**.

3.  Odaberite virtualne mreže da biste pokrenuli postupak prebacivanje test.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Kada je sekundarne okruženje prema gore, možete izvesti na vrednovanje.

2.  Nakon što završite na vrednovanje, kliknite **Provjera valjanosti dovršeno**. Okruženje za prebacivanje test će se očistiti, a zatim će se dovršiti postupak TFO.

## <a name="perform-an-unplanned-failover"></a>Izvođenje neplanirano prebacivanje

Tijekom neplanirano prebacivanje, količine StorSimple se nije uspjela putem virtualne uređaj, replike VM će biti uvezeni prema gore na Azure i jedinice su priložene na VM.

#### <a name="to-perform-an-unplanned-failover"></a>Da biste izvršili neplanirano prebacivanje

1.  Azure klasični portalu odaberite sigurnog za oporavak vašeg web-mjesta.

1.  Kliknite Stvori za poslužitelj datoteka VM plan oporavak.

2.  Kliknite **Prebacivanje** , a zatim odaberite **Neplanirano prebacivanje**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Odaberite ciljni mrežu, a zatim kliknite ikonu Provjera ✓ da biste pokrenuli postupak prebacivanje.

## <a name="perform-a-planned-failover"></a>Izvođenje planirane prebacivanje

Tijekom planiranog prebacivanje, na lokalni poslužitelj VM obavljanje isključiti i datoteka oblaka koristi se sigurnosno kopiranje snimku količine na StorSimple uređaju. Količine StorSimple se nije uspjela putem virtualne uređaj, replike VM se ne unese prema gore na Azure i jedinice su priložene na VM.

#### <a name="to-perform-a-planned-failover"></a>Da biste izvršili planiranog prebacivanje

1.  Azure klasični portalu odaberite sigurnog za oporavak vašeg web-mjesta.

1.  Kliknite Stvori za poslužitelj datoteka VM plan oporavak.

2.  Kliknite **Prebacivanje** , a zatim odaberite **Planirano prebacivanje**.

3.  Odaberite ciljni mrežu, a zatim kliknite ikonu Provjera ✓ da biste pokrenuli postupak prebacivanje.

## <a name="perform-a-failback"></a>Izvođenje na failback

Tijekom failback, StorSimple glasnoću spremnika su nije uspjelo putem natrag fizički uređaj nakon što se uzima sigurnosnu kopiju.

#### <a name="to-perform-a-failback"></a>Da biste izvršili u failback

1.  Azure klasični portalu odaberite sigurnog za oporavak vašeg web-mjesta.

1.  Kliknite Stvori za poslužitelj datoteka VM plan oporavak.

2.  Kliknite **Prebacivanje** i odaberite **Planirano prebacivanje** ili **neplanirano prebacivanje**.

3.  Kliknite **Promijeni smjer**.

4.  Odaberite mogućnosti za stvaranje VM i sinkronizacije odgovarajuće podatke.

5.  Kliknite ikonu provjeri ✓ da biste pokrenuli postupak failback.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Najbolje prakse

### <a name="capacity-planning-and-readiness-assessment"></a>Procjena planiranje i pripremu kapaciteta


#### <a name="hyper-v-site"></a>Web-mjesta Hyper-V

Pomoću [korisnika kapaciteta planer alata](http://www.microsoft.com/download/details.aspx?id=39057) za dizajniranje poslužitelj, za pohranu i mrežne infrastrukture za vaše okruženje replike Hyper-V.

#### <a name="azure"></a>Azure

[Alat za procjenu za pripremu Azure virtualnog računala](http://azure.microsoft.com/downloads/vm-readiness-assessment/) mogu se izvoditi na VMs da biste bili sigurni da su kompatibilni s Azure VMs i servise za oporavak Azure web-mjesta. Alat za procjenu spremnosti provjerava VM konfiguracije i upozorava kada konfiguracije nisu kompatibilni s Azure. Na primjer, je izdao upozorenje ako je pogon C: veća od 127 GB.


Planiranje kapaciteta sastoji se od najmanje dva važne procesa:

-   Mapiranje lokalnog Hyper-V VMs za Azure VM veličina (kao što su A6, A7, A8 i A9).

-   Određivanje potrebne propusnosti internetske veze.

## <a name="limitations"></a>Ograničenja

- Trenutno samo 1 StorSimple uređaja možete se nije uspjela iznad (u jednom potražite oblaka StorSimple). Scenarij datotečnom poslužitelju koji obuhvaća nekoliko uređaja StorSimple još nije podržana.

- Ako se pojavi pogreška prilikom omogućivanja zaštitu na VM, provjerite je li da ste povezani iSCSI ciljeve.

- Spremnika jedinicu koju ste je grupirane zbog sigurnosne pravilnike koje se protežu preko glasnoću spremnika će biti nije uspjelo putem zajedno.

- Sve jedinice spremnika jedinicu koju ste odabrali će nije uspjelo iznad.

- Količine koje se uklapaju više od 64 TB ne može biti uspjela putem jer je maksimalnog kapaciteta jedan potražite oblaka StorSimple 64 TB.

- Ako ne uspije, planirano/neplanirano prebacivanje na VMs stvaraju se u Azure, pa ne clean gore u VMs. No možete učiniti na failback. Ako izbrišete na VMs zatim VMs lokalnog ne može se ponovno uključiti.

- Nakon prebacivanje, ako se ne možete vidjeti jedinice, idite na VMs, otvorite Disk Management, ponovo skenirajte na disk, a zatim ih putem Interneta.

- U nekim slučajevima slova pogona DR web-mjestu mogu se razlikovati od na slova lokalnog. Ako se to dogodi, morat ćete ručno riješiti problem nakon završetka rada na prebacivanje.

- Višestruka provjera autentičnosti mora biti onemogućen zbog Azure vjerodajnica unesene u račun za automatizaciju kao sredstvo. Ako ovu provjeru autentičnosti je onemogućen, skripte neće moći će se automatski pokrenuti i oporavak plan neće uspjeti.

- Vremensko ograničenje posla prebacivanje: skripte u StorSimple će istek vremena ako prebacivanje glasnoću spremnika traje dulje od ograničenja oporavak Azure web-mjesta po skripte (trenutno 120 minuta).

- Sigurnosno kopiranje vremensko ograničenje: ističe skripte u StorSimple ako sigurnosnu kopiju jedinicama traje dulje od ograničenja oporavak Azure web-mjesta po skripte (trenutno 120 minuta).
 
    > [AZURE.IMPORTANT] Ručno pokrenuti sigurnosno kopiranje s portala za Azure, a zatim ponovno pokrenite plan za oporavak.

- Kloniraj vremensko ograničenje posla: ističe skripte u StorSimple ako kloniranje količine traje dulje od ograničenja oporavak Azure web-mjesta po skripte (trenutno 120 minuta).

- Vrijeme pogreške sinkronizacije: U StorSimple skripti pogreške out pričaju sigurnosnih kopija su uspješna čak i ako je sigurnosno kopiranje uspješno na portalu. Mogući uzrok za to može biti vrijeme potražite StorSimple može biti usklađen s trenutnog vremena u vremensku zonu.
 
    > [AZURE.IMPORTANT] Sinkronizirajte vrijeme uređaj s trenutnog vremena u vremensku zonu.

- Potražite prebacivanje pogreške: U StorSimple skripte možda neće uspjeti ako postoji prebacivanje na uređaj kada je pokrenut plan za oporavak.
    
    > [AZURE.IMPORTANT] Ponovno pokrenite plan za oporavak nakon prebacivanje uređaj.

## <a name="summary"></a>Sažetak

Pomoću oporavak Azure web-mjesta, možete stvoriti plan za oporavak dovršeno automatiziranog Izrada za poslužitelj datoteka VM pojavljuju zajedničke datoteke hostirane na StorSimple prostora za pohranu. Možete pokrenuti na prebacivanje sekundi s bilo kojeg mjesta u slučaju prekidima i get aplikacije prema gore i pokrenuta u nekoliko minuta.
