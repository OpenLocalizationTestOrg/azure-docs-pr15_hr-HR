<properties
    pageTitle="Rješenje spremnika u prijava analitiku | Microsoft Azure"
    description="Rješenja spremnika u prijava analitiku pomaže vam prikaz i upravljanje njima na Docker spremnik glavnog računala na jednom mjestu."
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



# <a name="containers-preview-solution-log-analytics"></a>Rješenje spremnika (pretpregled) zapisnika Analytics

U ovom se članku opisuje postavljanje i korištenje rješenje spremnika u zapisnik analize koji omogućuje prikaz i upravljanje njima na Docker spremnik glavnog računala na jednom mjestu. Docker je sustav virtualizacije softver za stvaranje spremnika koji automatizirati implementaciju softvera za svoju IT infrastrukturu.

Uz rješenje, vidjet ćete spremnike su pokrenuti na vašem domaćini spremnik te što slike se izvodi u spremnike. Možete pogledati detaljne informacije o nadzoru koji prikazuje naredbi koje se koriste s spremnika. Možete i otklonite spremnika prikaz i pretraživanje središnje zapisnika bez potrebe za daljinsko prikaz Docker domaćini. Možete pronaći spremnika koji je možda oscilirajuće i dosta suvišno resursima na glavnom računalu. A možete pogledati središnje procesora, memorije, za pohranu i informacije o korištenju i performanse mreže za spremnika.

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja

Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

Dodavanje spremnika rješenje OMS radnog prostora koristeći postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).

Za instalaciju i korištenje Docker s OMS na dva načina:

- Podržani Linux operacijskim sustavima, instalirajte i pokrenite Docker i zatim instalirate i konfigurirate OMS Agent za Linux
- Na CoreOS, instalacija i pokretanje Docker i konfiguriranje OMSAgent da biste pokrenuli u spremniku

