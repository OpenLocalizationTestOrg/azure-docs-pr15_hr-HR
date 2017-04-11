<properties
    pageTitle="Stvaranje ili uvoz runbook u automatizaciji Azure"
    description="U ovom se članku opisuje kako stvoriti novi runbook u Automatizacija Azure ili uvezli sliku iz datoteke."
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
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Stvaranje ili uvoz runbook u automatizaciji Azure

Možete dodati u runbook Azure Automatizacija, ili [Stvaranje novog](#creating-a-new-runbook) ili uvozom postojeće runbook iz datoteke ili iz [Galerije Runbook](automation-runbook-gallery.md). Ovaj članak sadrži informacije o stvaranju i uvoz runbooks iz datoteke.  Sve pojedinosti o pristupanju runbooks zajednice i module u [galerije Runbook i module za automatizaciju Azure](automation-runbook-gallery.md)možete dobiti.

## <a name="creating-a-new-runbook"></a>Stvaranje nove runbook

Možete stvoriti nove runbook u automatizaciji Azure pomoću jednog od Azure portalima ili komponente Windows PowerShell. Nakon što u runbook, možete ga pomoću informacija u [Tijek rada za učenje PowerShell](automation-powershell-workflow.md) i [grafički vremenu u Azure Automatizacija](automation-graphical-authoring-intro.md)urediti.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Da biste stvorili novi runbook Automatizacija Azure pomoću portala za klasični Azure

[Runbooks PowerShell tijek rada](automation-runbook-types.md#powershell-workflow-runbooks) možete raditi samo na portalu za Azure.

1. Na portalu Azure klasični kliknite, **novu** **aplikaciju servisa**, **Automatizacija**, **Runbook**, **Brzo stvaranje**.
2. Unesite potrebne informacije, a zatim kliknite **Stvori**. Naziv runbook mora započeti slovom i mogu imati slova, brojeve, podvlake i crtice.
3. Ako želite Uredi na runbook sada, a zatim kliknite **Uređivanje Runbook**. U suprotnom, kliknite **u redu**.
4. Vaš novi runbook pojavit će se na kartici **Runbooks** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Da biste stvorili novi runbook Automatizacija Azure pomoću portala za Azure

1. Na portalu Azure otvorite računa za automatizaciju.
2. Kliknite pločicu **Runbooks** da biste otvorili popis runbooks.
3. Kliknite gumb **Dodaj u runbook** i zatim **Stvori novu runbook**.
2. Unesite **naziv** u runbook, a zatim odaberite [vrstu](automation-runbook-types.md). Naziv runbook mora započeti slovom i mogu imati slova, brojeve, podvlake i crtice.
3. Kliknite **Stvori** da biste stvorili na runbook i otvorite uređivač.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Da biste stvorili novi runbook Automatizacija Azure s komponentom Windows PowerShell

Pomoću cmdleta [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) da biste stvorili prazan [runbook PowerShell tijeka rada](automation-runbook-types.md#powershell-workflow-runbooks). Ili možete navesti **naziv** parametra da biste stvorili prazan runbook koje možete naknadno urediti ili navedete parametar **put** za uvoz datoteke runbook. Parametar **Type** trebao bi biti uključen i navesti jednu od četiri vrste runbook.

Sljedeće primjere naredbi pokazati kako stvoriti novi prazan runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Uvoz u runbook iz datoteke u Automatizacija Azure

Možete stvoriti nove runbook u automatizaciji Azure uvozom skriptu PowerShell ili PowerShell tijeka rada (.ps1 kućni broj) ili izvezene grafički runbook (.graphrunbook).  Morate navesti [vrstu kompilacije](automation-runbook-types.md) koji će se stvoriti iz uvoza uzimajući u obzir Imajte na umu sljedeće.

- Datoteke .graphrunbook se samo moguće uvesti u novi [grafički runbook](automation-runbook-types.md#graphical-runbooks)i grafički runbooks moguće je stvoriti samo iz datoteke .graphrunbook.
- .Ps1 datoteku koja sadrži PowerShell tijeka rada moguće je uvesti u [runbook PowerShell tijeka rada](automation-runbook-types.md#powershell-workflow-runbooks).  Ako dokument sadrži više PowerShell tijekova rada, zatim uvoza neće uspjeti. Morate spremiti svaki tijek rada na vlastitu datoteku, a uvesti svaki zasebno.
- .Ps1 datoteke koje sadrže tijeka rada moguće je uvesti u [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) ili [runbook PowerShell tijeka rada](automation-runbook-types.md#powershell-workflow-runbooks).  Ako uvezena je u tijek rada PowerShell runbook, zatim pretvorit će se u tijeku rada, a komentari će biti obuhvaćene runbook navodeći promjene koje ste napravili.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Da biste uvezli u runbook iz datoteka s portala za klasični Azure
Koristite sljedeći postupak da biste uvezli skriptna datoteka u Azure automatizaciju.  Imajte na umu da .ps1 datoteke možete uvesti samo u komponente PowerShell tijeka rada runbook pomoću ovog portala.  Morate koristiti Azure portal za ostale vrste.

1. Na portalu za upravljanje Azure odaberite **Automatizacija** , a zatim odaberite račun za automatizaciju.
2. Kliknite **Uvezi**.
3. Kliknite **potražite datoteku** , a zatim pronađite datoteku skriptu koju želite uvesti.
4. Ako želite Uredi na runbook sada, a zatim kliknite **Uređivanje Runbook**. U suprotnom, kliknite u redu.
5. Novi runbook pojavit će se na kartici **Runbooks** za automatizaciju račun.
6. Morate [objaviti na runbook](#publishing-a-runbook) biste mogli pokrenuti.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Da biste uvezli u runbook iz datoteka s portala za Azure
Koristite sljedeći postupak da biste uvezli skriptna datoteka u Azure automatizaciju.  

>[AZURE.NOTE] Imajte na umu da .ps1 datoteke možete uvesti samo u komponente PowerShell tijeka rada runbook pomoću portala za.

1. Na portalu Azure otvorite računa za automatizaciju.
2. Kliknite pločicu **Runbooks** da biste otvorili popis runbooks.
3. Kliknite gumb **Dodaj u runbook** i zatim **uvoza**.
4. Kliknite **Runbook datoteku** da biste odabrali datoteku koju želite uvesti
2. Ako je polje **naziv** , zatim imate mogućnost da biste ga promijenili.  Naziv runbook mora započeti slovom i mogu imati slova, brojeve, podvlake i crtice.
3. [Vrsta runbook](automation-runbook-types.md) označit će se automatski, ali možete promijeniti vrstu nakon što poduzmete primjenjivo ograničenja račun. 
3. Novi runbook pojavit će se na popisu runbooks za automatizaciju račun.
4. Morate [objaviti na runbook](#publishing-a-runbook) biste mogli pokrenuti.

>[AZURE.NOTE] Nakon uvoza grafički runbook ili grafički runbook tijeka rada PowerShell, imate mogućnost da biste pretvorili u drugu vrstu ako istaknuti. Ne možete pretvoriti tekstnih.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Da biste uvezli u runbook iz datoteka skripte s komponentom Windows PowerShell

[Uvoz AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet možete koristiti da biste uvezli skriptna datoteka kao skicu runbook PowerShell tijeka rada. Ako već postoji u runbook, uvoz neće uspjeti ako ne koristite na *– prisilno* parametar. 

Sljedeće primjere naredbi pokazati kako uvesti skriptna datoteka na runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Objavljivanje na runbook

Prilikom stvaranja i uvoza novi runbook, morate je objaviti biste mogli pokrenuti.  Svaki runbook u automatizaciji ima skicu i objavljena verzija. Samo objavljena verzija dostupna će se pokrenuti, a mogu se uređivati samo verzija skice. Objavljena verzija je ne utječe promjene verzija skice. Kada verzija skice moraju biti dostupne, objavite koju prebrisuje objavljena verzija verzijom skice.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Da biste objavili runbook pomoću portala za klasični Azure

1. Otvorite na runbook Azure klasični portalu.
1. Pri vrhu zaslona kliknite **autoru**.
1. Pri dnu zaslona kliknite **Objavi** , a zatim **da** da biste poruku o potvrdi.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Da biste objavili runbook pomoću portala za Azure

1. Otvorite na runbook na portalu za Azure.
1. Kliknite gumb **Uređivanje** .
1. Kliknite gumb **Objavi** i zatim **da** da biste poruku o potvrdi.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Da biste objavili runbook pomoću komponente Windows PowerShell

Cmdlet [Objavi AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) možete koristiti da biste objavili runbook s komponentom Windows PowerShell. Sljedeće primjere naredbi pokazati kako objaviti uzorka runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Daljnji koraci
- Da biste saznali kako biste iskoristili Runbook i Galerija za modul ljuske PowerShell, pročitajte članak [galerije Runbook i module za automatizaciju Azure](automation-runbook-gallery.md)
- Da biste saznali više o uređivanju runbooks PowerShell i PowerShell tijeka rada s tekstnih editor, potražite u članku [Uređivanje tekstnih runbooks u automatizaciji Azure](automation-edit-textual-runbook.md)
- Da biste saznali više o grafički runbook za izradu, potražite u članku [Graphical vremenu u Automatizacija Azure](automation-graphical-authoring-intro.md)
