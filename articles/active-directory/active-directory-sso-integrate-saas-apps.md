<properties
    pageTitle="Azure Active Directory jedinstvenu prijavu integrirane aplikacije SaaS |  Microsoft Azure"
    description="Omogućivanje jedan prijave za provjeru autentičnosti i korisnički pristup središnje upravljanje SaaS aplikacija Azure Active Directory za dodjelu resursa. Pregled uputa za integraciju Azure Active Directory SaaS aplikacije."
    services="active-directory"
      keywords="Azure AD integrirane aplikacije SaaS"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Azure Active Directory jedinstvenu prijavu integrirane aplikacije SaaS  

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-enterprise-apps-manage-sso.md)
- [Azure klasični portal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Da biste započeli postavljanje jedinstvene prijave za aplikaciju koja se unošenje u vašoj tvrtki ili ustanovi, koristite postojeći imenik servisa Azure Active Directory (Azure AD). Možete koristiti u imeniku Azure AD koje ste nabavili kroz Microsoft Azure, Office 365 ili Windows Intune. Ako imate dvije ili više od sljedećeg, potražite u članku [administriranje direktorija Azure AD](active-directory-administer.md) da biste odredili onaj za korištenje.

## <a name="authentication"></a>Provjera autentičnosti

Za aplikacije koje podržavaju WS Federacija SAML 2.0 ili povezivanje OpenID protokola, koristi Azure Active Directory potpisivanje certifikata uspostaviti odnose pouzdanosti. Dodatne informacije o tome potražite u članku [Upravljanje certifikati za vanjsko jedinstvenu prijavu](active-directory-sso-certs.md).

Za aplikacije koje podržavaju samo HTML utemeljenu na obrascima prijavu, Azure Active Directory koristi "lozinku vaulting" da biste uspostavili odnosi pouzdanost. To omogućuje korisnicima u tvrtki ili ustanovi i automatski se prijaviti u aplikaciju za SaaS po Azure AD pomoću podaci korisničkog računa iz aplikacije SaaS. Azure AD prikuplja i sigurno pohranjuje podaci korisničkog računa i povezane lozinku. Dodatne informacije potražite u članku [operacijskim sustavom jedinstvenu prijavu](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorizacija

Dodjela resursa računa omogućuje korisniku se ovlasti za korištenje programa kada se oni imaju autentičnost kroz jedinstvenu prijavu. Dodjeljivanje korisnika možete riješiti ručno ili u nekim slučajevima možete dodavati i uklanjati korisničke informacije iz aplikacije SaaS na temelju promjene izvršene u Azure Active Directory. Dodatne informacije o korištenju postojeće Azure AD poveznici za automatiziranog dodjele resursa, potražite u članku [Automated korisniku dodjele resursa i Poništi dodjelu resursa za SaaS aplikacije](active-directory-saas-app-provisioning.md).

U suprotnom, možete ručno dodavanje korisničkih podataka u aplikaciju programa, ili druga rješenja dodjele resursa koji su dostupni na tržištu.

## <a name="access"></a>Pristup

Azure AD nekoliko načina prilagoditi za implementaciju aplikacije krajnjim korisnicima u tvrtki ili ustanovi. Nije zaključana u određenom implementacije ni rješenje programa access. Možete koristiti [rješenja koja najbolje odgovara vašim potrebama](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Dodatne napomene za aplikacije već koristi

Postavljanje znak na za aplikaciju koja je vaša tvrtka ili ustanova već koristi je drugi proces iz postupak stvaranja nove račune za nove aplikacije. Postoji nekoliko Preliminarni koraci uključujući: mapiranja identitetima korisnika u aplikaciji za Azure AD identiteta i objašnjenje kako korisnici će se dogoditi prijava u aplikaciju kada je integriran.

> [AZURE.NOTE] Da biste postavili SSO za postojeću aplikaciju, morate imati prava globalni administrator u oba Azure AD i SaaS računala.

### <a name="mapping-user-accounts"></a>Mapiranje korisničkih računa

Identitet korisnika obično ima jedinstveni identifikator koji bi mogli adresu e-pošte ili korisnikovo Glavno ime (UPN). Morat ćete veze (karta) identitet aplikacije za svakog korisnika na njima Azure AD identitet. Nekoliko je načina za to postigli ovisno o tome kako obavezne provjere autentičnosti vašeg računala.

Dodatne informacije o mapiranju identiteti aplikacije s identitetima Azure AD potražite u članku [izdavanja u SAML token zahtjevima za prilagođavanje](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) i [Prilagodba mapiranja atributa za dodjelu resursa](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Objašnjenje prijava zadovoljstva korisnika

Kada integrirali SSO za aplikaciju koja se već koristi, važno je da biste shvatili da će utjecati korisničkog sučelja. Za sve aplikacije, korisnici će početi koristiti svoja uvjerenja Azure AD za prijavu. Također može biti da su različite portal za pristup morate koristiti aplikacije.

SSO za neke aplikacije koje se mogu izvršiti na aplikacije za prijavu u sučelju, ali ne i za druge aplikacije, korisnik će morati proći kroz središnje portal kao što su ([Moje aplikacije](http://myapps.microsoft.com) ili [Office 365](http://portal.office.com/myapps)) da biste se prijavili. Dodatne informacije o različitim vrstama SSO i svoja iskustva korisnika u [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

Drugi resurs koristan je *pristanak korisničkog Suppressing* u članku [razvojnim inženjerima Guiding](active-directory-applications-guiding-developers-for-lob-applications.md) .

## <a name="next-steps"></a>Daljnji koraci


SaaS aplikacija koju ste pronašli u galeriju aplikacija Azure AD predstavlja broj [vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md).

Ako aplikacija nije u galeriju aplikacija, možete ga [dodati u galeriju aplikacija za Azure AD kao prilagođenu aplikaciju](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Postoji mnogo detaljnije na svim te probleme u biblioteci Azure.com, počevši od [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Vidi također

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
