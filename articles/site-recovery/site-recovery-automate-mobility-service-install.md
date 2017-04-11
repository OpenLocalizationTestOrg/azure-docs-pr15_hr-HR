<properties
    pageTitle="Replicirati VMware virtualnim strojevima za Azure pomoću web-mjesta oporavak Azure Automatizacija DSC | Microsoft Azure"
    description="U članku se opisuje pomoću Azure Automatizacija DSC automatski Implementacija servisa Azure web-mjesta oporavak mobilnost i Azure agent za virtualne/fizičke strojeva Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Replicirati VMware virtualnim strojevima za Azure pomoću web-mjesta oporavak DSC Automatizacija Azure

U paketu za upravljanje operacije, možemo vam ponuditi potpun sigurnosnu kopiju i rješenja za oporavak Izrada koje možete koristiti kao dio poslovni plan za continuity.

Ne možemo pokrenuti ovaj putovanja zajedno s Hyper-V pomoću značajke Hyper-V replike. No ćemo imati proširiti tako da podržava heterogenih postavljanje jer klijenti u njihove oblaka imaju više hypervisors i platformama.

Ako koristite VMware radnih opterećenja i/ili fizičke poslužitelje danas, na poslužitelju za upravljanje pokreće sve komponente za oporavak Azure web-mjesta u svom okruženju za rukovanje replikacije komunikacije i podatke s Azure, kada je Azure odredištu.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Implementacija servisa mobilnost oporavak web-mjesta pomoću DSC automatizacije
Započnimo tako da učinite brzi razrada svega što označava poslužitelju za upravljanje.

Poslužitelj za upravljanje pokreće nekoliko uloge poslužitelja. Jednu od tih uloga je *konfiguracije*koji koordinate komunikacije i upravlja postupaka replikacije i oporavak podataka.

Uz to, uloga *postupak* služi kao replikacije pristupnika. Ta uloga prima replikacije podatke iz zaštićenog izvor strojeva, optimizira predmemoriranja, sažimanja i šifriranje i šalje račun za Azure prostora za pohranu. Jedna od funkcija uloge postupak je i automatske instalacije servisa mobilnost na zaštićenom računalima i obaviti automatsko otkrivanje VMware VMs.

Ako postoji failback iz Azure, uloge *osnovne ciljne* će rukovati podacima replikacije kao dio ovog postupka.

Za zaštićeni strojeva smo oslanjate *mobilnost servisa*. Komponenta je implementiran na svakom računalu (VMware VM ili poslužitelj za fizičke) koje želite replicirati Azure. Snima zapisivanje podataka na računalu i prosljeđuje poslužitelj za upravljanje (postupak uloga).

Kada se bave poslovanje, važno je da biste shvatili vaše radnih opterećenja, vaše Infrastruktura i komponente koji je uključen. Zatim možete zadovoljava preduvjete za oporavak vrijeme cilj (RTO) i oporavak točke cilj (RPO). U ovom kontekstu mobilnost servis je ključ zajamčiti na radnih opterećenja zaštićene kao što biste očekivali.

Kako mogu, optimizirana način, bili sigurni da imate pouzdanog zaštićeni postavljanje pomoći iz neke komponente operacije upravljanja paket?

Ovaj članak sadrži primjera kako koji omogućuju Azure Automatizacija želji stanje konfiguracije (DSC), zajedno s oporavak web-mjesta, provjerite:

- Servis mobilnost i Azure VM agent su implementiran na računalima sustava Windows koji želite zaštititi.
- Servis mobilnost i Azure VM agent uvijek koristite kada je Azure replikacije cilj.

## <a name="prerequisites"></a>Preduvjeti

- Spremište za pohranu potrebno postavljanje
- Spremište za pohranu potrebno pristupni izraz za registriranje poslužitelju za upravljanje

 > [AZURE.NOTE] Za svaki poslužitelj za upravljanje generira se jedinstvena pristupni izraz. Ako namjeravate uvesti više poslužitelji za upravljanje, imate provjerite je li točan pristupni izraz pohranjen u datoteci passphrase.txt.

- Windows Management Framework (WMF) 5.0 instalirana na računalima koji želite omogućiti za zaštitu (obavezu Automatizacija DSC)

 > [AZURE.NOTE] Ako želite koristiti strojevima DSC za Windows koji imaju WMF 4.0 instaliran potražite u odjeljku [Korištenje DSC u okruženju prekinuta veza](#Use DSC in disconnected environments).

Servis za mobilnost moguće je instalirati putem naredbenog retka i prihvaća nekoliko argumenata. Zbog morate imati u binarne datoteke (nakon izdvajanje ih iz postavu) i pohraniti na mjestu gdje ih možete dohvatiti pomoću DSC konfiguracije.

## <a name="step-1-extract-binaries"></a>Korak 1: Binarne datoteke za izdvajanje

1. Da biste izdvojili datoteke koje su vam potrebne za ovaj će instalacijski program, idite na sljedeći direktorij na poslužitelju za upravljanje:

    **Recovery\home\svsystems\pushinstallsvc\repository \Microsoft azure web-mjesta**

    U toj mapi, trebali biste vidjeti na MSI datoteku pod nazivom:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Da biste izdvojili instalacijski program, koristite sljedeću naredbu:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Odaberite sve datoteke i pošaljite mu komprimiranu mapu (zipane).

Sada imate binarne datoteke koje su vam potrebne da biste automatizirali postavljanje servisa mobilnost pomoću DSC automatizaciju.

### <a name="passphrase"></a>Pristupni izraz

Nakon toga morate da biste odredili mjesto na koje želite smjestiti ovaj zipane mapu. Račun za Azure prostora za pohranu, možete koristiti kao što je prikazano kasnije, za pohranu pristupni izraz koje su vam potrebne za instalaciju. Agenta pa će registrirati s poslužitelja za upravljanje u sklopu postupka.

Pristupni izraz koju ste dobili prilikom implementiran poslužitelju za upravljanje moguće je spremiti u tekstnu datoteku kao passphrase.txt.

Postavite zipana mapa i pristupni izraz u spremniku namjenski u račun za Azure prostora za pohranu.

![Mjesto mape](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Ako biste radije da biste zadržali te datoteke na zajedničko korištenje na mreži, to možete učiniti. Samo želite li DSC resurs koji ćete koristiti kasnije ima pristup i možete dobiti postavljanje i pristupni izraz.

## <a name="step-2-create-the-dsc-configuration"></a>Korak 2: Stvaranje DSC konfiguraciju

Postavljanje ovisi o WMF 5.0. Za računalo pimjene konfiguraciju kroz Automatizacija DSC WMF 5.0 mora postojati.

Okruženje koristi sljedeću konfiguraciju DSC primjer:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Konfiguracija će učinite sljedeće:

- Varijable će govori konfiguraciju gdje da biste dobili u binarne datoteke za servis za mobilnost i agent za Azure VM gdje da biste dobili pristupni izraz i mjesto za pohranu izlaz.
- Konfiguracija uvoz resursa xPSDesiredStateConfiguration DSC da bi mogli koristiti `xRemoteFile` za preuzimanje datoteka u spremištu.
- Konfiguracija će stvoriti direktorij mjesto na koje želite spremiti u binarne datoteke.
- Arhiviranje resursa će izdvojiti datoteke iz zipane mape.
- Paket resursa za instalaciju instalirat će se servis mobilnost iz na UNIFIEDAGENT. EXE installer s konkretnih argumenata. (Varijabli koje sastavljanje argumenti morati mijenjati u skladu s vizualnim vaše okruženje.)
- Paket AzureAgent resursa instalirat će agent Azure VM, preporučuje se na svakoj VM koji se izvodi u Azure. Agent za Azure VM i omogućuje da biste dodali proširenja na VM nakon prebacivanje.
- Resurs za servis ili resurse će osigurati da servise vezane uz mobilnost i servisa Azure uvijek pokrenuti.

Konfiguracija spremanje u obliku **ASRMobilityService**.

>[AZURE.NOTE] Imajte na umu da biste zamijenili CSIP u konfiguraciji u skladu s vizualnim poslužitelja za stvarni upravljanje tako da agenta će ispravno povezani i će koristiti točan pristupni izraz.

## <a name="step-3-upload-to-automation-dsc"></a>Korak 3: Prijenos Automatizacija DSC

Jer DSC konfiguracije koje ste napravili će uvesti potreban modul DSC resursa (xPSDesiredStateConfiguration), morate uvesti taj modul u automatizacije prije prijenosa konfiguracije DSC.

Prijavite se na račun za automatizaciju, otiđite do **imovine** > **module**, kliknite **Pregledaj galerije**.

Ovdje možete potražiti modul i uvesti na račun.

![Modul za uvoz](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Kada završite s time, idite na računalu na kojem imate modula Azure Voditelj resursa instaliran, a zatim nastavite s uvesti konfiguraciju novostvorenu DSC.

### <a name="import-cmdlets"></a>Cmdleti za uvoz

U ljusci PowerShell, prijavite se u pretplatu za Azure. Izmjena Cmdlete da biste odražavaju okruženju sustava i snimite Automatizacija podataka o računu u varijablu:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Prenesite konfiguraciju Automatizacija DSC pomoću sljedeći cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Kompiliranje konfiguraciju u DSC automatizacije

Nakon toga ćete morati Kompiliranje konfiguraciju u DSC Automatizacija, tako da možete pokrenuti da biste registrirali čvorove na njega. Koje postići tako da pokrenete sljedeći cmdlet:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Budući da ste zapravo implementacije konfiguraciju za servis istaknuti DSC to može potrajati nekoliko minuta.

Nakon Kompiliranje konfiguraciju, možete dohvatiti informacije o zadatku pomoću komponente PowerShell (Get-AzureRmAutomationDscCompilationJob) ili pomoću [portala za Azure](https://portal.azure.com/).

![Dohvaćanje posla](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Imate sada uspješno objaviti i prenijeli konfiguraciju DSC DSC automatizaciju.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Korak 4: Onboard strojeva DSC automatizacije
>[AZURE.NOTE] Neki preduvjeti za dovršavanje scenarij je najnoviju verziju preglednika WMF ažuriraju strojevima sustava Windows. Možete preuzeti i instalirati odgovarajuću verziju za svoju platformu iz [Centra za preuzimanje](https://www.microsoft.com/download/details.aspx?id=50395).

Sada će stvoriti na metaconfig za DSC koji će se primijeniti na vašem čvorove. Da biste uspije uz to, morate dohvaćanje URL krajnjoj točki i primarni ključ za račun Automatizacija odabrane u Azure. Možete pronaći te vrijednosti u odjeljku **tipke** na plohu **sve postavke** za automatizaciju račun.

![Vrijednosti ključa](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

U ovom primjeru imate Windows Server 2012 R2 fizičke poslužitelju koji želite zaštititi pomoću oporavak web-mjesta.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Provjera za sve operacije Preimenovanje datoteke u registru na čekanju

Prije nego što pokrenete poslužiteljem pridružiti krajnju točku DSC Automatizacija, preporučujemo da provjerite za sve operacije Preimenovanje datoteke u registru na čekanju. Postavljanje oni sprječavaju iz dovršetka zbog neriješeno ponovno pokretanje.

Pokrenite sljedeći cmdlet da biste provjerili ima li bez neriješeno ponovno pokretanje na poslužitelju:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Ovo prikazuje u prazan, radite li u redu da biste nastavili. Ako nije, trebali biste to adresa po ponovnog pokretanja poslužitelj tijekom održavanja prozora.

Da biste primijenili konfiguraciju na poslužitelju, pokrenite na Očisti integrirani skriptiranje okruženje (filtar) i pokrenite sljedeću skriptu. Ovo je zapravo DSC lokalne konfiguraciju koja će uputiti modul Upravitelj konfiguracije lokalne da biste registrirali sa servisom za automatizaciju DSC i dohvaćanje određenu konfiguraciju (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Tu konfiguraciju uzrokovat će Upravitelj konfiguracije lokalne modul za registrirati DSC automatizaciju. Također će odrediti kako treba raditi modul što treba učiniti ako postoji konfiguracije drift (ApplyAndAutoCorrect) te kako ga nastavite s konfiguracijom ako je potrebno je ponovno pokretanje.

Nakon pokretanja ovu skriptu čvor trebala za registriranje DSC automatizaciju.

![Registracija čvor u tijeku](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Ako se vratite na portal sustava Azure, možete vidjeti je li čvor upravo registrirani sada prikazivala na portalu.

![Registrirani čvor na portalu](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Na poslužitelju, možete pokrenuti sljedeći cmdlet komponente PowerShell da biste potvrdili da čvor registrirani pravilno:

```powershell
Get-DscLocalConfigurationManager
```

Nakon konfiguriranja povlače je i primjenjuje se na poslužitelju, provjerite to tako da pokrenete sljedeći cmdlet:

```powershell
Get-DscConfigurationStatus
```

Rezultat prikazuje da poslužitelj je uspješno povlače svoju konfiguraciju:

![Izlaz](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Osim toga, Postava servisa mobilnost ima vlastitu zapisnik koji možete pronaći na *sistemski pogon*\ProgramData\ASRSetupLogs.

To je to. Imate sada uspješno implementiran i registrirana usluga mobilnost na računalu koje želite zaštititi pomoću oporavak web-mjesta. DSC će provjerite su uvijek pokrenuti potrebni servisi.

![Uspješno uvođenje](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Kada poslužitelj za upravljanje otkrije uspješno uvođenje, možete konfigurirati zaštitu i omogućiti replikaciju na računalu pomoću oporavak web-mjesta.

## <a name="use-dsc-in-disconnected-environments"></a>DSC koristi u okruženju prekinuta veza

Ako vaš strojeva niste povezani s Internetom, možete je i dalje oslanjate DSC implementacije i konfiguriranje servisa za mobilnost na radnih opterećenja koji želite zaštititi.

Možete stvoriti instancu svoj poslužitelj istaknuti DSC u svom okruženju zapravo omogućuju iste mogućnosti koji ste dobili od DSC automatizaciju. To jest, klijente će uvesti konfiguraciju (kada je registrirana) krajnjoj DSC. Međutim, tu je i mogućnost da biste ručno konfiguriranje DSC na računalima, lokalno ili udaljeno.

Imajte na umu da u ovom primjeru ima dodane parametar za naziv računala. Udaljene datoteke sada nalaze se na udaljenom zajedničko korištenje koji trebali biste moći pristupiti tako da strojevima koji želite zaštititi. Na kraj skripta enacts konfiguraciju i zatim pokrenuti da biste primijenili konfiguracije DSC ciljnog računala.

### <a name="prerequisites"></a>Preduvjeti

Provjerite je li instaliran modul ljuske PowerShell xPSDesiredStateConfiguration. Za Windows strojeva instaliranim WMF 5.0, možete instalirati modul xPSDesiredStateConfiguration tako da pokrenete sljedeći cmdlet na računalima cilj:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Možete preuzeti i spremiti modul u slučaju da morate distribuirati strojevima sustava Windows koji imaju WMF 4.0. Pokrenite ovaj cmdlet na računalo na kojem postoji PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

4.0 WMF provjeriti je li instaliran [Windows 8.1 ažuriranje KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) na strojeva.

Sljedeće konfiguracije mogu biti Završi strojevima sustava Windows koji imaju WMF 5.0 i WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Ako želite stvoriti vlastiti DSC istaknuti poslužitelj na mreži tvrtke oponašanje mogućnosti koje možete dobiti od DSC Automatizacija, pogledajte [Postavljanje DSC povlačite web-poslužitelj](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Neobavezno: Implementacija DSC konfiguraciju tako da pomoću predloška Azure Voditelj resursa

Ovaj članak sadrži filtriran na kako možete stvoriti vlastitu konfiguraciju DSC automatski implementacije i servis za mobilnost VM Agent Azure – i provjerite koji su pokrenuti na računalima koji želite zaštititi. Imamo i predložak Azure Voditelj resursa koji će uvesti konfiguraciju DSC na novi ili postojeći račun Azure automatizaciju. Predložak će koristiti ulaznih parametara za stvaranje Automatizacija sredstva koja će sadržavati varijable okruženju sustava.

Nakon uvođenje predloška, možete jednostavno se referirati na 4 u ovom vodiču onboard vašeg računala.

Predložak će učinite sljedeće:

1. Koristi postojeći račun Automatizacija ili stvorite novi
2. Vođenje ulaznih parametara za:
    - ASRRemoteFile – mjesto na kojem su pohranjena Postava servisa mobilnost
    - ASRPassphrase – mjesto na kojem ste spremili datoteku passphrase.txt
    - ASRCSEndpoint – IP adresu poslužitelja za upravljanje
3. Uvoz modul ljuske PowerShell sustava xPSDesiredStateConfiguration
4. Stvaranje i uređivanje DSC konfiguraciju

Prethodne korake će se dogoditi u redoslijedu desno tako da možete pokrenuti za uhodavanje sustava strojeva za zaštitu.

Predložak s uputama za implementaciju, nalazi se na [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Uvođenje predloška pomoću komponente PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Daljnji koraci

Nakon implementacije servisa agenata mobilnost možete [omogućiti replikaciju](site-recovery-vmware-to-azure.md#step-6-replicate-applications) virtualnih računala.
