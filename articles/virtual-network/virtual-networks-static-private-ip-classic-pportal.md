<properties 
   pageTitle="Upute za postavljanje statičke IP privatni klasičnog načina pomoću portala za Azure | Microsoft Azure"
   description="Razumijevanje statične privatne IP-ovi i upravljanje njima u klasičan način rada pomoću portala za Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Kako postaviti statičke privatne IP adrese (klasični) na portalu za Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Upravljanje statičke privatne IP adrese u modelu implementacije Voditelj resursa](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ogledna korake u nastavku očekivati okruženju jednostavne već stvorili. Ako želite da biste pokrenuli korake kako se prikazuju u ovom dokumentu, najprije sastavljanje opisane u [Stvaranje na vnet](virtual-networks-create-vnet-classic-pportal.md)okruženje za testiranje.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kako odrediti statičke privatne IP adrese prilikom stvaranja na VM
Da biste stvorili VM pod nazivom *DNS01* u *sučelju* podmreži VNet pod nazivom *TestVNet* pomoću statičke IP privatne od *192.168.1.101*, slijedite ove korake:

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **NOVO** > **izračunati** > **Windows Server 2012 R2 podatkovnog centra**, obavijest već prikazuje **Klasični**na popisu **Odaberite model implementacije** , a zatim kliknite **Stvori**.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. U plohu **Stvaranje VM** unesite naziv VM će biti stvoren (*DNS01* u našem scenariju), lokalni administratorski račun i lozinku.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Kliknite **Neobavezno konfiguracija** > **mrežni** > **Virtualne mreže**, a zatim kliknite **TestVNet**. Ako **TestVNet** nije dostupna, obavezno koristite mjesto za *Središnje SAD -a* i ste stvorili na početku u ovom se članku opisuje okruženje za testiranje.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. U plohu **mreže** provjerite je li odabran podmreži *sučelju*, a zatim kliknite **IP adrese**, u odjeljku **Dodjeljivanje IP adresa** kliknite **statične**, a zatim unesite *192.168.1.101* za **IP adresu** , kao što se vidi ispod.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Kliknite **u redu** u plohu **IP adrese** , a zatim kliknite **u redu** u plohu **mreže** pa kliknite **u redu** u plohu **neobavezno konfiguracije** .
7. U plohu **Stvaranje VM** kliknite **Stvori**. Obratite pozornost na pločicu nastavku prikazuju u nadzorne ploče.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Upute za dohvaćanje statične privatne IP adresa informacija o na VM

Da biste pogledali statički privatne podataka IP adrese za VM stvorene pomoću gore navedene korake, izvršiti korake u nastavku.

1. Na portalu Azure Azure kliknite **PREGLEDAJ sve** > **virtualnim strojevima (klasični)** > **DNS01** > **sve postavke** > **IP adrese** i obavijest dodjeljivanje IP adresa i IP adrese kao što se vidi ispod.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kako ukloniti statičke privatne IP adrese iz programa VM
Da biste uklonili VM stvorena iznad privatne statičke IP adrese, slijedite korake u nastavku.
    
1. Iz **IP adrese** plohu gore navedenoj sintaksi, kliknite **dinamički** s desne strane **Dodjeljivanje IP adresa**, a zatim kliknite **Spremi**, a zatim **da**.

    ![Stvaranje VM Azure portalu](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kako dodati statične privatne IP adrese postojeće VM
Da biste dodali statičke privatne IP adrese VM stvoren pomoću gore navedene korake, slijedite ove korake:

1. **IP adrese** plohu gore navedenoj sintaksi, kliknite **statične** s desne strane **Dodjeljivanje IP adresa**.
2. Upišite *192.168.1.101* za **IP adresu**, a zatim kliknite **Spremi**, a zatim **da**.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rezervirane javnu IP](virtual-networks-reserved-public-ip.md) adrese.
- Informacije o adresama u [instancu razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Pogledajte [Rezervirana IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx).