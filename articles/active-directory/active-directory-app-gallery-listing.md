<properties
   pageTitle="Popis aplikacija u galeriji aplikacije Azure Active Directory"
   description="Kako na popis programa koji podržava jedinstvenu prijavu u galeriji Azure Active Directory | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Popis aplikacija u galeriji aplikacije Azure Active Directory

Da biste dobili popis programa koji podržava jedinstvene prijave pomoću servisa Azure Active Directory u [galeriji Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/)aplikacije najprije mora implementirati jedan od sljedećih načina Integracija:

* **Povezivanje OpenID** - Izravni Integracija s Azure AD pomoću OpenID povezivanje za provjeru autentičnosti i API pristanak Azure AD za konfiguraciju. Ako tek počinjete programa integracije i aplikacija ne podržava SAML, to je način preporučeno.

* **SAML** – aplikacija već ima mogućnost konfiguriranje davatelja identiteta treće strane pomoću protokola SAML.

Preduvjeti za unos za svaki način su ispod.

##<a name="openid-connect-integration"></a>OpenID povezivanje Integracija

Da biste integrirali aplikacije s Azure AD, slijedite [upute za razvojne inženjere](active-directory-authentication-scenarios.md). Zatim dovršite pitanja u nastavku i poslati waadpartners@microsoft.com.

* Unesite vjerodajnice za testiranje klijenta ili račun s aplikacijom koje je moguće koristiti od tima za Azure AD za testiranje integraciju sa servisom.  

* Sadrži upute o kako tim za Azure AD prijavite se u ili ga povezati instance komponente Azure AD pomoću [Azure AD pristanak framework](active-directory-integrating-applications.md#overview-of-the-consent-framework)aplikacije. 

* Navedite potrebne za tim Azure AD da biste testirali jedinstvenu prijavu s aplikacijom za daljnje upute. 

* Unesite informacije u nastavku:

> Naziv tvrtke:
> 
> Web-mjesto tvrtke:
> 
> Naziv aplikacije:
> 
> Opis aplikacije (256 znakova od dopuštenog):
> 
> Aplikacija web-mjesta (Informativna):
> 
> Aplikacija tehničku podršku web-mjesta ili kontakt:
> 
> ID klijenta programa, kao što je prikazano u aplikaciji detalja pri https://manage.windowsazure.com:
> 
> URL adresa Sign-Up aplikacije kamo klijentima za prijavu i /or kupiti aplikaciju:
> 
> Odaberite do tri kategorije aplikacije navoditi u odjeljku (za dostupnih kategorija potražite u članku na trgovine Windows Azure Active Directory):
> 
> Dodavanje aplikacije malu ikonu (PNG datoteci, 45px po 45px, jednobojnu pozadinu):
> 
> Dodavanje aplikacije velike ikone (PNG datoteci, 215px po 215px, jednobojnu pozadinu):
> 
> Dodavanje logotipa aplikacije (PNG datoteci, 150px po 122px, prozirnu boju pozadine):

##<a name="saml-integration"></a>Integracija SAML

Bilo koju aplikaciju koja podržava SAML 2.0 može se integrirati izravno s klijentom sustava Azure AD pomoću [ove upute da biste dodali prilagođenu aplikaciju](active-directory-saas-custom-apps.md). Nakon što testirate funkcionira li vaša integraciju aplikacija sa Azure AD, poslati sljedeće informacije <waadpartners@microsoft.com>.

* Unesite vjerodajnice za testiranje klijenta ili račun s aplikacijom koje je moguće koristiti od tima za Azure AD da biste testirali integraciju sa servisom.  

* Navedite (pridruživanju potrošača servis) vrijednosti SAML prijave URL, URL izdavač (entitet ID) i odgovor URL-a za svoju aplikaciju, kao što je opisano [u nastavku](active-directory-saas-custom-apps.md). Ako obično sadrže te vrijednosti kao dio datoteke metapodataka SAML, zatim pošaljite koji kao i.

* Unesite kratak opis o konfiguriranju Azure AD kao davateljem identiteta u aplikaciji pomoću SAML 2.0. Ako aplikacija podržava konfiguriranje Azure AD kao davateljem identiteta putem samoposlužnog administratora portala, zatim provjerite je li vjerodajnice gore navedene obuhvaćaju mogućnost postaviti.

* Unesite informacije u nastavku:

> Naziv tvrtke:
> 
> Web-mjesto tvrtke:
> 
> Naziv aplikacije:
> 
> Opis aplikacije (256 znakova od dopuštenog):
> 
> Aplikacija web-mjesta (Informativna):
> 
> Aplikacija tehničku podršku web-mjesta ili kontakt:
> 
> URL adresa Sign-Up aplikacije kamo klijentima za prijavu i /or kupiti aplikaciju:
> 
> Odaberite do tri kategorije aplikacije navoditi u odjeljku (za dostupnih kategorija potražite u članku [Servisa Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Dodavanje aplikacije malu ikonu (PNG datoteci, 45px po 45px, jednobojnu pozadinu):
> 
> Dodavanje aplikacije velike ikone (PNG datoteci, 215px po 215px, jednobojnu pozadinu):
> 
> Dodavanje logotipa aplikacije (PNG datoteci, 150px po 122px, prozirnu boju pozadine):
