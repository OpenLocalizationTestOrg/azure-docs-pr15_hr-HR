<properties
    pageTitle="Stvaranje virtualnog računala skaliranje skup pomoću portala za Azure | Microsoft Azure"
    description="Implementacija skaliranje skupove pomoću portala za Azure."
    keywords="Skupovi skaliranje virtualnog računala" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Stvaranje virtualnog računala skaliranje skup pomoću portala za Azure

Pomoću ovog praktičnog vodiča prikazuje kako je jednostavno stvaranje postavljanje na skaliranje virtualnog računala u samo nekoliko minuta, pomoću portala za Azure. Ako nemate pretplatu na Azure, stvorite [pomoću računa](https://azure.microsoft.com/free/) prije nego što počnete.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Odaberite sliku VM iz trgovine

Na portalu jednostavno možete implementirati skalu postaviti CentOS, CoreOS, Debian, otvaranje Suse, crveno je vaša Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server i Windows Server slike.

Najprije pronađite [Azure portal](https://portal.azure.com) u web-pregledniku. Kliknite `New`, potražite `scale set`, a zatim odaberite u `Virtual machine scale set` unosa:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Stvaranje Linux virtualnog računala

Sada možete koristiti zadane postavke i brzo stvaranje virtualnog računala.

* Na na `Basics` plohu, unesite naziv za skup mjerilo. Ovaj naziv postane osnovni FQDN opterećenja ispred skup mjerilo, pa provjerite je li naziv jedinstveni preko svih Azure.

* Odaberite željeni OS-a upišite, unesite željenu korisničko ime i odaberite koje provjere autentičnosti upišite radije. Ako odaberete lozinke, mora biti najmanje 12 znakova dugi i tri iz četiri sljedeće složenosti koje zadovoljavaju: znak jedan malim slovima, jedan veliko, jedan broj i jedan posebnog znaka. Potražite dodatne informacije o [preduvjetima za korisničko ime i lozinku](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Ako ste odabrali `SSH public key`, provjerite jeste li samo Zalijepi u javni ključ, ne privatni ključ:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Unesite naziv grupe željeni resursa i lokaciju, a zatim kliknite `OK`.

* Na na `Virtual machine scale set service settings` plohu: Unesite naziv naljepnice željene domene (osnovni FQDN za opterećenja ispred skup skaliranje). Ova etiketa mora biti jedinstvena u svim Azure.

* Odaberite sliku na disku željeno operacijskog sustava, broj instanci i stroj veličina.

* Omogućiti ili onemogućiti automatsko skaliranje i konfiguracije ako je omogućen:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Na na `Summary` plohu, kad je provjera valjanosti, kliknite `OK`.

* Na kraju, na na `Purchase` plohu, kliknite `Purchase` da biste pokrenuli skale postavljanje implementacije.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Povezivanje s VM u skupu skala

Kada je vaš skaliranje skup implementiran, prijeđite na `Inbound NAT Rules` opterećenja za skup skaliranje na kartici:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Možete se povezati svaki VM u skale skupa pomoću tih NAT pravila. Na primjer, za skaliranje skup Windows ako je pravilo NAT na ulazni priključak 50000, povezati na tom računalu putem RDP na `<load-balancer-ip-address>:50000`. Za skup skaliranje Linux želite povezati pomoću naredbe `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Daljnji koraci

Dokumentacija o tome kako implementirati skupovima skala s na EŽA potražite u članku [ove dokumentacije](./virtual-machine-scale-sets-cli-quick-create.md).

Dokumentacija o tome kako implementirati skupovima skala s PowerShell potražite u članku [ove dokumentacije](./virtual-machine-scale-sets-windows-create.md).

Dokumentacija o tome kako implementirati skupove skala s Visual Studio, potražite u članku [ove dokumentacije](./virtual-machine-scale-sets-vs-create.md).

Općenito dokumentacije, pogledajte [postavlja dokumentaciju implicitno skaliranja](./virtual-machine-scale-sets-overview.md).

Opće informacije potražite u članku [glavni odredišna stranica za skupove mjerilo](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

