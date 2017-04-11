<properties
 pageTitle="Mada OpenFOAM paketom HPC Linux VMs | Microsoft Azure"
 description="Uvođenje paket Microsoft HPC klaster na Azure i Pokreni programa OpenFOAM na više čvorove računalnim Linux putem programa RDMA mreže."
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
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Mada OpenFoam s paket Microsoft HPC Linux RDMA klaster servisu Azure

U ovom se članku prikazuje jedan od načina da biste pokrenuli OpenFoam u Azure virtualnih računala. Ovdje, morate je implementirati paket Microsoft HPC klaster s Linux računalnim čvorove na Azure i pokrenite [OpenFoam](http://openfoam.com/) zadatak s Intel MPI. Možete koristiti zaslonom RDMA Azure VMs za čvorove računalnim čvorove računalnim razmjene podataka putem mreže Azure RDMA. Druge mogućnosti da biste pokrenuli OpenFoam u Azure obuhvaćaju potpuno konfiguriran komercijalnog slike dostupne na tržištu, kao što je UberCloud na [OpenFoam 2.3 na CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)i tako da pokrenete na [Obradu Azure](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (za otvaranje operacija polja i rukovanje) je paket softver Otvori izvor računalne tekuća dynamics (CFD) koja se koristi velikom broju korisnika u inženjerska i Znanstvena u komercijalne i akademska tvrtkama ili ustanovama. To obuhvaća alate za meshing čest snappyHexMesh, parallelized mesher složene geometries umetanja i prije i poslije obradi. Gotovo svi procesi pokrenuti paralelno, korisnicima u potpunosti iskoristiti računalnog hardvera na njihove rashoda.  

Paket Microsoft HPC nudi značajke za pokretanje veliki HPC i paralelno aplikacije, uključujući MPI aplikacije, na klastere virtualnim računalima sustava Microsoft Azure. Paket HPC podržava i pokrenuti Linux HPC čvor VMs implementiran na klasteru HPC paket za izračun aplikacije na Linux. Potražite u članku [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md) za Uvod u korištenje Linux računalnim čvorove HPC paketom.

>[AZURE.NOTE] U ovom se članku prikazuje kako pokrenuti Linux MPI radno opterećenje s HPC paket. Pretpostavlja se da su neke poznavanje s administracijom sustava Linux i sustavom radnih opterećenja MPI klastere Linux. Ako koristite verzije MPI i OpenFOAM drugačije od onih koji se prikazuju u ovom članku, možda ćete morati izmijeniti nekoliko koraka instalacije i konfiguracije. 

## <a name="prerequisites"></a>Preduvjeti

*   **Paket HPC klaster sa zaslonom RDMA Linux izračunati čvorove** - implementacija programa paketa HPC klaster s veličina A8, A9, H16r ili H16rm Linux računalnim čvorove pomoću [predloška Azure Voditelj resursa](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) ili [skriptu Azure PowerShell](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Potražite u članku [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md) preduvjeti i upute za obje mogućnosti. Ako odaberete mogućnost za implementaciju skriptu PowerShell, potražite u članku oglednu datoteku za konfiguraciju u ogledne datoteke na kraju ovog članka. Koristite tu konfiguraciju za implementaciju sustava klaster utemeljen na Azure HPC paket koji se sastoji od veličina glavni čvor A8 Windows Server 2012 R2 i 2 veličina čvorove računalnim A8 SUSE Linux Enterprise Server 12. Zamijenite odgovarajuće vrijednosti za naziva pretplate i usluga. 

    **Dodatne stvari koje morate znati**

    *   Linux RDMA umrežavanje preduvjeti u Azure potražite u članku [o H niza i računalnim ćete morati usko VMs odgovora niz](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Ako koristite mogućnost za implementaciju skriptu Powershell implementirati sve čvorove računalnim Linux unutar jedan u oblaku da biste koristili RDMA mrežnu vezu.

    *   Nakon implementacije čvorove Linux se povezati s SSH za obavljanje dodatne administratorskih zadataka. Pronađite pojedinosti SSH veze za svaki VM Linux na portalu za Azure.  
        
*   **Intel MPI** - pokrenuti OpenFOAM SLES 12 HPC izračunati čvorove u Azure, morate instalirati runtime Intel MPI biblioteke 5 sa [Intel.com web-mjesta](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 predinstaliran je na slikama utemeljenu na CentOS HPC.)  Ako je potrebno, u noviji koraka, instalirajte Intel MPI vaše čvorove računalnim Linux. Priprema za ovaj korak kada registrirate Intel, slijedite vezu u e-pošte za potvrdu povezanih web-stranicu. Zatim kopirajte vezu preuzimanje datoteke .tgz za odgovarajuću verziju Intel MPI. U ovom se članku temelji se na Intel MPI verzija 5.0.3.048.

*   **Paket izvora OpenFOAM** - preuzimanje softvera OpenFOAM izvora paket za Linux s [OpenFOAM Foundation web-mjesta](http://openfoam.org/download/2-3-1-source/). U ovom se članku se temelji na izvoru paket 2.3.1, dostupnih za preuzimanje kao OpenFOAM 2.3.1.tgz. Slijedite upute u nastavku ovog članka otpakiravanje i objedinjavanje OpenFOAM na računalnim čvorove Linux.

*   **EnSight** (neobavezno) – da biste vidjeli rezultate simulaciju OpenFOAM, preuzmite i instalirajte [EnSight](https://www.ceisoftware.com/download/) vizualizaciju i analize program. Informacije o licenciranju i preuzimanje su na web-mjestu EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Postavljanje međusobna pouzdanosti između računalnim čvorove

Posao unakrsno čvor sustavom više Linux čvorove zahtijeva čvorove pouzdanosti međusobno (po **rsh** ili **ssh**). Kada stvorite klaster HPC paket s implementacijsku skriptu Microsoft HPC paket IaaS, skripta automatski postavlja trajna međusobna pouzdanost za administratorski račun koji navedete. Za korisnike koji nisu administratori koje stvorite u na klaster domene, morate postaviti privremene međusobna pouzdanost među čvorove kada ih je dodijeliti zadatak, a uništiti odnosa nakon dovršetka posla. Da biste uspostavili pouzdanost za svakog korisnika, navedite RSA ključa par klaster koji koristi HPC paket za odnos pouzdanosti.

### <a name="generate-an-rsa-key-pair"></a>Generiranje programa RSA ključa par

Da biste generirali programa RSA ključa par, koja sadrži javni ključ i privatni ključ tako da pokrenete naredbu **ssh-keygen** Linux jednostavno je.

1.  Prijavite se na računalo Linux.

2.  Pokrenite sljedeću naredbu:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Pritisnite **Enter** da biste koristili zadane postavke dok se ne naredbu. Unesite pristupni izraz ovdje; Kada se pojavi upit za lozinku, samo pritisnite **Enter**.

    ![Generiranje programa RSA ključa par][keygen]

3.  Promijenite direktorija u direktoriju ~/.ssh. Privatni ključ se pohranjuju u id_rsa i javni ključ u id_rsa.pub.

    ![Privatni i javni tipke][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Dodajte ključ HPC paket klaster
1.  Provjerite vezu udaljene radne površine na glavni čvor s računom za administratora HPC paket (administratorski račun postavljanja kada ste pokrenuli implementacijsku skriptu).

2. Standardni postupci za Windows Server možete koristiti za stvaranje korisnički račun domene u domene servisa Active Directory u klaster. Ako, na primjer, koristite alat za Active Directory korisnika i računala na glavni čvor. Primjeri u ovom se članku pretpostavlja da stvorite domene korisnika s nazivom hpclab\hpcuser.

3.  Stvaranje datoteku pod nazivom C:\cred.xml i kopirajte RSA ključne podatke u nju. Ogledna datoteka cred.xml nalazi se na kraju ovog članka.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Otvorite naredbeni redak i upišite sljedeću naredbu da biste postavili podataka vjerodajnice za račun hpclab\hpcuser. Koristite parametar **extendeddata** proslijedite naziv C:\cred.xml datoteku koju ste stvorili za ključne podatke.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Ta se naredba uspješno dovršava bez izlaz. Nakon što vjerodajnice za korisničke račune morate pokrenuti zadacima, spremanje cred.xml datoteka na sigurnom mjestu, ili ih izbrisati.

5.  Ako generiran par ključa RSA na jedan od vašeg čvorove Linux, imajte na umu da biste izbrisali tipki nakon što završite njihova korištenja. Ako paket HPC pronađe postojeću datoteku id_rsa ili id_rsa.pub datoteke, on nije postavljen međusobna pouzdanost.

>[AZURE.IMPORTANT] Ne preporučujemo sustavom Linux posla kao administrator klaster zajedničke klaster, jer je zadatak koji je poslao korisnik administrator pokreće na računu korijenski čvorove Linux. Međutim, zadatak koji je poslao korisnik koji nisu administratori pokreće na lokalni Linux korisničkom računu s istim nazivom kao korisnik posao. U ovom slučaju HPC paket postavlja međusobna pouzdanost za tog korisnika Linux preko čvorove dodijeliti zadatak. Možete postaviti Linux korisniku ručno čvorove Linux prije pokretanja posao ili HPC paket korisniku automatski stvara prilikom slanja posao. Ako je paket HPC stvara korisnika, paket HPC briše nakon dovršetka posla. Da biste smanjili sigurnosnih prijetnji, paket HPC uklanja tipki nakon dovršetka posla.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Postavljanje zajedničke datoteke za čvorove Linux

Sada postaviti standardne SMB zajedničko korištenje za mapu na glavni čvor. Da biste omogućili čvorove Linux da biste pristupili datotekama aplikacije s uobičajenih put, dostupnosti zajedničku mapu na čvorove Linux. Ako želite, možete koristiti drugu mogućnost, primjerice Azure datoteke zajednički - preporučeno za mnoge scenariji - ili zajedničko korištenje programa NFS za zajedničko korištenje datoteka. Pogledajte informacije i detaljne upute u [Početak rada s Linux računalnim čvorove u programa HPC paket klaster u Azure](virtual-machines-linux-classic-hpcpack-cluster.md)za zajedničko korištenje datoteka.

1.  Stvorite mapu na glavni čvor, te je zajednički koristite svima postavljanjem ovlasti za čitanje/pisanje. Na primjer, zajednički koristiti C:\OpenFOAM na glavni čvor kao \\ \\SUSE12RDMA HN\OpenFOAM. Ovdje, *SUSE12RDMA HN* je naziv glavnog računala glavni čvora.

2.  Otvorite prozor komponente Windows PowerShell i izvršite sljedeće naredbe:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Prva naredba stvara mapu pod nazivom /openfoam na sve čvorove u grupi LinuxNodes. Drugi naredba mounts //SUSE12RDMA-HN/OpenFOAM zajedničku mapu na čvorove Linux s dir_mode i file_mode bitova postavite 777. *Korisničko ime* i *lozinku* u naredbi mora biti vjerodajnica korisnika na glavni čvor.

>[AZURE.NOTE]Na "\`" simbol u drugom naredba je simbol escape za PowerShell. "\`," znači "," (znak zareza) je dio naredbu.

## <a name="install-mpi-and-openfoam"></a>Instalirajte MPI i OpenFOAM

Da biste pokrenuli OpenFOAM kao zadatak programa MPI na mreži RDMA, morate Kompiliranje OpenFOAM s bibliotekama Intel MPI. 

Najprije se izvoditi nekoliko **clusrun** naredbe da biste instalirali Intel MPI biblioteke (Ako već nije instaliran) i OpenFOAM na vaše čvorove Linux. Koristite zajedničko korištenje glavni čvor prethodno konfigurirali za zajedničko korištenje datoteka instalacije među čvorove Linux.

>[AZURE.IMPORTANT]Ove instalacije i Kompiliranje korake su primjeri. Potreban vam je neke znanja Linux administracijom sustava da biste bili sigurni da zavisne compilers biblioteke su instalirani i ispravno. Možda ćete morati izmijeniti određene varijable okruženja ili drugih postavki za svoju verziju Intel MPI i OpenFOAM. Detalje potražite u članku [Intel MPI biblioteka za Linux vodiču za instalaciju](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) i [Instalacija paketa izvora OpenFOAM](http://openfoam.org/download/2-3-1-source/) okruženju sustava.


### <a name="install-intel-mpi"></a>Instalacija Intel MPI

Spremiti preuzete instalacijski paket za MPI Intel (l_mpi_p_5.0.3.048.tgz u ovom primjeru) u C:\OpenFoam na glavni čvor tako da se čvorove Linux pristup datoteci s /openfoam. Pokrenite **clusrun** da biste instalirali Intel MPI biblioteke na sve čvorove Linux.

1.  Sljedeće naredbe kopirajte instalacijski paket i Izdvoji /opt/intel na svakom čvor.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Da biste instalirali tihu Intel MPI biblioteke, pomoću datoteke silent.cfg. Primjer možete pronaći ogledne datoteke na kraju ovog članka. Postavite tu datoteku u zajedničkoj mapi /openfoam. Detalje o datoteci silent.cfg potražite u članku [Intel MPI biblioteka za Linux vodiču za instalaciju - tihu instalaciju](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Svakako spremite datoteku silent.cfg kao tekstnu datoteku s Linux crtom završetaka (samo LF, ne CR LF). Na taj način da se pokreće se ispravno čvorove Linux.

3.  Instalirajte Intel MPI biblioteke u pozadinskom načinu rada.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>Konfiguriranje MPI

Za testiranje, trebali biste /etc/security/limits.conf za svaku od čvorove Linux dodati sljedeće retke:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Ponovno pokrenite čvorove Linux nakon što ažurirate limits.conf datoteku. Na primjer, koristite sljedeću naredbu **clusrun** :

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Nakon ponovnog pokretanja, provjerite je li zajedničku mapu postavljen kao /openfoam.

### <a name="compile-and-install-openfoam"></a>Kompiliranje i instalirajte OpenFOAM

Spremiti preuzete instalacijski paket za paket OpenFOAM izvora (OpenFOAM-2.3.1.tgz u ovom primjeru) da biste C:\OpenFoam na glavni čvor tako da čvorove Linux pristup datoteci s /openfoam. Pokrenite naredbe **clusrun** prikupiti OpenFOAM na sve čvorove Linux.


1.  Stvaranje mape /opt/OpenFOAM na svakom čvor Linux, kopirajte paket izvora u ovu mapu i izdvojite ga.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Prikupiti OpenFOAM s bibliotekom MPI Intel, najprije postavite neke varijable okruženja za Intel MPI i OpenFOAM. Skripta tulumu pod nazivom settings.sh da biste postavili varijabli. Primjer možete pronaći ogledne datoteke na kraju ovog članka. Postavite ovu datoteku (spremljene Linux crte kod) u zajedničkoj mapi /openfoam. Datoteka sadrži i postavke za MPI i OpenFOAM runtimes kasnije koristili na Pokreni OpenFOAM.

3. Instalirajte zavisne paketa potrebne za Kompiliranje OpenFOAM. Ovisno o raspodjele vaše Linux možda najprije morate dodati spremište. Pokretanje naredbe **clusrun** sličnu ovoj:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Ako je potrebno, SSH svaki čvor Linux za izvođenje naredbe da biste potvrdili da se pokrenu pravilno.

4.  Pokrenite sljedeću naredbu za Kompiliranje OpenFOAM. Postupak sastavljanja traje dulje vrijeme i generira veliku količinu podataka zapisnika u standardni Izlaz, pa **/ interleaved** mogućnost da biste prikazali izlaz interleaved.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]Na "\`" simbol u naredbi je simbol escape za PowerShell. "\`&" znači da se "&" je dio naredbu.

## <a name="prepare-to-run-an-openfoam-job"></a>Priprema za Pokreni OpenFOAM

Sada pripremite se za pokretanje programa MPI zadatak pod nazivom sloshingTank3D koji je jedna od primjera OpenFoam na dva čvorove Linux. 

### <a name="set-up-the-runtime-environment"></a>Postavljanje okruženja za vrijeme izvođenja

Da biste postavili vrijeme izvođenja okruženja za MPI i OpenFOAM na čvorove Linux, pokrenite sljedeću naredbu u prozoru komponente Windows PowerShell glavni čvor. (Ta naredba nije valjana za SUSE Linux samo.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Priprema oglednih podataka

Koristite zajedničko korištenje glavni čvor ste prethodno konfigurirali za zajedničko korištenje datoteka među čvorove Linux (postavljena kao /openfoam).

1.  SSH na neki od vašeg Linux izračunati čvorove.

2.  Ako to već niste učinili, pokrenite sljedeću naredbu da biste postavili okruženje za izvođenje OpenFOAM.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Kopirajte ogledne sloshingTank3D zajedničku mapu, a zatim ga otvorite.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Kada koristite zadani parametri ovaj uzorak, to može potrajati tens minuta da biste pokrenuli, pa možda ćete morati izmijeniti neke parametre da biste brže izvoditi. Jedan odabir jednostavno je za promjenu vremena korak varijable deltaT i writeInterval u datoteci sustava/controlDict. Ta datoteka pohranjuje ulaznih podataka koji se odnose na kontrolu vrijeme i za čitanje i pisanje podataka rješenja. Ako, na primjer, promijenite vrijednost deltaT iz 0,05 0,5 i vrijednost writeInterval iz 0,05 0,5.

    ![Izmjena varijabli korak][step_variables]

5.  Određivanje željene vrijednosti za varijable u datoteci sustava/decomposeParDict. U ovom se primjeru koristi dvije Linux čvorove svaki s 8 jezgri, tako da postavite numberOfSubdomains 16 i n hierarchicalCoeffs da biste (1 1 16), što znači da je pokrenuti OpenFOAM paralelno sa 16 procesa. Dodatne informacije potražite u članku [OpenFOAM korisničkom priručniku: 3.4 pokretanje aplikacije paralelno](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Decompose procesa][decompose]

6.  Pokrenite sljedeće naredbe iz imenika sloshingTank3D Priprema oglednih podataka.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Na glavni čvor, trebali biste vidjeti oglednim podatkovnim datotekama kopiraju se u C:\OpenFoam\sloshingTank3D. (C:\OpenFoam nije zajednička mapa na glavni čvor.)

    ![Podatkovne datoteke na glavni čvor][data_files]

### <a name="host-file-for-mpirun"></a>Glavno računalo datoteke za mpirun

U ovom ćete koraku stvoriti datoteku glavnog računala (popis računalnim čvorove), koji koristi naredbu **mpirun** .

1.  Na jedan od čvorove Linux stvorite datoteku pod nazivom hostfile u odjeljku /openfoam, tako da datoteku proslijediti poziv /openfoam/hostfile na sve čvorove Linux.

2.  Napišite čvor naziva Linux u tu datoteku. U ovom primjeru datoteka sadrži nazive sljedeće:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Možete stvoriti i datoteka na C:\OpenFoam\hostfile na glavni čvor. Ako odaberete tu mogućnost, spremite ga u obliku tekstne datoteke s Linux crtom završetaka (samo LF, ne CR LF). Time se osigurava da se pokreće se ispravno čvorove Linux.

    **Tulum omot skripte**

    Ako imate mnogo čvorovi Linux, a želite pokrenuti na samo neke, nije dobro je koristiti datoteku nepromjenjivi glavno računalo jer ne znate koji se čvorovi će se dodijeliti posla. U ovom slučaju pisanje omot za skripte tulumu za **mpirun** da biste automatski stvorili datoteku glavnog računala. Možete pronaći omot primjer tulumu skripte naziva hpcimpirun.sh pri kraju ovog članka i spremanje u obliku /openfoam/hpcimpirun.sh. U ovom primjeru skripte čini sljedeće:

    1.  Postavlja varijable okruženja za **mpirun**i neke naredbe parametre dodatno da biste pokrenuli posao MPI putem mreže RDMA. U ovom slučaju postavlja sljedeće varijable:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = bajtovima v2 ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Stvara datoteku glavnog računala prema okruženje varijable $CCP_NODES_CORES koja je postavio čvor glavni HPC prilikom aktiviranja posao.

        Oblikovanje $CCP_NODES_CORES slijedi ovaj uzorak:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        gdje

        * `<Number of nodes>`-broj čvorove dodijeliti zadatak.  
        
        * `<Name of node_n_...>`-Naziv svaki čvor dodijeliti zadatak.
        
        * `<Cores of node_n_...>`-broj jezgri na čvor dodijeliti zadatak.

        Ako, na primjer, ako je zadatak potrebno dva čvorove da biste pokrenuli, $CCP_NODES_CORES slično je
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Poziva naredbe **mpirun** i dodaje dva parametara naredbenog retka.

        * `--hostfile <hostfilepath>: <hostfilepath>`-put datoteke glavno računalo stvara skripta

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-varijabla okruženja odredio čvor glavni HPC paket, koji pohranjuje broj ukupni jezgri dodijeliti zadatak. U ovom slučaju određuje broj postupaka za **mpirun**.


## <a name="submit-an-openfoam-job"></a>Slanje u OpenFOAM posla

Sada možete poslati u HPC klaster upravitelja za posao. Morate proći hpcimpirun.sh skripte u recima naredbu za neke zadatke posla.

1. Povezivanje s vašeg čvor glavni klaster i počnite HPC klaster Manager.

2. **Upravljanje resursima za u**računalnim čvorovi Linux provjerite jesu li u stanju **Online** . Ako nisu, odaberite ih i kliknite **Premjesti na mreži**.

3.  U odjeljku **Upravljanje**, kliknite **Novi zadatak**.

4.  Unesite naziv zadatka kao što su _sloshingTank3D_.

    ![Detalji o posla][job_details]

5.  **Resursi za posao**, odaberite vrstu resursa kao "Čvor" i postavite minimalne 2. Tu konfiguraciju pokreće posao dva Linux čvorove, od kojih svaka ima osam jezgri u ovom primjeru.

    ![Resursi za posao][job_resources]

6. Kliknite **Uređivanje zadaci** u lijevom navigacijskom oknu, a zatim **Dodaj** zadatak možete dodati na posao. Dodavanje četiri zadataka zadatak s sljedeće naredbe i postavke.

    >[AZURE.NOTE]Pokretanje `source /openfoam/settings.sh` postavlja runtime okruženja OpenFOAM i MPI tako da svaki od sljedećih zadataka poziva prije naredbu OpenFOAM.

    *   **Zadatak 1**. Pokrenite **decomposePar** da biste generirali podatkovne datoteke za pokretanje **interDyMFoam** paralelno.
    
        *   Jedan čvor dodijeliti zadatak

        *   **Naredbeni redak** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Rad direktorija** - / openfoam/sloshingTank3D
        
        Pogledajte na sljedećoj slici. Isto tako konfigurirati preostale zadatke.

        ![Detalji o zadatku 1][task_details1]

    *   **Zadatak 2**. Pokrenite **interDyMFoam** paralelno za izračunavanje uzorka.

        *   Dva čvorove dodijeliti zadatak

        *   **Naredbeni redak** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Rad direktorija** - / openfoam/sloshingTank3D

    *   **Zadatak 3**. Pokrenite **reconstructPar** da biste spojili skupova vremena direktorija iz imenika svaki processor_N_ u jednom skupu.

        *   Jedan čvor dodijeliti zadatak

        *   **Naredbeni redak** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Rad direktorija** - / openfoam/sloshingTank3D

    *   **Zadatak 4**. Pokrenite **foamToEnsight** paralelno pretvoriti rezultat datoteke OpenFOAM u EnSight oblik i upućivanje datoteka EnSight u direktoriju Ensight u direktoriju slučaja.

        *   Dva čvorove dodijeliti zadatak

        *   **Naredbeni redak** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Rad direktorija** - / openfoam/sloshingTank3D

6.  Dodajte ovisnosti zadaci uzlaznim redoslijedom zadatka.

    ![Međuzavisnosti zadataka][task_dependencies]

7.  Kliknite **Pošalji** da biste pokrenuli ovaj zadatak.

    Prema zadanim postavkama HPC paket šalje posla kao trenutni prijavljeni na korisnički račun. Nakon što kliknete **Pošalji**, možda će se prikazati dijaloški okvir koji se tražiti da unesete korisničko ime i lozinku.

    ![Vjerodajnice za posao][creds]

    U nekim uvjetima HPC paket pamti korisničke informacije prije za unos, a ne prikazuje taj dijaloški okvir. Da biste ponovno prikazali paket za HPC, unesite sljedeću naredbu u naredbenom retku, a zatim poslati posao.

    ```
    hpccred delcreds
    ```

8.  Posao vodi iz tens minuta do nekoliko sati prema parametre koje ste postavili za uzorak. U karte ekstrema vidjeti zadatak koji se izvode na čvorove Linux. 

    ![Karte ekstrema][heat_map]

    Na svakom čvor pokrenut će se osam procesa.

    ![Linux procesa][linux_processes]

9.  Kada se dovrši posao, pronađite posao rezultate u mapama u odjeljku C:\OpenFoam\sloshingTank3D i datoteka zapisnika C:\OpenFoam.


## <a name="view-results-in-ensight"></a>Prikaz rezultata u EnSight

Po želji pomoću [EnSight](https://www.ceisoftware.com/) vizualni prikaz i analiza rezultata OpenFOAM posao. Dodatne informacije o vizualizaciju i animacije u EnSight potražite u članku [Vodič za videozapis](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Nakon što instalirate EnSight na glavni čvor, pokrenite ga.

2.  Otvorite C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Vidjet ćete tank u pregledniku.

    ![Tank u EnSight][tank]

3.  Stvaranje **Isosurface** iz **internalMesh**, a zatim odaberite varijable **alpha_water**.

    ![Stvaranje programa isosurface][isosurface]

4.  Postavljanje boje za **Isosurface_part** stvorili u prethodnom koraku. Na primjer, postavite ga na vode plave.

    ![Uređivanje isosurface boja][isosurface_color]

5.  Stvaranje **Iso jedinice** iz **zidovi** tako da odaberete **zidovi** na ploči **dijelovi** , a zatim kliknite gumb **Isosurfaces** na alatnoj traci.

6.  U dijaloškom okviru odaberite **vrstu** kao **Isovolume** i postavite Min **Isovolume raspon** 0,5. Da biste stvorili na isovolume, kliknite **Stvori s odabranih dijelova**.

7.  Postavljanje boje za **Iso_volume_part** stvorili u prethodnom koraku. Ako, na primjer, postavite precizno vode plave.

8.  Postavljanje boje za **zidovi**. Na primjer, postavite ga na prozirne bijelo.

9. Sada kliknite **Reproduciraj** da biste vidjeli rezultate simulaciju.

    ![Rezultat tank][tank_result]

## <a name="sample-files"></a>Ogledne datoteke

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Konfiguracijska datoteka XML uzorka za implementaciju klaster pomoću skripte komponente PowerShell

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Ogledna datoteka silent.cfg da biste instalirali MPI

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Primjer settings.sh skripte

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Primjer hpcimpirun.sh skripte

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
