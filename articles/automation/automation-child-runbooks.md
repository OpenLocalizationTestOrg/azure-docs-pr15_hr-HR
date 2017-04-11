<properties 
   pageTitle="Podređeni runbooks u automatizaciji Azure | Microsoft Azure"
   description="U članku se opisuje različite načine za početak na runbook u automatizaciji Azure s drugom runbook i zajedničko korištenje informacija između njih."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Podređeni runbooks u automatizaciji Azure

Je najbolji način u automatizaciji Azure pisati ponovno iskoristiv, modularan runbooks samostalni funkcijom koje možete koristiti druge runbooks. Nadređeni runbook često podići runbooks podređeni za izvođenje funkciju. Dva su načina za pozivanje podređeni runbook i svaka ima jasne razlike koje trebate tako da možete odrediti koji će se najbolje odgovara različitim scenarijima.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Pozivanje podređeni runbook pomoću izvođenja u istoj razini

Da biste poziva runbook izravno iz drugog runbook, nazovite na kompilacije i unesite vrijednosti za parametara točno onako kao što je to koristite aktivnost ili cmdlet.  Sve runbooks u istom Automatizacija računa dostupni su za sve ostale će se koristiti na taj način. Nadređeni runbook čekati na podređeni runbook da biste dovršili prije prelaska na sljedeći redak, a sve izlazne se vraćaju izravno u nadređenog.

Kada pozovete runbook umetnute pokrene u isti posao kao runbook nadređenog. Pojavit će se bez oznaka u povijesti zadatka kompilacije podređeni koji je pokrenuo. Željene iznimke i sve strujanje Izlaz iz podređene runbook će biti povezan s nadređenog. Rezultira manje zadatke i jednostavnije pratiti i otklanjanje poteškoća s jer iznimaka izbačena runbook podređeni i bilo koji od strujanje rezultat su vezane uz zadatak nadređenog.

Kada je objavljen na runbook, morate već objavljene sve podređene runbooks koja se poziva. To je zato Azure Automatizacija sastavlja pridruživanje s bilo kojeg runbooks podređeni kada je runbook prikupljaju. Ako nisu, runbook nadređenog prikazat će se da biste objavili ispravno, ali će generirati iznimku prilikom pokretanja. Ako se to dogodi, možete ponovno objaviti runbook nadređenog da bi se ispravno referencirati runbooks podređenih. Ne morate ponovno objaviti runbook nadređenog Ako bilo koji od podređenih runbooks mijenjaju jer pridruživanja će već su stvorene.

