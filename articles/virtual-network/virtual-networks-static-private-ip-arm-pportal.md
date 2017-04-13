<properties 
   pageTitle="Kako postaviti statičke IP privatne u načinu ARM pomoću portala za Azure | Microsoft Azure"
   description="Razumijevanje IP-privatne ovi (DIPs) i upravljanje njima u načinu ARM pomoću portala za Azure"
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

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Kako postaviti statičke privatne IP adrese na portalu za Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [Upravljanje statičke privatne IP adrese u modelu klasični implementacije](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ogledna korake u nastavku očekivati okruženju jednostavne već stvorili. Ako želite da biste pokrenuli korake kako se prikazuju u ovom dokumentu, najprije sastavljanje opisane u [Stvaranje na vnet](virtual-networks-create-vnet-arm-pportal.md)okruženje za testiranje.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Kako stvoriti na VM za testiranje statičke privatne IP adrese

Ne možete postaviti statičke privatne IP adrese tijekom stvaranja VM u načinu rada za implementaciju upravljanja resursima pomoću portala za Azure. Najprije morate stvoriti na VM, tehn postavite njegov privatne IP biti statične.

Da biste stvorili VM pod nazivom *DNS01* u *sučelju* podmreži VNet pod nazivom *TestVNet*, slijedite korake u nastavku.

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **NOVO** > **izračunati** > **Windows Server 2012 R2 podatkovnog centra**, obavijest već prikazuje **Voditelj resursa**na popisu **Odaberite model implementacije** , a zatim kliknite **Stvori**, kao što se vidi na slici u nastavku.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. U plohu **Osnove** unesite naziv VM će biti stvoren (*DNS01* u našem scenariju), lokalni administratorski račun i lozinku, kao što se vidi na slici u nastavku.

    ![Osnove plohu](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Provjerite nalazi li se **mjesto** odabrali *Središnje SAD -a*, a zatim kliknite **Odaberite postojeći** u odjeljku **grupa resursa**, a zatim ponovno kliknite **grupa resursa** , a zatim kliknite *TestRG*pa pa kliknite **u redu**.

    ![Osnove plohu](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. U plohu **Odaberite veličinu** , odaberite **Standardnu A1**, a zatim **Odaberite**.

    ![Odaberite veličinu plohu](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. U plohu **Postavke** provjerite je li se postavljaju sljedeća svojstva postavljaju s vrijednostima u nastavku, a zatim kliknite **u redu**.

    -**Račun za pohranu**: *vnetstorage*
    - **Mrežni**: *TestVNet*
    - **Podmreže**: *sučelju*

    ![Odaberite veličinu plohu](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. U plohu **Sažetak** kliknite **u redu**. Obratite pozornost na pločicu nastavku prikazuju u nadzorne ploče.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Upute za dohvaćanje statične privatne IP adresa informacija o na VM

Da biste pogledali statički privatne podataka IP adrese za VM stvorene pomoću gore navedene korake, izvršiti korake u nastavku.

1. Na portalu Azure Azure kliknite **PREGLEDAJ sve** > **virtualnim strojevima** > **DNS01** > **sve postavke** > **sučelje mreže** , a zatim kliknite samo mrežnog sučelja na popisu.

    ![Implementacija VM pločica](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. U plohu **mrežno sučelje** kliknite **sve postavke** > **IP adrese** i obratite pozornost na to vrijednosti za **dodjelu** i **IP adresa** .

    ![Implementacija VM pločica](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kako dodati statične privatne IP adrese postojeće VM
Da biste dodali statičke privatne IP adrese VM stvoren pomoću gore navedene korake, slijedite ove korake:

1. **IP adrese** plohu gore navedenoj sintaksi, kliknite **statične** u odjeljku **Dodjela**.
2. Upišite *192.168.1.101* **IP**adresa, a zatim kliknite **Spremi**.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Ako nakon klika na **Spremi** primijetite da dodjelu i dalje postavljen tako da **dinamički**, to znači da IP adresu koju ste upisali već se koristi. Isprobajte različite IP adresa.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kako ukloniti statičke privatne IP adrese iz programa VM
Da biste uklonili VM stvorena iznad privatne statičke IP adrese, slijedite koraka u nastavku.
    
1. Iz **IP adrese** plohu gore navedenoj sintaksi, kliknite **dinamički** u odjeljku **Dodjela**, a zatim kliknite **Spremi**.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rezervirane javnu IP](virtual-networks-reserved-public-ip.md) adrese.
- Informacije o adresama u [instancu razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Pogledajte [Rezervirana IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx).