<properties 
    pageTitle="Pokretanje i zaustavljanje virtualnim strojevima sa Azure Automatizacija - PowerShell tijeka rada | Microsoft Azure"
    description="Grafički verzija Azure Automatizacija scenarij uključujući runbooks za pokretanje i zaustavljanje klasični virtualnim računalima."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automatizacija scenarij – pokretanje i zaustavljanje virtualnim strojevima

Scenarij Azure Automatizacija obuhvaća runbooks za pokretanje i zaustavljanje klasični virtualnih računala.  U ovom scenariju možete koristiti za nešto od sljedećeg:  

- Korištenje runbooks bez izmjene u vlastitu okruženju. 
- Izmjena runbooks za izvođenje prilagođene funkcije.  
- Pozivanje na runbooks iz drugog runbook kao dio ukupnog rješenja. 
- Upotrijebili na runbooks vodiče da biste saznali runbook authoring koncepata. 

> [AZURE.SELECTOR]
- [Grafički](automation-solution-startstopvm-graphical.md)
- [PowerShell tijeka rada](automation-solution-startstopvm-psworkflow.md)

Ovo je verzija runbook tijeka rada PowerShell scenarij. Dostupna je i pomoću [grafičkog runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Početak scenarija

Ovaj scenarij sastoji se od dva runbooks PowerShell tijeka rada koji možete preuzeti sa sljedećih veza.  Pogledajte [grafički verziju](automation-solution-startstopvm-graphical.md) scenarij za veze na grafički runbooks.

| Runbook | Veza | Vrsta | Opis |
|:---|:---|:---|:---|
| Početak AzureVMs | [Pokretanje Azure klasični VMs](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell tijeka rada | Pokreće sve klasični virtualnim strojevima u Azure subscriptionor sve virtualnim strojevima pod nazivom određenog servisa. |
| Zaustavi AzureVMs | [Zaustavljanje Azure klasični VMs](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell tijeka rada | Zaustavlja sve virtualnim strojevima u račun za automatizaciju ili sve virtualnim strojevima pod nazivom određenog servisa.  |


## <a name="installing-and-configuring-the-scenario"></a>Instaliranje i konfiguriranje scenarija

### <a name="1-install-the-runbooks"></a>1. na runbooks instalacija

Nakon preuzimanja s runbooks, možete ih pomoću postupka u odjeljku [Uvoz u Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)uvesti.

### <a name="2-review-the-description-and-requirements"></a>2. Pregledajte opis i preduvjeti
Na runbooks obuhvaćaju tekst komentirane pomoći koja sadrži opis i potrebni resursi.  Možete dobiti i iste podatke iz ovog članka. 

### <a name="3-configure-assets"></a>3. konfigurirajte resursi
Na runbooks potreban je sljedeći resursi koji morate stvoriti i popuniti odgovarajuće vrijednosti.

| Vrsta resursa | Naziv resursa | Opis |
|:---|:---|:---|:---|
| Vjerodajnica | AzureCredential | Sadrži vjerodajnice za račun za pokretanje i zaustavljanje virtualnim strojevima Azure pretplate.  Osim toga, možete odrediti drugi vjerodajnica resursa u parametru **vjerodajnica** **Dodaj AzureAccount** aktivnosti. |
| Varijabla | AzureSubscriptionId | Sadrži ID pretplate pretplate Azure. |

## <a name="using-the-scenario"></a>Korištenje scenarija

### <a name="parameters"></a>Parametri

Svaki runbooks imati sljedećih parametara.  Navedite vrijednosti za parametre obavezna, a po izboru možete unijeti vrijednosti za drugi parametri ovisno o svojim potrebama.

| Parametar | Vrsta | Obavezna | Opis |
|:---|:---|:---|:---|
| Naziv servisa | niz | ne | Ako je navedena vrijednost svih virtualnim strojevima tog naziva će se pokrenuti ili zaustaviti.  Ako nijedna vrijednost nije naveden, sve klasični virtualnim strojevima u Azure pretplata će se pokrenuti ili zaustaviti. |
| AzureSubscriptionIdAssetName | niz | ne | Sadrži naziv [varijable resursa](#installing-and-configuring-the-scenario) koji sadrži ID pretplate pretplate Azure.  Ako ne navedete vrijednost, koristit će se *AzureSubscriptionId* .  |
| AzureCredentialAssetName | niz | ne | Sadrži naziv [vjerodajnica resursa](#installing-and-configuring-the-scenario) koji sadrži vjerodajnice za runbook za korištenje.  Ako ne navedete vrijednost, koristit će se *AzureCredential* .  |

### <a name="starting-the-runbooks"></a>Pokretanje sustava runbooks

Koristite neku od metoda u [početnom runbook u automatizaciji Azure](automation-starting-a-runbook.md) da biste započeli ili na runbooks u ovom scenariju.

Sljedeće primjere naredbi koristi Windows PowerShell za izvođenje **StartAzureVMs** da biste započeli sve virtualnim strojevima naziv usluge *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Izlaz

Na runbooks će [izlazne poruke](automation-runbook-output-and-messages.md) za svaki virtualnog računala koja označava hoće li upute za početak ili prestanete uspješno poslan.  Možete potražiti određeni niz za izlaz da biste odredili rezultat za svaku runbook.  U tablici u nastavku navedene su nizovi mogući izlaz.

| Runbook | Uvjet | Poruka |
|:---|:---|:---|
| Početak AzureVMs | Već je pokrenut virtualnog računala  | MyVM već pokrenut |
| Početak AzureVMs | Pokretanje zahtjeva za virtualnog računala uspješno poslan | MyVM je pokrenut |
| Početak AzureVMs | Zahtjev za početak za virtualnog računala nije uspio  | Pokretanje MyVM nije uspjelo |
| Zaustavi AzureVMs | Virtualnog računala već zaustavljeno  | MyVM već zaustavljeno |
| Zaustavi AzureVMs | Zaustavljanje zahtjev za virtualnog računala uspješno poslan | Prestao MyVM |
| Zaustavi AzureVMs | Zaustavi zahtjev za virtualnog računala nije uspio  | Zaustavljanje MyVM nije uspjelo |

Ako, na primjer, sljedeće koda iz programa runbook pokušava sve virtualnim strojevima počinju naziv usluge *MyServiceName*.  Ako bilo koji od Neuspjelo zahtjevi za pokretanje, zatim akcije pogreška se može preuzeti. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Detaljne analitički

Slijedi detaljne razrada svega runbooks u ovom scenariju.  Ove informacije možete koristiti da biste prilagodili na runbooks ili da biste samo za stvaranje vlastite scenariji za automatizaciju dodatne od njih.

### <a name="parameters"></a>Parametri

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Tijek rada pokreće se i njegova vrijednosti za [ulazne parametre](#using-the-scenario).  Ako ne postoje nazivi resursa koriste se zadane nazive.

### <a name="output"></a>Izlaz

    # Returns strings with status messages
    [OutputType([String])]

Redak deklarira Izlaz na kompilacije će biti niz.  Ovo nije obavezno, ali je Najbolji postupci za kada na runbook koristi se kao [podređeni runbook](automation-child-runbooks.md) tako da nadređenog runbook će zna Vrsta izlaza očekivati.

### <a name="authentication"></a>Provjera autentičnosti

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Sljedeći reci postavljaju [vjerodajnice](automation-configuring.md#configuring-authentication-to-azure-resources) i Azure pretplatu koja će se koristiti za ostale u kompilacije.
Najprije koristimo **Get-AutomationPSCredential** da biste dobili resursa koja sadrži vjerodajnice s pristupom pokretanje i zaustavljanje virtualnim strojevima Azure pretplate. **Dodavanje AzureAccount** koristi ovaj resursa za postavljanje vjerodajnica.  Rezultat je dodijeljen sustava varijabla tako da ga nije uvršten u izlaz runbook.  

Varijable imovine s ID pretplate zatim dohvatiti s **Get-AutomationVariable** i postaviti **Odaberite AzureSubscription**pretplate.

### <a name="get-vms"></a>Početak VMs

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** koristi se za dohvaćanje virtualnim strojevima će raditi na runbook.  Ako je vrijednost u varijablu unos **naziv servisa** , dohvaćaju se samo na virtualnim strojevima tog naziva.  Ako je **naziv servisa** prazan, dohvaćaju se sve virtualnih računala.

### <a name="startstop-virtual-machines-and-send-output"></a>Početak i kraj virtualnim strojevima i slanje Izlaz

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Sljedeći reci prolaziti kroz svaki virtualnog računala.  Prvo provjerava jesu li već pokrenut ili zaustavite, ovisno o tome na runbook **Stanje napajanja** virtualnog računala.  Ako je već u stanju cilj, poruke se šalje izlazne i završava runbook.  Ako nije, zatim koristiti **Start AzureVM** ili **Zaustavi AzureVM** pokuša pokrenuti ili zaustaviti virtualnog računala rezultatom zahtjev pohranjene tjednog prikaza kalendara.  Da biste izlaz vrijednost koja određuje hoće li je zahtjev za pokretanje ili prekidanje uspješno poslan zatim slanja poruke.


## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o radu s runbooks podređeni, potražite u članku [runbooks podređeni u automatizaciji Azure](automation-child-runbooks.md) 
- Dodatne informacije o izlazne poruke tijekom izvođenja runbook i zapisivanje pomoći u rješavanju problema potražite u članku [Runbook izlazne i poruke u automatizaciji Azure](automation-runbook-output-and-messages.md)
