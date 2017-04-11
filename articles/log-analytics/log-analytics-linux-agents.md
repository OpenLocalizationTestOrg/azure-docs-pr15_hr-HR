<properties
    pageTitle="Povežite računala Linux prijava analitiku | Microsoft Azure"
    description="Korištenje zapisnika analize, možete prikupljanje i djelovanje na podacima generirao računala Linux."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="connect-linux-computers-to-log-analytics"></a>Povežite računala Linux zapisnika Analytics

Korištenje zapisnika analize, možete prikupljanje i djelovanje na podacima generirao računala Linux. Dodavanje podataka prikupljenih Linux OMS omogućuje upravljanje Linux sustavi i spremnik rješenja kao što su Docker bez obzira na to gdje se nalaze na računalima – gotovo bilo kojeg mjesta. Tako, tim izvorima podataka mogu se nalaziti u vaše lokalne podatkovnog centra kao fizičke poslužitelji virtualnog računala u oblak hostira servisa kao što su Amazon Web Services (AWS) ili Microsoft Azure ili čak i prijenosno računalo na stol. Osim toga, OMS i prikuplja podatke s računala sa sustavom Windows na sličan način tako da podržava je doista hibridno okruženje za IT.

Možete pregledavati i upravljanje podacima iz svih tih izvora s prijava analitiku u OMS pomoću portala za upravljanje jedan. Time se smanjuje potreba s mnogo različitih sustava, čini je lako zauzeti, a vi možete izvesti podatke koji vam se sviđa bilo poslovnog rješenja analize ili sustav koji ste već praćenje.

Ovaj se članak odnosi brzo pokretanje vodič koji će vam pomoći prikupljanje i upravljati podacima za računala Linux pomoću OMS Agent za Linux. Dodatne tehničke detalje kao što su konfiguracija proxy poslužitelja, informacije o CollectD metriku i prilagođenih izvora podataka JSON, pronaći ćete informacije na [OMS Agent za pregled Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) i [OMS Agent cijelog dokumentacije Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) na Github.


Trenutno ne prikupite sljedeće vrste podataka s računala Linux:

- Performanse mjerenja
- Syslog događaja
- Upozorenja o od Nagios i Zabbix
- Docker spremnik performanse metriku, zaliha i zapisnika

## <a name="supported-linux-versions"></a>Podržane Linux verzije

X86 i x64 verzije podržani su službeno raznih distribucija Linux. Međutim, OMS agente za Linux može pokrenuti i na druge distribucija nije naveden na popisu.

- Amazon Linux 2012.09 kroz 2015.09
- CentOS Linux 5, 6 i 7
- Oracle Linux 5, 6 i 7
- Crvena je vaša Enterprise Server Linux 5, 6 i 7
- Debian GNU/Linux 6, 7 i 8
- Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
- SUSE Linux Enterprise Server 11 i 12

## <a name="oms-agent-for-linux"></a>OMS Agent za Linux
Operacije Management paket Agent za Linux sastoji od više paketa. Datoteka izdanje sadrži sljedeće paketa, dostupna tako da pokrenete paket ljuske s `--extract`.

**Paket** | **Verzija** | **Opis**
----------- | ----------- | --------------
omsagent | 1.1.0 | Operacije Management paket Agent za Linux
omsconfig | 1.1.1 | Agent za konfiguraciju za OMS Agent
OMI | 1.0.8.3 | Otvaranje upravljanje infrastrukture (OMI) – laganih CIM poslužitelju
scx | 1.6.2 | Davatelji CIM OMI za metriku performanse operacijski sustav
Apache cimprov | 1.0.0 | Performanse Apache HTTP poslužitelj nadzor davatelj usluga za OMI. Instalirati samo ako je otkriven Apache HTTP poslužitelj.
MySQL cimprov | 1.0.0 | Performanse poslužitelj MySQL nadzor davatelj usluga za OMI. Instalirati samo ako je otkriven MySQL/MariaDB poslužitelja.
docker cimprov | 0.1.0 | Davatelj docker za OMI. Instalirati samo ako je otkriven Docker.

### <a name="additional-installation-artifacts"></a>Artefakte dodatne instalacije
Nakon instalacije OMS agent za pakete Linux sljedeće dodatne sistemskih konfiguracijske promjene će se primijeniti. Kada deinstalirate omsagent paketa, uklanjaju se te elemente.
- Korisnik velikim ovlastima pod nazivom: `omsagent` je stvorili. Ovo je omsagent daemon izvodi kao račun
- Stvara se sudoers "Uključi" datoteka na /etc/sudoers.d/omsagent to neadministratorskog omsagent da biste ponovno pokrenuli daemons syslog i omsagent. Ako upute sudo "Uključi" poslužitelja nisu podržane u instaliranoj verziji sudo, te stavke će biti napisani /etc/sudoers.
- Konfiguracija syslog je izmijenjena proslijediti podskup događaja agenta. Dodatne informacije potražite u članku odjeljku **Konfiguriranje zbirke podataka**

### <a name="linux-data-collection-details"></a>Linux pojedinosti za zbirke podataka

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka.

