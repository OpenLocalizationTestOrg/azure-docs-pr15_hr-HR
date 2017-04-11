<properties
 pageTitle="NAMD paket Microsoft HPC na Linux VMs | Microsoft Azure"
 description="Implementacija paket Microsoft HPC klaster na Azure i pokrenuti Simulacija NAMD s charmrun više čvorove računalnim Linux"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Mada NAMD s paket Microsoft HPC Linux računalnim čvorove u Azure

U ovom se članku prikazuje jedan od načina da biste pokrenuli radno opterećenje Linux računalstvo visokih performansi (HPC) za Azure virtualnih računala. Ovdje postavite [Paket Microsoft HPC](https://technet.microsoft.com/library/cc514029) klaster na Azure s čvorove računalnim Linux i pokrenite [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulacije da biste izračunali i vizualizacija strukturu velike biomolecular sustava.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (za program Nanoscale Molecular Dynamics) je paket paralelno molecular dynamics namijenjen simulacije visokih performansi sustava velike biomolecular koji sadrži do milijune atoms. Primjeri Ti sustavi obuhvaćaju virusa, ćeliju strukture i velike proteins. NAMD mijenja veličinu stotine jezgri za uobičajene simulations i više od 500 000 jezgri za najveću simulations.

* **Paket Microsoft HPC** nudi značajke da biste pokrenuli veliki HPC i paralelno aplikacije u klastere lokalnog računala ili Azure virtualnih računala. Izvorno razvijene kao rješenje za Windows HPC radnih opterećenja, a zatim HPC paketa sada podržava Linux HPC pokretanje aplikacija na Linux izračunati VMs čvor implementiran u programa paketa HPC klaster. Potražite u članku [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md) uvodni.

Druge mogućnosti da biste pokrenuli Linux HPC radnih opterećenja u Azure potražite u članku [računalstvo tehničke resurse za obradu i visokih performansi](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Preduvjeti

* **Paket HPC klaster s Linux izračunati čvorovi** - implementacija programa paketa HPC klaster s Linux računalnim čvorove na Azure pomoću [predloška Azure Voditelj resursa](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) ili [skriptu Azure PowerShell](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Potražite u članku [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md) preduvjeti i upute za obje mogućnosti. Ako odaberete mogućnost za implementaciju skriptu PowerShell, potražite u članku oglednu datoteku za konfiguraciju u ogledne datoteke na kraju ovog članka. Datoteka konfigurira programa klaster utemeljen na Azure HPC paket koji se sastoji od Windows Server 2012 R2 glavni čvor i četiri veličina čvorove računalnim velike CentOS 6.6. Prilagodite tu datoteku prema potrebi okruženju sustava.


* **Datoteka NAMD softver i Praktični vodič** – preuzimanje NAMD softver za Linux s [NAMD](http://www.ks.uiuc.edu/Research/namd/) web-mjesta (registracija obavezna). U ovom se članku temelji se na NAMD verzija 2.10 i koristi u arhivu [Linux x86_64 (64-bitni Intel/AMD s Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) . Preuzeti [NAMD vodiča datoteke](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Preuzimanja su .tar datoteke, a potrebno je alat za Windows da biste izdvojili datoteka na čvor glavni klaster. Da biste izdvojili datoteke, slijedite upute u nastavku ovog članka. 

* **VMD** (neobavezno) – da biste vidjeli rezultate NAMD posla, preuzmite i instalirajte program molecular vizualizacije [VMD](http://www.ks.uiuc.edu/Research/vmd/) na računalu po izboru. Trenutna verzija nije 1.9.2. Potražite u članku VMD web-mjesta da biste započeli preuzimanje.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Postavljanje međusobna pouzdanosti između računalnim čvorove
Posao unakrsno čvor sustavom više Linux čvorove zahtijeva čvorove pouzdanosti međusobno (po **rsh** ili **ssh**). Kada stvorite klaster HPC paket s implementacijsku skriptu Microsoft HPC paket IaaS, skripta automatski postavlja trajna međusobna pouzdanost za administratorski račun koji navedete. Za korisnike koji nisu administratori koje stvorite u domene na klaster, morate postaviti privremene međusobna pouzdanost među čvorove kada ih je dodijeliti zadatak. Nakon dovršetka posla, zatim uništiti odnos. Da biste to učinili za svakog korisnika, navedite RSA ključa par klaster koji koristi HPC paket da biste uspostavili odnos pouzdanosti. Slijedite upute.

### <a name="generate-an-rsa-key-pair"></a>Generiranje programa RSA ključa par
Da biste generirali programa RSA ključa par, koja sadrži javni ključ i privatni ključ tako da pokrenete naredbu **ssh-keygen** Linux jednostavno je.

1.  Prijavite se na računalo Linux.

2.  Pokrenite sljedeću naredbu:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Pritisnite **Enter** da biste koristili zadane postavke dok se ne naredbu. Unesite pristupni izraz ovdje; Kada se pojavi upit za lozinku, samo pritisnite **Enter**.

    ![Generiranje programa RSA ključa par][keygen]

3.  Promijenite direktorija u direktoriju ~/.ssh. Privatni ključ se pohranjuju u id_rsa i javni ključ u id_rsa.pub.

    ![Privatni i javni tipke][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Dodajte ključ klaster HPC paketa
1.  [Povezivanje putem udaljene radne površine](virtual-machines-windows-connect-logon.md) na glavni čvor VM koriste domenu vjerodajnice navedene kada implementiran klaster (na primjer, hpc\clusteradmin). Upravljanje klaster glavni čvorovi.

2. Standardni postupci za Windows Server možete koristiti za stvaranje korisnički račun domene u domene servisa Active Directory u klaster. Ako, na primjer, koristite alat za Active Directory korisnika i računala na glavni čvor. Primjeri u ovom se članku pretpostavlja da stvorite domene korisnika s nazivom hpcuser u domeni hpclab (hpclab\hpcuser).

3. Dodavanje korisnika domene klaster HPC paketa kao korisnik klaster. Upute potražite u članku [Dodavanje ili uklanjanje skupine korisnika](https://technet.microsoft.com/library/ff919330.aspx).

2.  Stvaranje datoteku pod nazivom C:\cred.xml i kopirajte RSA ključne podatke u nju. Primjer možete pronaći ogledne datoteke na kraju ovog članka.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Otvorite naredbeni redak i upišite sljedeću naredbu da biste postavili podataka vjerodajnice za račun hpclab\hpcuser. Koristite parametar **extendeddata** proslijedite naziv C:\cred.xml datoteku koju ste stvorili za ključne podatke.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Ta se naredba uspješno dovršava bez izlaz. Nakon što vjerodajnice za korisničke račune morate pokrenuti zadacima, spremanje cred.xml datoteka na sigurnom mjestu, ili ih izbrisati.

5.  Ako generiran par ključa RSA na jedan od vašeg čvorove Linux, imajte na umu da biste izbrisali tipki nakon što završite njihova korištenja. Paket HPC nije postavljen međusobna pouzdanost ako pronađe postojeću datoteku id_rsa ili id_rsa.pub datoteke.

>[AZURE.IMPORTANT] Ne preporučujemo sustavom Linux posla kao administrator klaster zajedničke klaster, jer je zadatak koji je poslao korisnik administrator pokreće na računu korijenski čvorove Linux. Zadatak koji je poslao korisnik koji nisu administratori s istim nazivom kao korisnik posao pokreće na lokalni Linux korisničkom računu. U ovom slučaju HPC paket postavlja međusobna pouzdanost za tog korisnika Linux preko sve čvorove dodijeliti zadatak. Možete postaviti Linux korisniku ručno čvorove Linux prije pokretanja posao ili HPC paket korisniku automatski stvara prilikom slanja posao. Ako je paket HPC stvara korisnika, paket HPC briše nakon dovršetka posla. Da biste smanjili prijetnju sigurnost, tipke uklanjaju se nakon dovršetka posla na čvorove.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Postavljanje zajedničke datoteke za čvorove Linux

Sada postaviti zajedničko korištenje datoteka programa SMB i dostupnosti zajedničku mapu na sve čvorove Linux dopustili čvorove Linux da biste pristupili datotekama NAMD s uobičajenih put. Slijede upute za postavljanje zajedničke mape na glavni čvor. Zajedničke mape preporučuje distribucija kao što su CentOS 6.6 koje trenutno ne podržava usluga Azure datoteka. Ako vaš čvorove Linux podržava Azure zajedničkom, potražite u članku [kako koristiti za pohranu datoteka Azure s Linux](../storage/storage-how-to-use-files-linux.md). Dodatne mogućnosti za zajedničko korištenje s HPC paket datoteke potražite u članku [Početak rada s Linux računalnim čvorove u programa HPC paket klaster u Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Stvorite mapu na glavni čvor, te je zajednički koristite svima postavljanjem ovlasti za čitanje/pisanje. U ovom primjeru \\ \\CentOS66HN\Namd je naziv mape, pri čemu je CentOS66HN naziv glavnog računala glavni čvora.

2. Stvorite podmapu s nazivom namd2 u zajedničkoj mapi. U namd2, stvorite drugi podmape pod nazivom namdsample.

3. Pomoću verzije sustava Windows **tar** ili drugi program sustava Windows koji radi na .tar arhive izdvojite NAMD datoteke u mapi. 
    * Izdvajanje arhive tar NAMD da biste \\ \\CentOS66HN\Namd\namd2.
    
    * Izdvajanje vodiča datoteka u odjeljku \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Otvorite prozor komponente Windows PowerShell i pokrenite sljedeće naredbe postavljanja zajedničku mapu na čvorove Linux.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Prva naredba stvara mapu pod nazivom /namd2 na sve čvorove u grupi LinuxNodes. Drugi naredba mounts //CentOS66HN/Namd/namd2 zajedničke mape premjestite u mapu s dir_mode i file_mode bitova postavite 777. *Korisničko ime* i *lozinku* u naredbi mora biti vjerodajnica korisnika na glavni čvor.

>[AZURE.NOTE]Na "\`" simbol u drugom naredba je simbol escape za PowerShell. "\`," znači "," (znak zareza) je dio naredbu.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Stvaranje tulumu skriptu da biste pokrenuli NAMD posla

Vaš posao NAMD mora datoteke *nodelist* **charmrun** da biste odredili broj čvorovi za korištenje prilikom pokretanja NAMD procesa. Koristite tulumu skripte koje generira nodelist datoteku i pokreće **charmrun** s tom datotekom nodelist. Zatim možete poslati NAMD posao u HPC klaster Manager koja poziva ovu skriptu.

U uređivaču teksta po izboru, stvoriti skriptu tulumu u /namd2 mapu koja sadrži programske datoteke NAMD i nazovite ih hpccharmrun.sh. Za brzi dokaz pojam kopirati skripte hpccharmrun.sh primjer na kraju ovog članka, a zatim idite na [Pošalji NAMD posao](#submit-a-namd-job).

>[AZURE.TIP] Spremi skriptu kao tekstnu datoteku s Linux crtom završetaka (samo LF, ne CR LF). Time se osigurava da se pokreće se ispravno čvorove Linux.

Slijede pojedinosti o čemu služi Ova skripta tulumu. 

1.  Definiranje neke varijabli.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Čvor informacije zatražite od varijable okruženja. $NODESCORES sprema popis Podjela riječi iz $CCP_NODES_CORES. $COUNT je veličina $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Oblik za $ je varijabla CCP_NODES_CORES na sljedeći način:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Ova varijabla prikazuje ukupni broj čvorove, čvor naziva i broj jezgri na svakom čvor koji je dodijeljen zadatak. Ako, na primjer, ako je zadatak potrebno 10 jezgri da biste pokrenuli, vrijednost $CCP_NODES_CORES slično je:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Ako je varijabla CCP_NODES_CORES $ nije postavljeno, izravno pokrenuti **charmrun** . (Ovo se samo događa kada pokrenete ovu skriptu izravno na vašem čvorove Linux.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Ili stvoriti datoteku nodelist za **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Pokrenite **charmrun** s datotekom nodelist, dobiti njezin status povrata i uklanjanje nodelist datoteke na kraju.

    ${CCP_NUMCPUS} je odredio čvor glavni HPC paket drugu varijablu okruženja. Sprema broj ukupni jezgri dodijeliti zadatak. Koristimo da biste naveli broj postupaka za charmrun.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Da biste napustili čiji je status povrata **charmrun** .

    ```
    exit ${RTNSTS}
    ```



Slijedi informacija u datoteci nodelist koje generira skripte:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Ako, na primjer:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Slanje NAMD posla

Sada ste spremni za slanje NAMD posla u HPC klaster Manager.

1.  Povezivanje s vašeg čvor glavni klaster i počnite HPC klaster Manager.

2.  U odjeljku **Upravljanje resursima**, računalnim čvorovi Linux provjerite jesu li u stanju **Online** . Ako nisu, odaberite ih i kliknite **Premjesti na mreži**.

2.  U odjeljku **Upravljanje**, kliknite **Novi zadatak**.

3.  Unesite naziv zadatka kao što su *hpccharmrun*.

    ![Novi zadatak HPC][namd_job]

4.  Stranice s **Detaljima o posla** , pod **Resurse posla**, odaberite željenu vrstu resursa kao **čvor** i postavite **minimalne** na 3. , ne možemo pokrenuti posla na tri Linux čvorove i svaki čvor ima četiri jezgri.

    ![Resursi za posao][job_resources]

5. Kliknite **Uređivanje zadaci** u lijevom navigacijskom oknu, a zatim **Dodaj** zadatak možete dodati na posao.    


6. Na stranici **Detalji o zadatku i preusmjeravanje/i** postaviti sljedeće vrijednosti:

    * **Naredbeni redak** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Prethodni naredbeni redak je jedan naredbe bez prijeloma retka. Se prelama na nekoliko redaka u odjeljku **naredbenog retka**.

    * **Rad direktorija** - /namd2

    * **Najmanji** - 3

    ![Detalji o zadatku][task_details]

    >[AZURE.NOTE] Postavite radni direktorij ovdje jer **charmrun** pokušava idite na isti radni direktorij na svakom čvor. Ako radni direktorij nije postavljena, HPC paket pokreće se naredba u nasumičnog naziva mape stvorene na jedan od čvorove Linux. Zbog toga sljedeće pogreške na ostale čvorove: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` da biste izbjegli taj problem, navedite put do mape koje se može pristupiti sve čvorove kao radni direktorij.

5.  Kliknite **u redu** , a zatim kliknite **Pošalji** da biste pokrenuli ovaj zadatak.

    Prema zadanim postavkama HPC paket šalje posla kao trenutni prijavljeni na korisnički račun. Dijaloški okvir bi mogao zatražiti da unesete korisničko ime i lozinku nakon što kliknete **Pošalji**.

    ![Vjerodajnice za posao][creds]

    U nekim uvjetima HPC paket pamti korisničke informacije prije za unos, a ne prikazuje taj dijaloški okvir. Da biste ponovno prikazali paket za HPC, unesite sljedeću naredbu u naredbenom retku, a zatim poslati posao.

    ```command
    hpccred delcreds
    ```

6.  Posao u svega nekoliko minuta da biste završili.

7.  Pronalaženje zapisniku posla na \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log i izlazna datoteka u \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Po želji, pokrenite VMD da biste vidjeli rezultate posao. Koraci za vizualizacija u NAMD izlaz datoteke (u ovom slučaju na ubiquitin protein molecule u veliki raster vode) su izvan raspona ovog članka. Detalje potražite u članku [Vodič NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Zadatak s rezultatima][vmd_view]

## <a name="sample-files"></a>Ogledne datoteke

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Konfiguracijska datoteka XML uzorka za implementaciju klaster pomoću skripte komponente PowerShell

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Ogledna cred.xml datoteka

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Primjer hpccharmrun.sh skripte

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
