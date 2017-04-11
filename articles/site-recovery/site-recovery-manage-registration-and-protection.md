<properties
    pageTitle="Uklanjanje poslužitelja i onemogućiti zaštitu | Microsoft Azure" 
    description="U ovom se članku opisuje način Odjava poslužitelje iz zbirke ključeva za oporavak web-mjesta, a da biste onemogućili zaštitu virtualnim strojevima i fizičke poslužiteljima." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Uklanjanje poslužitelja i onemogućiti zaštitu

Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md)

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako Odjava poslužitelje iz zbirke ključeva za oporavak web-mjesta i onemogućiti zaštitu virtualnim strojevima zaštićen oporavak web-mjesta. 

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="unregister-a-vmm-server"></a>Unregister VMM poslužitelja

Odjaviti VMM poslužitelja iz programa sigurnog brisanjem Normalni prikaz na kartici **poslužitelji** na portalu za oporavak Azure web-mjesta. Imajte na umu da:

-  **Povezani VMM poslužitelja**: preporučujemo unregister poslužitelja VMM kada je povezano s Azure. Time se osigurava da postavke poslužitelja na lokaciji VMM i poslužitelji VMM pridružen (VMM poslužiteljima koji sadrže oblaka koja su mapirana oblaka na poslužitelju koji želite izbrisati) očistiti pravilno. Preporučujemo vam da samo uklonite Ulazite poslužitelju ako postoji trajna problem s povezivanjem.
- **Ulazite VMM poslužitelja**: Ako poslužitelj VMM nije povezan nakon što izbrišete morat ćete pokrenuti skriptu ručno da biste izvršili za čišćenje diska. Skripta dostupan je u [Microsoftovu galeriju](http://aka.ms/asr-cleanup-script-vmm). Zabilježite ID VMM poslužitelja da biste dovršili postupak ručno čišćenje.
- **VMM poslužitelja u skupini**: Ako vam je potrebna odjava VMM poslužitelj koji je implementiran klasteru učinite sljedeće:

    - Ako je povezani poslužitelj, izbrišite povezanog poslužitelja VMM na kartici **poslužitelji** . Da biste deinstalirali davatelja usluge na poslužitelju, prijavite se svakoj klaster čvor i deinstalirati putem upravljačke ploče. Pokrenuti skriptu čišćenje referencirani u prethodnom odjeljku na sve pasivni čvorove u skupini da biste izbrisali unosi registracije.
    - Ako nije povezan s poslužiteljem morat ćete pokrenuti skriptu čišćenja na sve čvorove klaster.

### <a name="unregister-an-unconnected-vmm-server"></a>Unregister poslužitelj za Ulazite VMM

Na poslužitelju VMM koje želite ukloniti:

1. Odjaviti VMM poslužitelj s portala za Azure.
2. Na poslužitelju VMM preuzmite skripte za čišćenje diska.
3. Otvorite PowerShell s Pokreni kao Administrator mogućnost da biste promijenili izvođenja pravila za zadani (LocalMachine) opseg.
4. Slijedite upute u skripti. 

Na poslužiteljima VMM koji imaju oblaka koja su u paru s oblaka na poslužitelju koji uklanjate:

1. Pokrenuti skriptu čišćenje diska, a zatim slijedite korake 2 do 4.
2. Određivanje VMM ID-a za poslužitelj VMM koji nije registriran. 
3. Ova skripta uklonit će informacije o registraciji VMM poslužitelja i oblaka uparivanje informacije.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Unregister Hyper-V poslužitelja u web-mjestu tehnologije Hyper-V

Kada je oporavak Azure web-mjesta implementiran da biste zaštitili virtualnim strojevima koji se nalazi na poslužitelju Hyper-V na web-mjestu tehnologije Hyper-V (s poslužitelj ne VMM) poslužitelja Hyper-V iz programa sigurnog unregister na sljedeći način:

1. Onemogućivanje zaštitu virtualnih računala koja se nalazi na poslužitelju Hyper-V.
2. Na kartici **poslužitelji** na portalu za oporavak Azure web-mjesta odaberite poslužitelj > Izbriši. Poslužitelj ne mora biti povezano s Azure kada to učinite.
3. Pokrenite sljedeću skriptu da biste očistili postavke na poslužitelju i unregister iz na zbirke ključeva. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Zaustavi zaštitu Hyper-V virtualnog računala

Ako želite prestati zaštita Hyper-V virtualnog računala morat ćete da biste uklonili zaštitu za njega. Ovisno o kako ukloniti zaštitu bilo bi dobro da biste očistili postavke zaštite ručno na računalu. 

### <a name="remove-protection"></a>Uklanjanje zaštite

1. Na kartici **virtualnim strojevima** svojstva oblaka odaberite virtualnog računala > **Ukloni**.
2. Na stranici **potvrdite uklanjanje od virtualnog računala** , imate nekoliko mogućnosti:

    - **Onemogućivanje zaštitu**– Ako omogućite i spremiti tu mogućnost virtualnog računala više neće biti zaštićene pregledavanjem oporavak web-mjesta. Postavke zaštite za virtualnog računala će se očistiti automatski.
    - **Uklanjanje na sigurnog**– Ako odaberete tu mogućnost virtualnog računala uklonit će se samo iz zbirke ključeva za oporavak web-mjesta. Neće utjecati lokalne postavke zaštite za virtualnog računala. Morat ćete očistiti postavki ručno Uklanjanje postavki zaštite i uklanjanje virtualnog računala Azure pretplate i uklanjanje postavki zaštite morat ćete očistiti ih ručno pomoću upute u nastavku.

Ako odaberete da biste izbrisali virtualnog računala i njegov tvrdom disku bit će uklonjen s odredišnog mjesta.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Čišćenje postavke zaštite ručno (između VMM web-mjesta)

Ako ste odabrali **prekinuti upravljanje virtualnog računala**, ručno očistiti postavke:

1. Na konzoli sustava VMM da biste očistili postavke za primarni virtualnog računala, pokrenite ovaj skripte primarni poslužitelj. Na konzoli VMM kliknite gumb PowerShell da biste otvorili konzolu za VMM PowerShell. Zamijenite SQLVM1 naziv virtualnog računala.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Na sekundarnom VMM poslužitelj pokrenite ovaj skriptu da biste očistili postavke za sekundarne virtualnog računala:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Na poslužitelju sustava za sekundarne VMM nakon pokretanja skriptu osvježite virtualnim strojevima na poslužitelju za glavno računalo Hyper-V tako da se dobiva u konzole za VMM ponovno otkriven sekundarne virtualnog računala.
4. Gore navedeni koraci će očistiti replikacije postavke isključivo VMM poslužitelj. Ako želite ukloniti replikacije virtualnog računala za virtualnog računala. Morat ćete učiniti dolje navedene korake na oba primarni i sekundarni virtualnih računala. Pokretanje u nastavku skriptu da biste uklonili replikacije i zamijenili SQLVM1 pod nazivom virtualnog računala.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Čišćenje postavke zaštite ručno (između lokalnog VMM web-mjesta i Azure)

1. Na poslužitelju za izvor VMM pokrenite ovaj skriptu da biste očistili postavke za primarni virtualnog računala:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Gore navedeni koraci će očistiti replikacije postavke isključivo VMM poslužitelj. Nakon što uklonite replikacije VMM poslužitelja provjerite je li da biste uklonili replikacije za virtualnog računala koja se izvodi na poslužitelju Hyper-V glavno računalo s ovu skriptu. Zamijenite nazivom virtualnog računala i host01.contoso.com pod nazivom glavnog računala poslužitelja Hyper-V SQLVM1.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Čišćenje postavke zaštite ručno (između Hyper-V web-mjesta i Azure)

1. Na poslužitelju za glavno računalo Hyper-V izvora da biste uklonili replikacije za virtualnog računala pomoću sljedeće skripte. Zamijenite SQLVM1 naziv virtualnog računala.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Zaustavi zaštitu VMware virtualnog računala ili na poslužitelju za fizičke

Ako želite prestati zaštita VMware virtualnog računala ili na poslužitelju za fizičke morat ćete da biste uklonili zaštitu za njega. Ovisno o kako ukloniti zaštitu bilo bi dobro da biste očistili postavke zaštite ručno na računalu. 

### <a name="remove-protection"></a>Uklanjanje zaštite

1. Na kartici **virtualnim strojevima** svojstva oblaka odaberite virtualnog računala > **Ukloni**.
2. **Uklanjanje virtualnog računala** odaberite neku od mogućnosti:

    - **Onemogućivanje zaštitu (koristite za promjenu veličine dubinske analize i glasnoću oporavak)**– samo ćete vidjeti i omogućite tu mogućnost ako ste:
        - **Resized glasnoće virtualnog računala**– prilikom mijenjanja veličine glasnoću virtualnog računala prelazi u stanje od ključne važnosti. Tu mogućnost odaberite ako se to dogodi. Onemogućava zaštitu ali zadržava oporavak točke u Azure. Kada ponovno omogućili zaštitu za računalo podataka promijenjene veličine jedinice će se prenijeti Azure.
        - **Pokretanje programa prebacivanje**– kada ste testirati vaše okruženje tako da pokrenete u prebacivanje iz lokalnog VMware virtualnim strojevima ili fizičke poslužitelje Azure, odaberite ovu mogućnost da biste pokrenuli ponovno zaštita lokalnih virtualnim strojevima sa sustavom. Ta mogućnost onemogućuje svaki virtualnog računala, a zatim morat ćete ponovno omogućili zaštitu za njih. Imajte na umu da:
            - Onemogućivanje virtualnog računala s ta postavka utječe na virtualnog računala replike u Azure.
            - Servis za mobilnost mustn't Deinstalacija s virtualnog računala.
    
    - **Onemogućivanje zaštitu**– Ako omogućite i spremiti tu mogućnost na računalu će više neće biti zaštićen oporavak web-mjesta. Postavke zaštite za računalo će se očistiti automatski.
    - **Uklanjanje na sigurnog**– Ako odaberete tu mogućnost računalu uklonit će se samo iz zbirke ključeva za oporavak web-mjesta. Postavke zaštite od lokalnog za računalo neće ukloniti. Da biste uklonili postavke na računalu, a da biste uklonili virtualnog računala na Azure pretplate, a morat ćete očistiti postavke Deinstaliranjem mobilnost servisa.
    
        ![Uklanjanje mogućnosti](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















