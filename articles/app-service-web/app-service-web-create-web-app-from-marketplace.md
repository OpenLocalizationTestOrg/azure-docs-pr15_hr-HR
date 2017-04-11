<properties
    pageTitle="Stvaranje web-aplikacijama iz trgovine Azure | Microsoft Azure"
    description="Saznajte kako stvoriti novu WordPress web-aplikaciju iz trgovine Azure pomoću portala za Azure."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Stvaranje web-aplikacijama iz trgovine Windows Azure

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Trgovina Azure postaje dostupan brojne popularne web-aplikacije razvio Microsoft, tvrtke trećih strana, a zatim Otvori izvor softver inicijative. Na primjer, WordPress, a CMS Umbraco, Drupal, itd. Web aplikacije su programirane na brojne popularne okviri, kao što je [i] u ovom WordPress primjer, [.NET], [Node.js], [jezika Java]i [Python], nekoliko. Da biste stvorili web-aplikacijama iz trgovine Azure samo softver potreban je web-pregledniku koji koristite za [Azure Portal].

U ovom ćete praktičnom vodiču ćete saznati kako:

* Pronađite i stvorite web-aplikaciju u aplikacije servisa Azure koji se temelji na predlošku trgovine Windows Azure.
* Konfiguriranje postavki aplikacije servisa za Azure za novu web-aplikaciju.
* Pokretanje i upravljanje web-aplikaciju programa.

Za potrebe ovog praktičnog vodiča će implementirati na WordPress blog web-mjesto iz trgovine Azure Marketplace. Kada dovršite korake ovog praktičnog vodiča, imat ćete vlastitom web-mjestu WordPress prema gore i pokretanje u oblaku.

![Primjer WordPress wep aplikacije nadzorne ploče][WordPressDashboard1]

Web-mjesto WordPress koje ćete implementacije u ovom ćete praktičnom vodiču koristi MySQL za bazu podataka. Želite li umjesto toga koristite SQL baze podataka za bazu podataka, potražite u članku [Nami projekt], koji je dostupan putem servisa Azure Marketplace.

> [AZURE.NOTE]
> Da biste dovršili ovaj Praktični vodič, morate račun sustava Microsoft Azure. Ako nemate račun, možete ga [aktivirati svoje prednosti pretplatnika Visual Studio] [ activate] ili se [prijavite se za besplatnu probnu verziju][free trial].
>
> Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa]. Iz nje odmah možete stvoriti web-aplikacijama short-lived starter u aplikacije servisa za – potreban je bez kreditne kartice, te su bez p.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Pronalaženje i stvaranje web-aplikacijama u aplikacije servisa za Azure

1. Prijavite se na [Portal za Azure].

1. Kliknite **Novo**.
    
    ![Stvaranje novog Azure resursa][MarketplaceStart]
    
1. Traženje **WordPress**, a zatim kliknite **WordPress**. (Ako želite značajku SQL baze podataka MySQL potražiti **Projekta Nami**.)

    ![Traženje WordPress na tržištu][MarketplaceSearch]
    
