<properties
    pageTitle="Nadzor VMware rješenja u prijava analitiku | Microsoft Azure"
    description="Saznajte kako rješenje VMware nadzor olakšavaju upravljanje zapisnicima i praćenje ESXi domaćini."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>VMware nadzor (pretpregled) rješenja u zapisnik Analytics

Nadzor VMware rješenja u prijava analitiku je rješenje koji olakšava stvaranje središnje zapisivanje i nadzor pristup za velike VMware zapisnika. U ovom se članku opisuje kako možete otklanjanje poteškoća s, snimiti i upravljati domaćini ESXi na jednom mjestu korištenja rješenja. Uz rješenje, možete vidjeti detaljne podatke za sve svoje ESXi domaćini na jednom mjestu. Vidjet ćete broji gornji događaj, status i trendova VM i ESXi domaćini odvija pomoću zapisnike ESXi glavnog računala. Otklonite prikaz i pretraživanje središnje zapisnika ESXi glavnog računala. Možete i stvoriti upozorenja na temelju upita za pretraživanje zapisnika.

Rješenje koristi funkcija nativnog syslog ESXi glavnog računala da biste poslati podatke u ciljnu VM, koji sadrži OMS Agent. Međutim, rješenje ne zapisivati u syslog unutar cilj VM. Agent za OMS otvara priključak 1514 i očekuje podatke za to. Kad primi podatke, OMS agent ih gura podatke u OMS.

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja

Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Dodavanje rješenja VMware nadzor OMS radni prostor pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Podržani domaćini VMware ESXi
vSphere ESXi glavno računalo 5,5 i 6.0

