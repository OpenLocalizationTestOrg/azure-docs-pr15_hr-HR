<properties
 pageTitle="Automatsko skaliranje HPC paket klaster čvorove | Microsoft Azure"
 description="Automatski Povećaj i Smanji broj HPC paket klaster računalnim čvorove servisu Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatski Povećaj i Smanji resursa klaster HPC paketa u Azure prema radno opterećenje klaster




Ako implementacije čvorovi Azure "neprekidnog rada" u svoj klaster HPC paketa ili stvorite programa paketa HPC klaster Azure VMs, preporučuje se automatski Povećaj ili Smanji broj Azure računalnim resursa kao što su čvorovi ili jezgri prema trenutni radno opterećenje na klaster. Time učinkovitije korištenje Azure resursa i upravljanje njihove troškove.
Da biste to učinili, postavite svojstvo klaster HPC paket **AutoGrowShrink**. Umjesto toga možete pokrenuti skriptu HPC PowerShell **AzureAutoGrowShrink.ps1** koje se instaliraju pomoću HPC paket.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Osim toga, trenutno možete samo automatski Povećaj i Smanji čvorove računalnim HPC paket koji rade s operacijskim sustavom Windows Server.

## <a name="set-the-autogrowshrink-cluster-property"></a>Postavite svojstvo AutoGrowShrink klaster

### <a name="prerequisites"></a>Preduvjeti

* **HPC paket 2012 R2 ažuriranju 2 ili novije klaster** - čvor glavni klaster može biti implementiran bilo lokalno ili na VM Azure. U odjeljku [Postavljanje klaster hibridne HPC paket](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) za početak na lokalni glavni čvor i čvorove Azure "neprekidnog rada". U odjeljku [HPC paket IaaS implementacijsku skriptu](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) da biste brzo uvesti klaster HPC paketa u Azure VMs.