| Izvor | Izravni Agent | Agent za SCOM | Azure prostora za pohranu | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Zabbix|![Da](./media/log-analytics-linux-agents/oms-bullet-green.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|1 min|
|Nagios|![Da](./media/log-analytics-linux-agents/oms-bullet-green.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|pristignu|
|syslog|![Da](./media/log-analytics-linux-agents/oms-bullet-green.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|iz spremišta Azure: 10 minuta; agenta: pristignu|
|Linux mjerača performansi|![Da](./media/log-analytics-linux-agents/oms-bullet-green.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|Što su zakazani, minimalnu vrijednost 10 sekundi|
|Evidentiranje promjena|![Da](./media/log-analytics-linux-agents/oms-bullet-green.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|![ne](./media/log-analytics-linux-agents/oms-bullet-red.png)|hourly|



### <a name="package-requirements"></a>Preduvjeti za paketa
| **Potrebne paketa**  | **Opis**   | **Minimalna verzija**|
|--------------------- | --------------------- | -------------------|
|Glibc |    Biblioteka GNU C   | 2,5 12|
|Openssl    | Biblioteka OpenSSL | 0.9.8E ili 1.0|
|Zakretanja | Zakretanja web-klijentu | 7.15.5
|Python ctypes |funkcija biblioteke | n/d|
|PAM | Uključiv provjere autentičnosti moduli  |n/d |

>[AZURE.NOTE] Rsyslog ili syslog pokazuju moraju prikupljanje syslog poruke. Zadani daemona syslog verzije 5 crveno je vaša Enterprise Linux, CentOS i Oracle Linux verzije (sysklog) nije podržana za zbirke syslog događaja. Da biste prikupili podatke syslog iz ove verzije ovih distribucija, rsyslog daemon mora biti instalacije i konfiguracije da biste zamijenili sysklog.

## <a name="quick-install"></a>Brzi Instaliraj

Pokrenite sljedeće naredbe za preuzimanje na omsagent, provjera valjanosti na kontrolni zbroj, a zatim instalirati i onboard agenta. Naredbe su za 64-bitnu verziju. Radni prostor ID i primarni ključ nalaze se na portalu OMS u odjeljku **Postavke** na kartici **Povezani izvora** .

![Detalji o radnog prostora](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.1.0-28/omsagent-1.1.0-28.universal.x64.sh
sha256sum ./omsagent-1.1.0-28.universal.x64.sh
sudo sh ./omsagent-1.1.0-28.universal.x64.sh --upgrade -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Postoje različite druge načine da biste instalirali agenta i nadograditi. Dodatne informacije o njima na [korake da biste instalirali OMS Agent za Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Možete pogledati i [Azure videozapisa vodič](https://www.youtube.com/watch?v=mF1wtHPEzT0).

## <a name="choose-your-linux-data-collection-method"></a>Odaberite način za zbirke podataka Linux

Omogućuje odabir vrste podataka koje želite prikupiti ovisi o tome želite li koristiti OMS portal ili ako želite urediti različitih konfiguracija datoteka izravno na klijentima Linux. Ako odlučite koristiti portal za konfiguraciju automatski se šalje na sve svoje klijente Linux. Ako vam je potrebna različitih konfiguracija za različite klijente Linux, morat ćete pojedinačno – uredite klijenta datoteke ili koristite alternativu kao što su PowerShell DSC, Chef ili Puppet.

Možete navesti syslog događaja i mjerača performansi koje želite prikupiti pomoću konfiguracijske datoteke na računalima Linux. *Ako odaberete da biste konfigurirali prikupljanje podataka tako da uredite agent konfiguracijske datoteke, trebali biste onemogućiti središnje konfiguracije.*  Upute navedene u nastavku da biste konfigurirali prikupljanje podataka u datotekama na agent konfiguracije kao i da biste onemogućili središnje konfiguracije za sve OMS agente za Linux ili pojedinačna računala.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>Onemogućivanje OMS upravljanje za pojedino Linux računalo

Prikupljanje središnje podataka za konfiguraciju podataka nije omogućen za pojedinačne Linux računala uz pomoć skripte OMS_MetaConfigHelper.py. To može biti korisno ako podskup računala mora imati specijalizirane konfiguracije.

Da biste onemogućili središnje konfiguracije:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

Ponovno Omogućivanje središnje konfiguracije:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Linux mjerača performansi

Mjerača performansi Linux su slične mjerača performansi sustava Windows – i raditi na sličan način. Dodavanje i konfiguriranje ih možete koristiti sljedeće postupke. Kada se dodaju OMS, prikupljanja podataka za njih svakih 30 sekundi.

### <a name="to-add-a-linux-performance-counter-in-oms"></a>Da biste dodali brojač performanse Linux u OMS

1. Da biste konfigurirali OMS agente za Linux pomoću portala za OMS, možete dodati mjerača performansi Linux na stranici s postavkama, kliknite **Podaci**.  
2. Na stranici **Postavke** u odjeljku **Podaci** kliknite **mjerača performansi Linux** i zatim odaberite ili unesite naziv brojač koju želite dodati.  
    ![podataka](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Ako ne znate ime i prezime brojač, možete započeti pisati dio imena i prezimena, a prikazat će se popis dostupnih mjerača. Kada pronađete brojač koju želite dodati, kliknite naziv na popisu, a zatim kliknite ikonu plus da biste dodali brojač.
4. Nakon što dodate brojač, pojavljuje se na popisu mjerača označene boja trake.
5. Po zadanom je odabrana mogućnost **Primijeni ispod konfiguracije Moje strojeva** . Ako želite onemogućiti slanje konfiguracijske podatke, poništite odabir.
6. Kada ste gotovi izmjena mjerača performansi, pri dnu stranice kliknite **Spremi** da biste dovršavanje promjene. Promjene konfiguracije koje ste napravili pa šalju svi agenti OMS za Linux registrirane s OMS, obično unutar pet minuta.

### <a name="configure-linux-performance-counters-in-oms"></a>Konfiguriranje mjerača performansi Linux u OMS

Za mjerača performansi sustava Windows, možete odabrati instancu za svaki brojač performansi. Međutim, za mjerača performansi Linux, sve instance brojač koji odaberete primjenjuje se na sve podređene mjerača od brojač nadređeni. Sljedeća tablica prikazuje uobičajene slučajeve dostupne mjerača performansi Linux i Windows.

| **Naziv instance** | **Značenje** |
| --- | --- |
| \_Ukupni zbroj | Zbroj sve instance |
| \* | Sve instance |
| (/ & #124; / var) | Odgovara instance pod nazivom: / ili /var |


Isto tako, interval uzorka koju odaberete za nadređenog brojač odnosi se na sve njegove mjerača podređene. Drugim riječima, sve podređene brojač uzorka intervalima i instance je uz zajedno.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Dodavanje i konfiguriranje performanse metriku s Linux

Performanse metriku za prikupljanje kontrolira konfiguraciju u /etc/opt/microsoft/omsagent/conf/omsagent.conf. Potražite u članku [dostupne performanse metriku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) za dostupni Tečajevi i metrike za OMS Agent za Linux.

Svaki objekt ili kategorija performanse metriku za prikupljanje potrebno je definirati u konfiguracijskoj datoteci kao jedna `<source>` element. Vidjet ćete da sintaksa slijedi uzorak u nastavku.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Prilagodljivo parametre taj element su:

- **Objekt\_naziv**: naziv objekta zbirke.
- **Instance\_regex**: *Uobičajeni izraz* koji definira koje će se instance prikupljanje. Vrijednost: `.*` navodi sve instance. Da biste prikupili procesor metriku samo na \_ukupni instance možete odrediti `_Total`. Da biste prikupili postupak metriku samo instance crond ili sshd, možete odrediti: `(crond|sshd)`.
- **Brojač\_naziv\_regex**: *Uobičajeni izraz* koji definira koji mjerača (za objekt) da biste prikupili. Da biste prikupili sve mjerača objekta, navedite: `.*`. Da biste prikupili samo zamjena prostora mjerača memorije objekta, možete odrediti:`.+Swap.+`
- **Interval:**: učestalost kojom se prikupe mjerača objekta.

Zadana konfiguracija za performanse metriku je:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a>Omogućivanje mjerača performansi MySQL pomoću naredbi Linux

Ako je MySQL ili poslužitelju MariaDB otkrivena na računalu kada instalirate paket omsagent, performanse nadzor davatelj usluga za poslužitelj MySQL automatski se instalira. Ovaj davatelj povezuje s lokalnog poslužitelja MySQL/MariaDB da biste otkrili statistiku o izvedbi. Morate konfigurirati MySQL korisničke vjerodajnice da bi se davatelj možete pristupiti poslužitelju MySQL.

Da biste definirali zadani korisnički račun za poslužitelj MySQL localhost, koristite sljedeći primjer naredbe.

>[AZURE.NOTE] Datoteka vjerodajnice mora biti pročitati omsagent računa. Preporučuje se izvodi naredbu mycimprovauth kao omsgent.


```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'

sudo service omiserverd restart
```


Umjesto toga, možete odrediti traženih vjerodajnica MySQL u datoteci, kao što su stvaranje datoteka: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Dodatne informacije o upravljanju MySQL vjerodajnice za praćenje pomoću datoteke mysql autorizacija potražite u članku [Upravljanje MySQL nadzora vjerodajnice u datoteci za provjeru autentičnosti](#manage-mysql-monitoring-credentials-in-the-authentication-file).

Potražite u članku [potrebne dozvole za bazu podataka za mjerača performansi MySQL](#database-permissions-required-for-mysql-performance-counters) detalje o potrebnih MySQL korisnika za prikupljanje podataka o performansama poslužitelj MySQL dozvole za objekte.

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Omogućivanje mjerača performansi Apache HTTP poslužitelj pomoću naredbi Linux

Ako je Apache HTTP poslužitelj otkrivena na računalu kada instalirate paket omsagent, performanse davatelj usluga za Apache HTTP poslužitelj za nadzor automatski se instalira. Ovaj davatelj ovisi o Apache "modul", koji morate učitati HTTP poslužitelj Apache da biste pristupili podataka o performansama.

Možete učitati modul pomoću sljedeće naredbe:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Da biste memorije Apache modul za nadzor, pokrenite sljedeću naredbu:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="to-view-performance-data-with-log-analytics"></a>Prikaz podataka o performansama s zapisnika Analytics

1. Na portalu paket za upravljanje operacije kliknite pločicu zapisnika pretraživanja.
2. Traka za pretraživanje upišite `* (Type=Perf)` da biste pogledali sve mjerača performansi.


Jer OMS prikuplja i podataka performanse sustava Windows, koje treba opsega prema dolje pretraživanje podataka specifičnih za Linux. Tako, u sljedećem primjeru želite prikaz podataka o performansama određene s poslužiteljem Linux primjer pod nazivom Chorizo21.

```
Type=Perf Computer=chorizo*
```

![primjer poslužitelja koji se prikazuju u rezultatima pretraživanja](./media/log-analytics-linux-agents/oms-perfsearch01.png)

U rezultatima, možete kliknuti **metriku** za prikaz brojača koji trenutku prikupljanja podataka. Podaci u stvarnom vremenu prikazuje se kao grafikona za svaki brojač.

![mjerenja](./media/log-analytics-linux-agents/oms-perfmetrics01.png)


## <a name="syslog"></a>Syslog

Syslog je slična zapisnika događaja sustava Windows događaj protokol zapisivanje – i raditi na sličan način prilikom prikaza OMS.

### <a name="to-add-a-new-linux-syslog-facility-in-oms"></a>Da biste dodali novi funkcijom syslog Linux u OMS

1. Na stranici **Postavke** u odjeljku **Podaci** kliknite **Syslog** , a zatim s lijeve strane ikonu plus, unesite naziv funkciju syslog koji želite dodati.
    ![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2.  Ako ne znate ime i prezime funkciju, možete započeti pisati dio imena i prezimena, a prikazat će se popis dostupnih syslog funkcije. Kada pronađete funkciju syslog koji želite dodati, kliknite naziv na popisu, a zatim kliknite ikonu plus da biste dodali funkciju syslog.
3.  Nakon što dodate funkciju, pojavljuje se na popisu označene boja trake. Zatim odaberite severities (kategorije podataka funkcijom syslog) koje želite prikupiti.
4.  Pri dnu stranice kliknite **Spremi** da biste dovršavanje promjene. Promjene konfiguracije koje ste napravili pa šalju svi agenti OMS za Linux registrirane s OMS, obično unutar pet minuta.


### <a name="configure-linux-syslog-facilities-in-linux"></a>Konfiguriranje Linux syslog funkcije u Linux

Događaji Syslog šalju iz daemona syslog, primjerice rsyslog ili syslog pokazuju lokalnog priključka koji je agenta priključuje na. Po zadanome, priključak 25224. Kada instalirate agenta primjenjuje se zadana konfiguracija syslog. To nalazi se na:


Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog pokazuju: /etc/syslog-ng/syslog-ng.conf


Zadana OMS agent syslog konfiguracija prenosi syslog događaje iz sve objekte s težinu upozorenja ili noviji.

>[AZURE.NOTE] Ako uređujete konfiguracije syslog, morate ponovno pokrenuti daemon syslog da bi promjene stupile na snagu.

Zadani syslog konfiguraciju za OMS Agent za Linux za OMS je:

#### <a name="rsyslog"></a>Rsyslog

```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### <a name="syslog-ng"></a>Syslog pokazuju

```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="to-view-all-syslog-events-with-log-analytics"></a>Da biste pogledali sve Syslog događaje s zapisnika Analytics

1. Na portalu paket za upravljanje operacije kliknite pločicu **Zapisnika pretraživanja** .
2. U grupiranja **Upravljanje zapisnikom** odaberite unaprijed definirane syslog pretraživanje, a zatim odaberite jednu da biste ga pokrenuti.

U ovom se primjeru prikazuju se svi događaji Syslog.

![Syslog događaje zapisnika legende](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Sada možete dubinski rezultata pretraživanja.

## <a name="linux-alerts"></a>Linux upozorenja

Ako koristite Nagios ili Zabbix da biste upravljali vašeg računala Linux, OMS mogu primati upozorenja stvorene iz tih alata. Međutim, trenutno ne postoji način da biste konfigurirali dolaznih upozorenja podataka pomoću portala za OMS. Umjesto toga, morat ćete uređivanje konfiguracijska datoteka da biste pokrenuli slanje upozorenja OMS.



### <a name="collect-alerts-from-nagios"></a>Prikupljanje upozorenja od Nagios

Da biste prikupili upozorenja s poslužitelja Nagios, morate unijeti sljedeće promjene konfiguracije.

1. Dozvolite korisniku **omsagent** čitanje pristup datoteci zapisnika Nagios (odnosno /var/log/nagios/nagios.log/var/log/nagios/nagios.log). Datoteka nagios.log uz pretpostavku da je vlasnik grupe **nagios** **nagios** grupu možete dodati korisnika **omsagent** .

    ```
    sudo usermod –a -G nagios omsagent
    ```

2. Izmijenite datoteku omsagent.confconfiguration (/ etc/opt/microsoft/omsagent/conf/omsagent.conf). Provjerite sljedeće stavke su prisutne i ne komentiranim:

    ```
    <source>
    type tail
    #Update path to point to your nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```

3. Ponovno pokrenite omsagent daemon:

    ```
    sudo service omsagent restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Prikupljanje upozorenja od Zabbix

Da biste prikupili upozorenja s poslužitelja Zabbix, koji se izvršavaju slične korake onima za Nagios iznad, osim morat ćete navesti korisnika i lozinke u *Čišćenje teksta*. To je idealna, no vjerojatno će se promijeniti uskoro. Da biste riješili taj problem, preporučujemo stvaranje korisnika i dodijeliti dozvole samo za praćenje.

Primjer dio konfiguracijska datoteka omsagent.conf (/ etc/opt/microsoft/omsagent/conf/omsagent.conf) za Zabbix treba oblik sličan na sljedeći način:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a>Prikaz upozorenja u analize zapisnika pretraživanja

Nakon konfiguriranja računala Linux slanje upozorenja OMS, možete koristiti nekoliko jednostavnih zapisnika upita za pretraživanje da biste pogledali upozorenja. U sljedećem primjeru upita za pretraživanje vraća sva upozorenja snimljena koji su generirani. Ako, na primjer, ako neke sortiranje problem se pojavljuje u IT infrastrukturu, zatim rezultate za sljedeći ogledni upit mogu upućivati gdje mogu potjecati problem. Možete i omogućuje jednostavno dubinsku analizu da biste upozorenja izvor sustav olakšavaju sužavanje vaše istrage. Prednost je da ne moraju nužno biti morate idite na razne Sustavi upravljanja od početka – pod uvjetom da se upozorenja bit će poslani OMS, možete pokrenuti postoji.

```
Type=Alert
```

#### <a name="to-view-all-nagios-alerts-with-log-analytics"></a>Da biste vidjeli sva upozorenja Nagios s zapisnika Analytics
1. Na portalu paket za upravljanje operacije kliknite pločicu **Zapisnika pretraživanja** .
2. U traku upita upišite sljedeći upit za pretraživanje

    ```
    Type=Alert SourceSystem=Nagios
    ```
![Upozorenja Nagios prikazani u pretraživanju zapisnika](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Kada se prikaže rezultate pretraživanja, možete je dubinski dodatne detalje kao što su *AlertState*.

### <a name="to-view-all-zabbix-alerts-with-log-analytics"></a>Da biste vidjeli sva upozorenja Zabbix s zapisnika Analytics
1. Na portalu paket za upravljanje operacije kliknite pločicu **Zapisnika pretraživanja** .
2. U traku upita upišite sljedeći upit za pretraživanje

    ```
    Type=Alert SourceSystem=Zabbix
    ```
![Upozorenja Zabbix prikazani u pretraživanju zapisnika](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Kada se prikaže rezultate pretraživanja, možete je dubinski dodatne detalje kao što su *AlertName*.


## <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilnost s centar sustava Operations Manager

Agent za OMS za Linux agent binarne datoteke omogućio agent za sustav centar za komponentu Operations Manager. Instalacije OMS Agent za Linux u sustavu trenutno upravlja Operations Manager nadograđuje OMI i SCX paketa na računalu na noviju verziju. Kompatibilni su OMS Agent za Linux i sustava centra 2012 R2. Međutim, **sustava centra 2012 SP1 i starijim verzijama trenutno nisu kompatibilne ili podržane s OMS Agent za Linux.**

>[AZURE.NOTE] Ako OMS Agent za Linux je instaliran na računalo koje trenutno ne upravlja administrator Operations Manager, a kasnije želite upravljati računala uz pomoć Operations Manager, morate izmijeniti konfiguracije OMI prije otkrijte računala. **Ovaj korak nije potreban ako je instaliran agent za komponentu Operations Manager prije OMS Agent za Linux.**

### <a name="to-enable-the-oms-agent-for-linux-to-communicate-with-operations-manager"></a>Da biste omogućili OMS Agent za Linux možete komunicirati s Operations Manager

1. Uređivanje /etc/opt/omi/conf/omiserver.conf datoteka
2. Pobrinite se da na početak retka s **httpsport =** definira priključak 1270. Kao što su`httpsport=1270`
3. Pokrenite OMI poslužitelj:

    ```
    service omiserver restart or systemctl restart omiserver
    ```




## <a name="database-permissions-required-for-mysql-performance-counters"></a>Baza podataka potrebne dozvole za MySQL mjerača performansi

Da biste dodijelili dozvole korisnika za nadzor MySQL granting korisnik mora imati ovlasti "DODJELA mogućnost", kao i ovlasti se Odobreno.

Da bi korisnik MySQL da biste se vratili podataka o performansama korisnik će trebati pristupa sljedeći upit:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Osim tih upita u MySQL korisnika traži odaberite pristup zadani u tablicama u nastavku:

- information_schema
- MySQL

Ove ovlasti mogu se dodijeliti ponovnim pokretanjem sljedeće naredbe i dopuštanje.

```
GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-the-authentication-file"></a>Upravljanje MySQL nadzor vjerodajnice u datoteci za provjeru autentičnosti

U sljedećim se odjeljcima olakšavaju upravljanje vjerodajnicama MySQL.

### <a name="configure-the-mysql-omi-provider"></a>Konfiguriranje davatelja MySQL OMI

Davatelj MySQL OMI zahtijeva konfiguriranog MySQL korisnika i instalirali MySQL klijenta biblioteke da bi upit informacije o performansama/stanju iz MySQL instance.

### <a name="mysql-omi-authentication-file"></a>Datoteka MySQL OMI provjere autentičnosti

Davatelj MySQL OMI koristi datoteku provjere autentičnosti da biste odredili što vezanja adresu i priključak MySQL instancu priključuje na i što vjerodajnice koje će se koristiti za prikupljanje metriku. Tijekom instalacije MySQL OMI davatelja će skeniranje MySQL my.cnf konfiguracije datoteka (zadano mjesto) za vezanja adresu i priključak i djelomično postavite MySQL OMI datoteka za provjeru autentičnosti.

Da biste dovršili nadzor instance MySQL server, dodajte datoteku za provjeru autentičnosti unaprijed generirane MySQL OMI u ispravnom direktoriju.

### <a name="authentication-file-format"></a>Oblik datoteke za provjeru autentičnosti

Datoteka za provjeru autentičnosti MySQL OMI je tekstna datoteka koja sadrži informacije o:

- Priključak
- Vezanja adresa
- Korisničko ime MySQL
- Base64 kodirani lozinke

Datoteka za provjeru autentičnosti MySQL OMI samo daje ovlasti za čitanje/pisanje Linux korisnika koji je generirao ga.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Zadani MySQL OMI provjere autentičnosti datoteka sadrži zadane instance i broj priključka ovisno o tome koji se podaci iz pronađenih MySQL konfiguracijska datoteka je dostupan je i rastavljeni.

Zadane instance je znači da bi upravljanje više instanci MySQL na jedan Linux glavno računalo jednostavnije i označena instanca s priključak 0. Sve instance dodani će naslijediti svojstva postaviti zadane instance. Na primjer, ako se doda MySQL instancu slušanje na priključak '3308', vezanja adresu, korisničko ime i lozinku za Base64 kodirani zadane instance koristit će ga možete isprobati i praćenje instancu slušanje na 3308. Ako instancu na 3308 je binded na drugu adresu, a koristi iste MySQL korisničko ime i lozinku par potreban je samo respecification adresu vezanja i druga svojstva će biti nasljeđuju.

Primjeri datoteka provjere autentičnosti otprilike ovako.

Zadane instance i instanca s priključak 3308:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Zadane instance i instanca s priključak 3308 + različite osnovni 64 kodirani lozinku:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Svojstvo** | **Opis** |
| --- | --- |
| Priključak | Priključak predstavlja trenutnog priključka instancu MySQL priključuje na.  Priključak 0 podrazumijeva svojstva pratiti koriste za zadane instance. |
| Vezanja adresa | Adresa povezati je trenutno MySQL vezanja-adresa |
| korisničko ime | U ovom korisničko ime korisnika MySQL koju želite koristiti za praćenje instanca poslužitelja MySQL. |
| Base64 kodirani lozinke | To je lozinka MySQL kodirana Base64 nadzora korisnika. |
| Automatsko ažuriranje | Kad je ažuriran OMI davatelja MySQL davatelj će ponovni pregled promjena u datoteci my.cnf i prebrisati provjere autentičnosti OMI MySQL datoteku. Postavite tu oznaku na true ili false ovisno o ažuriranja koja su potrebna provjera autentičnosti datoteku MySQL OMI. |

#### <a name="authentication-file-location"></a>Mjesto datoteke za provjeru autentičnosti

Datoteka za provjeru autentičnosti MySQL OMI trebali biste koja se nalazi na sljedećem mjestu i pod nazivom "mysql autorizacija":

/VAR/Opt/Microsoft/MySQL-cimprov/AUTH/omsagent/MySQL-AUTH

Datoteka (i imenik provjere autentičnosti/omsagent) moraju biti vlasništvu omsagent korisnika.

## <a name="agent-logs"></a>Agent zapisnika

U zapisnicima OMS Agent za Linux nalazi se na:

/ var/uključivanje/microsoft/omsagent/zapisnika /

U zapisnicima OMS Agent za Linux za program omsconfig (agent konfiguracije) nalazi se na:

/ var/uključivanje/microsoft/omsconfig/zapisnika /

Zapisnici OMI i SCX komponenti (čime se omogućuje podataka o performansama metriku) nalazi se na:

/ var/uključivanje/omi/zapisnika/i /var/opt/microsoft/scx/log

## <a name="troubleshooting-the-oms-agent-for-linux"></a>Otklanjanje poteškoća s OMS agente za Linux

Poslužite se sljedećim informacijama za dijagnosticiranje i rješavanje uobičajenih problema.

Ako nijedan od informacije o otklanjanju poteškoća u ovom odjeljku pomaže vam, možete koristiti i u sljedećim resursima da biste riješili problem.

- Korisnici s vrhunska podrška može prijaviti slučaja podršku putem [Najbolji](https://premier.microsoft.com/)
- Korisnicima putem ugovore Azure podršku možete prijaviti slučajeva podršku [portal za Azure](https://manage.windowsazure.com/?getsupport=true)
- Datoteka [GitHub problem](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
- Forum za povratne informacije za ideje i da biste stvorili pogrešku izvješća [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

### <a name="important-log-locations"></a>Mjesta važne zapisnika

Datoteka | Put
---- | -----
OMS Agent za Linux zapisničke datoteke | `/var/opt/microsoft/omsagent/log/omsagent.log `
Datoteku zapisnika konfiguracije Agent za OMS | `/var/opt/microsoft/omsconfig/omsconfig.log`

### <a name="important-configuration-files"></a>Važno konfiguracijske datoteke

Catergory | Mjesto datoteke
----- | -----
Syslog | `/etc/syslog-ng/syslog-ng.conf`ili `/etc/rsyslog.conf` ili`/etc/rsyslog.d/95-omsagent.conf`
Performanse, Nagios, Zabbix, OMS izlazne i Općenito agent | `/etc/opt/microsoft/omsagent/conf/omsagent.conf`
Dodatna konfiguracija | `/etc/opt/microsoft/omsagent/conf.d/*.conf`

>[AZURE.NOTE] Uređivanje datoteka konfiguracije mjerača performansi i syslog se prebrisati ako je omogućena OMS Portal konfiguracija. Možete onemogućiti konfiguracije na portalu OMS (za sve čvorove) ili za jednu čvorove tako da pokrenete na sljedeći način:

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>Omogućite zapisivanje za ispravljanje pogrešaka

Da biste omogućili zapisivanje pogrešaka, možete koristiti dodatak za izlaz OMS i opširno izlaz.

#### <a name="oms-output-plugin"></a>Dodatak za izlaz OMS

FluentD omogućuje dodatak za određivanje razine zapisivanja za različite zapisnika razine za unose i izlaza. Da biste odredili razinu različitim zapisnika za izlaz OMS, uredite konfiguracija općih agent u na `/etc/opt/microsoft/omsagent/conf/omsagent.conf` datoteku.

Pri dnu konfiguracijskoj datoteci, promijenite u `log_level` svojstvo iz `info` da biste `debug`.

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Zapisivanje pogrešaka omogućuje vam da biste vidjeli odbacivanja prijenosi sa servisom OMS odvojene vrsta broj stavki podataka i vrijeme slanja.

*Primjer zapisnik pogrešaka omogućen:*
```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Opširno Izlaz
Umjesto korištenja dodatak za izlaz OMS, možete i poslati stavke podataka izravno na `stdout`, što je vidljivo u OMS Agent za Linux zapisničke datoteke.

U konfiguracijskoj datoteci OMS Općenito agent pri `/etc/opt/microsoft/omsagent/conf/omsagent.conf`, iz komentara na OMS izlaz dodatak dodavanjem u `#` ispred svakog retka.

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Ispod dodatak za izlaz ukloniti komentar u sljedećem odjeljku uklanjanjem na `#` simbol na početku svakog retka.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-the-log"></a>Proslijeđene poruke Syslog ne pojavljuju se u zapisniku

#### <a name="probable-causes"></a>Mogućih razloga

- Konfiguriranje primjenjuje se na poslužitelju Linux ne dopušta skup poslane funkcije i/ili razine zapisnika
- Syslog je ne prosljeđivanje pravilno s poslužiteljem Linux
- Broj poruka prosljeđivanje sekundi su prevelika za osnovnu konfiguraciju OMS agente za Linux učiniti

#### <a name="resolutions"></a>Rješenja

- Provjerite postoji li konfiguracija na portalu OMS za Syslog sve objekte i razine točan zapisnika
  - **OMS Portal > Postavke > podaci > Syslog**
-  Provjerite je li taj nativni syslog poruka daemons (`rsyslog`, `syslog-ng`) mogu primati proslijeđene poruke
- Provjerite postavke vatrozida na poslužitelju Syslog da biste bili sigurni da poruke koje se blokiraju
-  Kao zamjenu za poruke Syslog OMS pomoću na `logger` naredba – na primjer:
  - `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-to-oms-when-using-a-proxy"></a>Poteškoće s povezivanjem OMS kada koristite proxy poslužitelj

#### <a name="probable-causes"></a>Mogućih razloga

- Proxy poslužitelj naveli pri instalaciji i konfiguriranju agenta nije valjana
- Krajnje točke OMS usluga nisu whitelistested u vašem podatkovnim centrom

#### <a name="resolutions"></a>Rješenja

- Ponovno instalirajte OMS Agent za Linux pomoću sljedeće naredbe s mogućnošću `-v` omogućena. Time se omogućuje opširno izlaz agent povezujete putem proxy OMS usluga.
  - `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  - Pregledajte potražite u dokumentaciji OMS proxy pri [konfiguraciji agent za korištenje s HTTP proxy poslužiteljem](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)
- Provjerite jesu li krajnje točke za sljedeće OMS usluga whitelisted

Agent resursa | Priključci
---- | ----
& #42;. ods.opinsights.Azure.com | Priključak 443
& #42;. OMS.opinsights.Azure.com | Priključak 443
ods.systemcenteradvisor.com | Priključak 443
& #42;.blob.core.windows.net/ | Priključak 443

### <a name="a-403-error-is-displayed-when-onboarding"></a>Prikazat će se 403 pogreška prilikom za uhodavanje

#### <a name="probable-causes"></a>Mogućih razloga

- Datum i vrijeme nisu ispravni na poslužitelju Linux
- Radni prostor ID i ključa radnog prostora koji se koristi nisu ispravni

#### <a name="resolution"></a>Razlučivost

- Provjerite je li vrijeme na vašem poslužitelju Linux s na `date` naredbe. Ako podaci su veće ili manje od 15 minuta iz trenutnog vremena, zatim za uhodavanje neće uspjeti. Da biste to ispravili, ažurirati datum i/ili vremenska zona poslužitelja Linux.
- Najnoviju verziju OMS Agent za Linux obavještava vas ako vrijeme razlika uzrokuje neuspjeh za uhodavanje
- Ukloni-onboard pomoću odgovarajuće radni prostor ID i ključ radnog prostora. Dodatne informacije potražite [za Uhodavanje pomoću naredbenog retka](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .

### <a name="a-500-error-or-404-error-appears-in-the-log-file-after-onboarding"></a>500 Pogreška ili o pogrešci 404 pojavit će se u datoteci zapisnika nakon za uhodavanje

Ovo je poznat problem koji se pojavljuje tijekom prvog prijenosa Linux podataka u radni prostor programa OMS. To ne utječe na podataka koja se šalje ili drugi problemi. Možete zanemariti pogreške pri početku za uhodavanje.

### <a name="nagios-data-does-not-appear-in-the-oms-portal"></a>Nagios podataka prikazuju se na portalu OMS

#### <a name="probable-causes"></a>Mogućih razloga
- Korisnik omsagent imati dozvole za čitanje iz datoteke zapisnika Nagios
- Nagios sekcije izvora i filtar i dalje su komentar u datoteci omsagent.conf

#### <a name="resolutions"></a>Rješenja

- Dodavanje korisnika omsagent da bi se čitati iz datoteke Nagios. Dodatne informacije potražite u članku [Nagios upozorenja](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) .
- U OMS Agent za Linux Općenito konfiguracijska datoteka na `/etc/opt/microsoft/omsagent/conf/omsagent.conf`, provjerite je li taj **i** izvor Nagios i sekcije filtar su komentari uklonjeni, slično kao u sljedećem primjeru.

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-the-oms-portal"></a>Linux podataka ne prikazuje na portalu OMS

#### <a name="probable-causes"></a>Mogućih razloga

- Za Uhodavanje u servisu OMS nije uspjela
- Veza sa servisom OMS blokiran
- Agent za OMS Linux podataka je sigurnosne kopije

#### <a name="resolutions"></a>Rješenja

- Provjerite je li taj za uhodavanje u servisu OMS provjerom koji je uspješno na `/etc/opt/microsoft/omsagent/conf/omsadmin.conf` postoji.
- Ukloni-onboard pomoću omsadmin.sh naredbenog retka. Dodatne informacije potražite [za Uhodavanje pomoću naredbenog retka](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .
- Ako koristite proxy poslužitelj, koristite proxy gore navedene korake za otklanjanje poteškoća
- U nekim slučajevima kada OMS Agent za Linux ne možete komunicirati s uslugom OMS podatke na agenta se sigurnosno kopirane veličinu cijelog međuspremnik 50 MB. Ponovno pokrenite OMS Agent za Linux tako da pokrenete u bilo u `service omsagent restart` ili `systemctl restart omsagent` naredbe.
  >[AZURE.NOTE] Taj se problem ne riješi u 1.1.0-28 Agent verzije i novijim verzijama.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-the-oms-portal"></a>Syslog Linux performanse brojač konfiguracije nije primijenjena na portalu OMS

#### <a name="probable-causes"></a>Mogućih razloga

- Agent za konfiguraciju u OMS Agent za Linux sadrži dohvatiti najnovije konfiguracije na portalu OMS.
- Promijenjenih postavke na portalu bili primijenjeni

#### <a name="resolutions"></a>Rješenja

`omsconfig`agent za konfiguraciju u OMS Agent namijenjen Linux koji dohvaća promjene portala konfiguracije OMS svakih 5 minuta. Tu konfiguraciju zatim primjenjuje se na OMS Agent za Linux konfiguracijske datoteke koja se nalazi na `/etc/opt/microsoft/omsagent/conf/omsagent.conf`.

- U nekim slučajevima OMS Agent za agent za konfiguraciju Linux možda nećete moći komunicirati s uslugom Konfiguracija portala rezultira najnovije konfiguracije nije primijenjena.
- Provjerite na `omsconfig` agent se instalira uz sljedeće:
  - `dpkg --list omsconfig`ili`rpm -qi omsconfig`
  - Ako nije instaliran, ponovno instalirajte najnoviju verziju OMS Agent za Linux

- Provjerite na `omsconfig` agent mogli komunicirati sa servisom OMS
  - Pokretanje u `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` naredbe
    - Naredba iznad vraća konfiguracije tog agent dohvaća s portala sustava, uključujući postavke Syslog, mjerača performansi Linux i prilagođene zapisnika
    - Ako je naredba iznad ne uspije, pokrenite na `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` naredbe. Ta se naredba navodi omsconfig agent za komunikaciju s OMS usluga za dohvaćanje najnovijih konfiguracije.


### <a name="custom-linux-log-data-does-not-appear-in-the-oms-portal"></a>Prilagođeni podaci iz zapisnika Linux ne pojavi na portalu OMS

#### <a name="probable-causes"></a>Mogućih razloga

- Za Uhodavanje usluzi OMS nije uspjela
- Nije odabrana postavka **Primijeni sljedeće konfiguraciji Moje Linux poslužiteljima**
- omsconfig sadrži izdvojiti gore najnovije prilagođene zapisnika s portala
- Na `omsagent` korištenje ne može pristupiti prilagođene zapisnika zbog problema s dozvolama ili `omsagent` nije pronađen. U ovom slučaju, vidjet ćete sljedeći rezultat:
  - `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  - `[DATETIME] [error]: file not accessible by omsagent.`
- To je Poznati problem s trkaći uvjet koji je fiksiran OMS agente za 1.1.0-217 Linux verzija

#### <a name="resolutions"></a>Rješenja
- Provjerite koje ste uspješno onboarded, tako da odredite hoće li se `/etc/opt/microsoft/omsagent/conf/omsadmin.conf` postoji datoteka.
  - Ako je potrebno, onboard ponovno pomoću omsadmin.sh naredbenog retka. Dodatne informacije potražite [za Uhodavanje pomoću naredbenog retka](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .
- Na portalu OMS u odjeljku **Postavke** na kartici **Podaci** provjerite je li odabrana postavka **primijenite sljedeće konfiguraciju Moje Linux poslužiteljima**  
  ![Primjena konfiguracije](./media/log-analytics-linux-agents/customloglinuxenabled.png)

- Provjerite na `omsconfig` agent mogli komunicirati sa servisom OMS
  - Pokretanje u `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` naredbe
  - Naredba iznad vraća konfiguracije tog agent dohvaća s portala sustava, uključujući postavke Syslog, mjerača performansi Linux i prilagođene zapisnika
  - Ako je naredba iznad ne uspije, pokrenite na `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` naredbe. Ta se naredba nameće omsconfig agent za komunikaciju s uslugom OMS i dohvaćanje najnovijih konfiguracije.


Umjesto OMS Agent za Linux korisnika koji se izvodi kao povlaštene korisnik `root`, OMS agente za Linux izvodi kao u `omsagent` korisnika. U većini slučajeva izričite dozvole mora biti odobren korisniku da bi se čitati određene datoteke.

Da biste dodijelili dozvolu `omsagent` korisnika, pokrenite sljedeće naredbe:

1. Dodavanje u `omsagent` korisnik posebnu grupu s`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Dodjela univerzalni pristup za čitanje potrebnu datoteku s`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Postoji Poznati problem s trkaći uvjet koji je fiksiran OMS Agent za 1.1.0-217 verziju Linux. Nakon ažuriranja najnovije agent, pokrenite sljedeću naredbu da biste dobili najnoviju verziju dodatka izlaz:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/conf/omsagent.conf
```

## <a name="known-limitations"></a>Poznati ograničenja
Pregledajte u sljedećim se odjeljcima dodatne informacije o trenutnom ograničenja OMS agenta za Linux.

### <a name="azure-diagnostics"></a>Azure dijagnostiku

Za Linux virtualnim strojevima sa servisu Azure, dodatne korake možda morati omogućuje prikupljanje podataka Azure dijagnostike i paket za upravljanje operacije. Za kompatibilnost s OMS Agent za Linux potreban je **verzija 2.2** Dijagnostika proširenja za Linux.

Dodatne informacije o instaliranju i konfiguriranju dijagnostičkih proširenja za Linux potražite u članku [korištenje naredbe Azure EŽA da biste omogućili dijagnostičkih proširenje Linux](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**Proširenje Linux Dijagnostika nadogradnje sa 2.0 na 2.2 Azure EŽA ASM:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

U ovim se primjerima naredba reference datoteku pod nazivom PrivateConfig.json. Oblik datoteke treba oblik sličan sljedeći primjer.

```
    {
    "storageAccountName":"the storage account to receive data",
    "storageAccountKey":"the key of the account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog nije podržana

Rsyslog ili syslog pokazuju moraju prikupljanje syslog poruke. Zadani daemona syslog verzije 5 crveno je vaša Enterprise Linux, CentOS i Oracle Linux verzije (sysklog) nije podržana za zbirke syslog događaja. Da biste prikupili podatke syslog iz ove verzije ovih distribucija, rsyslog daemon mora biti instalacije i konfiguracije da biste zamijenili sysklog. Dodatne informacije o zamjena sysklog rsyslog, potražite u članku [Instaliranje nove ugrađeni rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti i njihovo prikupljanje podataka.
- Upoznavanje s [zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne podatke prikupljene putem rješenja.
- Da biste spremili i prikazivanje prilagođene pretraživanja pomoću [nadzornih ploča](log-analytics-dashboards.md) .
