<properties
    pageTitle="Web-aplikacija na Azure stogu – Poznati problemi i otklanjanje poteškoća | Microsoft Azure"
    description="Detaljne upute o implementaciji Web Apps u stogu Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Davatelja iz aplikacije resursa web - poznatih problema i otklanjanje poteškoća

> [AZURE.NOTE] Sljedeće informacije odnosi se samo na Azure stogu TP1 implementacije.

Ako imate problema s pri implementaciji ili rad s Web Apps na hrpu Azure pogledajte upute u nastavku.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure stogu Web Apps nije dostupno na portalu

![Azure stogu web-aplikacije stvorite novu web-aplikaciju][1]

Kada dovršite [Registracija Azure stogu Web Apps davatelja](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) ako ne možete pronaći Web + mobilne plohu pokušajte sljedeće:
* **Odjavite se na portal** i **zatvorite preglednik** , a zatim u **Novi prijava u prozoru preglednika na portal sustava** i pokušajte ponovno.

Ako i dalje ne vidite web i mobilne plohu pokušajte sljedeće:

1.  **Glavno računalo za Azure snop** u **Upravitelju Hyper-V** pronađite **xRPVM** i **Povezivanje** s njima u VM.
2.  Da biste otvorili **naredbenog retka kao Administrator** i pokretanje **IISRESET**
3.  Vratite **ClientVM** i otvorite **novi prozor preglednika**, **prijavite se na portal** i pokušajte ponovno.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Implementacija ne uspijeva kada je stvaranje web-aplikacijama u novu grupu resursa

U prvom pretpregled od web-aplikacije, **Sve web-aplikacije** moraju se stvoriti u istoj grupi resursa kao davatelja resursa Web Apps (**AppService-LOCAL**).  Ovo je poznat problem i riješiti u budućim pretpregled.

## <a name="other-issues"></a>Ostali problemi

Ako naiđete na druge probleme s Web Apps na hrpu Azure provjerite objavu u [Forum Azure stogu] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) kojem članovi naš tim nadzirete objave.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
