<properties 
   pageTitle="Upravljanje NSGs pomoću portala za pretpregled u upravitelju resursa | Microsoft Azure"
   description="Informirajte se o upravljanju exising NSGs pomoću portala za pretpregled u upravitelju resursa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Upravljanje NSGs pomoću portala za pretpregled

> [AZURE.SELECTOR]
- [Portal](virtual-network-manage-nsg-arm-portal.md)
- [PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure EŽA](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Dohvaćanje informacija

Možete vidjeti vaše postojeće NSGs, dohvaćanje pravila za postojeće NSG i saznati resursima sustava NSG je povezan.

### <a name="view-existing-nsgs"></a>Prikaz postojeće NSGs
Da biste pogledali sve postojeće NSGs pretplatu, slijedite korake u nastavku.

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **Pregled >** > **mreže sigurnosne grupe**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Provjerite popis NSGs u plohu **mreže sigurnosne grupe** .

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Da biste pogledali popis NSGs grupi **Ru NSG** resursa, slijedite korake u nastavku. 

1. Kliknite **grupa resursa >** > **Ru NSG** > **...**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Na popisu resursima potražite stavke prikaz ikonu NSG, kao što je prikazano u nastavku plohu **Resursi** .

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Popis svih pravila za programa NSG

Da biste pogledali pravila programa NSG pod nazivom **NSG sučelju**, slijedite korake u nastavku. 

1. Plohu **mreže sigurnosne grupe** ili plohu **Resursi** gore navedenoj sintaksi, kliknite **NSG sučelju**.
2. Na kartici **Postavke** kliknite **ulaznog sigurnosna pravila**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. **Ulazna pravila sigurnosti** plohu prikazuje se kao što je prikazano u nastavku.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Na kartici **Postavke** kliknite **pravila za izlazni sigurnost** da biste vidjeli izlaznog pravila.

>[AZURE.NOTE] Da biste pogledali zadana pravila, kliknite ikonu **Zadana pravila** pri vrhu plohu koji prikazuje pravila.

### <a name="view-nsgs-associations"></a>Prikaz NSGs pridruživanja

Da biste vidjeli što resursi NSG **NSG FrontEnd** se pridružiti, slijedite korake u nastavku.

1. Plohu **mreže sigurnosne grupe** ili plohu **Resursi** gore navedenoj sintaksi, kliknite **NSG sučelju**.
2. Na kartici **Postavke** kliknite **podmreže** da biste pogledali koje podmreže su povezani s NSG.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Na kartici **Postavke** kliknite **sučelje mreže** da biste pogledali koji su NIC-ovi povezan s NSG.

## <a name="manage-rules"></a>Upravljanje pravilima

Možete dodati pravila za postojeće NSG uredite postojeće pravila, a uklonite pravila.

### <a name="add-a-rule"></a>Dodavanje pravila

Da biste dodali pravilo dopuštanja **dolazni** promet za priključak **443** iz bilo kojeg uređaja za **NSG FrontEnd** NSG, slijedite korake u nastavku.

1. Plohu **mreže sigurnosne grupe** ili plohu **Resursi** gore navedenoj sintaksi, kliknite **NSG FrontEnd**.
2. Na kartici **Postavke** kliknite **ulaznog sigurnosna pravila**.
3. U plohu **Ulazna pravila sigurnost** kliknite **Dodaj**. Nakon toga u plohu **Dodaj pravilo ulazne sigurnost** ispune vrijednosti, kao što je prikazano u nastavku i pa kliknite **u redu**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Nakon nekoliko sekundi, imajte na umu novo pravilo u plohu **ulaznog sigurnosna pravila** .

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Promjena pravila

Da biste promijenili pravilo stvorio iznad da dopušta unutarnje promet s **Interneta** samo, slijedite korake u nastavku.

1. Plohu **mreže sigurnosne grupe** ili plohu **Resursi** gore navedenoj sintaksi, kliknite **NSG sučelju**.
2. Na kartici **Postavke** kliknite pravilo koje stvorio iznad.
3. U plohu **Dopusti https** promijeniti svojstvo **izvor** kao što je prikazano u nastavku, a zatim **Spremi**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Brisanje pravila

Da biste izbrisali pravilo stvorio iznad, slijedite korake u nastavku.

1. Plohu **mreže sigurnosne grupe** ili plohu **Resursi** gore navedenoj sintaksi, kliknite **NSG sučelju**.
2. Na kartici **Postavke** kliknite pravilo koje stvorio iznad.
3. U plohu **Dopusti https** kliknite **Izbriši**, a zatim kliknite **da**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Upravljanje pridruživanja

Možete povezati programa NSG da biste podmreže i NIC-ovi. Možete dissociate i programa NSG iz bilo kojeg resursa koji je pridružen.

### <a name="associate-an-nsg-to-a-nic"></a>Povezivanje programa NSG da biste na NIC

Da biste povezali **NSG sučelju** NSG za **TestNICWeb1** NIC, slijedite korake u nastavku.

1. Plohu **mreže sigurnosne grupe** ili plohu **Resursi** gore navedenoj sintaksi, kliknite **NSG sučelju**.
2. Na kartici **Postavke** kliknite **sučelje mreže** > **pridružiti** > **TestNICWeb1**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Dissociate programa NSG iz programa NIC

Da biste dissociate **NSG sučelju** NSG iz **TestNICWeb1** NIC, slijedite korake u nastavku.

1. Na portalu Azure kliknite **grupa resursa >** > **Ru NSG** > **...**  >  **TestNICWeb1**.
2. U plohu **TestNICWeb1** kliknite **Promijeni sigurnost...**  > **None**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] U ovom plohu možete koristiti i povezivati SLIKA za sve postojeće NSG.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Dissociate programa NSG iz podmreži

