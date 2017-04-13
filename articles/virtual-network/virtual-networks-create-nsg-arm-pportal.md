<properties 
   pageTitle="Kako stvoriti NSGs u načinu ARM pomoću portala za Azure | Microsoft Azure"
   description="Saznajte kako stvoriti i implementirati NSGs u OBLAK pomoću portala za Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Kako upravljati NSGs pomoću portala za Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [stvoriti NSGs u modelu klasični implementacije](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Na temelju uzorka PowerShell naredbe ispod očekivati okruženju jednostavne već stvorili scenarij iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje okruženje za testiranje uvođenjem [Ovaj predložak](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), zatim **Implementiraj za Azure**, zamjena zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu. Koraci u nastavku kao naziv grupe resursa predložak je uveden u koristite **Ru NSG** .

## <a name="create-the-nsg-frontend-nsg"></a>Stvaranje NSG NSG sučelju

Da biste stvorili **NSG sučelju** NSG kao što je prikazano u scenariju iznad, slijedite korake u nastavku.

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **Pregled >** > **mreže sigurnosne grupe**.

    ![Azure portala - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. U plohu **mreže sigurnosne grupe** , kliknite **Dodaj**.
  
    ![Azure portala - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. U plohu **Stvaranje mreže sigurnosne grupe** stvaranje programa NSG pod nazivom *NSG sučelju* u grupi *Ru NSG* resursa, a zatim **Stvori**.

    ![Azure portala - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Stvaranje pravila u postojeću NSG

Stvaranje pravila u postojeću NSG s portala za Azure, slijedite korake u nastavku.

2. Kliknite **Pregled >** > **mreže sigurnosne grupe**.

3. Na popisu NSGs kliknite **NSG sučelju** > **ulaznog sigurnosna pravila**

    ![Azure portala - NSG sučelju](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. Na popisu **dolazni sigurnosna pravila**kliknite **Dodaj**.

    ![Portal za Azure – Dodavanje pravila](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. U plohu **Dodaj ulazni sigurnost pravilo** stvorite pravilo pod nazivom *web-pravila* s prioritetom *200* dopuštanja pristupa putem *TCP-a* za priključak *80* do bilo koje VM iz bilo kojeg izvora, a zatim **u redu**. Obratite pozornost na to da su većinu tih postavki zadane vrijednosti već.

    ![Azure portala - postavki pravila](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Nakon nekoliko sekundi vidjet ćete novo pravilo u na NSG.

    ![Azure portala - novo pravilo](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Ponovite korake da biste 6 da biste stvorili pravilo ulazne pod nazivom *rdp pravila* s prioritet od *250* dopuštanja pristupa putem *TCP-a* za priključak *3389* na bilo kojem VM iz bilo kojeg izvora.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Pridruživanje NSG u sučelju podmreži

1. Kliknite **Pregled >** > **grupa resursa** > **Ru NSG**.
2. U plohu **Ru NSG** kliknite **...**  >  **TestVNet**.

    ![Azure portala - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. U plohu **Postavke** kliknite **podmreže** > **sučelju** > **mreže sigurnosne grupe** > **NSG sučelju**.

    ![Azure portala - podmreže postavke](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. U plohu **sučelju** kliknite **Spremi**.

    ![Azure portala - podmreže postavke](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Stvaranje NSG NSG pozadinskog

Da biste stvorili **NSG pozadinskog** NSG i povezati je podmreže **pozadinskog** , slijedite korake u nastavku.

1. Ponovite korake iz članka [Stvaranje NSG NSG sučelju](#Create-the-NSG-FrontEnd-NSG) da biste stvorili programa NSG pod nazivom *NSG pozadinskog*
2. Ponovite korake za [Stvaranje pravila u postojeću NSG](#Create-rules-in-an-existing-NSG) da biste stvorili **Ulazna** pravila u tablici u nastavku.

  	|Ulazna pravila|Izlazni pravila|
  	|---|---|
  	|![Azure portala - ulazna pravila](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure portala - izlaznog pravila](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Ponovite korake za pridruživanje **NSG pozadinskog** NSG podmreže **pozadinskog** [pridružiti NSG u sučelju podmreži](#Associate-the-NSG-to-the-FrontEnd-subnet) .

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [upravljati postojeće NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Omogućite zapisivanje](virtual-network-nsg-manage-log.md) NSGs.