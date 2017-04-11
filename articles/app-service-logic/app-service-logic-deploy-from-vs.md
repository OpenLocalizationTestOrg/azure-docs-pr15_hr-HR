<properties 
    pageTitle="Sastavljanje logike aplikacije u Visual Studio | Microsoft Azure" 
    description="Stvaranje projekta u Visual Studio stvaranje i implementacija aplikacije logike." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Stvaranje i implementaciju aplikacija logike u Visual Studio

Iako [Azure Portal](https://portal.azure.com/) daje odličan način dizajna te upravljati aplikacijama logike, možda želite dizajniranje i implementacija aplikacije logike s Visual Studio umjesto toga.  Logika aplikacije isporučuje se s obogaćenim Visual Studio alata koji omogućuje sastavljanje logike aplikacije pomoću dizajnera, konfiguriranje sve predlošci implementaciju i Automatizacija i implementacija u bilo kojem okruženje.  

## <a name="installation-steps"></a>Korake za instalaciju

U nastavku su korake za instalaciju i konfiguriranje Visual Studio tools za logike aplikacije.

### <a name="prerequisites"></a>Preduvjeti

- [Visual Studio 2015.](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Najnovije Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 ili noviji)
- [Azure PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Web-mjesto kada koristite ugrađeni designer

### <a name="install-visual-studio-tools-for-logic-apps"></a>Instalacija alata Visual Studio za logike aplikacije

Kada imate Preduvjeti instalacije 

1. Otvorite Visual Studio 2015 na izborniku **Alati** , a zatim odaberite **Extensions i ažuriranja**
1. Odaberite kategoriju **Online** da biste pretražili mrežu
1. Traženje **Logike aplikacije** da bi se prikazali **Alati za aplikacije logike Azure za Visual Studio**
1. Kliknite gumb **Preuzimanje** da biste preuzeli i instalirali datotečni nastavak
1. Ponovno pokrenite Visual Studio nakon instalacije

> [AZURE.NOTE] Proširenje možete preuzeti izravno iz [ove veze](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)

Jednom instaliranih će moći koristite project grupa resursa Azure pomoću dizajnera logike aplikacije.

## <a name="create-a-project"></a>Stvaranje projekta

1. Idite na izborniku **datoteka** , a zatim odaberite **Novo** >  **projekta** (ili možete kliknite **Dodaj** , a zatim odaberite **novi projekt** da biste ga dodali postojeće rješenje):  ![izbornik datoteka](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. U dijaloškom okviru Traženje **oblaka**, a zatim **Azure grupu resursa**. Upišite **naziv** , a zatim kliknite **u redu**.
    ![Dodavanje novog projekta](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Odaberite predložak **logike aplikacije** . To će započeti s stvorite predložak za implementaciju aplikacije praznu logike.
    ![Odabir predloška Azure](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Kada odaberete **predložak**, kliknite **u redu**.

    Logika projekta aplikacije sada se dodaje rješenje. Trebali biste vidjeti datoteke implementacije u pregledniku rješenja:  

    ![Uvođenje](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Pomoću dizajnera logike aplikacije

Nakon što dodate projekta programa Azure grupa resursa koji sadrži aplikaciju logike, možete otvoriti dizajner programa Visual Studio kao pomoć u stvaranju tijeka rada.  Dizajner zahtijeva internetsku vezu da bi upit poveznici za dostupnih svojstava i podataka (Ako, na primjer, ako koristite poveznik za Dynamics CRM Online, dizajnera će upit vašoj instanci CRM popis dostupnih Prilagođeno i Zadana svojstva).

1. Desnom tipkom miša kliknite na `<template>.json` datoteku i odaberite **Otvori pomoću dizajnera logike aplikacije** (ili `Ctrl+L`)
1. Odaberite pretplatu, grupa resursa i mjesto za implementaciju predložak
    - Nije važno Imajte na umu dizajniranje logike aplikacija će stvoriti **API** resursima za dohvaćanje svojstva tijekom dizajna.  Grupa resursa koji je odabran bit će grupa resursa koji se koriste za stvaranje te veze vrijeme dizajna.  Možete prikazati ili izmijeniti sve veze API tako da na portalu Azure i pregledavanje za **API veze**.
    ![Odabir pretplate](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. Dizajner treba prikazati na temelju definiciju u na `<template>.json` datoteku.
1. Sada možete stvoriti i dizajniranje logike aplikacije, a promjene će se ažurirati u predlošku implementacije.
    ![Dizajner u Visual Studio](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Vidjet ćete i `Microsoft.Web/connections` resursi koji su dodani u datoteku resursa za sve veze koja su potrebna za aplikaciju logiku funkcije.  Svojstva ove veze možete postaviti prilikom implementacije i upravlja nakon implementacije **API veze** na portalu za Azure.

### <a name="switching-to-the-json-code-view"></a>Prijelaz u kod JSON-prikaz

Možete odabrati na kartici **Prikaz koda** na dnu dizajnera da biste prešli na prikaz JSON logike aplikacije.  Da biste se vratili na punu resursa JSON, desnom tipkom miša kliknite na `<template>.json` datoteku i odaberite **Otvori**.

### <a name="saving-the-logic-app"></a>Spremanje aplikacije logike

Možete spremiti aplikaciju logike u bilo kojem trenutku putem gumba **Spremi** ili `Ctrl+S`.  Ako postoje pogreške s aplikacijom logike u trenutku kad spremite, će prikazati u prozoru **izlaze** Visual Studio.

## <a name="deploying-your-logic-app"></a>Implementacija aplikacije logike

Na kraju, kada ste konfigurirali aplikacije, može uvesti izravno iz Visual Studio u samo nekoliko koraka. 

1. Desnom tipkom miša kliknite na projektu u pregledniku rješenja i idite na **uvođenja** > **Novu implementaciju...** 
     ![Novu implementaciju](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Se od vas zatraži prijavite se u vašem Azure subscription(s). 

3. Sada ćete morati odabrati detalje o grupi resursa koje želite implementirati aplikaciju logike da biste. 
    ![Implementacija grupa resursa](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Obavezno odaberite desno datoteke predloška i parametara za grupu resursa (na primjer Ako implementirate radnog okruženja želite odaberite parametara datoteka radnog). 
4. Odaberite gumb uvođenja
 
    
6. Pojavit će se status implementacije u **izlaznom** prozoru (možda morati odabrati **Azure dodjele resursa**. 
    ![Izlaz](./media/app-service-logic-deploy-from-vs/output.png)

U budućnosti možete pregledati aplikacije logike u kontrolu izvorišnog i pomoću programa Visual Studio za implementaciju nove verzije. 

> [AZURE.NOTE] Ako mijenjate definiciju na portalu za Azure izravno, zatim kada sljedeći put implementacije iz Visual Studio te promjene će se prebrisati.

## <a name="next-steps"></a>Daljnji koraci

- Za početak rada s aplikacijama logike slijedite Praktični vodič za [Stvaranje logike aplikacije](app-service-logic-create-a-logic-app.md) .  
- [Primjeri uobičajenih prikaza i scenarijima](app-service-logic-examples-and-scenarios.md)
- [Možete automatizacija poslovnih procesa pomoću aplikacije logike](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Saznajte kako integrirati vaše sustavi s aplikacijama logike](http://channel9.msdn.com/Events/Build/2016/P462)