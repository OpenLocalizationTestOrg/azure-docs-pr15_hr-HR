<properties
    pageTitle="Upravljanje Azure servisa Bus korištenju automatizacije Azure | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure Automatizacija upravljanje Bus servisa Azure."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Upravljanje Bus servisa Azure pomoću automatizacije Azure

Ovaj vodič predstavljanje će vam na Azure Automation Services i kako ga koristiti da biste pojednostavnili upravljanje Bus servisa Azure.

## <a name="what-is-azure-automation"></a>Što je Automatizacija Azure?

[Automatizacija Azure](../automation/automation-intro.md) je Azure usluga za pojednostavljivanje oblaka upravljanje putem Automatizacija procesa i konfiguracija željenom stanju. Korištenje Azure Automatizacija, ručno ponavlja, dugoročnih i pogreške podložni zadaci mogu se automatizirati da biste povećali pouzdanosti, učinkovitost i vremena za vrijednost za tvrtku ili ustanovu.

Azure Automatizacija nudi modul izvođenja iznimno pouzdan, Visoko dostupna tijeka rada koji se mijenja veličinu da bi odgovarao vašim potrebama. U automatizaciji Azure procesa može biti kicked ručno, sustavi 3 proizvođača ili u određenim intervalima tako da se zadaci dogoditi kada je potrebno.

Smanjite radu indirektni i oslobodili IT osoblje DevOps obavljanje posla koji dodaje poslovne vrijednosti pomicanjem zadataka upravljanja oblaka će se automatski pokrenuti po Automatizacija Azure.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Kako Azure automatizaciju omogućuju upravljanje Bus servisa Azure?

Možete upravljati Bus servisa s Automatizacija Azure pomoću [Servisa Bus REST API -JA](https://msdn.microsoft.com/library/azure/mt639375.aspx). Unutar Azure Automatizacija možete pokrenuti skripte komponente PowerShell za provođenje mnoge servisa Bus zadataka pomoću REST API-JA. Možete uparivanje i te REST API-JA pozive u automatizaciji Azure cmdleta sustava za druge servise za Azure, da biste automatizirali zadatke složene preko servisa Azure i 3 sustavi proizvođača.

Evo nekoliko primjera pomoću komponente PowerShell za upravljanje Bus servisa Azure:

* [Prilagođenih cmdleta ljuske PowerShell za upravljanje redovima Bus servisa Azure](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Kako stvoriti redova Bus servisa, teme i pretplata pomoću skriptu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Stvaranje Azure servisa knjižna se prostori naziva pomoću komponente PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

Modul ljuske PowerShell za rad s Azure service bus u runbooks Automatizacija se možete preuzeti iz [Galerije PowerShell](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove Automatizacija Azure i kako ga možete koristiti za upravljanje Bus servisa Azure, slijedite ove veze da biste saznali više o Azure automatizaciju.

* Potražite u članku Azure Automatizacija [Uvod vodiča](../automation/automation-first-runbook-graphical.md)
* Pogledajte kako [upravljati Bus servisa sa servisom PowerShell](service-bus-powershell-how-to-provision.md)