Parametri kompilacije podređeni naziva u istoj razini može biti bilo koju vrstu podataka, uključujući složene objekte i nema bez [JSON serijalizacije](automation-starting-a-runbook.md#runbook-parameters) jer postoji prilikom pokretanja runbook pomoću portala za upravljanje Azure ili pomoću cmdleta Start AzureRmAutomationRunbook.


### <a name="runbook-types"></a>Vrste Runbook

Koje vrste da biste uputili poziv međusobno:

- [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) i [grafički runbooks](automation-runbook-types.md#graphical-runbooks) možete nazvati svakom retku (su oba PowerShell temelji).
- [Runbook PowerShell tijeka rada](automation-runbook-types.md#powershell-workflow-runbooks) i tijek rada za grafički PowerShell runbooks da biste uputili poziv svakom retku (su oba tijeka rada PowerShell temelje)
- Vrste PowerShell vrste PowerShell tijeka rada ne možete pozvati međusobno u istoj razini i morate koristiti Start AzureRmAutomationRunbook.
    
Kad objavite relevantnim redoslijedom:

- Objavljivanje redoslijeda runbooks samo zapise za runbooks PowerShell tijek rada i tijek rada za grafički PowerShell.


Prilikom pozivanja na Graphical ili PowerShell tijeka rada podređenim runbook pomoću izvođenja u istoj razini, koristite samo naziv u kompilacije.  Prilikom pozivanja runbook za dijete PowerShell je morate kojima prethodi nazivu s *.\\ * Da biste odredili skripta nalazi u lokalnom direktoriju. 

### <a name="example"></a>Primjer

U sljedećem primjeru poziva runbook podređeni test koji prihvaća tri parametara složenih objekata, cijeli te na Booleove vrijednosti. Izlaz kompilacije podređeni dodijeljena varijabli.  U ovom slučaju runbook podređeni je tijek rada PowerShell runbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Slijedi primjer iste pomoću komponente PowerShell runbook kao podređeni.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Pokretanje podređeni runbook pomoću cmdleta

Cmdlet [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) možete koristiti da biste započeli s runbook kao što je opisano u [Da biste pokrenuli runbook s komponentom Windows PowerShell](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Postoje dva načina korištenja za ovaj cmdlet.  U jednom načinu cmdlet vraća id zadatka čim za runbook podređeni stvoren je zadatak podređenih.  U drugi načinu, koje omogućite tako da navedete u **-Pričekajte** parametar cmdlet će čekati dijete zadatak završi i će vratiti Izlaz iz podređene runbook.

Zadatak iz podređene runbook novina je cmdlet funkcionirat će u zasebnom zadatak iz runbook nadređenog. Rezultira radnih od pozivanje umetnute runbook i čini teže za praćenje. Nadređeni možete pokrenuti više podređenih runbooks asinkrono bez čekanja za svaku da biste dovršili. Za tu istu vrstu paralelno izvođenja pozivanje runbooks izravno podređenih runbook nadređenog morate koristiti [paralelne ključnih riječi](automation-powershell-workflow.md#parallel-processing).

Parametri za dijete runbook novina je cmdlet su prikazuje se kao tablicu raspršivanja kao što je opisano u [Runbook parametara](automation-starting-a-runbook.md#runbook-parameters). Može se koristiti samo jednostavnim vrstama podataka. Ako je na runbook parametar s vrstom složenih podataka, pa ga mora biti naziva u istoj razini.

### <a name="example"></a>Primjer

U sljedećem primjeru započinje podređeni runbook parametara, a zatim čeka da ga da biste dovršili pomoću Start AzureRmAutomationRunbook-Pričekajte parametar. Nakon dovršetka, rezultat je prikupljenih runbook podređenih.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Usporedba načina za pozivanje podređenih runbook

U sljedećoj tablici nalaze se razlike između dva načina za pozivanje na runbook iz drugog runbook.

| | U istoj razini| Cmdlet|
|:---|:---|:---|
|Zadatak|Podređeni runbooks pokrenite isti posao kao nadređene.|Za runbook podređeni stvara se zasebnom posao.|
|Izvršavanje|Nadređeni runbook čeka runbook podređeni da biste dovršili prije nastavka.|Nadređeni runbook i dalje odmah nakon pokreće podređeni runbook *ili* runbook nadređenog čeka podređeni zadatak da biste završili.|
|Izlaz|Nadređeni runbook izravno možete pristupiti Izlaz iz podređene runbook.|Nadređeni runbook morate dohvatiti Izlaz iz podređene runbook posao *ili* nadređenog runbook možete izravno pristupiti Izlaz iz podređene runbook.|
|Parametri|Vrijednosti za parametre runbook podređeni su navedeni zasebno, a možete koristiti bilo koju vrstu podataka.|Vrijednosti za parametre runbook podređeni mora biti posložene jedan hashtable, a mogu sadržavati samo jednostavne, polja i vrste podataka objekta te serijalizacije JSON leverage.|
|Automatizacija računa|Nadređeni runbook možete koristiti samo podređenih runbook u istom Automatizacija računa.|Nadređeni runbook možete koristiti runbook dijete s bilo kojeg računa za automatizaciju iz iste pretplate na Azure i čak i neku drugu pretplatu na ako imate vezu s njom.|
|Za objavljivanje|Podređeni runbook mora objaviti kako bi se objavljuje runbook nadređenog.|Podređeni runbook mora objaviti bilo kojem trenutku prije pokretanja nadređenog runbook.|

## <a name="next-steps"></a>Daljnji koraci

- [Pokretanje programa runbook u automatizaciji Azure](automation-starting-a-runbook.md)
- [Web-mjesto Runbook izlazne i u okvir za poruke u automatizaciji Azure](automation-runbook-output-and-messages.md)
