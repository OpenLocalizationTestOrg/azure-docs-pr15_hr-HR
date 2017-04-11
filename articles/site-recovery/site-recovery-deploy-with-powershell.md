<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima u VMM oblaka pomoću oporavak Azure web-mjesta i PowerShell | Microsoft Azure"
    description="Saznajte kako automatizirati replikacije Hyper-V virtualnim strojevima u VMM oblaka pomoću oporavak web-mjesta i PowerShell."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Da biste Azure pomoću komponente Powershell – klasični replicirati Hyper-V virtualnim strojevima u VMM oblaka

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-azure.md)
- [PowerShell – Voditelj resursa](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasični Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell – klasični](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Pregled

Oporavak Azure web-mjesta doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima broj scenariji za implementaciju. Potpuni popis scenariji za implementaciju potražite u članku [Pregled oporavak Azure web-mjesta](site-recovery-overview.md).

U ovom se članku objašnjava pomoću komponente PowerShell Automatizacija uobičajenih zadataka koje morate izvesti kada postavljate oporavak Azure web-mjesta za replikaciju Hyper-V virtualnim strojevima u VMM centar sustava oblaka Azure za pohranu.

Članak sadrži preduvjeti za scenarij i objašnjava postavljanje zbirke ključeva za oporavak web-mjesta, instalirajte davatelja za oporavak Azure web-mjesta na poslužitelju VMM izvora, registraciju poslužitelja u na sigurnog, dodajte račun za Azure prostora za pohranu, instalirajte agent servisa Azure oporavak na poslužiteljima Hyper-V glavno računalo, konfiguriranje postavki zaštite za VMM oblaka koja će se primijeniti na sve zaštićene virtualnim strojevima , a zatim omogućiti zaštitu te virtualnim računalima. Dovršili postupak tako da testirate prebacivanje da biste provjerili funkcionira sve prema očekivanjima.

Ako naiđete na probleme postavljanje ovom scenariju, postavite pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa i Classic](../resource-manager-deployment-model.md). U ovom se članku opisuje pomoću modela implementacije klasični.



## <a name="before-you-start"></a>Prije početka

Provjerite je li te preduvjeti na mjestu:

### <a name="azure-prerequisites"></a>Preduvjeti za Azure

