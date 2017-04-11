<properties
    pageTitle="Replicirati Hyper-V virtualnim strojevima u VMM oblaka na sekundarnom VMM web-mjesto pomoću komponente PowerShell (Voditelj resursa) | Microsoft Azure"
    description="U članku se opisuje kako implementirati Azure oporavak web-mjesta da biste orkestrirali replikacije, prebacivanje i oporavak Hyper-V VMs u VMM oblaka na sekundarnom VMM web-mjesto pomoću komponente PowerShell (Voditelj resursa)"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Replicirati Hyper-V virtualnim strojevima u VMM oblaka na sekundarnom VMM web-mjesto pomoću komponente PowerShell (Voditelj resursa)

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-vmm.md)
- [Klasični Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell – Voditelj resursa](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Dobro došli u oporavak Azure web-mjesta! Koristite ovaj članak ako želite replicirati lokalnog Hyper-V virtualnim strojevima upravlja se iz oblaka sustava centra virtualnog računala Manager (VMM) da biste sekundarnog web-mjesta. 

U ovom se članku objašnjava pomoću komponente PowerShell Automatizacija uobičajenih zadataka koje morate izvesti kada postavljate oporavak Azure web-mjesta za replikaciju Hyper-V virtualnim strojevima u VMM centar sustava oblaka za VMM centar sustava oblaka sekundarne web-mjestu.

Članak sadrži preduvjeti za scenarij i prikazuje 

- Kako postaviti sigurnog za usluge oporavak
- Kliknite pločicu davatelja za oporavak Azure web-mjesta s izvorišnim poslužiteljem VMM i VMM poslužitelju ciljni
- Dnevnik VMM poslužitelji u zbirke ključeva
- Konfiguriranje replikacije pravila za VMM spremanja. Postavke replikacije u pravilo će se primijeniti na sve zaštićene virtualnim strojevima 
- Omogući zaštitu za virtualnih računala. 
- Testirajte prebacivanje VMs pojedinačno ili kao dio oporavak plan da biste provjerili funkcionira sve prema očekivanjima.
- Izvođenje planiranog ili neplanirano prebacivanje od VMs pojedinačno ili kao dio oporavak plan da biste provjerili funkcionira sve prema očekivanjima.

Ako naiđete na probleme postavljanje ovom scenariju, postavite pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije. U ovom se članku opisuje model implementacije Voditelj resursa.



## <a name="on-premises-prerequisites"></a>Lokalni preduvjeti

Evo što morate primarnih i sekundarnih lokalnog web-mjesta za implementaciju scenarij:

**Preduvjeti** | **Pojedinosti** 
--- | ---
**VMM** | Preporučujemo da implementacija poslužitelja VMM u primarni web-mjesta i VMM poslužitelja u sekundarnog web-mjesta.<br/><br/> Možete i [replicirati između oblaka na jednom VMM poslužitelju](site-recovery-single-vmm.md). Da biste to učinili potreban vam je najmanje dva oblaka konfiguriran na poslužitelju VMM.<br/><br/> VMM poslužitelja mora imati najmanje 2012 SP1 centar sustava s najnovijim ažuriranjima.<br/><br/> Svaki VMM poslužitelja mora biti na jednu ili više oblaka konfigurirati i sve oblaka moraju imati profil Hyper-V kapaciteta postavljen. <br/><br/>Oblaka mora sadržavati jednu ili više grupa VMM glavnog računala.<br/><br/>Dodatne informacije o postavljanju oblaka VMM u [konfiguraciji oblaka tkanina VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [vodič: Stvaranje privatne oblaka pomoću sustava centra 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM poslužitelji trebali biste povezani s Internetom. 
**Hyper-V** | Hyper-V poslužitelje morate imati barem Windows Server 2012 s ulogom Hyper-V i instalirali najnovija ažuriranja.<br/><br/> Hyper-V poslužitelja smiju sadržavati jedan ili više VMs.<br/><br/>  Glavno računalo Hyper-V poslužitelji se nalaze na glavno računalo grupe u VMM oblaka primarnih i sekundarnih.<br/><br/> Ako koristite Hyper-V klasteru u sustavu Windows Server 2012 R2 instalirajte [Ažuriranje 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Ako koristite Hyper-V klasteru bilješku Windows Server 2012 te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Morat ćete ručno konfiguriranje broker klaster. [Dodatne informacije potražite u](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Davatelj usluga** | Tijekom implementacije oporavak web-mjesta na poslužiteljima VMM instalirajte davatelja za oporavak Azure web-mjesta. Davatelj putem HTTP 443 da biste orkestrirali replikacije komunicira s oporavak web-mjesta. Replikacija podataka pojavljuje se između Hyper-V poslužitelja primarnih i sekundarnih putem LAN-a ili VPN veza.<br/><br/> Davatelj usluga na poslužitelju VMM treba pristup URL-ove: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Uz to dopustiti komunikaciju vatrozid s poslužitelja VMM [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i omogućuju protokol HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Preduvjeti za mapiranje mreže
Mrežni karte mapiranja između VMM VM mreže na poslužiteljima VMM primarnih i sekundarnih da biste:

- Optimalnog postavite replike VMs na sekundarnom Hyper-V domaćini nakon prebacivanje.
- Povezivanje replike VMs odgovarajuće VM mrežama.
- Ako ne konfigurirate mreže mapiranja replike VMs neće biti povezani s mreže nakon prebacivanje.
- Ako želite postaviti mrežu mapiranja tijekom oporavak web-mjesta implementacije ovdje je što vam je potrebno:

    - Provjerite je li VMs na izvornom poslužitelju Hyper-V glavno računalo povezani s mrežom VMM VM. Tu mrežu mora biti povezan s logičke mreže koji je pridružen oblaka.
    - Provjerite postoji li sekundarne oblak koji će se koristiti za oporavak odgovarajuće VM mreža konfigurirana. Tu mrežu VM mora biti povezan s logičke mreže povezane sa sekundarnom oblaka.


Dodatne informacije o konfiguriranju VMM mreža u u nastavku članci

- [Konfiguriranje logičke mreža u VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Konfiguriranje VM mreža i pristupnika u VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Dodatne informacije](site-recovery-network-mapping.md) o načinu funkcioniranja a preslikavanje.

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

1. Stvaranje grupe za resurse sustava Voditelj resursa Azure ako ga nemate već

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Stvorite novi oporavak servisa sigurnog i spremite stvoreni objekt sigurnog ASR u varijablu (koristit će se kasnije). Također možete dohvatiti ASR sigurnog objekt objavu stvaranje pomoću cmdleta Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Korak 3: Postavljanje konteksta oporavak servisa sigurnog

1.  Ako imate sigurnog već stvorili, pokrenite na ispod naredbu da biste na zbirke ključeva.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Postavljanje konteksta sigurnog tako da pokrenete u nastavku naredba.

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


## <a name="step-5-create-and-associate-a-replication-policy"></a>Korak 5: Stvaranje i pridruživanje pravilo replikacije

1.  Stvaranje pravila za replikaciju Hyper-V 2012 R2 ponovnim pokretanjem sljedeće naredbe:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] Oblak VMM može sadržavati Hyper-V domaćini s različitim verzijama sustava Windows Server (kao što je rečeno u preduvjeti Hyper-V), ali pravila replikacije je određenih verzija OS-a. Ako imate različite domaćini sustavom verzije drugi operacijski sustav, stvorite zaseban replikacije pravila za svaku vrstu verzija OS-a. Za npr: Ako imate pet domaćini sustavom Windows poslužitelje 2012 i tri na Windows Server 2012 R2, stvoriti dvije replikacije pravila – jedan za svaku vrstu verzija operacijskog sustava.

2.  Početak primarni zaštitu kontejner (primarni VMM oblaka) i oporavak zaštitu spremnik (oporavak VMM oblaka) ponovnim pokretanjem sljedeće naredbe:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Dohvaćanje pravila stvorenom u prvom koraku 1 pomoću neslužbeni naziv pravila

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Da biste pokrenuli pridruživanje spremnika zaštitu (VMM oblaka) s replikacije pravila:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Pričekajte dovršetak posla pridruživanja pravila. Ako je zadatak dovršen pomoću sljedeće komponente PowerShell isječak možete provjeriti.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Kada se zadatak završio obrada, pokrenite sljedeću naredbu:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Da biste provjerili nakon dovršetka postupka, slijedite korake u [Nadzornik aktivnosti](#monitor).

## <a name="step-5-configure-network-mapping"></a>Korak 5: Konfiguriranje mapiranja mreže

1. Prva naredba dobiva poslužitelji za trenutni sigurnog oporavak Azure web-mjesta. Naredba pohranjuje poslužitelje sustava Microsoft Azure web-mjesta oporavak varijabla polja $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer

2. U ispod naredbi nabaviti mreže oporavak web-mjesta za web-mjesto s izvorišnim poslužiteljem VMM i u okvir za VMM poslužitelju ciljni.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] Izvorni poslužitelj VMM može biti prve ili drugu u polju poslužitelji. Provjerite imena poslužitelje VMM i prikladno dobiti mreže


4. Konačni cmdlet stvara mapiranja između primarni mreže i oporavak mreže. Cmdlet određuje primarnu mreže kao prvi element $PrimaryNetworks i oporavak mrežu kao prvi element $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Korak 6: Konfiguriranje mapiranja prostora za pohranu

1. U ispod naredba može vidjeti popis klasifikacije prostora za pohranu u varijablu $storageclassifications.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. U ispod naredbi dobiti klasifikacija izvora u $SourceClassificaion varijabla i ciljne klasifikacija u varijablu $TargetClassification. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Izvorišno i odredišno klasifikacije može biti bilo kojeg elementa u polju. Pogledajte Izlaz na ispod naredbi na indeks izvorišno i odredišno klasifikacije u polju $storageclassifications. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Odaberite objekt - svojstvo FriendlyName, Id | Oblikovanje tablice


3. U nastavku cmdlet stvara mapiranja između klasifikacija izvora i klasifikacija cilj. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Korak 7: Omogući zaštitu za virtualnim strojevima

Nakon poslužitelje, oblaka i mreža ispravno konfigurirani, možete omogućiti zaštitu virtualnih računala u oblaku. 


  1. Da biste omogućili zaštitu, pokrenite sljedeću naredbu da biste dobili spremnik zaštite:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Da biste dobili zaštitu entitet (VM) ponovnim pokretanjem sljedeće naredbe:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Omogućivanje replikacije za na VM ponovnim pokretanjem sljedeće naredbe:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Provjera uvođenja

Da biste testirali implementaciju sustava možete pokrenuti prebacivanje test za jednu virtualnog računala ili stvorite plan oporavak koji se sastoji od više virtualnim strojevima i pokrenite test prebacivanje plana. Prebacivanje test simulira prebacivanje i oporavak mehanizam Izolirani mreže. 

> [AZURE.NOTE] Stvorite plan oporavak aplikacije za Azure portalu.

Da biste provjerili nakon dovršetka postupka, slijedite korake u [Nadzornik aktivnosti](#monitor).


### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

1.  Pokretanje u nastavku Cmdlete da biste dobili VM mreže koju želite testirati prebacivanje na VMs da biste.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Izvođenje testiranja prebacivanje od na VM tako da učinite sljedeće:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Izvođenje testiranja prebacivanje tarifa za oporavak tako da učinite sljedeće:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Pokretanje planiranog prebacivanje

1. Izvođenje na planirane prebacivanje na VM tako da učinite sljedeće:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Izvođenje planirane prebacivanje tarifa za oporavak tako da učinite sljedeće:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Pokretanje neplanirano prebacivanje

1. Izvođenje neplanirano prebacivanje od na VM tako da učinite sljedeće:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. izvođenje neplanirano prebacivanje tarifa za oporavak tako da učinite sljedeće:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

