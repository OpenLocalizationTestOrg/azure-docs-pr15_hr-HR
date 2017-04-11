<properties
 pageTitle="Proširenja virtualnog računala i značajke | Microsoft Azure"
 description="Saznajte što proširenja su dostupne za Azure virtualnim strojevima grupirane što oni omogućuju ili poboljšati njegovu."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>O proširenja virtualnog računala i značajkama

## <a name="azure-vm-extensions"></a>Azure VM proširenja

Azure proširenja virtualnog računala su male aplikacije koje omogućuju zadatka konfiguracije i Automatizacija objavu implementacije virtualnim računalima sustava na Azure. Na primjer, ako virtualnog računala potreban je softver zaštita instalirani, antivirusni ili Docker konfiguraciju, nastavkom VM može koristiti da biste dovršili ove zadatke. Azure VM proširenja mogu se izvoditi pomoću EŽA Azure, komponente PowerShell resursa Upravljanje predlošcima i Azure portal. Proširenja možete vezanoj instalaciji s novu implementaciju virtualnog računala ili pokrećete za sve postojeće sustava.

Ovaj dokument sadrži preduvjeti za nastavak Azure virtualnog računala i upute o tome kako prepoznati dostupna VM proširenja. 

## <a name="azure-vm-agent"></a>Agent za Azure VM

Agent za Azure VM upravlja interakciju između programa Azure virtualnog računala i kontrolerom tkanina Azure. Agent za VM je zadužen za mnoge funkcionalni aspekte uvođenju i upravljanju virtualnim računalima sustava Azure, uključujući pokretanje VM proširenja. Agent za Azure VM unaprijed instalirano Azure Galerija slika, a moguće je instalirati na podržani operacijski sustavi. 

Dodatne informacije o podržani operacijski sustavi i upute za instalaciju potražite [Azure Linux Agent korisničkom](./virtual-machines-linux-agent-user-guide.md)priručniku.

## <a name="discover-vm-extensions"></a>Otkrijte VM proširenja

Mnogo različitih VM proširenja dostupni su za korištenje s Azure virtualnim računalima. Da biste vidjeli popis, pokrenite sljedeću naredbu s EŽA Azure, mjesto zamjenjuje mjesto po izboru.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Uobičajeni VM proširenja

|Naziv proširenja   |Opis   |Dodatne informacije   |
|---|---|---|
|Prilagođene skripte proširenja za Linux  | Pokreću skripte na temelju programa Azure virtualnog računala  |[Prilagođene skripte proširenja za Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Proširenje docker |Instalira daemon Docker za podršku udaljene Docker naredbe.  | [Proširenje VM docker](./virtual-machines-linux-dockerextension.md)  |
|Nastavak VM programa Access | Ostvarili pristup za Azure virtualnog računala  |[Nastavak VM programa Access](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Proširenje Azure dijagnostiku |Upravljanje Azure dijagnostiku |[Proširenje Azure dijagnostiku](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

