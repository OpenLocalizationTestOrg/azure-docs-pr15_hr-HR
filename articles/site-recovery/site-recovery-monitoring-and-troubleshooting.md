<properties
    pageTitle="Praćenje i otklanjanje poteškoća s zaštitu virtualnim strojevima i fizičke poslužitelje | Microsoft Auzre" 
    description="Oporavak Azure web-mjesta koordinate replikacije, prebacivanje i oporavak virtualnih računala koja se nalazi na lokalnim poslužiteljima Azure ili sekundarni podatkovnog centra. U ovom se članku omogućuje praćenje i rješavanje problema s zaštite VMM i Hyper-V web-mjesta." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Praćenje i otklanjanje poteškoća s zaštitu virtualnim strojevima i fizičke poslužitelja

Ovaj vodič za otklanjanje poteškoća i praćenja omogućuje vam da biste saznali praćenja stanja replikacije i otklanjanjem za oporavak Azure web-mjesta.

## <a name="understanding-the-components"></a>Objašnjenje komponente

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>Implementacija web-mjesta VMware/fizičke za replikaciju između lokalnog i Azure.
Postavljanje DR između lokalnog računala VMware/fizičke; Poslužitelj za konfiguraciju, Matrica cilj i poslužitelj za postupak potrebno konfigurirati. Prilikom omogućivanja zaštitu s izvorišnim poslužiteljem Azure oporavak web-mjesta bit će instalirane mobilnost servisa. Objavite lokalnog prekida kada s izvorišnim poslužiteljem ne uspije postavite pokazivač na Azure, klijentima potrebe za postavljanje postupak Server Azure i cilj matrica informacije o lokalnom poslužitelju za zaštitu s izvorišnim poslužiteljem natrag da biste ponovno napravljena lokalnog. 

