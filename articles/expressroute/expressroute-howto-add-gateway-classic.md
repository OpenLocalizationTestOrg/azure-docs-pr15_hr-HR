<properties
   pageTitle="Konfiguriranje VNet pristupnik za ExpressRoute pomoću komponente PowerShell | Microsoft Azure"
   description="Konfiguriranje VNet pristupnika klasični implementacije modela VNet pomoću komponente PowerShell za konfiguriranje programa ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-classic-deployment-model-and-powershell"></a>Konfiguriranje pristupnik virtualne mreže za ExpressRoute pomoću model klasični implementacije i komponente PowerShell

> [AZURE.SELECTOR]
- [PowerShell – Voditelj resursa](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell – klasični](expressroute-howto-add-gateway-classic.md)

U ovom se članku će vas voditi kroz korake za dodavanje, promjena veličine i uklanjanje pristupnik virtualne mreže (VNet) za postojećih VNet. Koraci za konfiguraciju namijenjenu VNets koje su stvorene pomoću **model klasični implementacije** , a koje će se koristiti u konfiguraciji ExpressRoute. 

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Prije početka

Provjerite jeste li instalirali Azure PowerShell cmdleti koja su potrebna za tu konfiguraciju (1.0.2 ili noviji). Ako još niste instalirali na Cmdlete, morat ćete učiniti prije početka navedeni koraci za konfiguraciju. Dodatne informacije o instaliranju Azure PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

    
## <a name="next-steps"></a>Daljnji koraci

Nakon stvaranja VNet pristupnika možete povezati svoje VNet je elektronička ExpressRoute. Pogledajte [veze virtualne mreže da biste je elektronička ExpressRoute](expressroute-howto-linkvnet-classic.md).
