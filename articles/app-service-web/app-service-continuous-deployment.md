<properties
    pageTitle="Neprekinuti implementacije aplikacije servisa za Azure | Microsoft Azure"
    description="Saznajte kako omogućiti neprekinuti implementacije aplikacije servisa za Azure."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Neprekinuti implementacije aplikacije servisa za Azure

Pomoću ovog praktičnog vodiča objašnjava konfiguriranje neprekinuti implementacije tijeka rada za [Aplikacije servisa za Azure] aplikacije. Aplikacije servisa za integraciju s BitBucket, GitHub i Visual Studio tima Services (VSTS) omogućuje neprekinuti implementacije tijeka rada kojem Azure povlači u najnovija ažuriranja iz projekta objavljene na nekog od tih servisa. Neprekinuti implementaciju videobilješki vrlo je korisno za projekte gdje više i integrirana Česti prilozima.

## <a name="overview"></a>Omogućivanje neprekinuti implementacije

Da biste omogućili neprekinuti implementacije 

1. Objavljivanje sadržaja aplikacije u spremište koja će se koristiti za implementaciju neprekinuti.  
    Dodatne informacije o objavljivanju projekta za te servise, potražite u članku [Stvaranje repo (GitHub)], [Stvaranje repo (BitBucket)]i [Početak rada s VSTS].

2. Pokrenite aplikaciju plohu izbornik [Azure portal], kliknite **IMPLEMENTACIJU Aplikacije > Mogućnosti implementacije**. Kliknite **Odabir izvora**, a zatim odaberite izvor implementacije.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Konfiguriranje programa VSTS račun za implementaciju aplikacije servisa za potražite u ovom [ćete praktičnom vodiču](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Dovršavanje tijeka rada za autorizaciju. 

4. U plohu **implementacije izvor** odaberite projekta i podružnice za implementaciju s. Kada završite, kliknite **u redu**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Prilikom omogućivanja neprekinuti implementacija s GitHub ili BitBucket, prikazat će se javne i privatne projekata.

    Aplikacije servisa za stvara pridruživanje s odabranog spremište, povlači na popisu datoteke iz navedeni podružnice i održava Kloniraj vaše spremištu aplikacije servisa za aplikacije. Kada konfigurirate VSTS neprekinuti implementaciju s portala za Azure, Integracija koristi [Modul Kudu implementaciju](https://github.com/projectkudu/kudu/wiki)aplikacije servisa koji već automatizira Sastavi i implementacija zadataka s svaki `git push`. Ne moraju zasebno postavite neprekinuti implementacije u VSTS. Po dovršetku ovog postupka aplikacije plohu **Mogućnosti implementacije** vode aktivnog uvođenja koji označava implementacije je uspio.

5. Da biste provjerili aplikaciju uspješno implementiran, kliknite **URL** pri vrhu aplikacije plohu na portalu za Azure. 

6. Da biste potvrdili da su neprekinuti implementacije pojavljivanja u spremištu po izboru, automatske promjene u spremište. Pokrenite aplikaciju trebali biste ažurirati tako da odražavaju promjene uskoro završetku automatske u spremište. Možete provjeriti je li povlače ažuriranjem u plohu **Mogućnosti implementacije** aplikacije.

## <a name="VSsolution"></a>Neprekinuti implementacije rješenja za Visual Studio 

Samo dovoljno margina jednostavne index.html datoteka je margina Visual Studio rješenja za aplikacije servisa za Azure. Postupak za implementaciju aplikacije servisa za pojednostavnjuje sve detalje, uključujući vraćanje NuGet ovisnosti te izgradnju binarne datoteke iz aplikacije. Slijedite na izvor kontrole najbolje prakse održavanje kod samo u vašem spremište brojka i omogućivanje aplikacije servisa za implementaciju voditi brigu o ostale.

Koraci za Visual Studio rješenje margina za aplikacije servisa za su jednaki onima u [prethodnom odjeljku](#overview), pod uvjetom da konfigurirate rješenje i spremište na sljedeći način:

-   Koristiti mogućnost programa Visual Studio izvora kontrole za generiranje na `.gitignore` datoteke kao što su na slici u nastavku ili ručno dodati u `.gitignore` datoteke u korijensko spremište sa sadržajem slično [.gitignore uzorka](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore). 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Dodajte cijelu rješenje direktorija stabla spremište, s datotekom .sln u korijenskoj mapi spremište.

Kada ste postavili vaše spremište kao što je opisano i konfigurirali aplikacije u Azure za neprekinuti objavljivanje iz jednog od online spremištima brojka, možete razviti ASP.NET aplikacija lokalno u Visual Studio i neprestano implementacije kod jednostavno pritiskom promjene na mreži brojka spremištu.

## <a name="disableCD"></a>Onemogućivanje neprekidnog implementacije

Onemogućivanje neprekidnog implementacije 

1. Pokrenite aplikaciju plohu izbornik [Azure portal], kliknite **IMPLEMENTACIJU Aplikacije > Mogućnosti implementacije**. Zatim kliknite **Prekini vezu** u plohu **Mogućnosti implementacije** .

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Nakon **da** odgovorite na poruku o potvrdi, možete vratiti plohu pokrenite aplikaciju i kliknite **IMPLEMENTACIJU Aplikacije > Mogućnosti implementacije** ako želite da biste postavili objavljivanje iz drugog izvora.

## <a name="additional-resources"></a>Dodatni resursi

* [Kako Istražite uobičajene probleme s neprekinutim implementacije](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Kako pomoću ljuske PowerShell za Azure]
* [Upute za korištenje alata za Azure naredbenog retka za Mac i Linux]
* [Dokumentacija brojka]
* [Kudu projekta](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

[Aplikacije servisa za Azure]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Portal za Azure]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Kako pomoću ljuske PowerShell za Azure]: ../articles/powershell-install-configure.md
[Upute za korištenje alata za Azure naredbenog retka za Mac i Linux]: ../articles/xplat-cli-install.md
[Dokumentacija brojka]: http://git-scm.com/documentation

[Stvaranje repo (GitHub)]: https://help.github.com/articles/create-a-repo
[Stvaranje repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Početak rada s VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
