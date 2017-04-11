<properties 
pageTitle="Omogućivanje Remote Desktop Connection za uloga u servise u Oblaku za Azure" 
description="Kako konfigurirati servisnoj aplikaciji azure oblaka dopustiti daljinske veze za stolna računala" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Omogućivanje Remote Desktop Connection za uloga u servise u Oblaku za Azure

>[AZURE.SELECTOR]
- [Azure klasični portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Udaljena radna površina omogućuje vam pristup radnoj površini uloge izvodi u Azure. Pomoću veze udaljene radne površine da biste i otklanjanja poteškoća s aplikacijom dok se izvodi. 

Možete omogućiti veze udaljene radne površine u vaša uloga tijekom razvoj dodavanjem modula udaljene radne površine u definicije servisa ili možete odabrati želite li omogućiti udaljene radne površine putem proširenja za udaljene radne površine. Preferirani pristup jest korištenje proširenje udaljene radne površine kao čak i kada aplikacija je implementiran bez potrebe za implementirati aplikaciju možete omogućiti udaljene radne površine. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfiguriranje udaljene radne površine na portalu Azure klasični
Azure klasični portal koristi pristup udaljene radne površine kućni broj pa čak i kada je implementirati aplikaciju možete omogućiti udaljene radne površine. Stranici **Konfiguracija** servisa za oblak omogućuje vam da biste omogućili udaljene radne površine, promijenite lokalnog administratorskog računa s kojim se povezujete s virtualnim strojevima, certifikat koristi u provjeru autentičnosti i postavljanje datuma isteka. 


1. Kliknite **Servise u Oblaku**, kliknite naziv servisa u oblaku i kliknite **Konfiguriraj**.

2. Kliknite **udaljene**.
    
    ![Udaljena servise u oblaku](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Sve instance uloga će se ponovno pokrenuti kada prvi put omogućite udaljene radne površine i kliknite u redu (kvačica). Da biste spriječili ponovno pokretanje, certifikat koji za šifriranje lozinku mora biti instaliran na ulogu. Da biste spriječili ponovno pokretanje računala, [Prijenos certifikat za servis u oblaku](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) , a zatim se vratite u ovom dijaloškom okviru.
    

3. **Uloge**, odaberite ulogu želite ažurirati ili odaberite **sve** za sve uloge.

4. Provjerite sljedeće promjene:
    
    - Da biste omogućili udaljene radne površine, odaberite potvrdni okvir **Omogući udaljene radne površine** . Da biste onemogućili udaljene radne površine, poništite potvrdni okvir.
    
    - Stvorite račun za korištenje u veze udaljene radne površine na instance uloge.
    
    - Ažurirajte postojeći račun.
    
    - Odaberite prenesenih certifikat za provjeru autentičnosti (prijenos certifikata pomoću **prenijeti** na stranici **Potvrda** ) ili stvorite novi. 
    
    - Promjena datuma isteka za konfiguraciju udaljene radne površine.

5. Kada završite konfiguriranje ažuriranja, kliknite **u redu** (kvačica).


## <a name="remote-into-role-instances"></a>Alat za analizu daljinske u instance uloga
Kada je na ulogama omogućena udaljene radne površine možete alat za analizu daljinske u ulogu instancu putem različitih alata.

Da biste se povezali s instancom uloga s portala za Azure klasični:
    
  1.   Kliknite **instance** da biste otvorili stranicu **instance** .
  2.   Odaberite ulogu instancu koji je konfiguriran udaljene radne površine.
  3.   Kliknite **Poveži**pa slijedite upute da biste otvorili radnu površinu. 
  4.   Kliknite **Otvori** , a zatim **Poveži** da biste pokrenuli veze udaljene radne površine. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Korištenje programa Visual Studio udaljene u ulozi instanci

U Visual Studio Explorer poslužitelja:

1. Proširenje na **Azure\\servise u Oblaku\\[naziv usluge oblaka]** čvor.
2. Proširite **Održavanje** ili **proizvodnju**.
3. Proširite pojedinačne uloge.
4. Desnom tipkom miša kliknite jedan od instance uloga kliknite **Poveži se putem udaljene radne površine...**, a zatim unesite korisničko ime i lozinku. 

![Udaljena radna površina explorer poslužitelja](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Korištenje ljuske PowerShell za dohvaćanje datoteka RDP
Cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) možete koristiti za dohvaćanje datoteka RDP. Datoteka RDP možete koristiti s udaljene radne površine da biste pristupili servisa u oblaku.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programski preuzimanje datoteke RDP putem servisa upravljanje REST API-JA
OSTATAK postupka [Preuzmi RDP datoteku](https://msdn.microsoft.com/library/jj157183.aspx) možete koristiti da biste preuzeli RDP datoteci. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Da biste konfigurirali udaljene radne površine u datoteci definicije servisa

Ovaj postupak omogućuje Omogući udaljenu radnu površinu za aplikaciju tijekom razvoj. Taj se način zahtijeva šifriranu lozinku nalaziti u konfiguraciji servisa datoteke i promjene konfiguracije udaljene radne površine je potrebna ponovno uvođenje aplikacije. Ako želite da biste izbjegli te downsides trebali biste koristiti pristup udaljene radne površine kućni broj koji se temelji na prethodno opisan.  

Visual Studio možete koristiti da biste [omogućili udaljene radne površine](../vs-azure-tools-remote-desktop-roles.md) pomoću pristup datoteka za definiciju servisa.  
Koraci u nastavku opisuju promjene potrebne za datoteka modela servisa da biste omogućili udaljene radne površine. Visual Studio automatski stvara te promjene pri objavljivanju.

### <a name="set-up-the-connection-in-the-service-model"></a>Postavljanje veze u modelu servisa 
Pomoću element **uvozi** uvoz modul za **daljinski pristup** i modul **RemoteForwarder** [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) datoteku.

Datoteka za definiciju servisa mora biti sličan sljedećem primjeru s na `<Imports>` element dodali.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Datoteka [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) mora biti slično kao u sljedećem primjeru, imajte na umu u `<ConfigurationSettings>` i `<Certificates>` elemente. Potvrda navedena mora biti [prenijeli na servis u oblaku](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Dodatni resursi

[Konfiguriranje servisa u Oblaku](cloud-services-how-to-configure.md)