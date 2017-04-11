<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima u VMM oblaka pomoću oporavak Azure web-mjesta i PowerShell (Voditelj resursa) | Microsoft Azure"
    description="Replicirati Hyper-V virtualnim strojevima u VMM oblaka pomoću oporavak Azure web-mjesta i komponente PowerShell"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Replicirati Hyper-V virtualnim strojevima u VMM oblaka za Azure pomoću komponente PowerShell i upravljanja resursima za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-azure.md)
- [PowerShell – Voditelj resursa](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasični Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell – klasični](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Pregled

Oporavak Azure web-mjesta doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima broj scenariji za implementaciju. Potpuni popis scenariji za implementaciju potražite u članku [Pregled oporavak Azure web-mjesta](site-recovery-overview.md).

U ovom se članku objašnjava pomoću komponente PowerShell Automatizacija uobičajenih zadataka koje morate izvesti kada postavljate oporavak Azure web-mjesta za replikaciju Hyper-V virtualnim strojevima u VMM centar sustava oblaka Azure za pohranu.

Članak sadrži preduvjeti za scenarij i prikazuje

- Kako postaviti sigurnog za usluge oporavak
- Kliknite pločicu davatelja za oporavak Azure web-mjesta s izvorišnim poslužiteljem VMM
- Registraciju poslužitelja u na sigurnog, dodajte račun za Azure prostora za pohranu
- Instalacija agent servisa Azure oporavak na poslužiteljima glavno računalo Hyper-V
- Konfiguriranje postavki zaštite za VMM oblaka koja će se primijeniti na sve zaštićene virtualnim strojevima
- Omogući zaštitu za te virtualnih računala.
- Testiranje nije uspjelo pokazivač da biste provjerili funkcionira sve prema očekivanjima.

Ako naiđete na probleme postavljanje ovom scenariju, postavite pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa i Classic](../resource-manager-deployment-model.md). U ovom se članku opisuje pomoću modela implementacije Voditelj resursa.

## <a name="before-you-start"></a>Prije početka

Provjerite je li te preduvjeti na mjestu:

### <a name="azure-prerequisites"></a>Preduvjeti za Azure