1. Kad pročitate opis WordPress aplikaciju, kliknite **Stvori**.

    ![Stvorite WordPress web-aplikaciju][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfiguriranje postavki Azure aplikacije servisa za novu web-aplikaciju

1. Kada stvorite novu web-aplikaciju, postavke plohu WordPress, pojavit će se koji ćete koristiti da biste dovršili sljedeće korake:

    ![Konfiguriranje postavki za WordPress web-aplikacije][ConfigStart]

1. Unesite naziv za web-aplikacije u okvir **Web app** .

    Taj naziv mora biti jedinstvena u domeni azurewebsites.net jer URL web-aplikaciji bit će *{name}*. azurewebsites.net. Ako ne naziv unesite jedinstveni, pojavljuje se crveni uskličnik u tekstni okvir.

    ![Konfiguriranje naziv WordPress web app][ConfigAppName]

1. Ako imate više pretplata, odaberite onaj koji želite koristiti. 

    ![Konfiguriranje pretplatu za web-aplikaciju][ConfigSubscription]

1. Odaberite **Grupu resursa** ili stvorite novi.

    Dodatne informacije o grupama resursa potražite u članku [Pregled upravljanja resursima Azure][ResourceGroups].

    ![Konfiguriranje grupa resursa za web-aplikacije][ConfigResourceGroup]

1. Odaberite **Plan mjesto aplikacije servisa** ili stvorite novi.

    Dodatne informacije o tarifama za aplikacije servisa za potražite u članku [Pregled tarife aplikacije servisa za Azure][AzureAppServicePlans]. 

    ![Konfiguriranje tarifa za servis za web-aplikacije][ConfigServicePlan]

1. Kliknite **bazu podataka**, a zatim u **Nove baze podataka MySQL** plohu unesite tražene vrijednosti za konfiguriranje baze podataka MySQL.

    na. Unesite novi naziv ili ostavite zadani naziv.

    b. Ostavite **Vrsta baze podataka** postavljena na **zajedničko korištenje**.

    c. Odaberite na isto mjesto u onu koju odaberete za web-aplikacije.

    d. Odaberite sloj cijene. **Žive** - koji je besplatno s minimalnim veze i prostor na disku - je u redu za ovog praktičnog vodiča.

    e. U plohu **Nove baze podataka MySQL** prihvatili uvjete za pravne, a zatim kliknite **u redu**. 

    ![Konfiguriranje postavki baza podataka za web-aplikacije][ConfigDatabase]

1. U plohu **WordPress** prihvatili uvjete za pravne, a zatim kliknite **Stvori**. 

    ![Završi web-aplikacije postavke, a zatim kliknite u redu][ConfigFinished]

    Aplikacije servisa za Azure stvara web-aplikaciji obično manje od minute. U tijeku možete pogledati tako da kliknete ikonu zvona pri vrhu stranice portala.

    ![Pokazatelj tijeka][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Pokretanje i upravljanje web-aplikaciju programa WordPress
    
1. Po završetku web app stvaranje idite na portalu Azure grupi resursa u kojem je stvorena aplikacija, a vidjet ćete web-aplikacije i baze podataka.

    Dodatni resurs ikonom pronađen je [Aplikacija uvida][ApplicationInsights], koja omogućuje nadzor servise za web-aplikacije.

1. U plohu **grupa resursa** kliknite redak web app.

    ![Odaberite web-aplikaciju programa WordPress][WordPressSelect]

1. U aplikaciji plohu Web kliknite **Pregledaj**.

    ![Dođite na web-aplikaciju WordPress][WordPressBrowse]

1. Ako se od vas zatraži da biste odabrali jezik za WordPress blog, odaberite željeni jezik, a zatim kliknite **Nastavi**.

    ![Konfiguriranje jezika za web-aplikaciju programa WordPress][WordPressLanguage]

1. Na stranici WordPress **dobrodošlice** unesite podatke o konfiguraciji potrebnih WordPress, a zatim **Instalirati WordPress**.

    ![Konfiguriranje postavki web-aplikaciju programa WordPress][WordPressConfigure]

1. Prijavite se pomoću vjerodajnica koje ste stvorili na stranici **dobrodošlice** .  

1. Nadzorna ploča stranicu web-mjesta će otvoriti i prikaza podataka koju ste naveli.    

    ![Prikaz WordPress nadzorne ploče][WordPressDashboard2]

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču ste vidjeti kako stvoriti i implementirati primjer web-aplikaciji iz trgovine Windows Azure.

Dodatne informacije o radu s web-aplikacije servisa za aplikacije potražite putem veza na lijevoj strani stranice (za široki preglednika windows) ili pri vrhu stranice (za windows usko preglednika).

Dodatne informacije o razvoju aplikacija WordPress web apps na Azure potražite u članku [Razvoj WordPress na aplikacije servisa za Azure][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Pokušajte aplikacije servisa]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Portal za Azure]: https://portal.azure.com/
[Nami projekta]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
