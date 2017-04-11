<properties
    pageTitle="Implementirati predloške pomoću Visual Studio u stogu Azure | Microsoft Azure"
    description="Saznajte kako implementirati predloške pomoću Visual Studio u stogu Azure."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Implementacija predložaka u stogu Azure pomoću Visual Studio

Koristite Visual Studio predložaka Azure upravljanja resursima u stogu PNA Azure.

Voditelj resursa predlošci implementaciju, i dodjela resursa za aplikaciju u operaciji jedinstvenog i usklađenih.

1.  Otvorite Visual Studio 2015 Update 1.

2.  Kliknite **datoteka**, kliknite **Novo**, a u dijaloškom okviru **Novog projekta** kliknite **Grupu resursa Azure**.

3.  Unesite **naziv** za novi projekt, a zatim kliknite **u redu**.

4.  U dijaloškom okviru **Odabir predloška Azure** kliknite **Windows virtualnog računala**, a zatim kliknite **u redu**.

  U novi projekt, vidjet ćete popis predložaka dostupnih proširenjem čvor **Predlošci** u oknu **Eksplorer za rješenja** .

5.  U oknu **Eksplorer za rješenje** desnom tipkom miša kliknite naziv Projektna, zatim **Implementiraj**, a zatim kliknite **Nova implementacija**.

6.  U dijaloškom okviru **uvođenja grupu resursa** u **pretplatu** padajućem popisu, odaberite pretplate na Microsoft Azure stogu.

7.  Na popisu **Grupa resursa** odaberite postojeću grupu resursa ili stvorite novi.

8.  Na popisu **resursa grupi mjesto** odaberite mjesto, a zatim **Implementiraj**.

9.  U dijaloškom okviru **Uređivanje parametara** unesite vrijednosti za parametre (koji se razlikuju se predložak), a zatim **Spremi**.

## <a name="next-steps"></a>Daljnji koraci

[Implementacija predložaka uz naredbeni redak](azure-stack-deploy-template-command-line.md)