Da biste dissociate **NSG sučelju** NSG iz podmreže **sučelju** , slijedite korake u nastavku.

1. Na portalu Azure kliknite **grupa resursa >** > **Ru NSG** > **...**  >  **TestVNet**.
2. U plohu **Postavke** kliknite **podmreže** > **sučelju** > **mrežom sigurnosne grupe** > **ništa**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. U plohu **sučelju** kliknite **Spremi**.

![Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Povezivanje programa NSG da biste podmreži

Da biste ponovno povezali **NSG sučelju** NSG podmreže **FronEnd** , slijedite korake u nastavku.

1. Na portalu Azure kliknite **grupa resursa >** > **Ru NSG** > **...**  >  **TestVNet**.
2. U plohu **Postavke** kliknite **podmreže** > **sučelju** > **mreže sigurnosne grupe** > **NSG sučelju**.
3. U plohu **sučelju** kliknite **Spremi**.

>[AZURE.NOTE] Na NSG podmreži možete povezati i iz plohu thh NSG **Postavke** .

## <a name="delete-an-nsg"></a>Brisanje programa NSG

Na NSG možete izbrisati samo ako je nije povezana za neki resurs. Da biste izbrisali programa NSG, slijedite korake u nastavku.

1. Na portalu Azure kliknite **grupa resursa >** > **Ru NSG** > **...**  >  **NSG sučelju**.
2. U plohu **Postavke** kliknite **sučelje mreže**.
3. Postoje li sve mrežne kartice na popisu, kliknite NIC-a pa slijedite korak 2 u [Dissociate programa NSG iz programa NIC](#Dissociate-an-NSG-from-a-NIC).
4. Ponovite korak 3 za svaku NIC.
5. U plohu **Postavke** kliknite **podmreže**.
6. Ako je bilo koji podmreže na popisu, kliknite podmreži i slijedite korake 2 i 3 u [Dissociate programa NSG iz podmreži](#Dissociate-an-NSG-from-a-subnet).
7. Pomicanje ulijevo da biste plohu **NSG sučelju** pa kliknite **Izbriši** > **da**.

[Azure portala - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Daljnji koraci

- [Omogućite zapisivanje](virtual-network-nsg-manage-log.md) NSGs.
