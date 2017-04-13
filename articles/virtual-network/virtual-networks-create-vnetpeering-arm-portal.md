<properties
   pageTitle="Stvaranje VNet Peering pomoću portala za Azure | Microsoft Azure"
   description="Saznajte kako stvoriti virtualne mreže pomoću portala za Azure u upravitelju resursa."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Stvaranje na virtualne mreže peering pomoću portala za Azure

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Da biste stvorili na VNet peering ovisno o scenariju iznad pomoću portala za Azure, slijedite korake u nastavku.

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Da biste uspostavili VNET peering, morate stvoriti dvije veze, jedan za svaki smjer između dvije VNets. Vi možete stvoriti VNET peering vezu za VNET1 za VNET2. Na portalu, kliknite **Pregledaj** > **Odaberite virtualne mreže**

    ![Stvaranje VNet peering portalu za Azure](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. U plohu virtualne mreže, odaberite VNET1, kliknite Peerings, a zatim kliknite Dodaj

    ![Odaberite peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. U plohu dodavanje Peering imenujte peering vezu LinkToVnet2, odaberite pretplata i virtualne VNET2 mreže ravnopravnih članova, kliknite u redu.

    ![Veza na VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Nakon stvaranja VNET peering vezu. Stanje veza možete vidjeti sljedeće:

    ![Stanje veze](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Sljedeće stvorite vezu peering VNET VNET2 za VNET1. U plohu virtualne mreže, odaberite VNET2, kliknite Peerings, a zatim kliknite Dodaj

    ![Ravnopravnih članova iz drugih VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Dodavanje Peering plohu imenujte peering vezu LinkToVnet1 pa odaberite pretplate i ravnopravnim virtualne mreže, kliknite u redu.

    ![Stvaranje virtualne mreže pločica](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Nakon stvaranja VNET peering vezu. Stanje veza možete vidjeti sljedeće:

    ![Stanje konačni veza](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Provjera stanja LinkToVnet2, a sada mijenja se u povezan kao i.  

    ![Stanje konačno veza 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET peering samo uspostaviti ako su povezani i veze.

Postoji nekoliko konfigurirati svojstva za svaku vezu:

|Mogućnost|Opis|Zadani|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Je li adresa prostora ravnopravnih članova VNet biti dio oznake Virtual_network|Da|
|AllowForwardedTraffic|Hoće li promet ne potječu peered VNet prihvatio ili ispušteno|ne|
|AllowGatewayTransit|Omogućuje VNet da biste koristili pristupnikom VNet ravnopravnih članova|ne|
|UseRemoteGateways|Koristi vaš ravnopravnog VNet pristupnik. Ravnopravni VNet mora imati konfiguriran pristupnik i AllowGatewayTransit odabran. Ne možete koristiti tu mogućnost ako imate konfiguriran pristupnik|ne|

Svaka veza VNet peering sadrži skup iznad svojstva. Na portalu možete kliknite vezu Peering VNet i promijenite željene mogućnosti dostupne, kliknite Spremi da biste Promijeni efekt.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. U ovom primjeru koristit ćemo dvije pretplate A i B i dva korisnika UserA i UserB s ovlastima u pretplate odnosno
3. Na portalu, kliknite Pregledaj, odaberite virtualne mreže. Kliknite na VNET, a zatim kliknite Dodaj.

    ![Scenarij 2 Pregledaj](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Na access plohu Dodaj kliknite odaberite ulogu i odaberite mreža suradnika, kliknite Dodavanje korisnika, upišite znak UserB naziv i kliknite u redu.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Ovo nije potrebna, peering možete uspostaviti čak i ako korisnici pojedinačno Potenciranje peering zahtjeve za svoje odgovarajući Vnets pod uvjetom da zadovoljavaju zahtjeve. Dodavanje povlaštene korisnike na VNet kao korisnike u lokalnom VNet olakšava postavljanje na portalu.

5. Zatim Prijava na portal Azure s UserB koji je korisnik ovlasti za SubscriptionB. Slijedite gore korake da biste dodali UserA kao mreža suradnika.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Možete odjavite i prijavite se na oba korisničke sesije u pregledniku da biste bili sigurni autorizacija uspješno je omogućeno.

6. Prijava na portal kao UserA dođite do plohu VNET3, kliknite Peering, provjerite ' znam moj ID resursa "potvrdni okvir i upišite resursa ID za VNET5 u ispod oblik.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![ID resursa](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Prijavite se na portal kao UserB, a zatim slijedite prethodno korak da biste stvorili vezu peering VNET5 VNet3.

    ![ID resursa 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Peering će se uspostaviti i sve virtualnog računala u VNet3 trebali biste moći komunicirati s bilo kojeg virtualnog računala u VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Kao prvi korak, VNET peering veze iz HubVnet na VNET1. Imajte na umu da dopušta promet prosljeđuju mogućnost nije odabrana za vezu.

    ![Osnovni Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Kao sljedeći korak, mogu se kreirati peering veze iz VNET1 na HubVnet. Imajte na umu da je odabrana mogućnost Dopusti prosljeđuju promet.

    ![Osnovni Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Kada je pokrenut peering, možete potražite u [članku](virtual-network-create-udr-arm-ps.md) i definirati korisnički definirane Route(UDR) za preusmjeravanje prometa VNet1 putem virtualne uređaj da biste koristili njegove mogućnosti. Kada u usmjeravanje navedete adresu sljedeći parametra, možete je postaviti IP adresi virtualne potražite u ravnopravnih članova VNet HubVNet


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.

2. Da biste uspostavili VNET peering u ovom scenariju, morate stvoriti samo jedan vezu iz virtualne mreže u upravitelju Azure resursa onom u klasičnom. To je iz **VNET1** **VNET2**. Na portalu, kliknite **Pregled** > odaberite **Virtualne mreže**

3. U mrežama plohu virtualne odaberite **VNET1**. Kliknite **Peerings**, a zatim kliknite **Dodaj**.

4. U plohu dodavanje Peering naziv veze. Ovo se zove **LinkToVNet2**. U odjeljku Detalji ravnopravnih članova odaberite **Klasični**.

5. Zatim odaberite pretplata i ravnopravnih članova virtualne mreže **VNET2**. Zatim kliknite u redu.

    ![Povezivanje Vnet1 Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Nakon stvaranja tu vezu peering VNet su peered dvije virtualne mreže i će je moći vidjeti sljedeće:

    ![Provjeravanje peering veze](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Uklanjanje VNet Peering

1.  U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2.  Idite na plohu virtualne mreže kliknite Peerings, kliknite vezu koju želite ukloniti, a zatim kliknite gumb Izbriši.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Kada uklonite jedne veze u VNET peering, stanja veze ravnopravnih članova idu u prekinuta veza.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. U ovom stanju, ne možete ponovno stvoriti vezu dok se ne promijeni stanje veza ravnopravnih članova pokrenuo. Preporučujemo da uklonite oba veze da biste mogli ponovno stvoriti VNET peering.
