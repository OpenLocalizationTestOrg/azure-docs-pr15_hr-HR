<properties 
    pageTitle="Uređivanje tekstnih runbooks u automatizaciji Azure"
    description="U ovom članku navedene različite postupke za rad s PowerShell i tijek rada PowerShell runbooks u automatizaciji Azure tekstnih uređivaču."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Uređivanje tekstnih runbooks u automatizaciji Azure

Tekstnih editor u automatizaciji Azure mogu se uređivati [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) i [runbooks PowerShell tijeka rada](automation-runbook-types.md#powershell-workflow-runbooks). To je standardne značajke druge kod uređivača kao što su intellisense i kod boje s dodatnim značajkama posebno kao pomoć u pristupa resursima zajedničkih runbooks.  Ovaj članak sadrži detaljne upute za izvođenje različite funkcije pomoću ovog uređivača.

Tekstnih editor sadrži značajku da biste umetnuli kod za aktivnosti, resursi i podređenih runbooks na runbook. Umjesto upisivanja u kodu sami, možete odabrati s popisa dostupnih resursa i imate odgovarajuću šifru umetnut u runbook.

Svaki runbook u automatizaciji Azure ima dvije verzije skice i objavljena. Uređivanje verzija skice u kompilacije i ga objavite tako da možete izvršiti. Objavljena verzija nije moguće uređivati. Dodatne informacije potražite [u runbook za objavljivanje](automation-creating-importing-runbook.md#publishing-a-runbook) .

Da biste radili s [Grafički Runbooks](automation-runbook-types.md#graphical-runbooks), potražite u članku [Graphical vremenu u Azure automatizaciju](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Da biste uredili runbook s portala za Azure

Da biste otvorili runbook za uređivanje u uređivaču tekstualnih, koristite sljedeći postupak.

1. Na portalu Azure odaberite svoj račun za automatizaciju.
2. Kliknite pločicu **Runbooks** da biste otvorili popis runbooks.
3. Kliknite naziv kompilacije koji želite urediti, a zatim kliknite gumb **Uredi** .
6. Izvršite potrebne uređivanja.
7. Kada dovršite uređivanjem, kliknite **Spremi** .
8. Ako želite da se najnovija verzija skice kompilacije za objavljivanje, kliknite **Objavi** .

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Da biste umetnuli u cmdlet na runbook

2. U područje crtanja uređivaču tekstualnih, postavite pokazivač na mjesto na koje želite smjestiti cmdlet.
3. Proširite čvor **cmdleta** u kontroli biblioteke. 
3. Proširite modul koji sadrži cmdlet koji želite koristiti.
4. Desnom tipkom miša kliknite cmdlet za umetanje, a zatim odaberite **Dodaj u platna**.  Ako cmdlet ima više od jedne parametar postavljen, zatim zadani skup dodat će se.  Možete proširiti i cmdlet da biste odabrali skup različite parametara.
4. Šifra za cmdlet se umeće s njegova cijeli popis parametara.
5. Unesite odgovarajuću vrijednost umjesto vrstu podataka okruženo <> vitičastih zagrada za zahtijeva parametre.  Uklonite sve parametre koji vam nisu potrebni.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Da biste umetnuli kod za dijete runbook na runbook

2. U području crtanja tekstnih uređivača postavite pokazivač na mjesto na koje želite umetnuti kod za [dijete runbook](automation-child-runbooks.md).
3. Proširite čvor **Runbooks** u kontroli biblioteke. 
3. Desnom tipkom miša kliknite runbook za umetanje, a zatim odaberite **Dodaj u platna**.
4. Šifra za runbook podređeni se umeće s bilo kojeg rezerviranim mjestima za sve parametre runbook.
5. Zamjena rezerviranih mjesta odgovarajuće vrijednosti za svaki parametar.

### <a name="to-insert-an-asset-into-a-runbook"></a>Da biste umetnuli imovine s runbook

2. U području crtanja tekstnih uređivača postavite pokazivač na mjesto na koje želite umetnuti kod za runbook podređenih.
3. Proširite čvor **sredstvima** u kontroli biblioteke. 
4. Proširite čvor vrste resursa koje želite.
3. Desnom tipkom miša kliknite resursa za umetanje, a zatim odaberite **Dodaj u platna**.  [Varijable resursi](automation-variables.md)odaberite **Dodaj "Dohvati varijable" kako biste** ili **Dodavanje "Postavljanje varijable" kako biste** ovisno o tome želite li dobiti ili postavljanje varijable.
4. Kod za sredstvo je umetnut u runbook.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Da biste uredili runbook s portala za Azure

Da biste otvorili runbook za uređivanje u uređivaču tekstualnih, koristite sljedeći postupak.

1. Na portalu Azure odaberite **Automatizacija** , a zatim kliknite naziv računa za automatizaciju.
2. Odaberite karticu **Runbooks** .
3. Kliknite naziv koji želite urediti, a zatim odaberite karticu **Autor** kompilacije.
5. Kliknite gumb **Uređivanje** pri dnu zaslona.
6. Izvršite potrebne uređivanja.
7. Kada dovršite uređivanjem, kliknite **Spremi** .
8. Ako želite da se najnovija verzija skice kompilacije za objavljivanje, kliknite **Objavi** .

### <a name="to-insert-an-activity-into-a-runbook"></a>Da biste umetnuli aktivnosti u Runbook

1. U području crtanja tekstnih uređivača postavite pokazivač na mjesto na koje želite smjestiti aktivnosti.
1. Pri dnu zaslona kliknite **Umetni** , a zatim **aktivnosti**.
1. U stupcu **Integracija modul** odaberite modul koji sadrži aktivnosti.
1. U oknu **aktivnosti** odaberite aktivnost.
1. U stupcu **Opis** , imajte na umu opis aktivnosti. Ako želite, kliknite prikaz detaljne pomoć za pokretanje pomoći za aktivnosti u pregledniku.
1. Kliknite strelicu desno.  Ako je aktivnost parametara, bit će navedeni za vaše informacije.
1. Kliknite gumb Provjeri.  Kod da biste pokrenuli aktivnost umetnut će se u runbook.
1. Ako aktivnost zahtijeva parametre, navedite odgovarajuće vrijednosti umjesto vrstu podataka okruženo <> vitičastih zagrada.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Da biste umetnuli kod za dijete runbook na runbook

1. U području crtanja uređivaču tekstnih postavite pokazivač na mjesto na koje želite smjestiti [podređeni runbook](automation-child-runbooks.md).
2. Pri dnu zaslona kliknite **Umetanje** , a zatim **Runbook**.
3. Odaberite runbook da biste umetnuli iz centra za stupca i kliknite strelicu desno.
4. Ako je na runbook parametara, bit će navedeni za vaše informacije.
5. Kliknite gumb Provjeri.  Kod za izvođenje odabrane runbook umetnut će se trenutni runbook.
7. Ako na runbook zahtijeva parametre, navedite odgovarajuće vrijednosti umjesto vrstu podataka okruženo <> vitičastih zagrada.

### <a name="to-insert-an-asset-into-a-runbook"></a>Da biste umetnuli imovine s runbook

1. U području crtanja tekstnih uređivača postavite pokazivač na mjesto na koje želite smjestiti aktivnosti za dohvaćanje sredstava.
1. Pri dnu zaslona kliknite **Umetanje** , a zatim **Postavke**.
1. U stupcu **Akcija za postavku** odaberite akciju koju želite.
1. Odaberite dostupna sredstva u stupcu centra.
1. Kliknite gumb Provjeri.  Kod da biste dobili ili postaviti imovine umetnut će se u runbook.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Da biste uredili programa automatizacije Azure runbook pomoću komponente Windows PowerShell

Da biste uredili runbook s komponentom Windows PowerShell, koristite uređivač po izboru i spremite ga u .ps1 datoteku. Cmdlet [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) možete koristiti za dohvaćanje sadržaj u runbook, a zatim [Postavljanje AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) cmdlet da biste zamijenili postojeće runbook skica izmijenjene jedan.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Da biste dohvatili sadržaj kompilacije pomoću komponente Windows PowerShell

Sljedeće primjere naredbi pokazuju kako dohvatiti skripte za na runbook i spremite je skriptna datoteka. U ovom primjeru dohvaća verzija skice. Također je moguće dohvatiti objavljena verzija na kompilacije Iako ova verzija ne može se promijeniti.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Da biste promijenili sadržaj kompilacije pomoću komponente Windows PowerShell

Sljedeće primjere naredbi pokazati kako zamijeniti postojeći sadržaj na kompilacije sadržaj skriptna datoteka. Imajte na umu da je isti postupak uzorka kao [Da biste uvezli runbook iz datoteka skripte s komponentom Windows PowerShell](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Povezani članci

- [Stvaranje ili uvoz runbook u automatizaciji Azure](automation-creating-importing-runbook.md)
- [Učenje PowerShell tijeka rada](automation-powershell-workflow.md)
- [Grafički vremenu u Automatizacija Azure](automation-graphical-authoring-intro.md)
- [Certifikati](automation-certificates.md)
- [Veze](automation-connections.md)
- [Vjerodajnice](automation-credentials.md)
- [Rasporedi](automation-schedules.md)
- [Varijable](automation-variables.md)