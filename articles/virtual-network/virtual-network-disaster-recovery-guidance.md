<properties
    pageTitle="Što učiniti u slučaju programa prekidu Azure servisa Azure virtualne mreže koje utječu na | Microsoft Azure"
    description="Saznajte što učiniti u slučaju programa Azure service prekidu koje utječu na Azure virtualne mreže."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtualne mreže – poslovanje

##<a name="overview"></a>Pregled

Virtualne mreže (VNet) je logički prikaz mreže u oblaku. Omogućuje definiranje vlastitog privatnih prostora IP adresa i fazi s mrežom u podmreže. VNets služi kao pouzdanost granicu za hostiranje računalnim resurse kao što su virtualnim računalima sustava Azure i servise u Oblaku (web-tempiranja uloge). Na VNet omogućuje direktnu privatne IP komunikaciju između resursi koji se nalaze u njoj. Virtualne mreže također mogu biti povezane lokalne mreže kroz neku od mogućnosti hibridnog kao što je VPN pristupnika ili ExpressRoute.
 
Na VNet stvorit će se unutar opsega područja. Možete stvoriti VNets s isti prostor za adresu u dvije različite regije (odnosno NAM Istok i Zapad NAM, ali ne možete povezati ih međusobno izravno). 

##<a name="business-continuity"></a>Neprekidno poslovanje

Može postojati nekoliko različitih načina da program ne može biti nadziranje prekinuto. Dani regija nije moguće su potpuno odrezane zbog prirodnim Izrada ili djelomično Izrada zbog pogreške više uređaja i usluga. Utjecaj na servis VNet nije isti u svakoj od tih situacija.

**P: što učiniti u slučaju programa nedostupnosti cijelu područje? odnosno ako je područje potpuno umanjenu zbog prirodnim Izrada? Što se događa s virtualne mreže smješten u području?**

A: virtualne mreža i resursa u području problematične ostaje ne može pristupiti tijekom vremena prekidu servisa.

![Dijagram jednostavnog virtualne mreže](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**P: što može sam ponovno stvoriti istu virtualne mreže u nekoj drugoj regiji?**

A: virtualne mreže (VNet) je prilično laganih resurs. Možete pozvati Azure API-ji za stvaranje na VNet isti prostor za adresu u nekoj drugoj regiji. Da biste ponovno stvorili isti okruženju u kojem se nalazila u području problematične, imate API pozive za ponovno uvođenje servisa u Oblaku (web-tempiranja uloge) i virtualnim strojevima koji ste imali. Morat ćete i Okretni gore pristupnik VPN-a i povezivanje s lokalne mreže ako imali lokalnih povezivanja (primjerice kao hibridne implementacije).

Upute za stvaranje na VNet nalaze [u nastavku](./virtual-networks-create-vnet-arm-pportal.md). 

**P: mogu kopije VNet unutar zadanog područja se ponovno stvara u drugoj regiji na vrijeme?**

A: da, možete stvoriti dvije VNets koristi iste privatnih prostora IP adresa i resursima u dvije različite regije na vrijeme. Ako klijent je hosting internet nasuprotne servisi u na VNet, oni nije ste postavili promet Manager da biste zemlj usmjeravanje prometa u područje koje je aktivna. Međutim, klijent ne može povezati dva VNets s istom adresom prostora za njihove lokalne mreže kao prouzročiti usmjeravanje problema. U određeno vrijeme na Izrada gubitak VNet u jednom području, klijenta možete povezati VNet u području dostupne s usklađenim adresu prostora za svoje lokalne mreže.

Upute za stvaranje na VNet nalaze [u nastavku](./virtual-networks-create-vnet-arm-pportal.md).
