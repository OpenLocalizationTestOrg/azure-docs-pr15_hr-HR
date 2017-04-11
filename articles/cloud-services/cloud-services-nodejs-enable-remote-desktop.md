<properties 
    pageTitle="Omogući udaljenu radnu površinu za servise u oblaku (Node.js)" 
    description="Saznajte kako omogućiti pristup alat za analizu daljinske površini za hostiranje vaše aplikacije Azure Node.js virtualnim računalima sustava." 
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

# <a name="enabling-remote-desktop-in-azure"></a>Omogućivanje udaljene radne površine u Azure

Udaljena radna površina omogućuje vam pristup radnoj površini uloga instance izvodi u Azure. Da biste konfigurirali virtualnog računala ili otklanjanje poteškoća s aplikacijom možete koristiti udaljene radne površine.

> [AZURE.NOTE] Ovaj se članak odnosi Node.js aplikacijama hostira kao servis oblaka Azure.


## <a name="prerequisites"></a>Preduvjeti

- Instaliranje i konfiguriranje [Azure Powershell](../powershell-install-configure.md).
- Implementacija aplikacije Node.js sa servisom Azure oblaka. Dodatne informacije potražite u članku [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Korak 1: Korištenje Azure ljuske PowerShell za konfiguriranje servisa za pristup udaljene radne površine

Da biste koristili udaljene radne površine, trebate ažurirati definicije servisa Azure i konfiguraciji pomoću korisničkog imena, lozinke i certifikata. 

S računala koja sadrži izvorne datoteke za aplikaciju poduzeti sljedeće korake.

1. Pokrenite **Windows PowerShell** kao Administrator. (S **Izbornika Start** ili **Početnom zaslonu**pretraživanje za **Windows PowerShell**.)

2.  Dođite do direktorij koji sadrži servisa definicija (.csdef) i datoteke servisa konfiguracije (.cscfg).

3. Unesite sljedeći cmdlet komponente PowerShell:

        Enable-AzureServiceProjectRemoteDesktop

4. Kada se zatraži, unesite korisničko ime i lozinku.

    ![Omogući azureserviceprojectremotedesktop][enable-rdp]

3.  Unesite sljedeći cmdlet ljuske PowerShell za objavljivanje promjene:

        Publish-AzureServiceProject

    ![Objavljivanje azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Korak 2: Povezivanje s instancom uloga

Nakon objavljivanja definiciju servis za ažuriranje, možete se povezati na instancu uloga.

1.  [Azure klasični portala]odaberite **Servise u Oblaku** , a zatim na servisu.

    ![Azure klasični portal][cloud-services]

2.  Kliknite **instanci**, a zatim **proizvodnje** ili **pripremna** da biste vidjeli instanci servisa. Odaberite instancu, a zatim kliknite **Poveži se** pri dnu stranice.

    ![Instance stranice][3]

2.  Kada kliknete **Povezivanje**, web-pregledniku od vas traži da biste spremili .rdp datoteku. Otvorite tu datoteku. (Na primjer, ako koristite Internet Explorer, kliknite **Otvori**.)

    ![upit za otvaranje ili spremanje .rdp datoteku][4]

3.  Kada se datoteka otvori, pojavit će se sljedeći upit sigurnost:

    ![Upit za sigurnost sustava Windows][5]

4.  Kliknite **Poveži**i pojavit će se upit za sigurnost za unos vjerodajnica za pristup instanci. Unesite lozinku koju ste stvorili u [korak 1] [korak 1: Konfiguriranje servisa za pristup udaljene radne površine pomoću komponente PowerShell Azure], a zatim kliknite **u redu**.

    ![upit za korisničko ime i lozinku][6]

Kad upišete veze udaljene radne površine prikazuje radnu površinu instance Azure. 

![Sesiji udaljene radne površine][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Korak 3: Konfiguriranje servisa da biste onemogućili pristup udaljene radne površine 

Kada više ne zahtijeva udaljene radne površine veze s poslužiteljem instance uloga u oblaku, onemogućite pomoću [Komponente PowerShell Azure]daljinski pristup za stolna računala.

1.  Unesite sljedeći cmdlet komponente PowerShell:

        Disable-AzureServiceProjectRemoteDesktop

2.  Unesite sljedeći cmdlet ljuske PowerShell za objavljivanje promjene:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Dodatni resursi

- [Daljinski pristup uloga instance Azure] 
- [Putem udaljene radne površine Azure uloga]
- [Razvojni centar za Node.js](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure klasični portal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Daljinski pristup uloga instance Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Putem udaljene radne površine Azure uloga]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 