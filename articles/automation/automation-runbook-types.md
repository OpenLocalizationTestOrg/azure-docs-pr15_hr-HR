<properties 
   pageTitle="Vrste Runbook Azure automatizacije"
   description="U članku se opisuje različite vrste runbooks koje možete koristiti u Automatizacija Azure i značajkama koje biste trebali uzeti u obzir prilikom određivanja koju biste vrstu odabrali. "
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
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure vrste runbook automatizacije

Azure Automatizacija podržava četiri vrste runbooks koji su ukratko opisani u tablici u nastavku.  U nastavku se odjeljcima navode dodatne informacije o svakoj vrsta uključujući pitanja koja se odnose na kad koristiti određene.


| Vrsta |  Opis |
|:---|:---|
| [Grafički](#graphical-runbooks) | Na temelju komponente Windows PowerShell i stvorene i uređene potpuno u grafički uređivač Azure portalu. | 
| [Grafički PowerShell tijeka rada](#graphical-runbooks) | Ovisno o tijeku rada sustava Windows PowerShell i stvara i uređivati u potpunosti u uređivaču grafički Azure portalu. 
| [PowerShell](#powershell-runbooks) | Runbook teksta na temelju skripte komponente Windows PowerShell.
| [PowerShell tijeka rada](#powershell-workflow-runbooks) | Runbook teksta na temelju tijeka rada za Windows PowerShell. |


## <a name="graphical-runbooks"></a>Grafički runbooks

[Graphical](automation-runbook-types.md#graphical-runbooks) i grafički tijeka rada PowerShell runbooks su stvorene i uređene pomoću grafičkog uređivača na portalu za Azure.  Možete ih izvesti u datoteku i zatim ih uvezete u neki drugi račun za automatizaciju, ali ne možete stvarati ni uređivati neki drugi alat.  Grafički runbooks generiranje koda za PowerShell, ali ne možete izravno prikaz ili promjena koda. Grafički runbooks nije moguće pretvoriti u jedan od [oblikovanja teksta](automation-runbook-types.md), kao ni tekst runbook može pretvoriti grafičkom obliku. Grafički runbooks može pretvoriti u runbooks grafički tijeka rada PowerShell tijekom uvoza i obratno.

### <a name="advantages"></a>Prednosti

- Stvorite runbooks s minimalnim znanja [PowerShell](automation-powershell-workflow.md).
- Vizualno predstavili postupke upravljanja.
- Uključuju druge runbooks kao podređeni runbooks da biste stvorili visoke razine tijekova rada.


### <a name="limitations"></a>Ograničenja

- Ne možete uređivati runbook izvan Azure portal.
- Možda ćete morati aktivnost kod koji sadrže kod ljuske PowerShell za provođenje složene logike.
- Ne možete prikazati ni izravno uređivati PowerShell kod koji je stvorio grafički tijek rada. Imajte na umu da možete vidjeti kod koje stvorite u bilo koji kod aktivnosti.


## <a name="powershell-runbooks"></a>PowerShell runbooks

PowerShell runbooks temelje se na Windows PowerShell.  Izravno uređivanje koda kompilacije u uređivaču teksta na portalu za Azure.  Možete koristiti i sve uređivaču teksta izvan mreže i [Uvoz u runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) u Azure automatizaciju.

### <a name="advantages"></a>Prednosti

- Implementirate sve složene logike kod PowerShell bez dodatnih complexities PowerShell tijeka rada. 
- Runbook brže nego runbooks Graphical ili PowerShell tijek rada pokreće jer ona nije potrebno prevesti prije pokretanja.

### <a name="limitations"></a>Ograničenja

- Morate biti upoznati s skriptiranje PowerShell.
- [Paralelni obrada](automation-powershell-workflow.md#parallel-processing) ne može se koristiti za obavljanje više akcija paralelno.
- [Checkpoints](automation-powershell-workflow.md#checkpoints) ne može se koristiti za nastavak runbook u slučaju pogreške.
- Runbooks PowerShell tijeka rada i grafički runbooks može biti uključen kao podređeni runbooks pomoću cmdleta Start AzureAutomationRunbook koji stvara novi zadatak.

### <a name="known-issues"></a>Poznati problemi
Slijede trenutni Poznati problemi vezani uz PowerShell runbooks.

- PowerShell runbooks nije moguće nije moguće dohvatiti šifrirane [varijable resursa](automation-variables.md) u imaju vrijednost null.
- PowerShell runbooks nije moguće dohvatiti [varijable resursa](automation-variables.md) s *~* u nazivu.
- Get-postupak u petlji u programa PowerShell runbook mogu pasti nakon otprilike 80 iteracija. 
- PowerShell runbook možda neće uspjeti ako pokušava pisati vrlo veliku količinu podataka strujanje Izlaz odjednom.   Taj se problem možete riješiti najčešće stvaranjem samo potrebne informacije prilikom rada s velikim objekte.  Ako, na primjer, umjesto bilježiti nešto poput *Get-procesa*, možete poslati samo obavezna polja s *Get-postupak | Odaberite ProcessName, procesora*.

## <a name="powershell-workflow-runbooks"></a>Runbooks PowerShell tijeka rada

Tijek rada PowerShell runbooks su runbooks teksta na temelju [Tijeka rada za Windows PowerShell](automation-powershell-workflow.md).  Izravno uređivanje koda kompilacije u uređivaču teksta na portalu za Azure.  Možete koristiti i sve izvanmrežne uređivač i [Uvoz u runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) u Azure automatizaciju.

### <a name="advantages"></a>Prednosti

- Implementirate sve složene logike kod PowerShell tijeka rada.
- Koristite [checkpoints](automation-powershell-workflow.md#checkpoints) da biste nastavili runbook u slučaju pogreške.
- Koristite [paralelne obrada](automation-powershell-workflow.md#parallel-processing) više akcija paralelno.
- Možete uključiti druge grafički runbooks i runbooks PowerShell tijeka rada kao podređeni runbooks da biste stvorili visoke razine tijekova rada.


### <a name="limitations"></a>Ograničenja

- Autor mora biti upoznati s PowerShell tijeka rada.
- Runbook morate baviti dodatne složenost PowerShell tijeka rada kao što su [deserijalizirati objekte](automation-powershell-workflow.md#code-changes).
- Runbook traje dulje da biste započeli od PowerShell runbooks jer je potrebno prevesti prije pokretanja.
- PowerShell runbooks može biti uključen kao podređeni runbooks pomoću cmdleta Start AzureAutomationRunbook koji stvara novi zadatak.


## <a name="considerations"></a>Razmatranja

Biste trebali uzeti u obzir sljedeće Dodatne napomene prilikom određivanja koju biste vrstu odabrali za određeni runbook.

- Runbooks ne možete pretvoriti iz grafički tekstnih vrsta ili obratno.
- Postoje ograničenja korištenja runbooks različitih vrsta kao podređeni runbook.  Dodatne informacije potražite u članku [runbooks podređeni u automatizaciji Azure](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o grafički runbook prilikom stvaranja, potražite u članku [Graphical vremenu u Automatizacija Azure](automation-graphical-authoring-intro.md)
- Da biste razumjeli razlike između PowerShell i PowerShell tijekova rada za runbooks, potražite u članku [Tijek rada sustava Windows PowerShell za učenje](automation-powershell-workflow.md)
- Dodatne informacije o načinu stvaranja i uvoza u Runbook potražite u članku [Stvaranje ili uvoz u Runbook](automation-creating-importing-runbook.md)



