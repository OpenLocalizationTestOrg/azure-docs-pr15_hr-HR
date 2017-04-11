<properties
   pageTitle="Kako nabaviti AppSource Certificirano za Azure Active Directory | Microsoft Azure"
   description="Detalji o tome kako se aplikacija AppSource Certificirano za Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/28/2016"
   ms.author="skwan;bryanla"/>

#<a name="how-to-get-appsource-certified-for-azure-active-directory-ad"></a>Kako nabaviti AppSource Certificirano za Azure Active Directory (AD) 

Da biste dobili AppSource certifikata za Azure AD, aplikacije morate provesti znak više klijenta u uzorku s Azure AD pomoću protokola OpenID povezivanje, OAuth 2.0 ili SAML 2.0. 

Ako niste upoznati s Azure AD prijavu ili razvoj aplikacija za više klijentu:

1. Najprije čitanje o web- [preglednika na web-aplikacije scenarije u scenarijima provjere autentičnosti za Azure AD][AAD-Auth-Scenarios-Browser-To-WebApp].  
2. Nakon toga pogledajte Azure AD [vodi web aplikacije brzi početak rada][AAD-QuickStart-Web-Apps], koji Demonstracija implementirati prijave i sadrže primjere koda suradnika. 

    > [AZURE.TIP] Isprobajte pretpregled značajke naš novi [portala za razvojne inženjere](https://identity.microsoft.com/Docs/Web) koji će vam omogućiti brz početak rada s Azure Active Directory u samo nekoliko minuta!  Portala za razvojne inženjere će vas voditi kroz postupak registracije aplikacije i integracija Azure AD u kodu.  Kada završite, imat ćete jednostavan aplikacije koje možete provjere autentičnosti korisnika u klijentu i u pozadinskoj koje možete prihvatiti tokeni i izvršiti provjeru valjanosti.

3. Da biste saznali kako implementirati uzorak više klijent, prijavu s Azure AD, pogledajte [kako se prijaviti u bilo koji korisnik Azure Active Directory (AD) pomoću uzorak više klijentske aplikacije][AAD-Howto-Multitenant-Overview]

## <a name="related-content"></a>Srodni sadržaji
Dodatne informacije o izradu aplikacija koje podržavaju Azure AD prijavu ili da biste zatražite pomoć i podršku, pogledajte [Vodič za Azure AD programiranje][AAD-Dev-Guide].

Povratne informacije i Pomozite nam da budemo sužavanje i oblika sadržaj, koristite odjeljak Komentari Disqus praćenje ovog članka.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#web-application-quick-start-guides


<!--Image references-->