Pregledajte podržana verzija operacijskog sustava Docker i Linux za spremnik glavnog računala na [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Docker mora biti izvodi **prije** instalacije [OMS Agent za Linux](log-analytics-linux-agents.md) na vaše spremnik domaćini. Ako ste već instalirali agenta prije instalacije Docker, morat ćete ponovno instalirajte OMS Agent za Linux. Dodatne informacije o Docker potražite na [web-mjesto Docker](https://www.docker.com).

Potreban vam je prije moguće nadzirati spremnika konfigurirati na vašem domaćini spremnik sljedeće postavke.

## <a name="configure-settings-for-the-linux-container-host"></a>Konfiguriranje postavki za Linux spremnik glavno računalo

Nakon što ste instalirali Docker, koristite sljedeće postavke vašeg spremnik glavno računalo da biste konfigurirali agent za korištenje Docker. CoreOS ne podržava ovu metodu konfiguracije.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Konfiguriranje postavki za glavno računalo spremnik - systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x i Ubuntu 15.x i noviji)

1. Uređivanje docker.service da biste dodali sljedeće:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Dodavanje $DOCKER\_ODABERE u &quot;ExecStart = / korsnik pitanje/smeće/docker daemon&quot; u datoteci docker.service. Korištenje u sljedećem primjeru.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Ponovno pokrenite servis Docker. Ako, na primjer:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Konfiguriranje postavki za glavno računalo spremnik - Upstart (Ubuntu 14.x)

1. Uređivanje /etc/default/docker i dodajte sljedeće:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Spremite datoteku, a zatim ponovo Docker i OMS usluga.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Konfiguriranje postavki za glavno računalo spremnik - Amazon Linux

1. Uređivanje /etc/sysconfig/docker i dodajte sljedeće:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Spremite datoteku, a zatim ponovno pokrenite servis Docker.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Konfiguriranje postavki za CoreOS spremnika

Nakon što ste instalirali Docker, koristite sljedeće postavke CoreOS da biste pokrenuli Docker i stvaranje spremnika. Možete koristiti bilo koji podržanu verziju programa Linux – uključujući CoreOS, koristite taj način konfiguracije. Morat ćete svoje [OMS radni prostor ID i ključ](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>Da biste koristili OMS za sve spremnike s CoreOS

- Pokrenite spremnik OMS koje želite nadzirati. Izmijenite i koristiti u sljedećem primjeru.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Prelazak s korištenjem instaliran agent jednom u spremniku

Ako već koristi agent izravno instaliranu i želite umjesto toga koristite agent izvodi u spremniku, prvo morate ukloniti OMSAgent. Pogledajte [korake da biste instalirali OMS Agent za Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Spremnika pojedinosti za zbirke podataka

Rješenje spremnika prikuplja različite metriku i prijaviti se podataka o performansama iz spremnik domaćini i spremnika pomoću agenata OMS Linux koje ste omogućili i OMSAgent koji se izvodi u spremnika.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za spremnika.

| platforme | OMS Agent za Linux | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Linux|![Da](./media/log-analytics-containers/oms-bullet-green.png)|![ne](./media/log-analytics-containers/oms-bullet-red.png)|![ne](./media/log-analytics-containers/oms-bullet-red.png)|            ![ne](./media/log-analytics-containers/oms-bullet-red.png)|![ne](./media/log-analytics-containers/oms-bullet-red.png)| svake 3 minute|


U sljedećoj su tablici prikazuju Primjeri vrste podataka koji se prikupljaju spremnika rješenje:

| Vrsta podataka | Polja |
| --- | --- |
| Performanse za domaćini i spremnika | Računalo, ObjectName, CounterName i #40; % procesor vrijeme na disku čita MB, Disk piše MB, MB korištenje memorije, mreže primi bajtova, mreže Pošalji bajta, procesor sec korištenje mreže i #41; CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Spremnik zaliha | TimeGenerated, računalo, a zatim naziv spremnika ContainerHostname slike, ImageTag, ContinerState, ExitCode, EnvironmentVar, naredba, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Spremnik slika zaliha | TimeGenerated, računalo, slike, ImageTag, ImageSize, VirtualSize, kada je pokrenut, kojima je zaustavljena, zaustavite, nije uspjela, SourceSystem, ImageID, TotalContainer |
| Spremnik zapisnika | TimeGenerated, računalo, ID slike, naziv spremnika LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Spremnik servisa zapisnika | TimeGenerated, računalo, TimeOfCommand, slike, naredba, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Spremnici monitora

Kada je omogućena na portalu OMS rješenje, vidjet ćete **spremnika** pločica prikazuje sažetak informacija o vašem domaćini spremnik i spremnika u domaćini.

![Spremnika pločica](./media/log-analytics-containers/containers-title.png)

Pločice prikazuje pregled koliko spremnika imate u okruženje i hoće li se uspjela, pokrenut ili zaustavljen.

### <a name="using-the-containers-dashboard"></a>Korištenje na nadzornoj ploči spremnike

Kliknite pločicu **spremnika** . Iz nje će se prikazivati prikazi razvrstan po:

- Spremnik događaja
- Pogreške
- Status spremnika
- Spremnik slika zaliha
- Performanse procesora i memorije

Svakog okna na nadzornoj ploči je vizualni prikaz pretraživanja koji se izvodi na prikupljene podatke.

![Nadzornoj ploči spremnike](./media/log-analytics-containers/containers-dash01.png)

![Nadzornoj ploči spremnike](./media/log-analytics-containers/containers-dash02.png)

Na plohu **Spremnik Status** kliknite područje na vrhu, kao što je prikazano u nastavku.

![Status spremnika](./media/log-analytics-containers/containers-status.png)

Otvorit će se zapisnika pretraživanja prikazuje informacije o domaćini i spremnika koji se izvodi u njima.

![Traženje spremnika u zapisnik](./media/log-analytics-containers/containers-log-search.png)

Na tom mjestu možete urediti u upit za pretraživanje da biste ga izmijeniti da biste pronašli konkretne podatke koji vas zanima. Dodatne informacije o zapisniku pretraživanja potražite u članku [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md).

Na primjer, upit za pretraživanje možete izmijeniti tako da se prikazuje prestao spremnika umjesto izvodi spremnika tako da promijenite **izvodi** **prekine** u upit za pretraživanje.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Rješavanje problema s pronalaženjem spremniku nije uspio

OMS spremnik označava kao **nije uspjelo** ako je napusti kod izlaz od nule. Vidjet ćete pregled pogreške i pogreške u okruženju u plohu **Spremnika nije uspjelo** .

### <a name="to-find-failed-containers"></a>Da biste pronašli nije uspjela spremnika

1. Kliknite plohu **Spremnik događaja** .  
  ![spremnici događaja](./media/log-analytics-containers/containers-events.png)
2. Otvorit će se zapisnika pretraživanja Prikaz statusa spremnika, otprilike ovako.  
  ![Stanje spremnika](./media/log-analytics-containers/containers-container-state.png)
3. Nakon toga kliknite vrijednost nije uspjelo da biste pogledali dodatne informacije, kao što su veličina slike i broj slika Zaustavi, a nije uspjelo. Proširite **Prikaži više** da biste pogledali slika ID.  
  ![nije uspjelo spremnika](./media/log-analytics-containers/containers-state-failed.png)
4. Nakon toga pronađite spremniku koji se izvodi na ovoj se slici. U upit za pretraživanje upišite sljedeće.
  `Type=ContainerInventory <ImageID>`Prikazat će se zapisnike. Možete se pomicati da biste vidjeli spremnik nije uspjelo.  
  ![nije uspjelo spremnika](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Pretraživanje zapisnika spremnik podataka

Kada ste rješavanje konkretne pogreške potražite može pomoći da biste vidjeli gdje se pojavljuje u svom okruženju. Sljedeće vrste zapisnika će vam olakšati stvaranje upita da biste se vratili podatke koje želite.

- **ContainerInventory** – korištenje ove vrste kada želite da se informacije o kontejneru mjesto, koje su njihova imena i što slike ih koristite.
- **ContainerImageInventory** – korištenje ove vrste kada pokušavate pronaći informacije organizirati tako da sliku te prikaz slika podatke kao što su slike ID-a ili veličine.
- **ContainerLog** – koristite kada želite da biste pronašli informacije iz zapisnika pogrešci i stavke te vrste.
- **ContainerServiceLog** – korištenje ove vrste kada pokušavate pronaći informacije trag nadzora za daemon Docker, kao što su početka, tabulatora, brisanje ili povlačite naredbe.

### <a name="to-search-logs-for-container-data"></a>Da biste pronašli zapisnicima spremnik podataka

- Odaberite sliku koja znate nije uspjela nedavno i pronaći evidencije pogrešaka za njega. Najprije pronalaženje naziv spremnika sa sustavom tu sliku pomoću **ContainerInventory** pretraživanja. Na primjer, pretraživanje`Type=ContainerInventory ubuntu Failed`  
    ![Traženje Ubuntu spremnika](./media/log-analytics-containers/search-ubuntu.png)

  Imajte na umu naziv spremnika pokraj **naziva**i potražite te zapisnika. U ovom se primjeru je `Type=ContainerLog adoring_meitner`.

**Prikaz informacija o performansama**

Kada počinjete za sastavljanje upita, može pomoći da biste vidjeli što je moguće prvi put. Ako, na primjer, da biste vidjeli sve podataka o performansama, pokušajte općenite upita tako da upišete sljedeći upit za pretraživanje.

```
Type=Perf
```

![performanse spremnika](./media/log-analytics-containers/containers-perf01.png)



Vidjet ćete ovo više grafički obrascu kada kliknete word **metriku** u rezultatima.

![performanse spremnika](./media/log-analytics-containers/containers-perf02.png)



Možete ograničiti na podataka o performansama vidite određene spremniku tako da upišete naziv je s desne strane upit.

```
Type=Perf <containerName>
```

Koji se prikazuje na popisu performanse metrike koji se prikupljaju za pojedinačne kontejner.

![performanse spremnika](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Primjer upita za pretraživanje zapisnika

Često je korisno za sastavljanje upita počevši od primjera ili dva i izmjena ih tako da stane vaše okruženje. Kao početnu točku, možete isprobati plohu **Najvažnije upita** popunite naprednije upite.

![Spremnici upita](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Spremanje zapisnika upita za pretraživanje

Spremanje upita je standardni značajci zapisnika analize. Tako da ih spremite, imat ćete one koje pronađete koristan pri ruci za buduću upotrebu.

Kada stvorite upit koji vam korisni, spremite ga tako da kliknete **Favoriti** pri vrhu stranice zapisnika pretraživanja. Zatim možete jednostavno pristupiti ga kasnije na stranici **Moje nadzorne ploče** .

## <a name="next-steps"></a>Daljnji koraci

- [Pretraživanje zapisnika](log-analytics-log-searches.md) radi prikaza spremnik detaljne podatke zapisa.