- Potreban vam je račun sustava [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- Morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka. Račun mora zemlj replikacije omogućena. Ga mora biti u području isti kao sigurnog oporavak Azure web-mjesta i biti povezane s iste pretplate. [Dodatne informacije o Azure prostora za pohranu](../storage/storage-introduction.md).
- Morat ćete da biste bili sigurni virtualnim strojevima koji želite zaštititi pridržavate [preduvjeti Azure virtualnog računala](site-recovery-best-practices.md#virtual-machines).

### <a name="vmm-prerequisites"></a>Preduvjeti za VMM
- Morat ćete VMM poslužitelju koji se izvodi na sustav centar 2012 R2.
- Potreban vam je barem jedan oblak na poslužitelju VMM koji želite zaštititi. Oblak mora sadržavati:
    - Jedan ili više VMM glavno računalo grupe.
    - Jedan ili više Hyper-V glavnog računala poslužitelja ili klastere u svakoj grupi glavnog računala.
    - Jedan ili više virtualnim strojevima na izvornom Hyper-V poslužitelju.

### <a name="hyper-v-prerequisites"></a>Preduvjeti Hyper-V

- Glavno računalo Hyper-V poslužitelje morate imati barem **Windows Server 2012** Hyper-V uloge ili **Microsoft Hyper-V Server 2012** i instalirali najnovija ažuriranja.
- Ako koristite Hyper-V u bilješci klaster te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Morat ćete ručno konfiguriranje broker klaster. Da biste to učinili, u Upravitelj poslužitelja > povezati klaster upravitelja klaster prebacivanje, kliknite **Konfiguriranje uloge** i odaberite **Hyper-V replike Broker** na zaslonu **Odaberite ulogu** visoke dostupnosti čarobnjaka.
- Hyper-V glavnog poslužitelja ni klaster za koju želite upravljati zaštite moraju biti obuhvaćene VMM oblaka.

### <a name="network-mapping-prerequisites"></a>Preduvjeti za mapiranje mreže
Kada zaštitite virtualnim strojevima u Azure mreže karte mapiranja između VM mreže na izvornom poslužitelju VMM i ciljnu Azure mreže da biste omogućili sljedeće:

- Sve strojeva koje neće uspjeti pokazivač na istoj mreži možete povezati s drugoga, bez obzira koju tarifu za oporavak se ona nalazi u.
- Ako mrežni pristupnik je postavljena na odredištu Azure mreže, virtualnim strojevima možete povezati s drugim lokalnog virtualnim računalima sustava.
- Ako ne konfigurirate mreže mapiranje samo virtualnim strojevima koje ne prođu putem u istu tarifu oporavak će se moći povezati međusobno nakon prebacivanje na Azure.

Ako želite uvesti preslikavanje morat ćete sljedeće:

- Virtualnim strojevima koji želite zaštititi na izvornom poslužitelju VMM moraju biti povezani s mrežom VM. Tu mrežu mora biti povezan s logičke mreže koji je pridružen oblaka.
- Programa Azure mreža repliciranu virtualnim strojevima na koji možete povezati nakon prebacivanje. Ovu mrežu odabrat ćete trenutku prebacivanje. Mreža mora biti u području isti kao pretplate oporavak Azure web-mjesta.
- [Saznajte više](site-recovery-network-mapping.md) o preslikavanje:

###<a name="powershell-prerequisites"></a>Preduvjeti za PowerShell
Provjerite je li Azure PowerShell spremna za slanje. Ako već koristite PowerShell, morat ćete nadogradnje na verziju 0.8.10 ili noviji. Informacije o postavljanju PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Kada ste postavili i konfigurirali PowerShell, možete pregledati sve dostupne cmdleta za servis [ovdje](https://msdn.microsoft.com/library/dn850420.aspx).

Dodatne informacije o savjeta koji pomažu pomoću cmdleta, kao što su kako vrijednosti parametra, unose i izlaze se obično upravlja u Azure PowerShell potražite u članku [Početak rada s cmdleta Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Korak 1: Postavljanje pretplate

U PowerShell pokrenite sljedeće Cmdlete:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Elementi unutar "< >" zamijenite određenih informacija.

## <a name="step-2-create-a-site-recovery-vault"></a>Korak 2: Stvaranje sigurnog za oporavak web-mjesta

U komponente PowerShell elementi unutar "< >" zamijenite određenih informacija i pokrenite sljedeće naredbe:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Korak 3: Stvaranje sigurnog ključa za registraciju

Generiranje ključa za registraciju u na zbirke ključeva. Nakon preuzimanja davatelja za oporavak Azure web-mjesta i instalirajte ga na poslužitelj VMM, koristit ćete taj ključ da biste registrirali VMM poslužitelja u zbirke ključeva.

1.  Dohvaćanje datoteka postavku sigurnog i postavljanje konteksta:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Postavljanje konteksta sigurnog ponovnim pokretanjem sljedeće naredbe:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Korak 4: Instalacija davatelja oporavak Azure web-mjesta

1.  Na računalu VMM stvorite direktorij ponovnim pokretanjem sljedeće naredbe:

    ```

    pushd C:\ASR\

    ```

2.  Izdvajanje datoteke pomoću preuzete davatelja ponovnim pokretanjem sljedeće naredbe

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Instalacija davatelja pomoću sljedeće naredbe:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Pričekajte da se instalacija završi.

4.  Registriranje poslužitelja u sigurnog pomoću sljedeće naredbe:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Korak 5: Stvorite račun za Azure prostora za pohranu

Ako nemate račun za Azure prostora za pohranu, stvorite račun zemlj replikacije omogućeno ponovnim pokretanjem sljedeće naredbe:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Imajte na umu da račun za pohranu mora biti u području isti kao servisa Azure oporavak web-mjesta, i pridružiti iste pretplate.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Korak 6: Instalacija agenta servisa za Azure oporavak

Na portalu Azure instalirajte agent servisa Azure oporavak Services na poslužitelju glavno računalo Hyper-V koja se nalazi u oblaka VMM koji želite zaštititi.

Pokrenite sljedeću naredbu na sve VMM glavnog računala:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>7 korak: Konfiguriranje oblaka postavke zaštite

1.  Stvaranje profila zaštitu oblaka za Azure ponovnim pokretanjem sljedeće naredbe:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Da biste dobili spremniku zaštitu ponovnim pokretanjem sljedeće naredbe:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Pridruživanje spremnik zaštitu počinju s oblakom:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Kada se zadatak završio, pokrenite sljedeću naredbu:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Kada se zadatak završio obrada, pokrenite sljedeću naredbu:


        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



Da biste provjerili nakon dovršetka postupka, slijedite korake u [Nadzornik aktivnosti](#monitor).

## <a name="step-8-configure-network-mapping"></a>8 korak: Konfiguriranje preslikavanje
Prije nego što počnete preslikavanje provjerite je li virtualnim strojevima na izvornom poslužitelju VMM povezani s mrežom VM. Osim toga, stvorite jednu ili više mreža Azure virtualne. Imajte na umu da se više mreža VM mogu mapirati u jedne mreže Azure.

Imajte na umu da ako mreža cilj ima više podmreže, a zatim neku od tih podmreže ima isti naziv kao podmreže na kojem nalazi virtualnog računala izvora, a zatim virtualnog računala replike će se povezati s tom podmreže ciljne nakon prebacivanje. Ako ne podmreži cilj pod nazivom koji se podudaraju, virtualnog računala će se povezati s prvom podmreže u mreži.

Prva naredba dobiva poslužitelji za trenutni sigurnog oporavak Azure web-mjesta. Naredba pohranjuje poslužitelje sustava Microsoft Azure web-mjesta oporavak varijabla polja $Servers.

    $Servers = Get-AzureSiteRecoveryServer


Drugi naredba dohvaća mreže oporavak web-mjesta za prvi poslužitelj u polju $Servers. Naredba pohranjuje mreže u $Networks varijabli.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Treći naredba dobiva pretplate Azure pomoću cmdleta Get-AzureSubscription, a zatim sprema tu vrijednost u varijablu $Subscriptions.

    $Subscriptions = Get-AzureSubscription



Četvrti naredba dohvaća Azure virtualne mreže pomoću cmdleta Get-AzureVNetSite, a zatim tu vrijednost u $AzureVmNetworks varijabli.


    $AzureVmNetworks = Get-AzureVNetSite



Konačni cmdlet stvara mapiranja između primarni mreže i s mrežom Azure virtualnog računala. Cmdlet određuje primarnu mreže kao prvi element $Networks. Cmdlet određuje mreže virtualnog računala kao prvi element $AzureVmNetworks pomoću njegov ID-a. Naredba obuhvaća svoj ID Azure pretplate.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Korak 9: Omogućavanje zaštitu virtualnim strojevima

Nakon poslužitelje, oblaka i mreža ispravno konfigurirani, možete omogućiti zaštitu virtualnih računala u oblaku. Imajte na umu sljedeće:

Virtualnim strojevima mora zadovoljiti [preduvjeti Azure virtualnog računala](site-recovery-best-practices.md#virtual-machines).

Da biste omogućili zaštitu operacijski sustav i operacijski sustav morate postaviti na disku svojstva virtualnog računala. Kada stvorite virtualnog računala u VMM pomoću predloška virtualnog računala možete postaviti svojstvo. Možete postaviti i tih svojstava za postojeće virtualnim strojevima na karticama **Općenito** i **Hardverske konfiguracije** svojstva virtualnog računala. Ako ne postavite tih svojstava u VMM ćete moći ih konfigurirali na portalu za oporavak Azure web-mjesta.



1.  Da biste omogućili zaštitu, pokrenite sljedeću naredbu da biste dobili spremnik zaštite:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Da biste dobili zaštitu entitet (VM) ponovnim pokretanjem sljedeće naredbe:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Omogućivanje DR za na VM ponovnim pokretanjem sljedeće naredbe:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Provjera uvođenja

Da biste testirali implementaciju sustava možete pokrenuti prebacivanje test za jednu virtualnog računala ili stvorite plan oporavak koji se sastoji od više virtualnim strojevima i pokrenite test prebacivanje plana. Prebacivanje test simulira prebacivanje i oporavak mehanizam Izolirani mreže. Imajte na umu da:

- Ako se želite povezati virtualnog računala u Azure putem udaljene radne površine kada se prebacivanje omogućiti udaljene radne površine na virtualnog računala prije pokretanja prebacivanje test.
- Nakon prebacivanje, koristit ćete javnu IP adresa za povezivanje s virtualnog računala u Azure putem udaljene radne površine. Ako želite da biste to učinili, provjerite je li još nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.

Da biste provjerili nakon dovršetka postupka, slijedite korake u [Nadzornik aktivnosti](#monitor).

### <a name="create-a-recovery-plan"></a>Stvaranje plana oporavak

1. Stvaranje .xml datoteke kao predložak za oporavak tarifa pomoću podataka u nastavku, a zatim je spremite kao "C:\RPTemplatePath.xml".
2. Promjena RecoveryPlan čvor Id, naziv, PrimaryServerId i SecondaryServerId.
3. Promijenite čvor ProtectionEntity PrimaryProtectionEntityId (vmid iz VMM).
4. Možete dodati više VMs dodavanjem više ProtectionEntity čvorove.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Ispunite podatke u predložak:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Stvaranje na RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

1.  Da biste dobili objekt RecoveryPlan ponovnim pokretanjem sljedeće naredbe:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Pokrenite test prebacivanje ponovnim pokretanjem sljedeće naredbe:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Praćenje aktivnosti

Praćenje aktivnosti pomoću sljedeće naredbe. Imajte na umu da ćete morati pričekati između zadatke za obradu da biste završili.


    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije potražite u](https://msdn.microsoft.com/library/dn850420.aspx) cmdleta ljuske PowerShell za oporavak web-mjesta za Azure. </a>.
