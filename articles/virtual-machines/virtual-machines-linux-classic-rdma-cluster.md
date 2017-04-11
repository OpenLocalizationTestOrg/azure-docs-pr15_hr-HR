<properties
 pageTitle="Klaster Linux RDMA za pokretanje aplikacije MPI | Microsoft Azure"
 description="Stvaranje Linux klaster veličine H16r, H16mr, A8 ili A9 VMs koristi Azure RDMA mreže za izvođenje MPI aplikacije"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Postavljanje Linux RDMA klaster za pokretanje aplikacije MPI


Saznajte kako postaviti Linux RDMA klaster u Azure s [H niz ili računalnim ćete morati usko VMs odgovora niz](virtual-machines-linux-a8-a9-a10-a11-specs.md) za pokretanje aplikacije paralelno sučelja prosljeđivanje poruke (MPI). U ovom članku navedene korake da biste pripremili sliku Linux HPC pokrenuti Intel MPI klaster. Nakon toga implementirati klaster VMs pomoću sliku i jedan RDMA zaslonom Azure VM veličina (trenutno H16r, H16mr, A8 ili A9). Koristite klaster da biste pokrenuli MPI aplikacije koje komunikaciju učinkovito niske latencije, Visoko propusnost mreže na temelju udaljene izravan pristup memoriji tehnologije (RDMA).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Mogućnosti implementacije klaster

Slijede načini možete koristiti za stvaranje Linux RDMA klaster sa ili bez raspored poslova.


* **Azure EŽA skripte** – kao što je prikazano kasnije u ovom se članku pomoću [sučelja Azure naredbenog retka](../xplat-cli-install.md) (EŽA) skripte implementacije klaster zaslonom RDMA VMs. EŽA u načinu rada za upravljanje servisom serijskog spajanja stvara čvorove klaster u modelu uvođenje klasičnog tako da implementacija mnogo računalnim čvorove može potrajati nekoliko minuta. Da biste omogućili mrežnu vezu RDMA prilikom korištenja modela klasični implementaciju, implementirati VMs u istom servis u oblaku.

