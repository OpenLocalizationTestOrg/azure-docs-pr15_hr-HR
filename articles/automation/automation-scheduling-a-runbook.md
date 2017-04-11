<properties 
   pageTitle="Planiranje runbook u automatizaciji Azure | Microsoft Azure"
   description="U članku se opisuje stvaranje rasporeda u automatizaciji Azure tako da se može automatski pokrenuti na runbook u određenom trenutku ili raspored koji se ponavlja."
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
   ms.date="08/05/2016"
   ms.author="bwren" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Planiranje runbook u automatizaciji Azure

Da biste zakazali runbook u automatizaciji Azure da biste pokrenuli u određeno vrijeme, povežete je jedan ili više rasporeda. Raspored mogu konfigurirati pokretanje jednom ili je ponovno pojavljivanje zaračunava ili dnevni raspored za runbooks na portalu za Azure klasični i runbooks na portalu za Azure, možete dodatno zakazati ih za tjedno, mjesečno, određene dane u tjednu ili dana u mjesecu ili određenog dana u mjesecu.  Na runbook moguće je povezati s više rasporeda i raspored može imati više runbooks povezan s njom.


## <a name="creating-a-schedule"></a>Stvaranje rasporeda

Možete stvoriti novi raspored za runbooks Azure portalu na portalu klasični ili s komponentom Windows PowerShell. Imate mogućnost stvaranja novog rasporeda prilikom povezivanja s runbook pomoću Azure klasični ili Azure portala za raspored.

>[AZURE.NOTE] Kada je raspored pridružiti na runbook, automatizacija pohranjuje trenutne verzije module na vašem računu i povezuje ih tom rasporedu.  To znači da ako ste modulu verzijom 1.0 na vašem računu kada stvorili raspored, a zatim ažurirajte modul verzije 2.0, raspored će nastaviti koristiti 1.0.  Da biste koristili verziju modula ažurirane, morate stvoriti novi raspored. 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Da biste stvorili novi raspored na portalu za Azure klasični

1. Azure klasični portalu odaberite Automatizacija, a zatim odaberite naziv računa za automatizaciju.
1. Odaberite karticu **Resursi** .
1. Pri dnu prozora kliknite **Dodavanje postavku**.
1. Kliknite **Dodaj raspored**.
1. Upišite **naziv** i po želji **Opis** novi raspored schedule.your pokretati **Jedanput**, **svaki sat**, **Dnevni**, **Tjedni**ili **mjesečni**.
1. Odredite **Vrijeme početka** i druge mogućnosti ovisno o vrsti raspored koji ste odabrali.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Da biste stvorili novi raspored na portalu za Azure

1. Na portalu Azure s računa automatizacije kliknite pločicu **Resursi** da biste otvorili plohu **Resursi** .
2. Kliknite pločicu **rasporeda** da biste otvorili plohu **rasporeda** .
3. Kliknite **Dodaj raspored** pri vrhu na plohu.
4. Na plohu **Novi raspored** , upišite **naziv** i neobavezan **Opis** novog rasporeda.
5. Odaberite hoće li se raspored pokretati trenutku ili reoccurring rasporedu tako da odaberete **jednom** ili **Ponavljanje**.  Ako odaberete **jednom** odredite **vrijeme početka** , a zatim kliknite **Stvori**.  Ako ste odabrali **Ponavljanje**, odredite **vrijeme početka** i učestalost koliko često želite da se runbook da biste ponovili - **sat**, **dan**, **tjedan**ili **mjesec**.  Ako odaberete **Tjedni** ili **mjesečni** s padajućeg popisa, **mogućnost ponavljanja** pojavit će se u na plohu nakon odabira, prikazat će se **mogućnost ponavljanja** plohu i ako odaberete **dan**, možete odabrati dan u tjednu.  Ako ste odabrali **mjesec**, možete odabrati **tjedan dana** ili određenih dana u mjesecu u kalendaru i na kraju, učinite želite pokrenuti na posljednji dan u mjesecu ili ne, a zatim kliknite **u redu**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Da biste stvorili novi raspored s komponentom Windows PowerShell

Pomoću cmdleta [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) da biste stvorili novi raspored u automatizaciji Azure za klasični runbooks ili cmdleta [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) za runbooks na portalu za Azure. Odredite vrijeme početka za raspored i učestalost je trebale bi funkcionirati.

Sljedeće primjere naredbi pokazati kako stvoriti novi raspored koji se pokreće svaki dan u 3:30 po na siječanj 20, 2015 počevši cmdleta za upravljanje servisom Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

Sljedeće primjere naredbi pokazuje kako stvoriti raspored za 15th i 30 svakog mjeseca pomoću cmdleta sustava Voditelj resursa Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
    

## <a name="linking-a-schedule-to-a-runbook"></a>Povezivanje s runbook raspored

Na runbook moguće je povezati s više rasporeda i raspored može imati više runbooks povezan s njom. Ako je na runbook parametre, možete unijeti vrijednosti za njih. Navedite vrijednosti za parametre obavezno i može vam dati vrijednosti za parametre nije obavezno.  Ove vrijednosti koristit će se svaki put na runbook je pokrenuo raspored.  Možete priložiti iste runbook na drugi raspored i navedite vrijednosti različitih parametara.


### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Da biste se povezali raspored runbook s portala za Azure klasični

1. Azure klasični portalu odaberite **Automatizacija** , a zatim kliknite naziv računa za automatizaciju.
2. Odaberite karticu **Runbooks** .
3. Kliknite naziv kompilacije za planiranje.
4. Kliknite karticu **raspored** .
5. Ako na runbook trenutno nije povezan s raspored, zatim imat ćete mogućnost da biste **vezu da biste novi raspored** ili **postojeći raspored**.  Ako na runbook trenutno povezani s raspored, kliknite **vezu** pri dnu prozora da biste pristupili tim mogućnostima.
6. Ako je na runbook parametara, zatražit će se za njihove vrijednosti.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Da biste se povezali raspored runbook s portala za Azure

1. Na portalu Azure s računa automatizacije kliknite pločicu **Runbooks** da biste otvorili plohu **Runbooks** .
2. Kliknite naziv kompilacije za planiranje.
3. Ako na runbook trenutno nije povezan s raspored, zatim imat ćete mogućnost da biste stvorili novi raspored ili povezivanje s postojećeg rasporeda.  
4. Ako je na runbook parametre, možete odabrati mogućnost **Izmijeni pokrenuti postavke (zadani: Azure)** i plohu **parametara** raspoređene koji unosite informacije sukladno tome.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Da biste se povezali raspored runbook s komponentom Windows PowerShell

[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) možete koristiti da biste se povezali raspored klasični runbook ili [Register AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet za runbooks na portalu za Azure.  Možete odrediti vrijednosti za parametre u runbook s parametrom parametara. Potražite u članku [pokretanje Runbook u automatizaciji Azure](automation-starting-a-runbook.md) dodatne informacije o određivanju vrijednosti parametara.

Sljedeće primjere naredbi pokazalo kako se povezati pomoću cmdleta za upravljanje servisom Azure parametara raspored.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

Sljedeće primjere naredbi pokazuju kako povezati raspored runbook pomoću cmdleta sustava Voditelj resursa Azure parametara.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Onemogućivanje raspored

Kada onemogućite raspored, sve runbooks povezan s njom više neće se izvoditi na tom rasporedu. Ručno možete onemogućiti raspored ili postaviti vrijeme isteka rasporeda s na učestalost kada ih stvarate. Kada je dostigne vrijeme isteka, onemogućit će se raspored.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Da biste onemogućili raspored s portala za Azure klasični

Možete onemogućiti zakazivanje na portalu Azure klasični na stranici Detalji o raspored za raspored.

1. Azure klasični portalu odaberite Automatizacija, a zatim kliknite naziv računa za automatizaciju.
1. Odaberite karticu resursi.
1. Kliknite naziv raspored da biste otvorili njezinu stranicu pojedinosti.
2. Promijenite **omogućeno** **bez**.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Da biste onemogućili raspored s portala za Azure

1. Na portalu Azure s računa automatizacije kliknite pločicu **Resursi** da biste otvorili plohu **Resursi** .
2. Kliknite pločicu **rasporeda** da biste otvorili plohu **rasporeda** .
2. Kliknite naziv raspored da biste otvorili plohu pojedinosti.
3. Promijenite **omogućeno** **bez**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Da biste onemogućili raspored s komponentom Windows PowerShell

[Postavljanje AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet možete koristiti da biste promijenili svojstva postojeći raspored za klasični runbook ili [Skup AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet za runbooks na portalu za Azure. Da biste onemogućili raspored, navedite **false** za parametar **IsEnabled** .

Sljedeće primjere naredbi pokazati kako onemogućiti raspored pomoću cmdleta za upravljanje servisom Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

Sljedeće primjere naredbi pokazati kako onemogućiti raspored za runbook pomoću cmdleta sustava Voditelj resursa Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o radu s rasporeda, potražite u članku [Raspored resursima u Automatizacija Azure](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- Početak rada s runbooks u automatizaciji Azure, potražite u članku [pokretanje Runbook u automatizaciji Azure](automation-starting-a-runbook.md) 