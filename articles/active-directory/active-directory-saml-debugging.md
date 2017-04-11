<properties 
    pageTitle="Upute za ispravljanje pogrešaka utemeljene na SAML jedinstvenu prijavu aplikacijama servisa Azure Active Directory | Microsoft Azure" 
    description="Upute za ispravljanje pogrešaka utemeljene na SAML jedinstvenu prijavu aplikacijama servisa Azure Active Directory " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Upute za ispravljanje pogrešaka utemeljene na SAML jedinstvenu prijavu aplikacijama servisa Azure Active Directory

Kada ispravljanje pogrešaka aplikacije utemeljene na SAML integracije, korisno je da biste koristili alat kao što su [Fiddler](http://www.telerik.com/fiddler) da biste vidjeli zahtjev SAML, odgovor SAML i stvarne SAML tokena koje izdaje aplikaciji. Tako da Provjera SAML token može osigurati da sve potrebne atribute, korisničko ime koje u predmetu SAML i izdavatelj URI uskoro prema očekivanjima.

![][1]

Odgovor Azure AD koja sadrži SAML token obično je onaj koji se pojavljuje kada je 302 HTTP preusmjeravanje iz https://login.windows.net i šalju se u konfigurirano **Odgovor URL-a** aplikacije. 
 
Tokena SAML možete vidjeti tako da odaberete redak, a zatim odaberete u **Provjera > WebForms** kartica u desnom oknu. Iz nje, desnom tipkom miša kliknite **SAMLResponse** vrijednost, a zatim odaberite **Pošalji TextWizard**. Zatim odaberite **Base64 iz** izbornika **transformacija** dekodiranje token i pogledajte njegov sadržaj.
 
**Napomena**: da biste vidjeli sadržaj HTTP zahtjev, Fiddler može od vas zatražiti da biste konfigurirali dešifriranje HTTPS prometa koji morat ćete učiniti.

## <a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Konfiguriranje jedinstvene prijave za aplikacije koje nisu u galeriji aplikacije Azure Active Directory](active-directory-saas-custom-apps.md)
- [Kako prilagoditi zahtjevima izdavanja u tokena SAML za unaprijed integrirane aplikacije](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