* **Voditelj resursa azure predložaka** - model implementacije Voditelj resursa možete koristiti i za implementaciju klaster zaslonom RDMA VMs koja se povezuje s mrežom RDMA. Možete [stvoriti vlastiti predložak](../resource-group-authoring-templates.md), ili provjera [predložaka Azure brzi početak rada](https://azure.microsoft.com/documentation/templates/) za predloške pridonio Microsoft ili zajednice za uvođenje rješenja želite. Voditelj resursa predloške možete omogućuje brz i pouzdan za implementaciju klaster Linux. Da biste omogućili mrežnu vezu RDMA prilikom korištenja modela implementacije Voditelj resursa, implementirati VMs unutar istog dostupnosti skupa.

* **Paket HPC** - stvorite paket Microsoft HPC klaster u Azure i dodajte zaslonom RDMA čvorove računalnim koji se izvode podržani Linux distribucije za pristup mreži RDMA. Potražite u članku [Početak rada s Linux računalnim čvorovi u klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Ogledna implementacije koraka u klasični modela

Sljedeći koraci pokazalo kako se koristi EŽA Azure implementacija VM HPC SP1 SUSE Linux Enterprise Server (SLES) 12 iz trgovine Azure, ga prilagoditi i stvoriti prilagođenu VM sliku. Nakon toga pomoću slika skripte implementacije klaster zaslonom RDMA VMs. 

>[AZURE.TIP]Korištenje slične korake za implementaciju klaster zaslonom RDMA VMs koji se temelji na druge podržane HPC slike u Azure Marketplace. Neki od sljedećih koraka malo, kao što je naznačeno mogu se razlikovati. Na primjer, Intel MPI je uključen i konfiguriran u samo neke od tih slike. A ako pokrenete VM HPC 12 SLES umjesto SLES 12 SP1 HPC VM, morate ažurirati upravljačke programe za RDMA. Detalje potražite u članku [o A8 A9, A10, te A11 računalnim ćete morati usko instance](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Preduvjeti

*   **Klijentsko računalo** - Mac, Linux i utemeljen na sustavu Windows klijentskog računala možete komunicirati s Azure. Ove korake pretpostavlja da koristite klijent Linux.

*   **Azure pretplate** – ako nemate pretplatu, možete stvoriti na [slobodno računa](https://azure.microsoft.com/free/) u samo nekoliko minuta. Za veće klastere, razmislite o pay-as-you-go pretplatu ili druge mogućnosti za kupnju. 

*   **Dostupnost veličina VM** – trenutno su sljedeće veličine instancu RDMA koji podržavaju: H16r, H16mr, A8 i A9. [Proizvodi koje su dostupne po regijama](https://azure.microsoft.com/regions/services/) potražiti dostupnost u Azure regijama. 

*   **Kvota jezgri** – možda ćete morati povećati kvote jezgri za implementaciju klaster računalnim ćete morati usko VMs. Ako, na primjer, potrebno je barem 128 jezgri ako želite uvesti 8 A9 VMs, kao što je prikazano u ovom članku. Vaša pretplata možda i ograničavanje broja jezgri možete implementirati u linije veličina određene VM, uključujući H niz. Da biste zatražili ograničenja povećali, [otvorite zahtjev za mrežna Služba podrške](../azure-supportability/how-to-create-azure-support-request.md) bez troškova. 

*   **Azure EŽA** - [Instalacija](../xplat-cli-install.md) EŽA Azure i [povezati pretplate Azure](../xplat-cli-connect.md) s klijentskog računala.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Korak 1. Dodjela HPC VM 12 SP1 za SLES

Nakon prijave u Azure s EŽA Azure, pokrenite `azure config list` da biste potvrdili da izlaz prikazuje način rada za upravljanje servisom. Ako nije, postavite u način tako da pokrenete sljedeću naredbu:


    azure config mode asm


Upišite sljedeće da biste dobili popis svih pretplata imaju dozvolu za korištenje:


    azure account list

Trenutno aktivna pretplata povezuje se s `Current` postavljena na `true`. Ako ove pretplate nije onaj koji želite koristiti za stvaranje klaster, postavite odgovarajuće pretplate Id kao aktivnu pretplatu:

    azure account set <subscription-Id>

Da biste vidjeli javno dostupna SLES 12 SP1 HPC slike u Azure, pokrenite naredbu koja je slična sljedećoj, uz pretpostavku da vaše ljuske okruženje podržava **grep**:


    azure vm image list | grep "suse.*hpc"

Sada Dodjela zaslonom RDMA VM s SLES 12 SP1 HPC slike tako da pokrenete naredbu sličnu ovoj:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

gdje

* Veličinu (A9 u ovom primjeru) jedan je od VM RDMA zaslonom veličine.

* Vanjski SSH broj priključka (22 u ovom primjeru, što je zadana postavka SSH) je bilo koji valjani broj priključka. Interni broj priključka SSH postavljen je na 22.

* U području Azure određen mjesto stvara se novi servis u oblaku. Navedite mjesto u kojoj je dostupan veličina VM koje odaberete.

* Naziv slike SLES 12 SP1 trenutno može biti `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` ili `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` SUSE prioritet podršku (Dodatne naplaćuje).

### <a name="step-2-customize-the-vm"></a>Korak 2. Prilagodba u VM

Nakon što u VM dovršava dodjele resursa SSH da biste VM pomoću na VM vanjske IP adrese (ili naziv DNS-a) i vanjski priključak broj koji je konfiguriran i prilagoditi ga. Veza detalje potražite u članku [upute za prijavu radi Linux virtualnog računala](virtual-machines-linux-mac-create-ssh-keys.md). Izvođenje naredbe kao korisnik ste konfigurirali VM, osim ako pristup korijenski potreban je da biste dovršili koraka.

>[AZURE.IMPORTANT]Microsoft Azure ne nudi pristup korijenski Linux VMs. Da biste pristupili administratora nakon uspostave veze kao korisnik s VM, pokrenite naredbi pomoću `sudo`.

* **Ažuriranja** – instaliraj ažuriranja pomoću **zypper**. Možda želite instalirati NFS uslužni programi. 

    >[AZURE.IMPORTANT]U na SLES 12 SP1 HPC VM, preporučujemo da ne primijenite otklanjanje ažuriranja, koje mogu uzrokovati probleme s Linux RDMA upravljačke programe.

* **Intel MPI** - dovršiti instalaciju Intel MPI na SLES 12 VM za HPC SP1 pokretanjem sljedeće naredbe:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Zaključavanje memorije** – kodovi za MPI zaključajte slobodne memorije za RDMA, dodajte ili promijenite sljedeće postavke u datoteci /etc/security/limits.conf. (Morate korijenski pristup da biste uredili tu datoteku.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Za testiranje, možete postaviti i memlock da neograničeno. Na primjer: `<User or group name>    hard    memlock unlimited`. Dodatne informacije potražite u članku [Poznati najbolje načine za postavku zaključan memorije](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **Tipke SSH za SLES VMs** - generiranje SSH tipke da biste odredili pouzdanost za korisnički račun među čvorove računalnim u klaster SLES kada se pokrene MPI poslove. (Ako implementiran VM HPC za utemeljen na CentOS, nemojte slijediti ovaj korak. Pročitajte upute u nastavku članka da biste postavili passwordless SSH pouzdanost među čvorove klaster kada je snimiti i implementacija klaster.) 

    Pokrenite sljedeću naredbu da biste stvorili ključeve SSH. Kada se od vas zatraži unos, pritisnite tipku Enter da biste generirali tipki na zadano mjesto bez postavljanja pristupni izraz.

        ssh-keygen

    Javni ključ dodati datoteku authorized_keys poznati javni ključ.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    U direktoriju ~/.ssh, uređivanje ili stvaranje datoteke "config". Navedite rasponu IP adresa privatna mreža za koju namjeravate koristiti u Azure (10.32.0.0/16 u ovom primjeru):

        host 10.32.0.*
        StrictHostKeyChecking no

    Osim toga, popis privatna mreža IP adrese svaki VM u svoj klaster na sljedeći način:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfiguriranje `StrictHostKeyChecking no` možete stvoriti ugroziti sigurnost prilikom određene IP adrese ili raspona nije naveden.

* **Aplikacija** – instalirati sve programe na ovom VM ili izvedite druge prilagodbe prije snimili sliku.

### <a name="step-3-capture-the-image"></a>Korak 3. Snimite sliku

Da biste snimili sliku, najprije pokrenite sljedeću naredbu u Linux VM. Ta naredba deprovisions na VM, ali zadržava korisničke račune i SSH ključeva koji ste postavili.

```
sudo waagent -deprovision
```

Zatim od klijentskog računala, pokrenite sljedeće naredbe Azure EŽA da biste snimili sliku. Detalje potražite u članku [kako snimiti klasični virtualnog računala Linux kao sliku](virtual-machines-linux-classic-capture-image.md) .  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nakon pokretanja te naredbe slika VM zabilježene je za korištenje, a na VM se briše. Sada imate prilagođenu sliku jeste li spremni za implementaciju klaster.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Korak 4. Implementacija klaster sa slikom

Izmjena sljedeću skriptu tulumu s odgovarajućim vrijednostima okruženju sustava i pokretanje s klijentskom računalu. Jer Azure uvodi VMs serijskog spajanja u modelu klasični implementaciju, svega nekoliko minuta za implementaciju 8 VMs A9 predložena u ovu skriptu.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Zahtjevi za CentOS HPC klaster

Ako želite da biste postavili klaster na temelju jednog utemeljen na CentOS HPC slike iz trgovine Azure umjesto SLES 12 za HPC, slijedite općenite korake u prethodnom odjeljku. Prilikom dodjele resursa i konfiguriranje na VM Imajte na umu sljedeće razlike:

1. Intel MPI već instaliran na VM dodjeli sa sustavom CentOS HPC slike. 

2. Postavke zaključavanja memorije već dodaju se u datoteci /etc/security/limits.conf na VM.

2. Generiranje ključeva SSH na VM vam Dodjela resursa za snimanje. Umjesto toga preporučujemo postavljanja provjere autentičnosti utemeljene na korisnik nakon implementacije u cluser. U odjeljku sljedeće.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Postavljanje passwordless pouzdanost SSH klaster

Na koji se temelji na CentOS klasteru HPC-a, postoje dva načina za uspostavljanje pouzdanosti između čvorove računalnim: provjere autentičnosti utemeljene na glavno računalo i provjeru autentičnosti utemeljenu na korisnika. Provjere autentičnosti utemeljene na glavno računalo je izvan raspona ovog članka i obično mora poduzeti putem skripte proširenje tijekom implementacije. Provjere autentičnosti utemeljene na korisnički prikladno za uspostavljanje pouzdanost nakon implementacije i zahtijeva za generiranje i zajedničko korištenje ključeva SSH među čvorove računalnim u klasteru. Ovaj postupak passwordless SSH prijavi se obično naziva i potreban je kada se pokrene MPI zadatke. 

Ogledne skripte pridonio iz zajednice dostupna je na [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) da biste omogućili provjere autentičnosti jednostavno korisnika na klasteru HPC utemeljen na CentOS-a. Preuzmite i pomoću sljedeće skripte na sljedeći način. Možete izmijeniti ovu skriptu ili koristiti neki drugi način da biste uspostavili passwordless SSH provjera autentičnosti između čvorove računalni klaster.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Da biste pokrenuli skriptu, morate znati prefiks za IP adrese podmreže. Ponovnim pokretanjem sljedeće naredbe u jednoj aplikaciji sustava čvorove Klaster se prefiks. Izlaz bi morao izgledati slično 10.1.3.5 i prefiks je na 10.1.3 dijela.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Sada pokrenuti skriptu pomoću tri parametara: uobičajene korisničko ime na čvorove računalnim, uobičajeni lozinku za tog korisnika na čvorove računalnim i predbroj koji je vratio prethodne naredbe.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Ova skripta čini sljedeće:

* Stvara direktorij pod nazivom .ssh, na koje je potrebno za prijavu na passwordless čvor glavnog računala. 
* Stvara se datoteka konfiguracije u direktoriju .ssh koji upućuje passwordless prijava da biste omogućili prijava svi čvorovi u klasteru. 
* Stvara datoteka koja sadrži nazive čvor i čvor IP adresa za sve čvorove u klasteru. Te se datoteke ostaju nakon pokretanja skripta za kasnije. 
* Stvara privatni i javni ključ par za svaki čvor klaster uključujući čvor glavnog računala i stvara unose u datoteci authorized_keys.

>[AZURE.WARNING]Izvršavanje ove skripte možete stvoriti ugroziti sigurnost. Provjerite je li javno važne informacije u ~/.ssh ne distribuirati.


## <a name="configure-intel-mpi"></a>Konfiguriranje Intel MPI

Da biste pokrenuli MPI aplikacije na Azure Linux RDMA, morate konfigurirati određene specifične za Intel MPI varijable okruženja. Evo ogledne skripte tulumu da biste konfigurirali varijable potrebno za pokretanje programa. Promijenite put mpivars.sh kao što je potrebno za instalaciju programa Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Oblik datoteke glavno računalo je u nastavku. Jedan redak za svaki čvor dodati u svoj klaster. Navedite privatne IP adrese iz na VNet definirani ranije, ne DNS naziva. Na primjer, za dva domaćini s IP adresama 10.32.0.1 i 10.32.0.2 datoteka sadrži sljedeće:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Pokrenite MPI na osnovni dva čvor klaster

Ako to još niste učinili, najprije postavite okruženje za Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Jednostavan MPI Naredba Pokreni

Izvođenje jednostavni MPI naredbe na jedan od čvorove računalnim da biste prikazali MPI pravilno instaliran, a možete komunikaciju između barem dva izračunati čvorove. Sljedeća naredba **mpirun** izvodi naredbu **naziv glavnog računala** na dva čvorove.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Izlaz treba popis imena svih čvorove proslijeđena kao unos za `-hosts`. Ako, na primjer, i **mpirun** naredbi s dva čvorove vraća izlaz sličnu ovoj:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Pokretanje programa usporednih MPI

Sljedeća naredba Intel MPI pokreće usporednih pingpong da biste provjerili klaster konfiguracija i povezivanje s mrežom RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Na klasteru rad s dva čvorove, trebali biste vidjeti izlaz otprilike ovako. Na mreži Azure RDMA očekivati Latencija na ili ispod 3 microseconds poruke veličine do 512 bajtova.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Daljnji koraci

* Pokušajte implementacija i pokretanje aplikacija na Linux MPI na svoj klaster Linux.

* Potražite u [dokumentaciji Intel MPI biblioteke](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) upute o Intel MPI.

* Da biste stvorili programa Intel Lustre klaster pomoću utemeljenu na CentOS HPC sliku, pokušajte [brzi početak rada predloška](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) . Dodatne informacije pročitajte sljedeću [objavu na blogu](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