* **Za klaster s glavni čvor u Azure** – ako koristite HPC paket IaaS implementacijsku skriptu da biste stvorili klaster, omogućite klaster svojstvo **AutoGrowShrink** tako da postavite mogućnost AutoGrowShrink u konfiguracijskoj datoteci klaster. Detalje potražite u dokumentaciji popratnim [skripte preuzimanje](https://www.microsoft.com/download/details.aspx?id=44949). 

    Umjesto toga možete omogućiti klaster svojstvo **AutoGrowShrink** nakon implementacije klaster pomoću naredbe ljuske PowerShell HPC opisani u sljedećem odjeljku. Priprema za to, slijedite sljedeće korake:
    1. Konfiguriranje certifikata Azure upravljanja na glavni čvor i u Azure pretplate. Za implementaciju test možete koristiti zadani Microsoft Azure HPC samopotpisani certifikat koji se instalira paket HPC na glavni čvor i jednostavno prenijeti taj certifikat u pretplatu za Azure. Mogućnosti i upute potražite u članku [smjernice biblioteci TechNet](https://technet.microsoft.com/library/gg481759.aspx).
    2. Pokrenite **regedit** na glavni čvor, idite na HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo i dodati novu vrijednost niza. Postavite naziv vrijednosti da biste "Otisak prsta", a vrijednost podataka otisak prsta potvrde u koraku 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>Naredbe ljuske PowerShell za HPC da biste postavili svojstvo AutoGrowShrink

Sljedeće su ogledne naredbe ljuske PowerShell HPC da biste postavili **AutoGrowShrink** i ugađanje ponašanja s dodatne parametre. Pogledajte [AutoGrowShrink parametara](#AutoGrowShrink-parameters) u nastavku ovog članka cjelovit popis postavki. 

Da biste pokrenuli te naredbe, pokrenite HPC PowerShell na čvor glavni klaster kao administrator.

**Da biste omogućili svojstvo AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 1

**Da biste onemogućili svojstvo AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Da biste promijenili razmak Povećaj u minutama**

    Set-HpcClusterProperty –GrowInterval <interval>

**Da biste promijenili razmak Stisni u minutama**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Da biste vidjeli trenutnu konfiguraciju AutoGrowShrink**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>Parametri AutoGrowShrink

Slijede AutoGrowShrink parametre koje se mogu mijenjati pomoću naredbe " **Postavljanje HpcClusterProperty** ".

* **EnableGrowShrink** - parametar radi omogućivanja i onemogućivanja svojstvo **AutoGrowShrink** .
* **ParamSweepTasksPerCore** – broj parametric Počisti zadataka radi povećanja jedan core. Zadana je vrijednost radi povećanja jedan core svakog zadatka. 
 
    >[AZURE.NOTE] HPC paket uspio KB3134307 mijenja **ParamSweepTasksPerCore** **TasksPerResourceUnit**. Temelji se na vrsti resursa za posao i može biti čvor, socket ili core.
    
* **GrowThreshold** - prag zadataka u redu čekanja za pokretanje automatskog rasta. Zadano je 1, što znači koji postoje li 1 ili više zadataka u stanje u redu čekanja automatski Povećaj čvorovi.
* **GrowInterval** - Interval za pokretanje automatskog growth u minutama. Zadani je interval pet minuta.
* **ShrinkInterval** – pokretanje automatskog smanjivanjem Interval u minutama. Zadani je interval pet minuta. |
* **ShrinkIdleTimes** – broj provjerava kontinuirani smanjiti da biste naznačili čvorove su neaktivan. Zadano je 3 puta. Ako, na primjer, ako je **ShrinkInterval** pet minuta, HPC paket provjerava je li čvor neaktivnosti svakih 5 minuta. Ako čvorove u stanja neaktivnosti nakon 3 neprekinuti provjerava (15 minuta), paket HPC smanjuje taj čvor.
* **ExtraNodesGrowRatio** - dodatni postotak čvorove radi povećanja za poslove sučelja prosljeđivanje poruke (MPI). Zadana vrijednost je 1, što znači da paket HPC rastom čvorove % 1 za MPI zadatke. 
* **GrowByMin** - parametar koji određuju hoće li pravila autogrow temelju minimalne resurse potrebne za posao. Zadana je vrijednost false, što znači da paket HPC rastom čvorove za zadatke na temelju Maksimalna resursa potrebnih za zadatke.
* **SoaJobGrowThreshold** - prag zahtjevi SOA za automatsko pokretanje Povećaj postupak. Zadana vrijednost je 50000.  
    
    >[AZURE.NOTE] Ovaj parametar podržano je uveden u HPC Service Pack 2012 R2 ažuriranje 3.
    
* **SoaRequestsPerCore** – broj dolazne SOA zahtjeva radi povećanja jedan core. Zadana vrijednost je 20000.  

    >[AZURE.NOTE] Ovaj parametar podržano je uveden u HPC Service Pack 2012 R2 ažuriranje 3.

### <a name="mpi-example"></a>Primjer MPI

Prema zadanim postavkama rastom HPC Service Pack 1% dodatni čvorove za MPI poslove (**ExtraNodesGrowRatio** je postavljen na 1). Razlog je MPI može zahtijevati više čvorove, a posao može se pokrenuti samo kada sve čvorove spremni. Prilikom pokretanja Azure čvorove povremeno jedan čvor možda trebate još jedanput da biste započeli od ostalih uzrokuje ostale čvorove biti neaktivno tijekom čekanja za taj čvor pripremiti. Po rast dodatni čvorove HPC paket smanjuje ovaj put na čekanju resursa i potencijalno sprema troškove. Da biste povećali postotak dodatni čvorove MPI posla (Ako, na primjer, da biste 10%), pokrenite naredbu sličnu

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Primjer SOA

Prema zadanim postavkama **SoaJobGrowThreshold** postavljen na 50000 i **SoaRequestsPerCore** postavljen do 200 000. Ako pošaljete jedan zadatak SOA 70000 zahtjevima, neće biti jedan zadatak po redu čekanja i zahtjevi za su 70000. U ovom slučaju rastom HPC Service Pack 1 core u redu čekanja zadatka i zahtjevi za razgovore, će rasti (70000 50000) / Jezgra 20000 = 1, tako da se ukupno će rasti 2 jezgri za posao SOA.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Pokrenuti skriptu AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Preduvjeti

* **HPC Pack 2012 R2 Update 1 ili noviji klaster** – skripte **AzureAutoGrowShrink.ps1** je instaliran u mapi za smeće % CCP_HOME %. Može biti čvor glavni klaster implementiran bilo lokalno ili na VM Azure. U odjeljku [Postavljanje klaster hibridne HPC paket](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) za početak na lokalni glavni čvor i čvorove Azure "neprekidnog rada". Potražite u članku [HPC paket IaaS implementacijsku skriptu](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) da biste brzo uvesti klaster HPC paketa u Azure VMs ili pomoću [predloška Azure brzi početak rada](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** – trenutno skripte ovisi o određenim verziju Azure PowerShell. Ako koristite noviju verziju na glavni čvor, možda ćete morati prijeći na nižu Azure PowerShell [verzije 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) pokrenuti skriptu. 

* **Za klaster s čvorove Azure neprekidnog rada** – pokrenuti skriptu na klijentskom računalu na kojem je instaliran paket HPC ili na glavni čvor. Ako je pokrenut na klijentskom računalu, provjerite postavljate varijabla $env: CCP_SCHEDULER pravilno tako da pokazuje na glavni čvor. Čvorovi Azure "neprekidnog rada" mora biti dodan već klaster, ali može biti u stanju nije implementiran.


* **Klaster u uveden u Azure VMs** - pokrenuti skriptu na glavni čvor VM, jer ovisi o **Start HpcIaaSNode.ps1** i **Stop HpcIaaSNode.ps1** skripte koje se instaliraju postoji. Te skripte uz to zatraži potvrdu Azure upravljanja ili objavljivanje datoteka postavki (pročitajte članak [Upravljanje izračunati čvorovi u klasteru HPC paketa u Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Provjerite je li sve čvor računalnim VMs morate već dodali klaster. Može biti u stanju zaustavljanja.

### <a name="syntax"></a>Sintaksa

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parametri

 * **NodeTemplates** - nazivi predložaka čvor da biste odredili doseg za čvorove Povećaj i Smanji. Ako ne navedete (Zadana vrijednost je @()), sve čvorove u grupi čvor **AzureNodes** su u opsegu kada **NodeType** ima vrijednost AzureNodes, a sve čvorove u grupi čvor **ComputeNodes** u opseg kada **NodeType** ima vrijednost ComputeNodes.

 * **JobTemplates** - nazivi predložaka za posao da biste odredili doseg za čvorove radi povećanja.

 * **NodeType** - vrstu čvora Povećaj i Smanji. Podržani vrijednosti su:

     * **AzureNodes** – za čvorove Azure PaaS (neprekidnog rada) u lokalnog ili Azure IaaS klaster.

     * **ComputeNodes** - za računalnim čvor VMs samo u programa Azure IaaS klaster.

* **NumOfQueuedJobsPerNodeToGrow** – broj poslovi potrebna radi povećanja jedan čvor.

* **NumOfQueuedJobsToGrowThreshold** – broj praga Poslovi da biste pokrenuli postupak Povećaj.

* **NumOfActiveQueuedTasksPerNodeToGrow** – Broj aktivnih zadataka u redu čekanja potrebna radi povećanja jedan čvor. Ako je **NumOfQueuedJobsPerNodeToGrow** navedena s vrijednošću većom od 0, zanemaruje se taj parametar.

* **NumOfActiveQueuedTasksToGrowThreshold** - najmanji potreban broj aktivnih zadataka u redu čekanja da biste pokrenuli postupak Povećaj.

* **NumOfInitialNodesToGrow** – Početna najmanji broj čvorove radi povećanja ako su sve čvorove opseg **Nije implementiran** ili **zaustavljen (Deallocated)**.

* **GrowCheckIntervalMins** - interval u minutama između provjere radi povećanja.

* **ShrinkCheckIntervalMins** - intervala u minutama između provjere smanjiti.

* **ShrinkCheckIdleTimes** – broj neprekinuti smanjiti provjere (odvojene **ShrinkCheckIntervalMins**) koji navode čvorove neaktivan.

* **UseLastConfigurations** - konfiguracije prethodno spremljene u datoteci argument.

* **ArgFile**- naziv datoteke argument koristi da biste spremili i ažurirali konfiguracija pokrenuti skriptu.

* **LogFilePrefix** - prefiks naziv datoteke zapisnika. Možete navesti put. Prema zadanim postavkama zapisnik zapisuje na trenutnom radnom direktoriju.

### <a name="example-1"></a>Primjer 1

U sljedećem primjeru konfigurira čvorovi Azure neprekidnog rada implementiran pomoću predloška za zadani AzureNode Povećaj i Smanji automatski. Ako sve čvorove su početno stanje **Nije implementiran** , najmanje 3 čvorove pokrenute. Ako je broj poslovi premašuje 8, skripta pokreće čvorove dok njihov broj prelazi omjer poslovi **NumOfQueuedJobsPerNodeToGrow**. Ako čvor nije pronađen neaktivnosti u 3 puta uzastopnih neaktivan, je zaustavljen.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Primjer 2

U sljedećem primjeru konfigurira čvor Azure računalnim VMs implementiran pomoću predloška za zadani ComputeNode Povećaj i Smanji automatski.
Poslovi konfigurirao zadani posla predložak definiranje opsega povećavaju na klaster. Ako su sve čvorove prethodno Zaustavi, najmanje 5 čvorovi pokrenute. Ako je broj aktivnih zadataka u redu čekanja premašuje 15, skripta pokreće čvorove dok njihov broj prelazi omjer aktivnih zadataka u redu čekanja za **NumOfActiveQueuedTasksPerNodeToGrow**. Ako čvor nije pronađen neaktivnosti u 10 uzastopnih vremena neaktivnosti, je zaustavljen.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
