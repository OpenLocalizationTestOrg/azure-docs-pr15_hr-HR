<properties
    pageTitle="Zaštita poslužitelji za Azure pomoću komponente PowerShell Azure s Azure Voditelj resursa | Microsoft Azure"
    description="Automatiziranje zaštitu poslužitelji za Azure s oporavak Azure web-mjesta pomoću komponente PowerShell i upravljanja resursima Azure."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replicirati između lokalnog Hyper-V virtualnim strojevima i Azure pomoću komponente PowerShell i upravljanja resursima za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell – Voditelj resursa](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klasični Portal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Pregled

Oporavak Azure web-mjesta doprinosa tvrtke continuity i Izrada oporavak strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima broj scenariji za implementaciju. Potpuni popis scenariji za implementaciju potražite u članku [Pregled oporavak Azure web-mjesta](site-recovery-overview.md).

Azure PowerShell je modul koji sadrži cmdleti za upravljanje Azure putem komponente Windows PowerShell. Možete raditi dvije vrste modula: Modul Azure profila ili modul Azure Voditelj resursa.

Web-mjesta oporavak cmdleta ljuske PowerShell, dostupno u sklopu Azure PowerShell za Upravitelj Azure resursa, pomoć zaštitu i oporavak poslužitelja u Azure.

U ovom se članku opisuje kako pomoću komponente Windows PowerShell, zajedno s Upravitelj Azure resursa, za implementaciju oporavak web-mjesta možete konfigurirati i orkestrirali zaštitu server Azure. Primjer koristi u ovom se članku objašnjava da biste zaštitili, neće uspjeti iznad i oporavak virtualnim strojevima na glavnom računalu Hyper-V tako da biste Azure, pomoću komponente PowerShell Azure s Azure Voditelj resursa.

> [AZURE.NOTE] Cmdleta ljuske PowerShell za oporavak web-mjesto trenutno omogućuju vam da biste konfigurirali sljedeće: jednog virtualnog računala Upravitelj web-mjesta na drugo, virtualnog računala Upravitelj web-mjesta za Azure i web-mjestu tehnologije Hyper-V Azure.

Ne morate biti na PowerShell stručnjak za korištenje ovog članka, ali potrebno je razumjeti osnovni koncepti, kao što su module, cmdleta i sesije. Dodatne informacije o komponente Windows PowerShell potražite u članku [Početak rada s komponentom Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Također možete potražite dodatne informacije o [Korištenju Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Microsoftovih partnera koje su dio programa oblaka rješenje davatelj (CSP) možete konfigurirati i upravljati zaštitu svoje kupce poslužitelje za svoje kupce odgovarajući CSP pretplate (pretplate za klijent).

## <a name="before-you-start"></a>Prije početka

Provjerite je li te preduvjeti na mjestu:

- Račun za [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). Osim toga, možete čitati o [cijene Upravitelj za oporavak Azure web-mjesta](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Informacije o ovom izdanju i kako ga instalirati potražite u članku [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- Moduli [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) i [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) . Najnoviju verziju sustava te module možete pristupiti iz [galerije PowerShell](https://www.powershellgallery.com/)

U ovom se članku prikazuje kako pomoću ljuske Powershell Azure s Azure Voditelj resursa za konfiguriranje i upravljanje zaštita poslužitelja. Primjer koristi u ovom se članku objašnjava da biste zaštitili virtualnog računala na glavno računalo Hyper-V, u Azure. U ovom se primjeru karakteristične sljedeće preduvjete. Detaljnije skup preduvjeti za različite scenariji za oporavak web-mjesta potražite u dokumentaciji vezani uz u taj scenarij.

- Hyper-V glavno računalo sa sustavom Windows Server 2012 R2 ili koja sadrži jedan ili više virtualnim računalima sustava Microsoft Hyper-V Server 2012 R2.
- Poslužitelji Hyper-V povezani s Internetom izravno ili putem proxy poslužitelj.
- Virtualnim strojevima koji želite zaštititi mora biti u skladu s [preduvjeti virtualnog računala](site-recovery-best-practices.md#virtual-machines).


## <a name="step-1-sign-in-to-your-azure-account"></a>Korak 1: Prijavite se na račun za Azure


1. Otvorite konzolu za PowerShell i pokrenite sljedeću naredbu da biste se prijavili račun za Azure. Cmdlet pojavljuju se na web-stranicu koja će vas tražiti vjerodajnice za račun.

        Login-AzureRmAccount

    Umjesto toga možete navesti vjerodajnice za račun kao parametar u `Login-AzureRmAccount` cmdlet pomoću na `-Credential` parametar.

    Ako ste partnera CSP rad ime klijenta, navedite kupca kao klijent, pomoću njihov naziv primarne domene tenantID ili klijent.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Račun može imati više pretplata, da bi se trebali biste pridružiti pretplatu koju želite koristiti s računom.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Provjerite je li vaša pretplata registriran za korištenje Azure davatelja usluga za oporavak i oporavak web-mjesta pomoću sljedeće naredbe:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    U rezultatu naredbe, ako je **RegistrationState** postavljen na **registriran**, možete nastaviti korak 2. Ako nije, nedostaju davatelja trebala bi se registrirati za pretplatu.

    Da biste registrirali Azure davatelj usluga za oporavak web-mjesta i servisi za oporavak, pokrenite sljedeće naredbe:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Provjerite je li uspješno registriran za davatelje pomoću sljedeće naredbe: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` i `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Korak 2: Postavljanje oporavak servisa sigurnog

1. Stvaranje grupe resursa sustava Azure Voditelj resursa u kojem ćete stvoriti na sigurnog, ili koristite postojeću grupu resursa. Možete stvoriti novu grupu resursa pomoću sljedeće naredbe:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    gdje varijabla $ResourceGroupName sadrži naziv koji želite stvoriti grupu resursa, a varijabla $Geo Azure regija u kojoj želite stvoriti grupu resursa (na primjer, "Brazil Jug").

    Popis grupa resursa u svoju pretplatu možete dobiti korištenjem na `Get-AzureRmResourceGroup` cmdlet.

2. Stvaranje nove zbirke ključeva servisa Azure oporavak na sljedeći način:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Možete dohvatiti popis postojeće sefovi pomoću na `Get-AzureRmRecoveryServicesVault` cmdlet.

> [AZURE.NOTE] Ako želite izvođenje operacije na sefovi oporavak web-mjesta stvorili pomoću portala za klasični ili modul ljuske PowerShell za upravljanje servisa Azure možete dohvatiti popis takvih sefovi pomoću na `Get-AzureRmSiteRecoveryVault` cmdlet. Stvorite novi sigurnog oporavak servise za sve nove operacije. Sefovi oporavak web-mjesta koje ste stvorili ranije podržani, no nemate najnovije značajke.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Korak 3: Postavljanje oporavak servisa sigurnog kontekstu

1.  Postavljanje konteksta sigurnog pokretanjem sljedeće naredbe:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Korak 4: Stvaranje web-mjesta Hyper-V i stvoriti novi ključ Registracija sigurnog web-mjesta.

1. Stvaranje novog web-mjesta Hyper-V na sljedeći način:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Ovaj cmdlet pokreće zadatka Oporavak web-mjesta za stvaranje web-mjesta i vraća objekt zadatka Oporavak web-mjesta. Pričekajte zadatak da biste dovršili i provjerite zadatak uspješno dovršena.

    Možete dohvatiti objekt posla, a taj način provjerite trenutno stanje posla, pomoću cmdleta Get-AzureRmSiteRecoveryJob.
2. Stvaranje i preuzeli ključa za registraciju za web-mjesto na sljedeći način:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopirajte preuzetu ključ glavno računalo za Hyper-V. Potreban vam je ključ da biste registrirali Hyper-V glavno računalo web-mjesto.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Korak 5: Instalacija oporavak Azure web-mjesta davatelja i agenta servisa za oporavak Azure na vašem računalu koje hostira Hyper-V

1. Preuzmite instalacijski program za najnoviju verziju davatelja usluge tvrtke [Microsoft](https://aka.ms/downloaddra).

2. Pokreni instalacijski program na vašem računalu koje hostira Hyper-V i na kraju instalacije prijeđite na korak registracije.

3. Kada se to od vas zatraži, navedite ključa za registraciju preuzete web-mjesta i dovršavanje Registracija Hyper-V glavno računalo web-mjesto.

4. Provjerite je li voditelju Hyper-V registriran na web-mjesto pomoću sljedeće naredbe:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Korak 6: Stvaranje pravila za replikaciju i pridružiti spremnik zaštitu

1. Stvaranje pravila replikacije na sljedeći način:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Provjerite vraćeni posla da biste bili sigurni uspješnog replikacije stvaranje pravila.

    >[AZURE.IMPORTANT] Račun za pohranu koji je naveden mora biti u istom Azure području kao vaše oporavak servisa sigurnog, a trebali biste dobiti zemlj replikacije omogućena.
    >
    > - Ako su navedeni račun za pohranu za oporavak vrsta Azure prostora za pohranu (klasični), prebacivanje zaštićeni strojeva oporavak računala za Azure IaaS (klasični).
    > - Ako su navedeni račun za pohranu za oporavak vrsta Azure prostora za pohranu (Voditelj resursa za Azure), prebacivanje zaštićeni strojeva oporaviti računala za Azure IaaS (Voditelj resursa za Azure).

2. Pojavljuje se spremnik zaštitu koja odgovara web-mjestu, na sljedeći način:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Započnite pridruživanje spremnik zaštitu pravila replikacije na sljedeći način:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Pričekajte dovršetak posla pridruživanje i bili sigurni da je uspješno dovršena.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Korak 7: Omogući zaštitu za virtualnim strojevima

1. Pojavljuje se entitet zaštitu odgovara VM koji želite zaštititi, na sljedeći način:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Pokrenite zaštita virtualnog računala na sljedeći način:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Račun za pohranu koji je naveden mora biti u istom Azure području kao vaše oporavak servisa sigurnog, a trebali biste dobiti zemlj replikacije omogućena.
    >
    > - Ako su navedeni račun za pohranu za oporavak vrsta Azure prostora za pohranu (klasični), prebacivanje zaštićeni strojeva oporavak računala za Azure IaaS (klasični).
    > - Ako su navedeni račun za pohranu za oporavak vrsta Azure prostora za pohranu (Voditelj resursa za Azure), prebacivanje zaštićeni strojeva oporavak računala za Azure IaaS (Voditelj resursa za Azure).

    > Ako VM su zaštita ima više od jednog diska povezan, navedite disk operacijski sustav pomoću *OSDiskName* parametra.

3. Pričekajte virtualnim strojevima dosegne zaštićeno stanje nakon početnog replikacije. To može potrajati neko vrijeme, ovisno o čimbenicima kao što su količinu podataka da biste je replicirati i dostupne upstream propusnosti za Azure. Stanje posla i StateDescription će se ažurirati na sljedeći način, nakon VM Dostizanje zaštićeno stanje.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Ažuriranje svojstava za oporavak, kao što su veličina uloga VM i Azure mreže da biste priložili virtualnog računala mrežne kartice da biste nakon prebacivanje.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Korak 8: Pokrenite test prebacivanje

1. Pokrenite test posao za prebacivanje na sljedeći način:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Provjerite da test VM se stvara u Azure. (Prebacivanje posao test obustavljena je, nakon stvaranja test VM u Azure. Posao dovršava po čišćenje stvoren artefacts nakon nastavljanje posla, kao što je prikazano u sljedećem koraku.)

3. Prebacivanje test ispunite na sljedeći način:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Daljnji koraci

[Dodatne informacije potražite u](https://msdn.microsoft.com/library/azure/mt637930.aspx) oporavak web-mjesta Azure pomoću cmdleta ljuske PowerShell za upravljanje resursima Azure.
