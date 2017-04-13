<properties
   pageTitle="Izmjena lokalne mreže pristupnik IP adresa prefiksi i IP pristupnika | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz promjena IP adresa prefiksi za pristupnik za lokalnu mrežu"
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Izmjena postavki pristupnika lokalne mreže pomoću komponente PowerShell

Ponekad promijenite postavke lokalne mreže pristupnika AddressPrefix ili GatewayIPAddress. Upute u nastavku pomoći će vam izmijenite postavke lokalne mreže pristupnika. Možete izmijeniti i ove postavke na portalu za Azure.

## <a name="before-you-begin"></a>Prije početka
    
Morat ćete instalirati najnoviju verziju programa Azure resursima PowerShell cmdleti. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="to-modify-ip-address-prefixes"></a>Da biste izmijenili IP adresa prefiksa

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Da biste izmijenili IP adresa pristupnika

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Daljnji koraci

Vezu pristupnika možete provjeriti. Potražite u članku [Potvrda pristupnika veze](vpn-gateway-verify-connection-resource-manager.md).

