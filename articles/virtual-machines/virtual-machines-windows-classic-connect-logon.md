<properties
    pageTitle="Prijavi se na klasični VM Azure | Microsoft Azure"
    description="Koristite Azure klasični portal za prijavu na Windows virtualni stroj stvorene pomoću modela klasični uvođenja."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Prijavite se na Windows virtualni stroj pomoću Azure klasični portal

Azure klasični portalu za pokretanje sesije udaljene radne površine i prijavite se na Windows VM koristite gumb **za povezivanje** .

Želite li povezati Linux VM? Pogledajte [kako prijaviti na virtualni stroj sa sustavom Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Saznajte kako [izvesti ove korake koristeći novi Azure portal](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Video vodič

Ovdje je video prođite kroz korake u ovaj vodič. Također pokriva krajnje točke i javne i privatne koristi za povezivanje s Windows VM u Azure priključke.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Povezivanje s virtualnog računala

1. Prijavite se u Azure klasični portal.

2. Kliknite **virtualnih računala**, a zatim odaberite virtualni stroj.

3. Na traci naredbi na dnu stranice kliknite **Poveži**.

    ![Prijavite se na virtualni stroj](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Ako nije dostupan gumb **Poveži** pogledajte savjete za rješavanje problema na kraju ovog članka.

## <a name="log-on-to-the-virtual-machine"></a>Prijavite se na virtualni stroj

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Sljedeći koraci

-   Ako je gumb **Poveži** Neaktivni ili imate drugih problema s udaljenom površinom, pokušajte ponovno postavljanje konfiguracije. Iz nadzorne ploče virtualni stroj pod **Brzi Glance**, kliknite **Vrati izvorno udaljene konfiguracije**.
-   Za probleme s lozinku, pokušajte ga vraćanje. Iz nadzorne ploče virtualni stroj pod **Brzi Glance**, pritisnite **resetiranje lozinke**.

Ako one savjeti ne rade ili nisu što trebate, pogledajte [Otklanjanje poteškoća s udaljenom radnom površinom na temelju Windows Azure virtualni stroj](virtual-machines-windows-troubleshoot-rdp-connection.md). Ovaj članak vodi putem dijagnosticiranje i rješavanje uobičajenih problema.


