<properties
    pageTitle="Upravljanje Azure ključ sigurnog korištenja Automatizacija Azure | Microsoft Azure"
    description="Saznajte kako se servisa Azure Automatizacija može koristiti da biste upravljali sigurnog ključ Azure."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Upravljanje Azure ključ sigurnog korištenja Automatizacija Azure

Ovaj vodič predstavljanje će vam servisa Azure Automatizacija i kako ga koristiti da biste pojednostavnili upravljanje tajne u sigurnog ključ Azure te tipki.

## <a name="what-is-azure-automation"></a>Što je Automatizacija Azure?

[Automatizacija Azure](../automation/automation-intro.md) je Azure usluga za pojednostavljivanje oblaka upravljanje putem Automatizacija procesa i konfiguracija željenom stanju. Korištenje Azure Automatizacija, ručno ponavlja, dugoročnih i pogreške podložni zadaci mogu se automatizirati da biste povećali pouzdanosti, učinkovitost i vremena za vrijednost za tvrtku ili ustanovu.

Azure Automatizacija nudi modul izvođenja iznimno pouzdan, Visoko dostupna tijeka rada koji se mijenja veličinu da bi odgovarao vašim potrebama. U automatizaciji Azure procesa može biti kicked ručno, sustavi 3 proizvođača ili u određenim intervalima tako da se zadaci dogoditi kada je potrebno.

Smanjite radu indirektni i oslobodili IT osoblje DevOps obavljanje posla koji dodaje poslovne vrijednosti pomicanjem zadataka upravljanja oblaka će se automatski pokrenuti po Automatizacija Azure.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Kako Azure Automatizacija pomaže u upravljanje sigurnog ključ Azure?

Ključ sigurnog upravlja u Automatizacija Azure pomoću [AzureRM ključ sigurnog cmdleta] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) i [cmdleta Azure klasični ključ sigurnog](https://msdn.microsoft.com/library/azure/dn868052.aspx). Modul Azure upravljanja klasični sigurnog ključ je dostupna automatski Azure Automatizacija, i [AzureRM KeyVault modul](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) možete uvesti Azure Automatizacija, tako da možete izvršavaju mnoge zadatke upravljanja ključ sigurnog unutar servisa. Možete uparivanje i tih cmdleta u automatizaciji Azure pomoću cmdleta za druge servise za Azure, da biste automatizirali zadatke složene preko servisa Azure i 3 sustavi proizvođača.

Pomoću cmdleta Azure ključ sigurnog možete izvršiti sljedeće zadatke između ostalog: 

- Stvaranje i konfiguriranje ključa zbirke ključeva
- Stvaranje ili uvoz ključa
- Stvaranje ili ažuriranje na tajna
- Ažurirati atribute ključ
- Nabavite ključ ili tajna
- Izbrišite ključ ili tajna

Evo nekoliko primjera pomoću komponente PowerShell za upravljanje sigurnog ključ:  

* [Azure ključa sigurnog - korak po korak](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Postavljanju i konfiguriranju Azure ključa sigurnog](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove Automatizacija Azure i kako ga možete koristiti za upravljanje Azure ključ sigurnog, slijedite ove veze da biste saznali više o Azure automatizaciju.

* Pročitajte članak Azure Automatizacija [Uvod vodiča](../automation/automation-first-runbook-graphical.md).
* U odjeljku [skripte Azure ključ sigurnog PowerShell](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
