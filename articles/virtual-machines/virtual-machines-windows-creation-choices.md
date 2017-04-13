<properties
    pageTitle="Drugi načini stvaranja Windows VM | Microsoft Azure"
    description="Popis različitih načina za stvaranje virtualnog računala za Windows s Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Drugi načini stvaranja virtualnog računala za Windows s Voditelj resursa

Azure nudi različitih načina stvaranja virtualnog računala jer virtualnim strojevima prilagođenih različitim korisnicima i svrhe. To znači da morate izvršiti neke odabir o virtualnog računala i kako ga stvoriti. U ovom se članku daje sažetak tih mogućnosti i veza na upute.

## <a name="azure-portal"></a>Portal za Azure

Pomoću portala za Azure je jednostavan način da biste isprobali virtualnog računala, osobito ako ste tek započinjete s Azure. 

[Stvaranje virtualnog računala sa sustavom Windows pomoću portala](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Predložak

Virtualnim strojevima zahtijevaju kombinaciju resursa (kao što je dostupnost skupova i pohranu računi). Umjesto uvođenju i upravljanju svaki resurs zasebno, možete stvoriti predložak Azure Voditelj resursa koji implementira dodjeljuje sve resurse u jednom, usklađenih postupak.

- [Stvaranje virtualnog računala za Windows s predloškom Voditelj resursa](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Ako biste radije rad u ljusci za naredbu, možete koristiti Azure PowerShell.

- [Stvaranje Windows VM pomoću komponente PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Pomoću programa Visual Studio omogućuje stvaranje, upravljanje i implementacija VMs pomoću alata za Azure za Visual Studio i Azure SDK.

[Azure alate za Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