- Potreban vam je račun sustava [Microsoft Azure](https://azure.microsoft.com/) . Ako ga nemate, započnite s [besplatan račun](https://azure.microsoft.com/free). Osim toga, možete čitati o [cijene Upravitelj za oporavak Azure web-mjesta](https://azure.microsoft.com/pricing/details/site-recovery/).
- Morat ćete CSP pretplatu ako isprobavate replikacije na scenariju CSP pretplate. Dodatne informacije o programu CSP u [postupku uključivanja u program u programu CSP](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Morat ćete račun Azure v2 prostora za pohranu (Voditelj resursa) da biste pohranili podatke replicirati na Azure. Račun mora zemlj replikacije omogućena. Trebali biste se na istom području servisa Azure oporavak web-mjesta i pridružiti ili CSP pretplatu za istu pretplatu. Dodatne informacije o postavljanju Azure prostora za pohranu potražite u članku [Uvod u spremište na platformi Microsoft Azure](../storage/storage-introduction.md) za referencu.
- Morat ćete da biste bili sigurni virtualnim strojevima koji želite zaštititi pridržavate [preduvjeti Azure virtualnog računala](site-recovery-best-practices.md#azure-virtual-machine-requirements).

> [AZURE.NOTE] Trenutno VM razine moguće su samo operacije putem komponente Powershell. Podrška za oporavak plana razinu operacije bit će uskoro.  Zasad ste ograničeni su na izvođenje nije uspjelo – overs samo na preciznosti za 'zaštićeni VM', a ne na razini Plan za oporavak.

### <a name="vmm-prerequisites"></a>Preduvjeti za VMM
- Morat ćete VMM poslužitelju koji se izvodi na sustav centar 2012 R2.
- Bilo koji poslužitelj VMM koja sadrži virtualnim strojevima koji želite zaštititi mora biti pokrenut davatelja za oporavak Azure web-mjesta. Instalira se tijekom implementacije oporavak Azure web-mjesta.
- Potreban vam je barem jedan oblak na poslužitelju VMM koji želite zaštititi. Oblak mora sadržavati:
    - Jedan ili više VMM glavno računalo grupe.
    - Jedan ili više Hyper-V glavnog računala poslužitelja ili klastere u svakoj grupi glavnog računala.
    - Jedan ili više virtualnim strojevima na izvornom Hyper-V poslužitelju.
- Dodatne informacije o postavljanju oblaka VMM:
    - Dodatne informacije potražite u o privatnim oblaka VMM [što je novo u programu Excel privatno s sustava centra 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) i [VMM 2012 i oblaka](http://go.microsoft.com/fwlink/?LinkId=324956).
    - Informirajte se o [konfiguriranju oblaka tkanina VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Nakon tkanina elemenata na oblaku će se prikazati dodatne informacije o stvaranju privatne oblaka u [stvaranju privatne oblaka u VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) i u [vodič: Stvaranje privatne oblaka pomoću sustava centra 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Preduvjeti Hyper-V

- Glavno računalo Hyper-V poslužitelje morate imati barem **Windows Server 2012** Hyper-V uloge ili **Microsoft Hyper-V Server 2012** i instalirali najnovija ažuriranja.
- Ako koristite Hyper-V u bilješci klaster te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Morat ćete ručno konfiguriranje broker klaster. Za
- Upute potražite [u](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx)članku konfiguriranje Broker replike Hyper-V.
- Hyper-V glavnog poslužitelja ni klaster za koju želite upravljati zaštite moraju biti obuhvaćene VMM oblaka.

### <a name="network-mapping-prerequisites"></a>Preduvjeti za mapiranje mreže
Kada zaštitite virtualnim strojevima u Azure, karte mapiranje mreže virtualnog računala mreže na izvornom poslužitelju VMM i ciljnu Azure mreže da biste omogućili sljedeće:

- Sve strojeva koje neće uspjeti pokazivač na istoj mreži možete povezati s drugoga, bez obzira koju tarifu za oporavak se ona nalazi u.
- Ako mrežni pristupnik je postavljena na odredištu Azure mreže, virtualnim strojevima možete povezati s drugim lokalnog virtualnim računalima sustava.
- Ako ne konfigurirate preslikavanje, samo virtualnim strojevima koje ne prođu putem u istu tarifu oporavak bit će moći povezati međusobno nakon prekida pokazivač Azure.

Ako želite uvesti preslikavanje morat ćete sljedeće:

- Virtualnim strojevima koji želite zaštititi na izvornom poslužitelju VMM moraju biti povezani s mrežom VM. Tu mrežu mora biti povezan s logičke mreže koji je pridružen oblaka.
- Programa Azure mreža repliciranu virtualnim strojevima na koji možete povezati nakon prekida postavite pokazivač. Ovu mrežu odabrat ćete trenutku prekida postavite pokazivač. Mreža mora biti u području isti kao pretplate oporavak Azure web-mjesta.

Dodatne informacije o preslikavanje u

- [Konfiguriranje logičke mreža u VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Konfiguriranje VM mreža i pristupnika u VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Kako konfigurirati i praćenje virtualne mreže u Azure](https://azure.microsoft.com/documentation/services/virtual-network/)


###<a name="powershell-prerequisites"></a>Preduvjeti za PowerShell
Provjerite je li Azure PowerShell spremna za slanje. Ako već koristite PowerShell, morat ćete nadogradnje na verziju 0.8.10 ili noviji. Informacije o postavljanju PowerShell potražite u [vodiču za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md). Kada ste postavili i konfigurirali PowerShell, možete pregledati sve dostupne cmdleta za servis [ovdje](https://msdn.microsoft.com/library/dn850420.aspx).

Dodatne informacije o savjeta koji pomažu pomoću cmdleta, kao što su kako vrijednosti parametra, unose i izlaze se obično upravlja u Azure PowerShell potražite u članku [Vodič za početak rada s cmdleta Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Korak 1: Postavljanje pretplate

1. Iz komponente powershell Azure, prijavite se na račun za Azure: pomoću sljedeće Cmdlete

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred


2. Dobit ćete popis pretplate. To će popisa na subscriptionIDs za svaku od pretplata. Imajte na umu dolje subscriptionID pretplate u kojoj želite stvoriti servisa sigurnog oporavak

        Get-AzureRmSubscription

3. Postavljanje pretplate u kojem je servisa sigurnog oporavak bit će biti stvoren tako da dopušta ID pretplate

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Korak 2: Stvaranje oporavak servisa sigurnog

1. Stvaranje grupe resursa u Azure Voditelj resursa ako ga nemate već

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Stvorite novi oporavak servisa sigurnog i spremite stvoreni objekt sigurnog ASR u varijablu (koristit će se kasnije). Također možete dohvatiti ASR sigurnog objekt objavu stvaranje pomoću cmdleta Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Korak 3: Postavljanje konteksta oporavak servisa sigurnog

1.  Postavljanje konteksta sigurnog tako da pokrenete u nastavku naredba.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Korak 4: Instalacija davatelja oporavak Azure web-mjesta

1.  Na računalu VMM stvorite direktorij ponovnim pokretanjem sljedeće naredbe:

        New-Item c:\ASR -type directory

2.  Izdvajanje datoteke pomoću preuzete davatelja ponovnim pokretanjem sljedeće naredbe

        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q


3.  Instalacija davatelja pomoću sljedeće naredbe:

        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Pričekajte da se instalacija završi.

4.  Registriranje poslužitelja u sigurnog pomoću sljedeće naredbe:

        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-an-azure-storage-account"></a>Korak 5: Stvorite račun za Azure prostora za pohranu

1. Ako nemate račun za Azure prostora za pohranu, stvorite račun za zemlj replikacije omogućen u istoj zemlj kao na sigurnog ponovnim pokretanjem sljedeće naredbe:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Imajte na umu da račun za pohranu mora biti u području isti kao servisa Azure oporavak web-mjesta, i pridružiti iste pretplate.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Korak 6: Instalacija agenta servisa za Azure oporavak

1. Preuzmite agent servisa Azure oporavak pri [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) i instalirajte ga na poslužitelju glavno računalo Hyper-V koja se nalazi u oblaka VMM koji želite zaštititi.

2. Pokrenite sljedeću naredbu na sve VMM glavnog računala:

    /nu marsagentinstaller.exe/q

## <a name="step-7-configure-cloud-protection-settings"></a>7 korak: Konfiguriranje oblaka postavke zaštite

1.  Stvaranje pravila replikacije za Azure ponovnim pokretanjem sljedeće naredbe:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Da biste dobili spremniku zaštitu ponovnim pokretanjem sljedeće naredbe:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Dohvaćanje detalja pravila tjednog prikaza kalendara pomoću posla koji je stvoren i Spominjanje naziv neslužbeni pravila:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Pridruživanje spremnik zaštitu počinju replikacije pravila:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Kada se zadatak završio, pokrenite sljedeću naredbu:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Kada se zadatak završio obrada, pokrenite sljedeću naredbu:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Da biste provjerili nakon dovršetka postupka, slijedite korake u [Nadzornik aktivnosti](#monitor).

## <a name="step-8-configure-network-mapping"></a>8 korak: Konfiguriranje preslikavanje

Prije nego što počnete preslikavanje provjerite je li virtualnim strojevima na izvornom poslužitelju VMM povezani s mrežom VM. Osim toga, stvorite jednu ili više mreža Azure virtualne.

Dodatne informacije o stvaranju virtualne mreže pomoću upravitelja resursa Azure i komponente PowerShell u [Stvaranje virtualne mreže s vezom za VPN-a web-mjesto pomoću upravitelja resursa Azure i komponente PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Imajte na umu da se više mreža virtualnog računala mogu mapirati u jedne mreže Azure. Ako mreža cilj ima više podmreže i jednu od tih podmreže ima isti naziv kao podmreže na kojem se nalazi virtualnog računala izvora, zatim virtualnog računala replike će se povezati s tom podmreže ciljne nakon prekida postavite pokazivač. Ako nema cilj podmreže pod nazivom koji se podudaraju, virtualnog računala će se povezati s prvom podmreže u mreži.

1. Prva naredba dobiva poslužitelji za trenutni sigurnog oporavak Azure web-mjesta. Naredba pohranjuje poslužitelje sustava Microsoft Azure web-mjesta oporavak varijabla polja $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Drugi naredba dohvaća mreže oporavak web-mjesta za prvi poslužitelj u polju $Servers. Naredba pohranjuje mreže u $Networks varijabli.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Treći naredba dohvaća Azure virtualne mreže, a zatim tu vrijednost u $AzureVmNetworks varijabli.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Konačni cmdlet stvara mapiranja između primarni mreže i s mrežom Azure virtualnog računala. Cmdlet određuje primarnu mreže kao prvi element $Networks. Cmdlet određuje mreže virtualnog računala kao prvi element $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Korak 9: Omogućavanje zaštitu virtualnim strojevima

Nakon poslužitelje, oblaka i mreža ispravno konfigurirani, možete omogućiti zaštitu virtualnih računala u oblaku.

 Imajte na umu sljedeće:

 - Virtualnim strojevima mora zadovoljavati Azure uvjete. U vodiču za planiranje, provjerite sljedeće [preduvjete](site-recovery-best-practices.md) i podrška.

 - Da biste omogućili zaštitu, operacijski sustav i operacijski sustav morate postaviti na disku svojstva virtualnog računala. Kada stvorite virtualnog računala u VMM pomoću predloška virtualnog računala možete postaviti svojstvo. Možete postaviti i tih svojstava za postojeće virtualnim strojevima na karticama **Općenito** i **Hardverske konfiguracije** svojstva virtualnog računala. Ako ne postavite tih svojstava u VMM ćete moći ih konfigurirali na portalu za oporavak Azure web-mjesta.


  1. Da biste omogućili zaštitu, pokrenite sljedeću naredbu da biste dobili spremnik zaštite:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Da biste dobili zaštitu entitet (VM) ponovnim pokretanjem sljedeće naredbe:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Omogućivanje DR za na VM ponovnim pokretanjem sljedeće naredbe:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Provjera uvođenja

Da biste testirali implementaciju sustava možete pokrenuti na test nije uspjelo pokazivač za jednu virtualnog računala ili stvorite plan oporavak koji se sastoji od više virtualnim strojevima i pokrenuti na test nije uspjelo pokazivač plana. Nije uspjelo pokazivač test simulira nije uspjelo postavite pokazivač i oporavak mehanizam Izolirani mreže. Imajte na umu da:

- Ako se želite povezati virtualnog računala u Azure putem udaljene radne površine nakon prekida pokazivač omogućiti udaljene radne površine na virtualnog računala prije pokretanja prebacivanje test.
- Nakon prekida postavite pokazivač, koristit ćete javnu IP adresa za povezivanje s virtualnog računala u Azure putem udaljene radne površine. Ako želite da biste to učinili, provjerite je li još nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.

Da biste provjerili nakon dovršetka postupka, slijedite korake u [Nadzornik aktivnosti](#monitor).


### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

1.  Pokrenite test prebacivanje ponovnim pokretanjem sljedeće naredbe:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Pokretanje planiranog prebacivanje

1. Najprije planiranog prebacivanje pokretanjem sljedeće naredbe:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Pokretanje neplanirano prebacivanje

1. Najprije neplanirano prebacivanje pokretanjem sljedeće naredbe:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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

[Dodatne informacije potražite u](https://msdn.microsoft.com/library/azure/mt637930.aspx) oporavak web-mjesta Azure pomoću cmdleta ljuske PowerShell za upravljanje resursima Azure.
