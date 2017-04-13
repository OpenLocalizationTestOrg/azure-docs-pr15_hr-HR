<properties 
   pageTitle="Dodavanje Azure Active Directory pomoću povezani servisi u Visual Studio | Microsoft Azure"
   description="Dodavanje Azure Active Directory pomoću dijaloškog okvira Visual Studio dodavanje povezani servisi"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Dodavanje Azure Active Directory pomoću povezani servisi u Visual Studio 

##<a name="overview"></a>Pregled
Pomoću Azure Active Directory (Azure AD) podržavaju jedan prijavu (SSO) za ASP.NET MVC web-aplikacijama ili provjere autentičnosti AD Web API Services. S provjerom autentičnosti za Azure AD vaši korisnici možete koristiti svoje račune iz Azure AD za povezivanje s web-aplikacije. Prednosti Azure AD provjera autentičnosti s Web API obuhvaćaju sigurnost poboljšane podataka kada će se API iz web-aplikacije. S Azure AD nemate za upravljanje sustavom zasebnom provjeru autentičnosti s vlastitom račun i korisnik upravljanje.

## <a name="supported-project-types"></a>Vrste podržanih projekta

Dijaloški okvir povezani servisi možete koristiti za povezivanje s Azure AD u sljedećim vrstama projekta.

- Projekti MVC platforme ASP.NET

- API na webu ASP.NET projekata


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Povezivanje s Azure AD pomoću dijaloškog okvira povezani servisi

1. Provjerite je li račun za Azure. Ako nemate račun za Azure, možete se prijaviti za [besplatnu probnu verziju](http://go.microsoft.com/fwlink/?LinkId=518146).

1. U Visual Studio otvorite izbornički prečac čvora **reference** u projektu, a zatim odaberite **Dodaj povezani servisi**.
1. Odaberite **Azure AD provjeru autentičnosti** , a zatim odaberite **Konfiguriraj**.

    ![Odaberite Dodaj Azure AD provjere autentičnosti](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Na prvoj stranici **Konfiguriranje Azure AD autentičnosti**provjerite **Konfiguriranje jedinstvene prijave pomoću Azure AD**.

    Ako projekt je konfiguriran konfiguraciji drugi provjeru autentičnosti, čarobnjak će vas upozoriti da nastavite će onemogućiti prethodne konfiguracije.

    ![U čarobnjaku za konfiguriranje Azure AD](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Na drugoj stranici s padajućeg popisa **domene** odaberite domenu. Na popisu domena sadrži sve dostupne domene tako da računi koji je naveden u dijaloškom okviru Postavke računa. Ako ne pronađete onu koju tražite, kao što su mydomain.onmicrosoft.com, umjesto toga, unesite naziv domene. Možete odabrati mogućnost da biste stvorili novu aplikaciju Azure AD ili koristiti postavke iz postojeće Azure AD aplikacije. 

    ![U čarobnjaku za konfiguriranje Azure AD](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Na trećoj stranici čarobnjaka provjerite je li **čitanje podataka direktorija** je potvrđen. Čarobnjak će ispuniti **tajna klijenta**. 

    ![U čarobnjaku za konfiguriranje Azure AD](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Odaberite gumb **Završi** . Dijaloški okvir dodaje koda za konfiguraciju potrebne i referenci da biste omogućili projekta za Azure AD provjeru autentičnosti. Vidjet ćete domene servisa Active Directory za [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Pregledajte stranici početak rada koji se prikazuje u pregledniku sinonima na sljedeće korake i stranice što se dogodilo da biste vidjeli kako se projekt izmjene. Ako želite provjeriti sve uspješnog, otvorite neku izmijenjene konfiguracijskoj datoteci i provjerite postavke spomenute u što se dogodilo postoje. Ako, na primjer, glavni web.config u projektu ASP.NET MVC će su vam ove postavke dodali:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Kako je izmijenjena projekta

Kada pokrenete čarobnjak, Visual Studio dodaje Azure AD i pridruženi reference na projektu. Konfiguraciju i kod datoteke u projektu i mijenjati da biste dodali podrška za Azure AD. Određene izmjene koje čini Visual Studio ovise o vrsti projekta. Detaljne informacije o kako se mijenjaju ASP.NET MVC projektima, potražite u članku [Što projekata MVC za se dogodilo –](http://go.microsoft.com/fwlink/p/?LinkID=513809). Web API projektima, potražite u članku [što se dogodilo – projekata API -JA za Web](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Daljnji koraci

Postavljati pitanja i pomoć.

 - [MSDN Forum: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Dokumentacija za Azure AD](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Članak na blogu: Uvod u Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

