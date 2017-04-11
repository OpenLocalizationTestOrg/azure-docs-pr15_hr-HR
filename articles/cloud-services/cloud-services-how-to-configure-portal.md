<properties 
    pageTitle="Kako konfigurirati servis u oblaku (portal) | Microsoft Azure" 
    description="Saznajte kako konfigurirati servise u oblaku u Azure. Saznajte kako ažurirati konfiguracija servisa oblaka i konfiguriranje daljinski pristup uloga instance. U ovim se primjerima pomoću portala za Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Konfiguriranje servisa u Oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-configure-portal.md)
- [Azure klasični portal](cloud-services-how-to-configure.md)

Na portalu za Azure možete konfigurirati najčešće korištenih postavki za servise u oblaku. Ili, ako želite izravno ažuriranje datoteka konfiguracije preuzmite konfiguracijska datoteka servisa da biste ažurirali, i prenijeli ažuriranu datoteku i ažurirajte servisa u oblaku promjene konfiguracije. U svakom slučaju, ažuriranja konfiguracija pomiču se na sve instance uloge.

Možete upravljati i instance uloge servisa oblaka ili udaljene radne površine u njih.

Azure možete samo osiguraj postotak 99.95 servisa tijekom ažuriranja konfiguracija ako imate barem dvije instance uloge za svaku ulogu. Koji omogućuje da obradi zahtjeve klijenta dok drugi ažurira jednu virtualnog računala. Dodatne informacije potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Promijenite neki servis u oblaku

Nakon otvaranja [Azure portal](https://portal.azure.com/), idite na servis u oblaku. Na tom mjestu upravljanje mnogo aspekti. 

![Postavke stranice](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Postavke** ili **sve postavke** veze, otvorit će plohu **Postavke** gdje možete promijeniti **Svojstva**, promjenu **konfiguracije**, upravljanje **certifikata**, postavljanje **upozorenja pravila**i upravljanje **korisnicima** koji imaju pristup ovaj servis u oblaku.

![Plohu za postavke servisa Azure oblaka](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Operacijski sustav koji se koristi za servis u oblaku nije moguće promijeniti pomoću **portala za Azure**, samo mogu promijeniti ovu postavku putem [Azure klasični portal](http://manage.windowsazure.com/). To je detaljan [ovdje](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Nadzor

Upozorenja možete dodati na servis u oblaku. Kliknite **Postavke** > **Upozorenje pravila** > **Dodaj upozorenje**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Na tom mjestu možete postaviti upozorenja. S **Mertic** padajućem okviru možete postaviti upozorenja za sljedeće vrste podataka.

- Disk Čitaj
- Pisanje na disku
- Mrežni u
- Izvan mreže
- Postotak CPU-a 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfiguriranje nadzora iz metričkim pločica

Umjesto korištenja **Postavke** > **Upozorenja pravila**, možete kliknuti na jedan od metričkim pločice u odjeljku **nadzor** plohu **servis u Oblaku** .

![Nadzor servis u oblaku](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Na tom mjestu možete prilagoditi grafikon koji se koristi s pločicu ili dodajte pravilo upozorenja.


## <a name="reboot-reimage-or-remote-desktop"></a>Ponovno pokrenite računalo, reimage ili udaljene radne površine

Trenutno ne možete konfigurirati pomoću **portala za Azure**udaljene radne površine. Međutim, možete postaviti ga putem sustava [Azure klasični portal](cloud-services-role-enable-remote-desktop.md) [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)ili [Visual Studio](../vs-azure-tools-remote-desktop-roles.md). 

Najprije kliknite instanca servisa oblaka.

![Instanca servisa u oblaku](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Iz na plohu da će se otvoriti uou možete započeti udaljene radne površine, daljinski ponovno instanci ili daljinski reimage (pokrenite slikom Osvježi) instancu.

![Gumbi za instancu servisa oblaka](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Konfiguracija sustava .cscfg

Možda ćete morati ponovno ste konfigurirali servis u oblaku putem [servisa config (cscfg)](cloud-services-model-and-package.md#cscfg) datoteke. Najprije morate preuzeti datoteku .cscfg, izmijenite ga, a zatim prenijeti.

1. Kliknite ikonu **Postavke** ili **sve postavke** veze da biste otvorili gore plohu **Postavke** .

    ![Postavke stranice](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Kliknite na stavci **konfiguracije** .

    ![Konfiguriranje plohu](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Kliknite gumb za **Preuzimanje** .

    ![Preuzimanje](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Nakon što ažurirate konfiguracijska datoteka servisa, prijenos i ažuriranje konfiguracije:

    ![Prijenos](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Odaberite datoteku .cscfg, a zatim kliknite **u redu**.

            
## <a name="next-steps"></a>Daljnji koraci

* Saznajte kako [implementirati servis u oblaku](cloud-services-how-to-create-deploy-portal.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name-portal.md).
* [Upravljanje servis u oblaku](cloud-services-how-to-manage-portal.md).
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate-portal.md).