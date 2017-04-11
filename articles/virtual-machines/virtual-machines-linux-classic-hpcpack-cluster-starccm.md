<properties
 pageTitle="Pokretanje ZVIJEZDA-CCM + HPC paket na Linux VMs | Microsoft Azure"
 description="Implementacija paket Microsoft HPC klaster na Azure i pokretanje programa ZVIJEZDA-CCM + posao više Linux izračunati čvorove putem programa RDMA mreže."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Pokretanje ZVIJEZDA-CCM + s paket Microsoft HPC na Linux RDMA skupine servisu Azure
U ovom se članku objašnjava za implementaciju klaster HPC paket sustava Microsoft Azure i pokretanje na [CD adapco ZVIJEZDA-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) posao više čvorovi računalnim Linux koje međusobno InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Paket Microsoft HPC nudi značajke da biste pokrenuli raznih veliki HPC i paralelno aplikacije, uključujući MPI aplikacije, na klastere virtualnim računalima sustava Microsoft Azure. Paket HPC podržava i aktivnih programa Linux HPC na Linux računalnim čvor VMs koje su uvedene u klasteru HPC paket. Uvod u korištenje Linux čvorove HPC paket za izračun, potražite u članku [Početak rada s Linux računalnim čvorove u programa paketa HPC klaster u Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Postavljanje programa paketa HPC klaster
Preuzmite skripte za implementaciju HPC paket IaaS iz [Centra za preuzimanje](https://www.microsoft.com/en-us/download/details.aspx?id=44949) i izdvojiti ih lokalno.

Azure PowerShell je na preduvjeta. Ako PowerShell nije konfiguriran na lokalnom računalu, pročitajte članak [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Prilikom pisanja ovog Linux slike iz trgovine Azure (koja sadrži InfiniBand upravljačke programe za Azure) su SLES 12, CentOS 6.5 i CentOS 7.1. U ovom se članku temelji se na korištenje SLES 12. Da biste dohvatili naziv svih slika Linux koji podržavaju HPC na tržištu, možete pokrenite sljedeću naredbu komponente PowerShell:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Izlaz popis na mjesto na kojem te slike dostupne su i naziv slike (**ImageName**) koja će se koristiti u predlošku implementacije kasnije.

Prije implementacije klaster, imate stvaranja HPC paket uvođenje predloška datoteke. Jer smo si ciljanja small klaster, čvor glavni bit će kontrolerom domene i glavnog računala s lokalnom bazom podataka SQL.

Sljedeći predložak će uvesti takve glavni čvor, stvoriti XML datoteku pod nazivom **MyCluster.xml**i zamijenite vaš vrijednosti **SubscriptionId**, **StorageAccount**, **mjesto**, **VMName**i **naziv servisa** .

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Početak stvaranja glave čvor tako da pokrenete naredbu komponente PowerShell u dodatnim naredbeni redak:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Nakon 20 do 30 minuta glavni čvor mora biti spreman. Možete povezati ga s portala za Azure tako da kliknete ikonu **Povezivanje** virtualnog računala.

Naposljetku možda rješavanje forwarder DNS-a. Da biste to učinili, pokrenite upravitelja DNS-om.

1.  Desnom tipkom miša kliknite naziv poslužitelja u upravitelju DNS, odaberite **Svojstva**, a zatim kliknite karticu **prosljeđivanja** .

2.  Kliknite gumb **Uređivanje** da biste uklonili sve prosljeđivanja, a zatim kliknite **u redu**.

3.  Provjerite je li potvrđen okvir **koristi naznake korijena ako nema prosljeđivanja dostupni** , a zatim kliknite **u redu**.

## <a name="set-up-linux-compute-nodes"></a>Postavljanje Linux računalnim čvorove
Implementacija čvorove računalnim Linux pomoću istog implementacije predloška koji ste koristili za stvaranje glavni čvor.

Kopirajte datoteku **MyCluster.xml** s lokalnog računala na glavni čvor i ažuriranje **NodeCount** oznaka s brojem čvorove koje želite uvesti (< = 20). Pripazite da imate dovoljno dostupne jezgri u Azure ograničenja, jer svaku instancu A9 zauzeti će 16 jezgri u svoju pretplatu. Ako želite koristiti više VMs u isti proračun, umjesto A9 možete koristiti A8 instance (8 jezgri).

Na glavni čvor kopirajte skripte za implementaciju HPC paket IaaS.

Pokrenite sljedeće naredbe ljuske PowerShell Azure u dodatnim naredbeni redak:

1.  Pokrenite **Dodaj AzureAccount** povezati Azure pretplatu.

2.  Ako imate više pretplata, pokrenite **Get-AzureSubscription** da biste ga naveli.

3.  Postavljanje zadanog pretplatu tako da pokrenete u **xxxx odaberite AzureSubscription - SubscriptionName-zadane** naredbe.

4.  Pokrenite **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** da biste pokrenuli implementacija Linux izračunati čvorove.

    ![Lakši čvor implementacije u akciji][hndeploy]

Otvorite alat HPC paket klaster Manager. Nakon nekoliko minuta Linux računalnim čvorove redovito prikazat će se na popisu klaster računalnim čvorove. S načinom uvođenje klasičnog IaaS VMs stvaraju sekvencijalno. Pa ako je broj čvorovi važne početak ih sve implementiran može potrajati veliku količinu vremena.

![Čvorovi Linux u upravitelju klaster HPC paketa][clustermanager]

Sada se sve čvorove s radom u klasteru, postoje dodatna Infrastruktura postavke da biste.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Postavljanje programa Azure za zajedničko korištenje datoteka za Windows i Linux čvorove
Možete koristiti servisa Azure datoteke za spremanje skripte, paketa aplikacije i podatkovne datoteke. Azure datoteke pruža mogućnosti CIFS pri vrhu spremište blobova platforme Azure kao stalni spremište. Imajte na umu da nije najčešće skalabilni rješenja, ali je najjednostavnije i ne zahtijeva namjenski VMs.

Stvaranje Azure zajedničkom slijedeći upute u članku [Početak rada s Azure pohrani u sustavu Windows](..\storage\storage-dotnet-how-to-use-files.md).

Kao **saname**, naziv zajedničko korištenje datoteke kao **zajednički naziv**i ključ za račun za pohranu kao i zadržati postojeći naziv vašeg računa za pohranu **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Postavljanje zajedničko korištenje datoteka Azure na glavni čvor
Otvorite povećane naredbeni redak, a zatim pokrenite sljedeću naredbu za pohranu vjerodajnica u lokalnom stroju sigurnog:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Nakon toga postavljanje na zajedničkom mjestu datoteka Azure, pokrenite:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Postavljanje zajedničko korištenje datoteka Azure na Linux računalnim čvorove
Jedan koristan alat koji se isporučuju s HPC paket je alat za clusrun. Koristite ovaj alat naredbenog retka za izvođenje naredbe isti istodobno na skupu računalnim čvorove. U našem slučaju koristi se za postavljanje zajedničko korištenje datoteka Azure i zadržavaju da preživi ponovna.
U na dodatnim naredbenog retka na glavni čvor, pokrenite sljedeće naredbe.

Da biste stvorili postavljanja direktorija:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Postavljanje na zajedničkom mjestu Azure datoteka:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Održati postavljanja zajedničko korištenje:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Instalacija ZVIJEZDA-CCM +
Azure instance VM A8 i A9 pružaju podršku za InfiniBand i RDMA mogućnosti. Upravljačke programe za otklanjanje koji omogućuju te mogućnosti dostupne su za Windows Server 2012 R2, SUSE 12, CentOS 6.5 i CentOS 7.1 slike u trgovine Windows Azure. Microsoft MPI i Intel MPI (izdanje 5.x) su dvije MPI biblioteke koje podržava te upravljačke programe u Azure.

ZVIJEZDA CD adapco-CCM + pustite 11.x i kasnije je vezanoj instalaciji verzijom Intel MPI 5.x, pa je InfiniBand podrška za Azure uključene.

Početak u Linux64 ZVIJEZDA-CCM + paket s [portala za CD adapco](https://steve.cd-adapco.com). U našem slučaj koristi 11.02.010 u različitim preciznosti verzija.

Na glavni čvor zajedničko korištenje datoteka Azure **/hpcdata** , stvorite Jezgrena skripta pod nazivom **setupstarccm.sh** sa sljedećim sadržajem. Ova skripta će se izvoditi na svakom računalnim čvor da biste postavili ZVIJEZDA-CCM + lokalno.

#### <a name="sample-setupstarcmsh-script"></a>Primjer setupstarcm.sh skripte
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Sada da biste postavili ZVIJEZDA-CCM + na sve Linux izračunati čvorove, otvorite povećane naredbeni redak i pokrenite sljedeću naredbu:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Dok se izvodi naredbu, možete nadzirati procesora koristi pomoću karte ekstrema upravitelja klaster. Nakon nekoliko minuta, sve čvorove trebaju imati ispravno postavljene.

## <a name="run-star-ccm-jobs"></a>Pokretanje ZVIJEZDA-CCM + poslova
Paket HPC koristi se za njegovim mogućnostima raspored posla da biste pokrenuli ZVIJEZDA-CCM + zadatke. Da biste to učinili, potrebna podrška nekoliko skripte koje se koriste za posao pokrenuti i koristiti ZVJEZDICA-CCM +. Unos podataka se drži zajedničkog korištenja datoteka Azure prvo jednostavnosti.

Koristi sljedeću skriptu komponente PowerShell u red čekanja ZVJEZDICA-CCM + posao. Traje tri argumenta:

*  Naziv modela

*  Broj čvorovi koja će se koristiti

*  Broj jezgri na svakom čvor koja će se koristiti

Budući da ZVIJEZDA-CCM + je moguće ispuniti ćelije propusnosti memorije obično je bolje koristiti manje jezgri po računalnim čvorove i dodajte novi čvorovi. Točan broj jezgri po čvor ovisi o obitelji procesora i brzinu vezu.

Čvorove su dodijeliti isključivo za posao i nije moguće zajednički koristiti s drugim zadacima. Posao nije pokrenut kao zadatak programa MPI izravno. Jezgrena skripta **runstarccm.sh** počet će pokretač MPI.

Unos modela i skripte **runstarccm.sh** spremaju se zajednički koristi **/hpcdata** koji je prethodno postavljen.

Zapisničke datoteke imenovane su s ID-om za posao i spremaju se u na **zajedničko korištenje /hpcdata**, zajedno s ZVIJEZDA-CCM + Izlazna datoteka.


#### <a name="sample-submitstarccmjobps1-script"></a>Primjer SubmitStarccmJob.ps1 skripte
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Zamjena **runner.java** na preferiranom ZVIJEZDA-CCM + Java modela pokretač i zapisivanje kod.

#### <a name="sample-runstarccmsh-script"></a>Primjer runstarccm.sh skripte
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

U našem test koristi token licencu za Power na zahtjev. Za tu token ćete morati postaviti **$CDLMD_LICENSE_FILE** varijablu okruženja **1999@flex.cd-adapco.com** i tipku u mogućnosti **- podkey** naredbenog retka.

Nakon neke Inicijalizacija skriptu izdvaja – iz **$CCP_NODES_CORES** varijable okruženja koje paket HPC postaviti – popis čvorove da biste sastavili hostfile koji koristi pokretač MPI. U ovom hostfile sadržavat će na popisu računalnim čvor naziva koji se koriste za zadatak, jedan naziv po retku.

Oblikovanje **$CCP_NODES_CORES** slijedi ovaj uzorak:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Pri čemu je:

* `<Number of nodes>`je broj čvorove dodijeliti zadatak.

* `<Name of node_n_...>`je naziv svaki čvor dodijeliti zadatak.

* `<Cores of node_n_...>`je broj jezgri na čvor dodijeliti zadatak.

Broj jezgri (**$NBCORES**) i izračunava se ovisno o broju čvorove (**$NBNODES**) i broj jezgri po čvor (prikazuje se kao parametar **$NBCORESPERNODE**).

Mogućnosti MPI one koja se koristi s MPI Intel na Azure su:

*   `-mpi intel`Da biste odredili Intel MPI.

*   `-fabric UDAPL`Da biste koristili glagoli Azure InfiniBand.

*   `-cpubind bandwidth,v`Da biste optimizirali propusnosti za MPI s ZVIJEZDA-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Da biste Intel MPI rad s Azure InfiniBand, a da biste postavili potreban broj jezgri po čvor.

*   `-batch`Da biste pokrenuli ZVIJEZDA-CCM + u načinu rada za obradu s nema korisničkog Sučelja.


Na kraju, da biste započeli posla, provjerite svoje čvorove su s radom, a povezani s Internetom u upravitelju klaster. Zatim iz naredbenog retka za PowerShell pokrenite sljedeće:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Zaustavljanje čvorove
Kasnije nakon što završite s testiranja, možete koristiti sljedeće naredbe ljuske PowerShell HPC paket da biste zaustavili i počeli čvorove:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Daljnji koraci
Pokušajte pokrenuti drugih radnih opterećenja Linux. Na primjer, u sljedećim člancima:

* [Mada NAMD s paket Microsoft HPC Linux računalnim čvorove u Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Mada OpenFOAM s paket Microsoft HPC Linux RDMA klaster servisu Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
