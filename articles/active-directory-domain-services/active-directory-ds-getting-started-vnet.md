<properties
    pageTitle="Azure servisa Active Directory Domain Services: Stvaranje ili odabir virtualne mreže | Microsoft Azure"
    description="Uvod u Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Stvaranje ili odabir virtualne mreže za Azure servisa Active Directory Domain Services

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Smjernice za odabir Azure virtualne mreže
> [AZURE.NOTE] **Prije nego što počnete**: odnose se na [mreži zahtjevi za Azure servisa Active Directory Domain Services](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Zadatak 2: Stvaranje Azure virtualne mreže
Sljedeći zadatak konfiguracije je da biste stvorili Azure virtualne mreže i podmreže unutar njega. Omogućivanje Azure servisa Active Directory Domain Services u ovom podmreže unutar virtualne mreže. Ako već imate postojeće virtualne mreže koju želite koristiti, možete preskočiti ovaj korak.

> [AZURE.NOTE] Provjerite je li Azure virtualne mreže stvaranje ili odabir će se koristiti za Azure AD domenske servise pripada Azure regija koji podržavaju Azure servisa Active Directory Domain Services. Posjetite stranicu [servisa Azure po regijama](https://azure.microsoft.com/regions/#services/) znati Azure regije u kojoj je dostupan Azure servisa Active Directory Domain Services.

Imajte na umu dolje naziv virtualne mreže tako da odaberete desno virtualne mreže prilikom omogućivanja Azure AD domenske servise u koraku kasnije konfiguracije.

Izvršite sljedeće korake za konfiguriranje da biste stvorili Azure virtualne mreže u kojoj želite omogućiti Azure servisa Active Directory Domain Services.

1. Dođite do **Azure klasični portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Odaberite čvor **mreža** u lijevom oknu.

    ![Čvor mreža](./media/active-directory-domain-services-getting-started/networks-node.png)

3. U oknu zadatka pri dnu stranice kliknite **NOVO** .

    ![Čvor virtualne mreže](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. U čvor **Mrežnim servisima** odaberite **Virtualne mreže**.

5. Kliknite da biste stvorili virtualne mreže **Brzo stvaranje** .

    ![Stvaranje virtualne mreže – brzi](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Navedite **naziv** virtualne mreže. Možete i odabrati da biste konfigurirali **prostor adrese** ili **VM najveći broj** za ovu mrežu. Za sada možete ostaviti postavku **DNS poslužitelj** postavljen na "Ništa". Možete ažurirati DNS poslužitelj postavljanje nakon Omogući Azure AD domene servisa.

7. Provjerite je li na **mjesto na** padajućem popisu odaberite podržanih regija Azure. Posjetite stranicu [servisa Azure po regijama](https://azure.microsoft.com/regions/#services/) znati Azure regije u kojoj je dostupan Azure servisa Active Directory Domain Services.

8. Da biste stvorili virtualne mreže, kliknite gumb **Stvori virtualne mreže** .

    ![Stvaranje virtualne mreže za Azure servisa Active Directory Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Nakon stvaranja virtualne mreže odaberite virtualne mreže, a zatim kliknite karticu **KONFIGURACIJA** .

    ![Stvaranje podmreži](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Dođite do odjeljka **virtualne mreže adresu razmake** . Kliknite **Dodaj podmreže** i navedite podmreži pod nazivom **AaddsSubnet**. Kliknite **Spremi** da biste stvorili podmreži.

    ![Stvaranje podmreži za Azure servisa Active Directory Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Zadatak 3 – Omogući Azure servisa Active Directory Domain Services
Sljedeći zadatak konfiguracije jest [Omogućivanje Azure servisa Active Directory Domain Services](active-directory-ds-getting-started-enableaadds.md).
