<properties 
   pageTitle="Postavke Runbook"
   description="U članku se opisuje konfiguracijske postavke runbook u Automatizacija Azure i kako ih promijeniti pomoću portala za upravljanje Azure i komponente Windows PowerShell."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Postavke Runbook

Svaki runbook u automatizaciji Azure ima više postavke kojih se provjeriti vaš identitet i da biste promijenili ponašanja zapisivanje. Svaki od tih postavki opisan ispod slijedi postupke kako ih mijenjati.

## <a name="settings"></a>Postavke

### <a name="name-and-description"></a>Naziv i opis

Ne možete promijeniti naziv u kompilacije kada je stvorena. Opis nije obavezan, a može biti do 512 znakova.

### <a name="tags"></a>Oznaka

Oznake omogućuju dodijeliti različite riječi i izraza da biste lakše prepoznali na runbook. Na primjer, kada pošaljete runbook u [Galeriji Runbook](https://msdn.microsoft.com/library/dn781422.aspx), navedite određeni oznake da biste odredili kategorija u runbook mora biti naveden u. Možete navesti više oznaka za na runbook tako da ih zarezima.

### <a name="logging"></a>Bilježenje u zapisnik

Prema zadanim postavkama, tekstni i napredak zapisi se ne zapisuju dosadašnje iskustvo. Možete promijeniti postavke za određeni runbook da biste se prijavili tih zapisa. Dodatne informacije o tih zapisa potražite u članku [Runbook izlazne i poruke](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Promjena postavki runbook

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Promjena postavki runbook s portala za upravljanje Azure

Na stranici **Konfiguracija** za na runbook možete promijeniti postavke za runbook na portalu za upravljanje Azure.

1. Na portalu za upravljanje Azure odaberite **Automatizacija** , a zatim kliknite naziv računa za automatizaciju.
1. Odaberite karticu **Runbooks** .
1. Kliknite naziv programa kompilacije.
1. Odaberite karticu **Konfiguracija** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Promjena postavki runbook s komponentom Windows PowerShell

[Postavljanje AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) cmdlet možete koristiti da biste promijenili postavke na runbook. Ako želite navesti više oznaka ili možete unijeti polje ili jednog niza s vrijednostima razdvojeno zarezom parametar oznake. Možete dobiti trenutne oznake s [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

Sljedeće primjere naredbi pokazati kako postaviti svojstva na runbook. Ovaj primjer dodaje tri oznake postojeće oznake i određuje moraju biti prijavljeni opširno zapisa.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Povezani članci
- [Izlaz Runbook i poruke](../automation-runbook-output-and-messages) 
- [Stvaranje ili uvoz u Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 