![VMware/fizičke implementaciju web-mjesta za replikaciju između lokalnog & Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>Implementacija web-mjesta VMM za replikaciju između lokalnog web-mjesta.

Kao dio postavljanja DR između dva lokalne lokacije; Da biste preuzeli i instalirali na poslužitelju VMM mora se Azure davatelja oporavak web-mjesta. Davatelj mora internetske veze da biste bili sigurni da će sve operacije koji se prikazuje s portala Azure prevesti u lokalne operacije kao Omogući zaštitu, isključivanje primarni strani virtualnim strojevima kao dio failovers itd.

![VMM implementaciju web-mjesta za replikaciju između lokalnog web-mjesta](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>Implementacija web-mjesta VMM za replikaciju između lokalnog & Azure.

Kao dio postavljanja DR između lokalnog & Azure; Azure davatelja oporavak web-mjesta mora biti preuzeli i instalirali na poslužitelju VMM uz Azure Services agenta za oporavak koje je potrebno je instalirati na svakom računalu koje hostira Hyper-V. Pogledajte [Objašnjenje web-mjesta Azure zaštita](./site-recovery-understanding-site-to-azure-protection.md) za dodatne informacije.

![VMM implementaciju web-mjesta za replikaciju između lokalnog & Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Hyper-V implementaciju web-mjesta za replikaciju između lokalnog & Azure

To je isti kao VMM implementaciju – samo razlika se davatelja & Agent dobiva instaliran na računalu koje hostira Hyper-V sam. Pogledajte [Objašnjenje web-mjesta Azure zaštita](./site-recovery-understanding-site-to-azure-protection.md) za dodatne informacije.

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Praćenje konfiguraciji, zaštitu i oporavak operacije

Svaka operacija u ASR dobiva nadzirati i praćenja na kartici "POSLOVI". U slučaju konfiguracije, zaštitu i oporavak pogreške idite na karticu ZADACIMA i jesu li sve pogreške.

![Praćenje konfiguraciji, zaštitu i oporavak operacije](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Kada pronađete pogreške u odjeljku Prikaz zadataka, odaberite POSAO, a zatim kliknite Detalji o pogrešci za taj zadatak.

![Praćenje konfiguraciji, zaštitu i oporavak operacije](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Detalje o pogrešci će vam olakšati prepoznavanje moguće uzroke i preporuke za problem.

![Praćenje konfiguraciji, zaštitu i oporavak operacije](media/site-recovery-monitoring-and-troubleshooting/image5.png)

U slučaju da iznad postoji čini se druga operacija koja je u tijeku zbog koji ne daje zaštitu konfiguracije da. Provjerite je li riješiti problem u skladu sa preporuke – nakon njega kliknite RESART da biste ponovno pokrenuli postupak.

![Praćenje konfiguraciji, zaštitu i oporavak operacije](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Mogućnost da biste ponovno pokretanje nije dostupna za sve operacije – za one koje ne nudi mogućnost ponovno pokretanje vratite se na objekt i opet ponovite postupak. Svaki zadatak možete biti poništeni bilo kojem trenutku tijekom u tijeku pomoću gumba Odustani.

![Praćenje konfiguraciji, zaštitu i oporavak operacije](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Praćenje stanja replikacije za virtualnog računala

Davatelj ASR središnje i udaljenom nadzor putem portala za Azure za svaku zaštićeni entiteti. Pomaknite se do stavke ZAŠTIĆENE nakon njega odaberite OBLAKA VMM ili ZAŠTITA GRUPE. Kartica OBLAKA VMM je samo za implementacijama VMM koji se temelji, a svim drugim situacijama su zaštićeni entiteti kartici ZAŠTITA GRUPE.

![Praćenje stanja replikacije za virtualnog računala](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Nakon njega odaberite zaštićeni entitet u odjeljku odgovarajući oblačić ili grupi zaštita. Kada odaberete zaštićeni entitet sve dopuštene operacije prikazane su u dnu okna.

![Praćenje stanja replikacije za virtualnog računala](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Kao što je prikazano gore u velikim slovom virtualnog računala stanja je važnosti – možete kliknuti gumb detalje o pogrešci pri dnu da biste vidjeli pogrešku. Na temelju "mogući uzroci" i "Preporuke" navedenim riješili problem.

![Praćenje stanja replikacije za virtualnog računala](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Praćenje stanja replikacije za virtualnog računala](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Napomena: Ako postoje aktivne operacije koje su u tijeku ili nije uspjelo zatim otiđite do prikaz ZADATKE kao što je ranije spomenutih da biste pogledali POSAO konkretne pogreške.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Otklanjanje poteškoća s lokalnim Hyper-V

Povezivanje s konzole za Upravitelj lokalnog Hyper-V odaberite virtualnog računala i pogledajte stanje replikacije.

![Otklanjanje poteškoća s lokalnim Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image12.png)

U ovom slučaju *Replikacije stanja* se ikonom kao kritično – *Prikaz replikacije stanja* da biste vidjeli detalje.

![Otklanjanje poteškoća s lokalnim Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Za predmete u kojoj je zaustavljeno replikacije za virtualnog računala, desnom tipkom miša kliknite odabir *replikacije*->*životopis replikacije*
![otklanjanje poteškoća s lokalnim problemi Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image19.png)

U slučaju da virtualnog računala migrira novi Hyper-V glavno računalo (unutar klaster ili samostalnu računalu) koji je konfiguriran pomoću ASR, replikacije za virtualnog računala ne može utjecati. Provjerite je li na novo Hyper-V glavno računalo zadovoljava sve na po – requisites i konfiguriran pomoću ASR.

### <a name="event-log"></a>Zapisnik događaja

| Izvori događaja                | Pojedinosti                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Aplikacije i zapisnika/Microsoft/VirtualMachineManager/poslužitelja/administrator servisa** (VMM Server)   |  To omogućuje za otklanjanje poteškoća s mnogo različite vrste problema VMM korisne zapisivanje. |
| **Aplikacija i servisa zapisnika/MicrosoftAzureRecoveryServices/replikacije** (Hyper-V glavnog računala)   | To sadrži korisne zapisivanje za otklanjanje problema s za mnoge agenta servisa za Microsoft Azure oporavak. <br/> ![Izvor događaja za glavno računalo Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Aplikacija i servisa Microsoft/zapisnika/Azure web-mjesta oporavak/davatelja/operativnih** (Hyper-V glavnog računala)   | To sadrži korisne zapisivanje za otklanjanje problema s za mnoge oporavak servis sustava Microsoft Azure web-mjesta. <br/> ![Izvor događaja za glavno računalo Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Aplikacije i zapisnika/Microsoft/Windows/Hyper-V-VMMS/administrator servisa** (Hyper-V glavnog računala) | To omogućuje za otklanjanje problema s za upravljanje virtualnog računala za mnoge Hyper-V korisne zapisivanje. <br/> ![Izvor događaja za glavno računalo Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Mogućnosti zapisivanja replikacije Hyper-V

Svi događaji vezani uz u Hyper-V replike prijavljeni Hyper-V-VMMS\\administrator zapisnika koja se nalazi ispod **Zapisnici programa i servisa\\Microsoft\\Windows**. Analitički zapisnika, k tome, možete biti omogućen za Hyper-V-VMMS. Da biste omogućili ovaj zapisnik, najprije provjerite zapisnike analitička i ispravljanje pogrešaka u preglednik događaja. Otvorite preglednik događaja, zatim na **izborniku Prikaz**, kliknite **Prikaži analitički i ispravljanje pogrešaka zapisnike**.

![Otklanjanje poteškoća s lokalnim Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analitički zapisnika vidljiv u odjeljku Hyper-V-VMMS

![Otklanjanje poteškoća s lokalnim Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image15.png)

U oknu **Akcije** kliknite **Omogući zapisnika**. Kada je omogućen, ona se pojavljuje u **Nadzor performansi** kao sesiju praćenje događaja koji se nalazi u odjeljku **skupova podataka prikupljanje.**

![Otklanjanje poteškoća s lokalnim Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Da biste prikazali podaci prikupljeni, najprije prekid sesije praćenje onemogućivanjem zapisnik, spremite zapis i ponovno je otvorite u preglednik događaja i koristiti druge alate za pretvaranje po želji.



## <a name="reaching-out-for-microsoft-support"></a>Obraćate za Microsoft Support

### <a name="log-collection"></a>Prikupljanje zapisnika

Za zaštitu VMM web-mjesta, pogledajte [Prikupljanje zapisnika ASR pomoću alata za podršku Dijagnostika platformu (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) za prikupljanje potrebnih zapisnike.

Radi zaštite Hyper-V web-mjesta, preuzmite [Alat](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) i izvoditi na glavnom računalu Hyper-V prikupiti zapisnike.

Za scenarije VMware/fizičke, pogledajte [Azure zbirke web-mjesta oporavak zapisnika Zaštita od web-mjesta za VMware i fizičke](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) Prikupljanje potrebnih zapisnika.

Alat za prikuplja zapisnike lokalno u odjeljku nasumičnog naziva podmapi u odjeljku **%LocalAppData%\ElevatedDiagnostics**

![Poslušajte koraci prikazani iz zaštite Hyper-V web-mjesta.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Otvorite zahtjev za podršku možete

Da biste podigli zahtjev za podršku možete za ASR, stupili za podršku Azure pomoću URL-a na <http://aka.ms/getazuresupport>

## <a name="kb-articles"></a>Članci iz baze znanja

-   [Kako sačuvati slovo za zaštićeni virtualnim strojevima koji nije uspjela tijekom ili migrira se na Azure](http://support.microsoft.com/kb/3031135)
-   [Upravljanje lokalnim da biste korištenja propusnosti mreže Azure zaštitu](https://support.microsoft.com/kb/3056159)
-   [ASR: "klaster resursa nije moguće pronaći" Pogreška prilikom da biste omogućili zaštitu za virtualnog računala](http://support.microsoft.com/kb/3010979)
-   [Razumijevanje i otklanjanje poteškoća s vodič replike Hyper-V](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Uobičajene pogreške ASR i njihove razlučivosti

U nastavku su uobičajene pogreške koje se mogu pritisnete i njihova rješenja. Svaku pogrešku navedenih u zasebnu WIKI stranicu.

### <a name="general"></a>Općenito
-   <span style="color:green;">NOVO</span> [Poslove ne uspijeva uz poruku o pogrešci "operacija je u tijeku." Pogreška 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">NOVO</span> [Zadataka koji se ne uspijeva uz poruku o pogrešci "Poslužitelj nije povezan s Internetom". Pogreška 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Postavljanje
-   [Poslužitelj VMM se ne može registrirati zbog Interna pogreška. Pogledajte na prikaz zadataka na portalu za oporavak web-mjesta više pojedinosti o pogrešci. Ponovno pokrenite instalacijski program registraciju poslužitelja.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Upravitelj za oporavak Hyper-V sigurnog ne može uspostaviti vezu. Provjerite postavke proxy poslužitelja ili pokušajte ponovno.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguracija
-   [Nije moguće stvoriti grupu zaštita: Pojavila se pogreška prilikom preuzimanja popis poslužitelja.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Glavno računalo Hyper-V klaster sadrži najmanje jedan statične mrežnog prilagodnika ili bez povezanog prilagodnika su konfigurirana za korištenje DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM imaju dozvole za dovršavanje akcije](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Ne možete odabrati račun za pohranu pretplate tijekom konfiguriranja zaštitu](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Zaštita
- <span style="color:green;">NOVO</span> Neuspjeh [omogućiti zaštitu s pogreške "zaštite nije moguće konfigurirati za virtualnog računala". Pogreška 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">NOVO</span> Neuspjeh [omogućiti zaštitu s pogreške "zaštite nije omogućen za virtualnog računala." Pogreška 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">NOVO</span> [Uživo pogreška pri migraciji 23848 - virtualnog računala će moguće premještati pomoću vrsta Live. To može prekinuti status zaštite oporavak virtualnog računala.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Omogući zaštitu nije uspjelo jer na glavnom računalu nije instaliran Agent](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Nije moguće pronaći na odgovarajuću glavno računalo za virtualnog računala replike - zbog najniža izračunati resursi](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Odgovarajuću glavno računalo za replike virtualnog računala nije moguće pronaći - zbog logičke mreža priložene](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Ne možete povezati na glavno računalo replike - veze nije moguće uspostaviti](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Oporavak
- VMM ne možete dovršiti postupak glavno računalo-
    -   [Nije uspjelo iznad odabrane oporavak točku za virtualnog računala: Općenito pristup zabranjen pogreške.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Nije uspjelo iznad odabrane oporavak točku za virtualnog računala Hyper-V nije uspjelo: operacija prekinuta pokušajte novija točka vraćanja. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Veza s poslužiteljem nije moguće uspostaviti (0x00002EFD)
        -   [Nije uspjelo omogućiti obrnutim replikaciju za virtualnog računala Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Nije uspjelo omogućiti replikaciju za virtualnog računala virtualnog računala Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Nije moguće izvršiti prebacivanje za virtualnog računala](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Plan za oporavak sadrži virtualnim strojevima koji nisu jeste li spremni za planirane prebacivanje](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Nije spreman za planirane prebacivanje virtualnog računala](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtualnog računala je operacijski sustav, a ne isključeno](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Za vrijeme odsutnosti grupiranje operacija se dogodilo virtualnog računala i prebacivanje potvrdi nije uspjela](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Prebacivanje test
    -   [Prebacivanje ne može se inicirati Budući da je testiranje prebacivanje u tijeku](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">NOVO</span>  Prebacivanje vrijeme uz 'PreFailoverWorkflow zadatka WaitForScriptExecutionTaskTimeout' zbog konfiguracijske postavke na mreži sigurnosne grupe vezane uz virtualnog računala ili podmreže kojem računalu pripada. Pogledajte ['PreFailoverWorkflow zadatka WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) detalje.


### <a name="configuration-server-process-server-master-target"></a>Konfiguriranje Server, poslužitelj za postupak matrica cilj
Poslužitelj za konfiguraciju (CS), postupak Server (PS), Matrica Targer (MT)
-   [ESXi glavnog računala na kojem se PS/CS hostira kao što je VM ne uspijeva s ljubičastom zaslona smrti.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Otklanjanje poteškoća nakon prebacivanje udaljene radne površine
-   Mnogi korisnici imaju prečica problemi povezati s nije uspio putem VM u Azure. [Pomoću otklanjanje poteškoća dokument RDP na VM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Dodavanje javnu IP na računalo virtualne upravitelj resursa
Ako je gumb **Poveži** na portalu zasivljen, a niste povezani s Azure putem Express usmjeravanje ili web-mjesto VPN vezu, morate je stvaranje i dodjela vaše VM javnu IP adresu mogli koristiti RDP/SSH. Slijedite korake u nastavku da biste dodali javnu IP mrežnog sučelja virtualnog računala.  

![Javnu IP na mreži sučelje nije uspjelo dodavanje putem virtualnog računala](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)