<properties
   pageTitle="Kako dodati više veza pristupnika web-mjesto za VPN virtualne mreže za implementaciju model upravljanja resursima pomoću portala za Azure | Microsoft Azure"
   description="Dodavanje veza sa S2S više web-mjesta u pristupnik za VPN-a koji sadrži postojeće veze"
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
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Dodavanje veze za web-mjesto VNet s postojeće veze pristupnika VPN-a

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasični - PowerShell](vpn-gateway-multi-site.md)

U ovom se članku vodit će vas kroz pomoću portala za Azure da biste dodali veze web-mjesta web-na-mjesta (S2S) pristupnika VPN-a koji sadrži postojeće veze. Tu vrstu veze je često se nazivaju "više web-mjesta" konfiguracije. 

U ovom se članku možete koristiti da biste dodali veze S2S VNet na kojem je već instalirana S2S veze, točke web vezu ili VNet VNet veze. Postoje određena ograničenja prilikom dodavanja veze. Pogledajte sekciju [prije nego što počnete](#before) u ovom članku da biste provjerili prije nego što počnete konfiguraciju. 

Ovaj se članak odnosi na VNets stvoren pomoću model implementacije Voditelj resursa koji imaju RouteBased VPN pristupnika. Ove korake ne primjenjuju se na ExpressRoute/web-mjesta-na-web-mjesta coexisting veze konfiguracije. Informacije o coexisting vezama potražite u članku [ExpressRoute/S2S coexisting veze](../expressroute/expressroute-howto-coexist-resource-manager.md) .

### <a name="deployment-models-and-methods"></a>Uvođenje modela i postupci

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

U ovoj su tablici ažuriramo kao novih članaka i dodatne alate postaju dostupne za tu konfiguraciju. Kada je članak dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Prije početka

Provjerite sljedeće stavke:

- Ne stvarate vezu coexisting s ExpressRoute/S2S.
- Imate virtualne mreže koja je stvorena pomoću model implementacije resursima postojeće veze.
- Pristupnik virtualne mreže vaše VNet je RouteBased. Ako imate pristupnik PolicyBased VPN, morate izbrisati pristupnika virtualne mreže i stvaranje novog pristupnika VPN-a kao RoutBased.
- Ništa rasponi adresa preklapaju za bilo koju od VNets koji ovaj VNet povezati.
- Imate kompatibilan uređaj VPN-a i nekome tko se može konfigurirati. Potražite u članku [o uređajima VPN-a](vpn-gateway-about-vpn-devices.md). Ako ne poznajete konfiguriranje uređaju VPN, ili nisu poznati s IP adresom raspona koji se nalazi u vaše lokalne mreže konfiguracije, potrebno je koordinaciji s nekim tko može zatražiti te detalje za vas.
- Imate vanjsko dostupnog javnu IP adresa za svoj uređaj VPN-a. U ovom IP adresa nije ga moguće pronaći iza na NAT.


## <a name="part1"></a>Dio 1 - Konfiguriranje veze

1. U pregledniku idite na [portal za Azure](http://portal.azure.com) i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **sve resurse** i pronađite **virtualne mreže pristupnika** na popisu resursa, a zatim ga.
3. Na plohu **virtualne mrežni pristupnik** kliknite **veze**.

    ![Plohu veze](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Na plohu **veze** kliknite **+ Dodaj**.

    ![Dodavanje gumba veze](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Plohu **Dodaj vezu** popunite sljedeća polja:
    - **Naziv:** Naziv želite dati stvarate vezu s web-mjesta.
    - **Vrsta veze:** Odaberite **web-mjesta web-na-mjesta (IPsec)**.

    ![Dodavanje veze plohu](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Dio 2 – dodavanje pristupnika lokalne mreže

1. Kliknite **lokalne mreže pristupnika** ***Odaberite pristupnik lokalnoj mreži***. Otvorit će se plohu **pristupnika odaberite lokalne mreže** .

    ![Odaberite lokalnu mrežu pristupnika](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Kliknite **Stvori novi** da biste otvorili plohu **Stvaranje lokalne mreže pristupnika** .

    ![Stvaranje lokalne mreže pristupnika plohu](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Plohu **Stvaranje lokalne mreže pristupnika** popunite sljedeća polja:
    - **Naziv:** Naziv koji želite dati resursa za pristupnik za lokalnu mrežu.
    - **IP adresa:** Javnu IP adresu uređaja za VPN-a na web-mjesto koje želite povezati.
    - **Adresa prostora:** Prostor za adrese koje želite usmjeriti na novo mjesto u lokalnoj mreži.
4. Na plohu **pristupnika stvaranje lokalne mreže** da biste spremili promjene, kliknite **u redu** .

## <a name="part3"></a>Dio 3 - dodavanje zajedničke ključa i stvaranje veze

1. Na plohu **Dodaj vezu** dodajte zajedničko ključ koji želite koristiti da biste stvorili vezu. Možete zajednički ključ na uređaju VPN-a ili napravite nešto se ovdje i konfiguriranje uređaju VPN-a da biste koristili isti zajednički ključ. Važno je da tipke jednake.

    ![Zajednički ključ](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Pri dnu zaslona u plohu, kliknite **u redu** da biste stvorili vezu.

## <a name="part4"></a>Dio 4 - provjerite je li VPN veza

Možete provjeriti VPN veza na portalu ili pomoću komponente PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Daljnji koraci

- Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Pogledajte na virtualnim strojevima [tečaj](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) dodatne informacije.