#### <a name="prepare-a-linux-server"></a>Priprema Linux poslužitelja
Stvaranje Linux operacijski sustav VM sve podatke syslog primanje ESXi domaćini. [OMS Linux Agent](log-analytics-linux-agents.md) je točka zbirke za sve ESXi podatke syslog glavnog računala. Više glavnih računala ESXi možete koristiti za prosljeđivanje evidencije jedan Linux poslužitelj, kao u sljedećem primjeru.  

   ![tijek syslog](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Konfiguriranje zbirke syslog

1. Postavljanje prosljeđivanja syslog za VSphere. Detaljne informacije za postavljanje prosljeđivanja syslog potražite u članku [konfiguraciju syslog na ESXi 5.x i 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Idite na **Konfiguracija Hosta ESXi** > **softver** > **Napredne postavke** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. U polju *Syslog.global.logHost* dodajte poslužitelj Linux i broj priključka *1514*. Na primjer, `tcp://hostname:1514` ili`tcp://123.456.789.101:1514`

3. Otvorite ESXi vatrozida za glavno računalo za syslog. **Konfiguracija Hosta ESXi** > **softver** > **Sigurnosni profil** > **vatrozid** i otvaranje **Svojstva**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Provjerite vSphere konzole da biste potvrdili da taj syslog ispravno postavljen. Potvrda na glavnom računalu ESXI te priključak **1514** konfiguriran.

5. Testiranje veze između poslužitelja Linux i voditelju ESXi pomoću na `nc` naredbe na glavnom računalu ESXi. Ako, na primjer:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Preuzmite i instalirajte OMS Agent za Linux na poslužitelju Linux. Dodatne informacije potražite u [dokumentaciji OMS Agent za Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Nakon instalacije OMS Agent za Linux otvorite direktorija /etc/opt/microsoft/omsagent/sysconf/omsagent.d i kopirati datoteku vmware_esxi.conf u direktoriju /etc/opt/microsoft/omsagent/conf/omsagent.d i promjena vlasnika/grupe i dozvole za datoteku. Ako, na primjer:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Ponovno pokrenite OMS Agent za Linux tako da pokrenete `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. Na portalu OMS potražite zapisnika `Type=VMware_CL`. Kada OMS prikuplja podatke syslog, zadržava oblikovanje syslog. Na portalu neke određene polja snimaju, kao što su *naziv glavnog računala* i *ProcessName*.  

    ![Vrsta](./media/log-analytics-vmware/type.png)  

    Ako slično kao na slici iznad rezultata pretraživanja prikaz zapisnika se postaviti za korištenje na nadzornoj ploči rješenje OMS VMware nadzor.  

## <a name="vmware-data-collection-details"></a>VMware detalja za zbirke podataka

Rješenja za nadzor VMware prikuplja različite metriku i prijaviti se podataka o performansama iz ESXi domaćini pomoću agenata OMS Linux koje ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka.

| platforme | OMS Agent za Linux | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Linux|![Da](./media/log-analytics-vmware/oms-bullet-green.png)|![ne](./media/log-analytics-vmware/oms-bullet-red.png)|![ne](./media/log-analytics-vmware/oms-bullet-red.png)|            ![ne](./media/log-analytics-containers/oms-bullet-red.png)|![ne](./media/log-analytics-vmware/oms-bullet-red.png)| svake 3 minute|


U sljedećoj tablici prikazuju Primjeri polja podataka koji se prikupljaju rješenje VMware nadzor:

| Naziv polja | Opis |
| --- | --- |
| Device_s| Uređaji za pohranu VMware |
| ESXIFailure_s | vrste pogreške |
| EventTime_t | vrijeme kad došlo je do događaja |
| HostName_s | Naziv glavnog računala ESXi |
| Operation_s | Stvaranje VM ili brisanje VM |
| ProcessName_s | Naziv događaja |
| ResourceId_s | Naziv glavnog računala VMware |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | Status VMware SCSI |
| SyslogMessage_s | Syslog podataka |
| UserName_s | stvaranja ili brisanja VM korisnika |
| VMName_s | Naziv VM |
| Računalo | glavno računalo |
| TimeGenerated | vrijeme je generirana podataka |
| DataCenter_s | VMware podatkovnim centrom |
| StorageLatency_s | prostor za pohranu kašnjenje (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Nadzor VMware pregled rješenja

Pojavit će se pločica VMware na portalu OMS. Pruža više razine prikaza sve pogreške. Kada kliknete pločicu, prijeđite u prikaz nadzorne ploče.

![pločica](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Navigacija u prikazu nadzorne ploče

U prikazu nadzorne ploče **VMware** blades organiziranih prema:

- Zbroj Status neuspjeha
- Broji gornjoj glavno računalo događaj
- Broji gornjoj događaja
- Aktivnosti virtualnog računala
- Glavno računalo ESXi Disk događaja


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Kliknite bilo koju plohu da biste otvorili okno za pretraživanje zapisnika analize koji prikazuje informacija specifičnih za na plohu.

Na tom mjestu možete urediti upit za pretraživanje da biste izmijenili za određeni. Vodič za osnove OMS pretraživanja, pročitajte članak u [Vodič za pretraživanje zapisnika OMS.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Traženje ESXi glavno računalo događaja

Jedno glavno računalo ESXi generira više zapisnike, ovisno o njihovim procesa. Rješenja za nadzor VMware objedinjuje ih i navedene broji događaj. Središnje prikaz olakšava razumijevanje glavno računalo koje ESXi sadrži velik broj događaja i koji su se događaji najčešće pojavljuju se u okruženju sustava.

![događaja](./media/log-analytics-vmware/events.png)

Omogućuje dubinsku analizu daljnje tako da kliknete na glavno računalo ESXi ili vrstu događaja.

Kada kliknete naziv glavnog računala na ESXi, prikaz informacija iz tog ESXi glavnog računala. Ako želite da biste suzili rezultate s vrsta događaja, dodajte `“ProcessName_s=EVENT TYPE”` u upit za pretraživanje. Možete odabrati **ProcessName** u filtar za pretraživanje. Koje narrows podatke umjesto vas.

![Dubinska analiza](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Pronalaženje visoke VM aktivnosti

Virtualnog računala možete stvoriti i izbrisati ESXi glavnog računala. Ovo je korisno administrator da biste odredili koliko VMs stvara sustava ESXi glavnog računala. Taj u – uključivanje, olakšava razumijevanje performanse i planiranje kapaciteta. Čuvanja pratio VM aktivnosti događaje je ključnih upravljanje vaše okruženje.

![Dubinska analiza](./media/log-analytics-vmware/vmactivities1.png)

Ako želite da biste vidjeli dodatne ESXi glavno računalo VM stvaranja podatke, kliknite je ESXi naziv glavnog računala.

![Dubinska analiza](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Uobičajeni upita za pretraživanje

Rješenje sadrži druge korisne upite koji omogućuju upravljanje ESXi domaćini, kao što je prostor za pohranu visoka, Latencija prostora za pohranu i put do pogreške.

![Upiti](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Spremanje upita

Spremanje upita za pretraživanje je standardni značajci OMS web-mjesta i olakšavaju praćenje eventualne upite koje ste pronaći korisne. Kada stvorite upit koji vam korisni, spremite ga tako da kliknete **Favoriti**. Spremljeni upit možete jednostavno ponovnu ga na stranici [Moje nadzorne ploče](log-analytics-dashboards.md) koje možete stvoriti vlastitu prilagođenu nadzornu ploču.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Stvaranje upozorenja iz upita

Nakon stvaranja upita, možda ćete morati koristiti upite upozorenje kada određenih događaja. Informacije o stvaranju upozorenja potražite u članku [upozorenja u zapisnik analize](log-analytics-alerts.md) . Primjeri upozorenjem upite i ostale primjere upita, potražite u članku na [VMware monitora korištenjem OMS prijava analitiku](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) objavu na blogu.

## <a name="frequently-asked-questions"></a>Najčešća pitanja

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Što je potrebno da biste na na ESXi hostira postavku? Kakav učinak će se nalaziti na moje trenutno okruženje?
Rješenje koristi prosljeđivanje mehanizam nativni Syslog za ESXi glavnog računala. Ne morate sve dodatne Microsoftov softver na glavnom računalu ESXi da biste snimili zapisnike. Treba imati manje utjecaj na postojeće okruženje. Međutim, morate da biste postavili prosljeđivanje syslog koji je funkcija ESXI.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Morate ponovno pokrenuti glavno računalo za Moje ESXi?
ne. Taj postupak ne zahtijeva ponovno pokretanje računala. Ponekad vSphere nije ispravno ažuriranje na syslog. U tom slučaju, prijavite se na glavno računalo za ESXi i ponovno učitali u syslog. Ponovno ne morate ponovno pokrenuti glavno računalo, tako da se taj postupak ne disruptive svoje radno okruženje.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Možete li se povećati ili smanjiti količinu podaci iz zapisnika šalje OMS?
Da, možeš. Koristite postavke ESXi glavno računalo zapisnika razina u vSphere. Prikupljanje zapisnika temelji se na razini *informacije* . Tako, ako želite nadzirati VM stvaranje ili brisanje, morate zadržati razinu *informacije* na Hostd. Dodatne informacije potražite u članku [iz baze znanja VMware](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Zašto je Hostd ne pruža podataka da biste OMS? Moje postavke zapisnika postavljen je na informacije.
Došlo je do pogrešku ESXi glavno računalo za vremensku oznaku syslog. Dodatne informacije potražite u članku [iz baze znanja VMware](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Nakon primjene rješenje Hostd moraju se obično funkcionirati.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Mogu imati više glavnih računala ESXi prosljeđivanje syslog podataka na jednom VM s omsagent?
Da. Možete imati više glavnih računala ESXi prosljeđivanje jedan VM s omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Zašto ne vidim podataka slijedi u OMS?

Mogu postojati više razloga:

- Glavno računalo ESXi je nije ispravno margina podataka da biste VM pokrenut omsagent. Da biste testirali, poduzmite sljedeće korake:
    1. Da biste potvrdili, prijavite se na glavno računalo za ESXi pomoću ssh i pokrenite sljedeću naredbu:`nc -z ipaddressofVM 1514`

        Ako to ne uspije, postavke vSphere u Napredna konfiguracija vjerojatno ne ispravlja. Informacije o postavljanju ESXi glavno računalo za prosljeđivanje syslog potražite u članku [Konfiguriranje syslog zbirke](#configure-syslog-collection) .

    2. Ako syslog priključak povezivanje ne uspije, ali i dalje ne vidite sve podatke, zatim ponovno Učitaj syslog na glavnom računalu ESXi pomoću ssh pokrenite sljedeću naredbu:` esxcli system syslog reload`

- VM s OMS Agent nije ispravno postavljen. Da biste to provjerili, poduzmite sljedeće korake:
    1. OMS očekuje podatke s priključkom 1514 te ih gura podataka u OMS. Da biste provjerili je li otvoren, pokrenite sljedeću naredbu:`netstat -a | grep 1514`
    2. Trebali biste vidjeti priključak `1514/tcp` otvaranje. Ako to ne učinite, provjerite je li pravilno instaliran na omsagent. Ako ne vidite priključak informacije, zatim priključak syslog nije otvorena na na VM.
        1. Provjerite je li OMS Agent sustavom pomoću `ps -ef | grep oms`. Ako nije pokrenut, pokretanje postupka tako da pokrenete naredbu` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Otvaranje u `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` datoteku.

            Provjerite je li proper korisnika i grupa postavka valjana, slično kao:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Ako se datoteka ne postoji ili korisnika i grupa postavka nije u redu, poduzeti korektivne tako da [pripremite poslužitelju Linux](#prepare-a-linux-server).

## <a name="next-steps"></a>Daljnji koraci

- Korištenje [Zapisnika pretraživanja](log-analytics-log-searches.md) u prijava analitiku da biste pogledali detaljne podatke VMware glavnog računala.
- [Stvorite vlastitu nadzornu ploču](log-analytics-dashboards.md) s podacima VMware glavnog računala.
- [Stvaranje upozorenja](log-analytics-alerts.md) kada određenih događaja VMware glavnog računala.
