<properties
   pageTitle="Provjerite vezu pristupnika | Microsoft Azure"
   description="U ovom se članku objašnjava Provjerite vezu pristupnika u modelu implementacije Voditelj resursa"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Provjerite je li pristupnik veze

Vezu pristupnika možete provjeriti na nekoliko različitih načina. U ovom se članku vidjet ćete kako provjeriti stanje veze za pristupnik za upravljanje resursima pomoću portala za Azure i pomoću komponente PowerShell.


## <a name="verify-using-powershell"></a>Provjerite je li pomoću komponente PowerShell

Morat ćete instalirati najnoviju verziju programa Azure resursima PowerShell cmdleti. Informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Dodatne informacije o korištenju upravitelja resursa cmdleta potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Korak 1: Prijavite se na račun za Azure

1. Otvorite konzole za PowerShell s dodatnim ovlastima i povezati s računom.

        Login-AzureRmAccount

2. Provjerite pretplate za račun.

        Get-AzureRmSubscription 

3. Navedite pretplatu u koju želite koristiti.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Korak 2: Provjerite je li vaša veza


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Provjerite je li pomoću portala za Azure

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Daljnji koraci

- Možete dodati virtualnim strojevima virtualne mreže. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

