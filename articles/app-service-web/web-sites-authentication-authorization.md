<properties 
    pageTitle="Autentičnost s lokalnim servisom Active Directory u aplikaciji programa Azure | Microsoft Azure" 
    description="Dodatne informacije o različitim mogućnostima redak tvrtke aplikacije u Azure aplikacije servisa za provjeru autentičnosti s lokalnim servisom Active Directory" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Autentičnost s lokalnim servisom Active Directory u aplikaciji programa Azure #

U ovom se članku objašnjava provjeru s lokalnim servisom Active Directory (AD) na [Servisu Azure aplikacije](../app-service/app-service-value-prop-what-is.md). Azure aplikacije nalazi u oblaku, ali dostupni su načini provjere autentičnosti lokalne korisnike AD sigurno. 

## <a name="authenticate-through-azure-active-directory"></a>Provjeru autentičnosti putem Azure Active Directory
Klijent za Azure Active Directory može biti direktorija sinkroniziraju s lokalnog servisa Active Directory. Taj se način AD korisnicima omogućuje pristup aplikaciju putem Interneta i provjeru autentičnosti pomoću vjerodajnica za lokalni. Osim toga, aplikacije servisa za Azure nudi [Uključi ključ rješenje za ovaj postupak](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Uz samo nekoliko klikova gumba možete omogućiti provjeru autentičnosti s klijentom direktorija sinkronizirali Azure aplikacije. Taj se način ima sljedeće prednosti:

-   Potreban kod provjere autentičnosti u svojoj aplikaciji. Omogućivanje aplikacije servisa učinite provjere autentičnosti za i potrošiti vremena na ponuda funkcija u svojoj aplikaciji.
-   [Azure AD grafikonu API](http://msdn.microsoft.com/library/azure/hh974476.aspx) omogućuje pristup podacima direktorija iz aplikacije programa Azure.
-   Daje SSO [svih aplikacija podržava Azure Active Directory](/marketplace/active-directory/), uključujući Office 365, Dynamics CRM Online, Microsoft Intune i tisuće aplikacije koje nisu Microsoft cloud. 
-   Azure Active Directory podržava kontrola pristupa na temelju uloga. Da biste kod možete koristiti uzorak [Authorize(Roles="X")] s minimalnim promjene.

Da biste vidjeli kako napisati redak tvrtke Azure aplikacije koja potvrđuje s Azure Active Directory potražite u članku [Stvaranje redak tvrtke Azure aplikacije s provjerom autentičnosti Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Provjeru autentičnosti putem programa STS lokalnog
Ako imate na lokalni sigurne tokena (STS) kao što je Active Directory Federation Services (AD FS), koristite koji za združivanje provjere autentičnosti za aplikaciju programa Azure. Taj se način najbolje je kada pravila tvrtke ne dopuštaju AD podataka pohranjivanje u Azure. Međutim, imajte na umu sljedeće:

-   Topologija STS mora biti distribuiranih lokalnim s indirektni trošak i upravljanje.
-   Samo administratori za AD FS možete konfigurirati [potrebe za oslanjanjem strana trusts i pravila zahtjeva](http://technet.microsoft.com/library/dd807108.aspx), što može vam ograničiti mogućnosti za razvojne inženjere. S druge strane, moguće je upravljanje i prilagođavanje [zahtjevima](http://technet.microsoft.com/library/ee913571.aspx) na temelju web-aplikacije.
-   Pristup na lokalni AD podataka zahtijeva zasebne rješenje vatrozidu za tvrtke.

Da biste vidjeli kako napisati redak tvrtke Azure aplikacije koja potvrđuje s STS na lokalni, potražite u članku [Stvaranje redak tvrtke Azure aplikacije s provjerom autentičnosti AD FS](web-sites-dotnet-lob-application-adfs.md).
 
