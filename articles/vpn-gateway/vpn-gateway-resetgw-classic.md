<properties
   pageTitle="Ponovno postavljanje pristupnika za Azure VPN | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz vraćanje pristupnika za VPN Azure. U članku odnosi se na pristupnika VPN-a i na klasični i implementaciju modelima Voditelj resursa."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Ponovno postavljanje pristupnika VPN Azure koja se pomoću komponente PowerShell


U ovom se članku vodit će vas kroz ponovnom pristupnikom VPN Azure pomoću cmdleta ljuske PowerShell. Ove upute obuhvaćaju model za klasični uvođenje i implementaciju model upravljanja resursima.

Vraćanje pristupnika Azure VPN je korisno ako izgubite više lokacija grupa podaci na jednu ili više tunnels S2S VPN-a. U tom slučaju uređajima VPN lokalnog sve rade ispravno, ali ne moći uspostaviti IPsec tunnels s Azure VPN pristupnika. 

Svaki pristupnika Azure VPN sastoji se od dvije instance VM koji se izvodi u konfiguraciji aktivno čekanja. Kada pomoću cmdleta ljuske PowerShell za vraćanje pristupnika, izvršen pristupnika i zatim ponovno primjenjuje konfiguracije lokacija u nju. Pristupnik čuva javnog već ima IP adresa. To znači da nećete morati ažurirati konfiguracije usmjerivača VPN novu javnu IP adresu za pristupnik za Azure VPN-a.  

Kada se izda naredbu, aktivni trenutne instance pristupnika Azure VPN ponovo ne pokrene odmah. Tijekom prebacivanje iz aktivne instance (koja se sustava), čekanja instanci će biti kratak praznina. Razmak mora biti manja od jedne minute.

Ako nakon prvo pokretanje se obnovi veza, problema ista naredba da biste ponovno drugu pojavu VM (novi aktivni pristupnik). Ako dva ponovna se od vas traži da biste vratili, pojavit će se malo dulje razdoblje gdje su oba VM instance (aktivna i čekanja) koja se sustava. Time će dulje razmak na povezivanje VPN, do 2 do 4 minuta za VMs da biste dovršili na ponovna.

Nakon dvije ponovna ako i dalje imate problema s povezivanjem više lokacija, otvorite zahtjev za podršku na portalu Azure.

## <a name="before-you-begin"></a>Prije početka

Prije nego što ponovno pristupnikom, provjerite je li ključne stavke navedene za svaku tunelom VPN IPsec web-mjesta web-na-mjesta (S2S). Odspajanje S2S VPN tunnels rezultirat će bilo koji ne podudaraju u stavke. Provjera i ispravljanje konfiguracije za lokalni i Azure VPN pristupnika štedite nepotrebne ponovna i to izbjeglo za ostale rad veze na pristupnika.

Prije pristupnikom, provjerite sljedeće stavke:

- Internet IP adrese (VIPs) za pristupnik za Azure VPN i VPN pristupnika lokalnog ispravno konfigurirane u Azure i pravilnicima za VPN-a na lokalni.
- Zajednički ključ mora biti jednak na Azure i lokalnih VPN pristupnika.
- Ako primijenite određenu konfiguraciju IPsec-IKE, kao što je šifriranje, raspršivanje algoritmi i PFS (savršen naprijed očuvanje), Azure i lokalnih pristupnika VPN-a provjerite imaju li iste konfiguracije.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Ponovno postavljanje VPN pristupnika pomoću model za implementaciju upravljanja resursa

Cmdlet ljuske PowerShell Voditelj resursa za vraćanje pristupnika je `Reset-AzureRmVirtualNetworkGateway`. Sljedeći primjer vraća Azure VPN pristupnika, "VNet1GW" u grupu resursa "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Ponovno postavljanje VPN pristupnika pomoću model klasični implementacije

Cmdlet ljuske PowerShell za vraćanje pristupnika Azure VPN je `Reset-AzureVNetGateway`. Sljedeći primjer vraća pristupnika Azure VPN-a za virtualne mreže pod nazivom "ContosoVNet".
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Rezultat:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Daljnji koraci
    
Potražite u članku [Upravljanje servisom PowerShell cmdlet reference](https://msdn.microsoft.com/library/azure/mt617104.aspx) i [reference cmdlet Voditelj resursa PowerShell](http://go.microsoft.com/fwlink/?LinkId=828732) dodatne informacije.






