<properties
   pageTitle="Stvaranje poslužitelju komponente Analysis Services u Azure | Microsoft Azure"
   description="Saznajte kako stvoriti instancu komponente Analysis Services poslužitelja u Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Stvaranje poslužitelju komponente Analysis Services
U ovom se članku vodit će vas voditi kroz stvaranje novog resursa server Analysis Services za Azure pretplatu.

## <a name="before-you-begin"></a>Prije početka
Da bismo započeli, potrebno je:

- **Azure pretplatu**: posjetiti [Azure besplatnu probnu verziju](https://azure.microsoft.com/offers/ms-azr-0044p/) da biste stvorili račun.
- **Grupa resursa**: korištenje grupa resursa već imate ili [stvorite novi](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Stvaranje poslužitelju komponente Analysis Services može rezultirati novog servisa za naplatu. Dodatne informacije potražite u članku cijene Analysis Services.

## <a name="create-an-analysis-services-server"></a>Stvaranje poslužitelju komponente Analysis Services

1. Prijavite se na [portal za Azure](https://portal.azure.com).

2. Kliknite **+ Novo** > **Obavještavanje + analize** > **Analysis Services**.

3. U plohu **Analysis Services** ispunite obavezna polja i zatim pritisnite **Stvori**.

    ![Stvaranje poslužitelja](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Naziv poslužitelja**: upišite jedinstveni naziv koji se koristi za pozivanje na poslužitelj.

    - **Pretplate**: Odaberite ovaj poslužitelj računi za pretplatu.

    - **Grupa resursa**: to su spremnika omogućuju upravljanje skupovima Azure resursa. Dodatne informacije potražite u članku [grupe resursa](../resource-group-overview.md).

    - **Lokacije**: lokaciju podatkovnog centra ovaj Azure hostira poslužitelj. Odaberite mjesto na najbliži vaše najveću osnovni korisnika.

    - **Određivanje cijena sloju**: odaberite sloj cijene. Tabličnih modela do 100 GB podržani. Vaš cijene sloju uvijek možete kasnije promijeniti.

4. Kliknite **Stvori**.

Stvaranje obično traje u odjeljku minuta; često samo nekoliko sekundi. Ako odaberete **Dodaj portal**, idite na portal da biste vidjeli novi poslužitelj. Ili dođite do **druge usluge** > **Analysis Services** da biste vidjeli je li vaš poslužitelj spremna. Ako se ne pojavi osvježavanje popisa.

 ![Nadzorna ploča](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Daljnji koraci
Nakon što stvorite na poslužitelju, možete [uvesti model](analysis-services-deploy.md) da biste ga pomoću SSDT ili s SSMS.

Ako se model implementacija poslužitelja povezuje se s lokalnim izvorima podataka, morate instalirati [pristupnik lokalnim podacima](analysis-services-gateway.md) na računala u vašoj mreži.
