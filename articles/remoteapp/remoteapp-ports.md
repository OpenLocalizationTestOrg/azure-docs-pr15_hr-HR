
<properties
    pageTitle="Popis priključaka i URL-ovi za whitelist za Azure RemoteApp implementiran u kupca virtualne mreže | Microsoft Azure"
    description="Saznajte koji su priključci i URL-ovi morat ćete konfigurirati za komunikaciju putem Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Popis priključaka i URL-ovi dopustiti pristup za Azure RemoteApp implementiran u kupca virtualne mreže 

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Sljedeće primjenjuje Azure RemoteApp oblaka ili hibridnog zbirke ako su implementacija u virtualne mreže (VNET). Dodatne informacije o virtualne mreže, pročitajte [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md). Ako ste stvorili mreže sigurnosne grupe (NSG) ograničavanje promet virtualne mreže resursa koji ste odabrali za Azure RemoteApp, provjerite sljedeće su dostupne i dopuštene kroz sigurnosna pravila na virtualne mreže. Dodatne informacije o sigurnosnim grupama s mrežom, pročitajte [što je sigurnosne grupe mreže? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp podmreže mora pristup te krajnje točke i URL-ova: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Izlazni: TCP: 443, TCP: 10101 10175 
*    Neobavezni – UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp klijenti potreban je pristup te krajnje točke i URL-ova: 

Klijenti znači li stolna računala, uređaji etc. koja je korisnicima povezati pomoću aplikacije u uveden u zbirci Azure RemoteApp.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (nije obavezno UDP priključci su za tu adresu) 
-  https://login.Windows.NET  
-  https://login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Izlazni: TCP: 443  
-  Neobavezno - UDP: 3391 