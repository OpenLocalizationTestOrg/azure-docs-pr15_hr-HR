<properties
    pageTitle="Stvaranje farme poslužitelja SharePoint | Microsoft Azure"
    description="Brzo stvaranje nove farme sustava SharePoint 2013 ili 2016 sustava SharePoint u Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>Stvaranje farme poslužitelja sustava SharePoint

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klasični model.

## <a name="sharepoint-2013-farms"></a>Farme sustava SharePoint 2013

S portala trgovine Microsoft Azure, možete brzo stvoriti unaprijed konfigurirana farme sustava SharePoint Server 2013. To možete uštedjeti dosta vremena kada vam je potrebna osnovne ili visoku dostupnost farme sustava SharePoint u okruženju razvojni i testiranje sustava ili ako su procjene SharePoint Server 2013 kao rješenja za suradnju za tvrtku ili ustanovu.

> [AZURE.NOTE] Uklonjena je stavku **Farme sustava SharePoint Server** Azure Marketplace Azure portal. To je zamijenjena stavke web-mjesto **Sustava SharePoint 2013 nisu SLIKA farme** i u okvir za **SharePoint 2013 HA farme** .

Osnovni farme sustava SharePoint sastoji se od tri virtualnim strojevima u ovoj konfiguraciji.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Možete koristiti tu konfiguraciju farme za pojednostavljeni postavljanje za razvoj aplikacija za SharePoint ili evaluacijsku prvo sustava SharePoint 2013.

Da biste stvorili osnovni farme sustava SharePoint (tri server):

1. Kliknite [ovdje](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Kliknite **Implementacija**.
3. U oknu za **SharePoint 2013 nisu SLIKA farme** , kliknite **Stvori**.
4. Određivanje postavki na korake **Stvaranje SharePoint 2013 nisu SLIKA farme** okna, a zatim kliknite **Stvori**.

Farme sustava SharePoint visoku dostupnost sastoji se od devet virtualnim strojevima u ovoj konfiguraciji.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Da biste testirali veći opterećenje klijent, visoke dostupnosti vanjskog web-mjesta sustava SharePoint i SQL Server AlwaysOn dostupnost grupe za farme sustava SharePoint možete koristiti tu konfiguraciju farme. Tu konfiguraciju možete koristiti i za razvoj aplikacija za SharePoint u okruženju visoku dostupnost.

Da biste stvorili farmi sustava SharePoint (devet server) visoku dostupnost:

1. Kliknite [ovdje](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Kliknite **Implementacija**.
3. U oknu **Farmi HA za SharePoint 2013** , kliknite **Stvori**.
4. Određivanje postavki na sedam koraka u oknu za **Stvaranje farmi sustava SharePoint 2013 SLIKA** , a zatim kliknite **Stvori**.

> [AZURE.NOTE] Nije moguće stvoriti **SharePoint 2013 nisu SLIKA farme** ili **SharePoint 2013 HA farmu** s besplatnu probnu verziju programa Azure.

Portal za Azure stvara obje te farme samo oblak virtualne mreže pomoću programa podaci o prisutnosti za web-mjesto na Internetu. Ne postoji web-mjesto VPN-a ili ExpressRoute veza natrag u vašoj mreži tvrtke ili ustanove.

> [AZURE.NOTE] Kada stvorite osnovni ili farmama sustava SharePoint visoku dostupnost pomoću portala za Azure, ne možete navesti postojeću grupu resursa. Da biste zaobišli to ograničenje, stvorite te farme s Azure PowerShell. Dodatne informacije potražite u članku [Stvaranje SharePoint 2013 razvojni i testiranje farmama s Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>Farme sustava SharePoint 2016

Pogledajte [članak](https://technet.microsoft.com/library/mt723354.aspx) upute da biste sastavili na jednom poslužitelju sustava SharePoint Server 2016 farmi poslužitelja.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Upravljanje farmama sustava SharePoint

Možete upravljati poslužitelje te farme putem veze udaljene radne površine. Dodatne informacije potražite u članku [prijavite se na virtualnog računala](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

S web-mjesta Središnja administracija sustava SharePoint, možete konfigurirati Moja web-mjesta, aplikacija sustava SharePoint i druge funkcije. Dodatne informacije potražite u članku [Konfiguriranje sustava SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Daljnji koraci

- Otkrijte dodatna [Konfiguracija sustava SharePoint](https://technet.microsoft.com/library/dn635309.aspx) u servisa Azure infrastrukture.
