
<properties
    pageTitle="Provjera valjanosti VNET Azure će se koristiti za Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako provjerite je li vaša Azure VNET spremna za korištenje sa Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Provjerite valjanost Azure VNET će se koristiti za Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Prije korištenja programa Azure VNET s Azure RemoteApp, možda ćete na VNET za provjeru valjanosti. To pomaže u sprječavanju problema s povezivanjem.

Da biste provjerili valjanost vaše VNET Azure, učinite sljedeće:

1. Stvaranje Azure virtualnog računala unutar podmreže VNET Azure koji želite koristiti s Azure RemoteApp.

2. Povezivanje s tom VM pomoću mogućnosti **povezivanja** na portalu za upravljanje.
3. Pridružite virtualnog računala istu domenu koju želite koristiti s Azure RemoteApp. Ako stvarate hibridnog zbirke koja se povezuje s lokalne mreže, pridružite virtualnog računala lokalne domene.

Ako ne uspije, Azure VNET spreman je za korištenje s RemoteApp.

Dodatne informacije o tijeku rada za prikupljanje hibridnog završetka do kraja potražite u sljedećim člancima:

- [Kako planirati virtualne mreže Azure RemoteApp](remoteapp-planvnet.md)
- [Stvaranje zbirke hibridnog](remoteapp-create-hybrid-deployment.md)
- [Implementacija Azure RemoteApp zbirke Azure virtualne mreže (s podrškom za ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
