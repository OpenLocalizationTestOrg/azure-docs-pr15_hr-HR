<properties
 pageTitle="Proširenja virtualnog računala i značajke | Microsoft Azure"
 description="Saznajte što proširenja su dostupne za Azure virtualnim strojevima grupirane što oni omogućuju ili poboljšati njegovu."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>O proširenja virtualnog računala i značajkama

## <a name="azure-vm-extensions"></a>Azure VM proširenja

Azure proširenja virtualnog računala su male aplikacije koje omogućuju zadatka konfiguracije i Automatizacija objavu implementacije virtualnim računalima sustava na Azure. Na primjer, ako virtualnog računala potreban je softver zaštita instalirani, antivirusni ili Docker konfiguraciju, nastavkom VM može koristiti da biste dovršili ove zadatke. Azure VM proširenja mogu se izvoditi pomoću EŽA Azure, komponente PowerShell resursa Upravljanje predlošcima i Azure portal. Proširenja možete vezanoj instalaciji s novu implementaciju virtualnog računala ili pokrećete za sve postojeće sustava.

Ovaj dokument sadrži preduvjeti za nastavak Azure virtualnog računala i upute o tome kako prepoznati dostupna VM proširenja. 

## <a name="azure-vm-agent"></a>Agent za Azure VM

Agent za Azure VM upravlja interakciju između programa Azure virtualnog računala i kontrolerom tkanina Azure. Agent za VM je zadužen za mnoge funkcionalni aspekte uvođenju i upravljanju virtualnim računalima sustava Azure, uključujući pokretanje VM proširenja. Agent za Azure VM unaprijed instalirano Azure Galerija slika, a moguće je instalirati na podržani operacijski sustavi. 

Informacije o podržani operacijski sustavi i upute za instalaciju, potražite u članku [Agent Azure virtualnog računala](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Otkrijte VM proširenja

Mnogo različitih VM proširenja dostupni su za korištenje s Azure virtualnim računalima. Da biste vidjeli popis, pokrenite sljedeću naredbu s EŽA Azure, mjesto zamjenjuje mjesto po izboru.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Uobičajeni VM proširenja

|Naziv proširenja   |Opis   |Dodatne informacije   |
|---|---|---|
|Proširenje prilagođene skripte za Windows  | Pokreću skripte na temelju programa Azure virtualnog računala  |[Proširenje prilagođene skripte za Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC proširenja za Windows | Nastavak DSC (željena stanja konfiguracija) PowerShell.  | [Proširenje VM docker](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Proširenje Azure dijagnostiku | Upravljanje Azure dijagnostiku |[Proširenje Azure dijagnostiku](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
