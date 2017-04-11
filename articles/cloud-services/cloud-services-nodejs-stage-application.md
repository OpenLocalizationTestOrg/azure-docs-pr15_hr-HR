<properties 
    pageTitle="Faze oblaka servisa implementacije (Node.js) | Microsoft Azure" 
    description="Saznajte kako implementirati Azure aplikacije u okruženju pripremna, a zatim implementacija radnog okruženja pomoću zamjena virtualne IP (VIP)." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Pripremna aplikacije u Azure

Pripremna okruženje u Azure testira prije prelaska na radnog okruženja u kojem se može pristupiti na Internetu aplikacija može uvesti zapakirani aplikacije. Pripremna okruženje je isto kao i u radnom okruženju osim možete pristupiti samo postupnu aplikacijom zatamnjenog URL koji je generirao Azure. Nakon što ste provjerili funkcionira li vaša aplikacija pravilno, on može uvesti u radnog okruženja izvođenjem virtualne IP (VIP) Zamijeni.

> [AZURE.NOTE] Koraci u ovom članku odnose se samo na čvor aplikacije hostira kao servis oblaka Azure.

## <a name="step-1-stage-an-application"></a>Korak 1: Faze aplikacije

Ovaj zadatak opisuje kako faze aplikacije pomoću **Microsoft Azure PowerShell**.

1.  Kada servisa za objavljivanje, jednostavno prenesite na **-vremensko razdoblje** parametar cmdlet **Objavi AzureServiceProject** .

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Prijavite se na [portal za Azure klasični] i odaberite **Servise u Oblaku**. Nakon stvaranja servisa u oblaku i stupac statusu **pripremna** ažurirana **pokrenut**, kliknite naziv usluge.

    ![Portal za prikazivanje pokrenuti servis][cloud-service]

3.  Odaberite **nadzornu ploču**, a zatim odaberite **pripremna**.

    ![Nadzorna ploča službe za oblaka][cloud-service-dashboard]

4. Imajte na umu vrijednost u stavci **URL web-mjesta** udesno. Naziv DNS-a je zatamnjenog Interna ID koji je generirao Azure.

    ![url web-mjesta][cloud-service-staging-url]

Sada možete provjeriti aplikacije radi li ispravno u pripremna okruženje pomoću pripremna URL web-mjesta.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Korak 2: Nadogradnju aplikacije u proizvodnje po zamjena VIPs

Nakon potvrde nadograđenoj verziji aplikacije u okruženju pripremna koju možete brzo učiniti dostupnom u radni po zamjena na virtualne IP-ovi (VIPs) od pripremna i radnog okruženja.

> [AZURE.NOTE] U ovom se koraku podrazumijeva imate li već implementirati aplikaciju radnog i kopirana bez postavljanja nadograđenoj verziji aplikacije.

1.  Prijavite se na [portal za Azure klasični], kliknite **Servise u Oblaku** , a zatim odaberite naziv usluge.

2.  Iz **nadzorne ploče**, odaberite **pripremna**, a zatim **Zamijeni** pri dnu stranice. Otvorit će se dijaloški okvir Zamijeni VIP.

    ![dijaloški okvir Zamijeni VIP][vip-swap-dialog]

3.  Pregledajte podatke, a zatim kliknite **u redu**. Dva implementacijama započnite ažuriranje kao pripremna implementacije prebacuje na radni i implementaciju radni se prebaciti na pripremna.

Uspješno kopirana bez postavljanja implementacije i nadograditi radnog implementacije po zamjena VIPs s implementacije u pripremna.

## <a name="additional-resources"></a>Dodatni resursi

- [Kako implementirati nadogradnje servisa da biste radni po zamjena VIPs servisu Azure]

[Azure klasični portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Kako implementirati nadogradnje servisa da biste radni po zamjena VIPs servisu Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
