<properties
   pageTitle="Konfiguriranje virtualne mreže i pristupnik za ExpressRoute na portalu klasični | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz postavljanje virtualne mreže ExpressRoute pomoću model klasični implementacije i klasični portal."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Stvaranje virtualne mreže za ExpressRoute na portalu klasični

Koraci u ovom članku će vas voditi kroz konfiguriranje virtualne mreže i pristupnika virtualne mreže za korištenje s ExpressRoute pomoću model klasični implementacije i klasični portal.

Ako tražite upute za implementaciju model Voditelj resursa, možete koristiti u sljedećim člancima: [Stvaranje virtualne mreže pomoću komponente PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) i [Dodavanje VPN pristupnik za upravljanje resursima VNet za ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Stvaranje klasične VNet i pristupnika

Sljedeći koraci stvorite klasični VNet i pristupnika virtualne mreže. Ako već imate klasični VNet, u odjeljku [Konfiguriranje postojeće klasični VNet](#config) u ovom članku.

1. Prijavite se na [portal za Azure klasični](http://manage.windowsazure.com).

2. U donjem lijevom kutu zaslona kliknite **Novo**. U navigacijskom oknu kliknite **Mrežnim servisima**, a zatim **Virtualne mreže**. Kliknite **Stvaranje Prilagođeno** da biste pokrenuli čarobnjak za konfiguraciju.

3. Na stranici **Virtualne mreže pojedinosti** unesite sljedeće:

    - **Naziv** – naziv virtualne mreže. Ovaj naziv virtualne mreže ćete koristiti prilikom implementacije VMs i PaaS slučajevima, tako da ne preporučujemo vam da promijenite naziv previše složeno.
    - **Mjesto** – mjesto izravno povezani s fizičke mjesto (regija) koje želite da se Resursi (VMs) da biste nalaze. Na primjer, ako želite VMs koji implementirati virtualne mreži fizički nalazi u SAD-a za istočnoazijske, odaberite mjesto. Ne možete promijeniti regija povezani s mrežom virtualne kad ga stvorite.

4. Na stranici **DNS poslužitelji i grupa podaci** , unesite sljedeće podatke, a zatim sljedeće strelicu dolje desno. 

    - **DNS poslužitelji** - unesite naziv poslužitelja za DNS-a i IP adresa ili odaberite prethodno registrirani DNS poslužitelj na izborniku prečaca. Ta postavka nije moguće stvoriti DNS poslužitelj. Omogućuje određivanje DNS poslužitelji koji želite koristiti za razrješavanje imena u pošti virtualne mreže.
    - **Povezivanje web-mjesta na web** - odaberite potvrdni okvir za **Konfiguriranje VPN-a web-mjesto**.
    - **ExpressRoute** – odaberite potvrdni okvir **Koristi ExpressRoute**. Ova mogućnost prikazuje se samo ako ste odabrali **Konfiguriranje VPN-a web-mjesto**.
    - **Lokalne mreže** - se od vas zatraži da bi sadržavao lokalne mreže web-mjesta za ExpressRoute. Međutim, ako je veza ExpressRoute prefiksi adresu za lokalnu mrežu web-mjesta će se zanemariti. Umjesto toga prefiksi adresu objavljeno Microsoftu putem elektronička ExpressRoute će se koristiti za usmjeravanje svrhe.<BR>Ako već imate lokalne mreže stvorene za ExpressRoute vezu, možete je odabrati na padajućem izborniku. Ako nije, odaberite **Navedite novu lokalnu mrežu**.

5. Ako ste odabrali da biste odredili novi lokalne mreže u prethodnom koraku, pojavljuje se stranica za **Povezivanje s web-mjesto** . Da biste konfigurirali lokalnoj mreži, unesite sljedeće podatke, a zatim kliknite Dalje strelicu. 

    - **Ime** - naziv koji želite uputiti poziv svog lokalnog (lokalno) mreže web-mjesta.
    - **Prostor adrese** – uključujući početni IP i CIDR (adresu zbroj). Bilo koji raspon adresa možete odrediti sve dok se ne preklapa s rasponom adresa za virtualne mreže. Obično je to želite navesti rasponi adresa za vaše lokalne mreže, ali slučaju ExpressRoute, te se postavke ne koriste. No tu postavku potreban je da biste stvorili lokalnu mrežu kada koristite klasični portal.
    - **Dodavanje adresnog prostora** - Ova postavka nije prikladno za ExpressRoute.


6. Na stranici **Virtualne razmake mreže adresa** unesite sljedeće podatke, a zatim kliknite kvačicu dolje desno da biste konfigurirali mreže. 

    - **Prostor adrese** – uključujući Početni broj IP i adresa. Provjerite je li da za to predviđen adresu navedete ne preklapa neku adresu razmake koje imate u lokalnoj mreži.
    - **Dodavanje podmreže** – uključujući vremena i pokretanje IP adresa. Potrebni su dodatni podmreže.
    - **Dodavanje pristupnika podmreže** – kliknite da biste dodali podmreže pristupnika. Pristupnik podmreži koristi se samo za pristupnik za virtualne mreže te je potreban za tu konfiguraciju.<BR>Pristupnik podmreže CIDR (adresu broj) za ExpressRoute mora biti /28 ili veća (/ 27, / 26 itd.). Time za dovoljno IP adresa u tom podmreže dopustili konfiguraciju za rad. Na portalu klasični ako ste odabrali potvrdni okvir da biste koristili ExpressRoute, na portalu određuje podmreži pristupnika /28.  Ne možete prilagoditi count adresu CIDR klasični portalu. Podmreže pristupnika prikazat će se kao **pristupnika** na portalu klasični iako stvarni naziv pristupnika podmreže koja je stvorena je zapravo **GatewaySubnet**. Naziv možete pogledati pomoću programa PowerShell ili na portalu za Azure.

7. Kliknite kvačicu pri dnu stranice, a virtualne mreže će početi da biste stvorili. Kada završi, prikazat će se **stvoreno** navedene u odjeljku **Status** na stranici **mreža** klasični portalu.

## <a name="gw"></a>Stvaranje pristupnika

1. Na stranici **mreža** kliknite virtualne mreže koju ste upravo stvorili, a zatim kliknite **nadzorna ploča** pri vrhu stranice.

2. Na dnu stranice **nadzorne ploče** , kliknite **Stvaranje pristupnika** i odaberite **Dinamičke usmjeravanja**. Kliknite **da** da biste potvrdili da želite da biste stvorili pristupnik.

3. Prilikom pokretanja stvaranje pristupnika prikazat će vam se poruka obavještava znati da je pokrenut pristupnik. Može potrajati do 45 minuta za pristupnik za stvaranje.

4. Veza mreže s instalacijom. Slijedite upute u članku [kako povezati VNets za ExpressRoute krugova](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfiguriranje postojeće klasični VNet za ExpressRoute

Ako već imate klasični VNet, možete je konfigurirati za povezivanje s ExpressRoute klasični portalu. Postavke isti su kao odlomcima, stoga pročitajte tih sekcija da biste se upoznali s potrebne postavke. Ako želite stvoriti vezu coexisting ExpressRoute/web-mjesta-na-web-mjesta, pogledajte [članak](expressroute-howto-coexist-classic.md) korake. Se razlikuju od koraka u ovom članku.
 
1. Morate stvoriti lokalnu mrežu prije ažurirate ostalih postavki VNet. Da biste stvorili novi lokalne mreže koje je potrebno prilikom konfiguriranja ExpressRoute putem portala za klasični, kliknite **Novo** **>** **Mrežnim servisima** **>** **Virtualne mreže** **>** **Dodaj lokalnoj mreži**. Slijedite korake čarobnjaka da biste stvorili lokalnu mrežu.

2. **Konfiguriranje** stranicu koristite da biste ažurirali ostale postavke za vaše VNet i povezivati VNet na lokalnu mrežu.

3. Nakon konfiguriranja postavki, prijeđite na odjeljak [Stvaranje pristupnika](#gw) ovog članka da biste stvorili pristupnik.


## <a name="next-steps"></a>Daljnji koraci

- Ako želite dodati virtualnim strojevima virtualne mreže potražite u članku [virtualnim strojevima Tečajevi](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Ako želite da biste saznali više o ExpressRoute, potražite u članku [Pregled ExpressRoute](expressroute-introduction.md).


 
