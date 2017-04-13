<properties 
   pageTitle="Implementacija VM pomoću statičke IP javno pomoću portala za Azure u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako implementirati VMs pomoću statičke IP javno pomoću portala za zure u upravitelju resursa"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Implementacija VM pomoću statičke IP javno pomoću portala za Azure

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Stvaranje na VM pomoću statičke IP javno 

Da biste stvorili na VM statičke javnu IP adrese na portalu za Azure, slijedite korake u nastavku.

1. U pregledniku idite na [portal za Azure](https://portal.azure.com) i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Na u gornjem lijevom kutu portala, kliknite **Novo**>>**izračunati**>**Windows Server 2012 R2 podatkovnog centra**.
3. Na popisu **Odaberite model implementacije** odaberite **Upravitelj resursa** , a zatim kliknite **Stvori**.
4. U plohu **Osnove** unesite VM podatke kao što je prikazano u nastavku, a zatim kliknite **u redu**.

    ![Azure portala - osnove](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. U plohu **Odaberite veličinu** kliknite **A1 standardne** kao što je prikazano u nastavku, a zatim **Odaberite**.

    ![Portal za Azure – odabir veličine](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. U plohu **Postavke** kliknite **javnu IP adresa**, a zatim u plohu **Stvori javni IP adresa** u odjeljku **Dodjela** **statične** kao što je prikazano u nastavku. A zatim kliknite **u redu**.

    ![Portal za Azure – Stvaranje javne IP adrese](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. U plohu **Postavke** , kliknite **u redu**.
8. Pregledajte plohu **Sažetak** kao što je prikazano u nastavku, a zatim kliknite **u redu**.

    ![Portal za Azure – Stvaranje javne IP adrese](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Obratite pozornost na novu pločicu u nadzorne ploče.

    ![Portal za Azure – Stvaranje javne IP adrese](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Nakon stvaranja na VM plohu **Postavke** prikazat će se kao što je prikazano u nastavku

    ![Portal za Azure – Stvaranje javne IP adrese](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)