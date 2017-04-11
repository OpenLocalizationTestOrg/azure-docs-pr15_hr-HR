<properties
   pageTitle="Vodič za Azure Active Directory Integracija s programima početak rada |  Microsoft Azure"
   description="Ovaj je članak dohvaćanje rada vodič za integraciju Azure Active Directory (AD) s lokalnim aplikacije i oblaka aplikacije."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Vodič za Azure Active Directory Integracija s programima početak rada
## <a name="overview"></a>Pregled
U ovoj se temi namijenjen da vam je vodič za integraciju aplikacije s Azure Active Directory (AD). Svaki odjeljak u nastavku sadrži kratak sažetak detaljnije temu tako da možete prepoznati dijelove vodiču za dohvaćanje rada su vam relevantno.  Slijedite veze na detaljnije dive na svaki predmet.

## <a name="before-you-begin-take-inventory"></a>Prije početka, poduzmite zalihama
Prije prelaska u u integriranje aplikacije s Azure AD važno je znati gdje se nalazili i mjesto na koje želite povezati.  Sljedeća pitanja su namijenjene razmislite o projektu Integracija Azure AD aplikacije.

### <a name="application-inventory"></a>Zalihe aplikacije
- Gdje su sve aplikacija? Tko je njihov vlasnik?
- Kakvu vrstu provjere autentičnosti zahtijeva aplikacija?
- Kome je potreban pristup s kojim aplikacijama?
- Želite li Implementirajte novu aplikaciju?
  - Će sastavljanje sesije i implementirati na instancu Azure računalnim?
  - Hoćete li koristiti onaj koji je dostupan u galeriji aplikacije Azure?

### <a name="user-and-group-inventory"></a>Zalihe korisnika i grupa
- Gdje ne nalaze korisničkih računa?
 - Lokalni Active Directory
 - Azure AD
 - U zasebnom aplikacije baze podataka da ste vlasnik
 - U unsanctioned aplikacijama
 - Sve navedeno
- Koje dozvole i dodjele uloga pojedinačni korisnici trenutno Imam? Je li potrebno pregledati njihov pristup ili jeste li korisnik pristup i uloge dodijeljene jesu li odgovarajuću sada?
- Su grupe već pokrenut lokalni Active Directory?
 - Kako se vaše grupe su organizirane?
 - Tko su članovi grupe?
 - Što dodjele dozvola/uloga grupe trenutno Imam?
- Hoćete li trebati da biste očistili baza podataka za korisnika/grupe prije integracije?  (To je vrlo važne pitanje. Smeća u smeća out.)

### <a name="access-management-inventory"></a>Pristup upravljanje zalihama
- Kako vam trenutno upravlja korisnički pristup aplikacijama? Koji moraju li promijeniti?  Imate smatra da drugi načini upravljanja access, kao što su s [RBAC](role-based-access-control-configure.md) , primjerice?
- Kome je potreban pristup čemu?

Možda nemate odgovore na sva ta pitanja unaprijed, ali to je u redu.  Ovaj vodič omogućuju odgovorite neke od tih pitanja, a zatim neki donesete odluke.

## <a name="prerequisites"></a>Preduvjeti
- Azure pretplata i u imeniku Azure Active Directory.  Ako već nemate Azure pretplatu, možete je isprobajte Azure besplatno za 30 dana. [Isprobajte!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Aplikacije za integraciju s Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Traženje unsanctioned oblaka aplikacije s otkrivanje aplikacije oblaka
Kao što je rečeno iznad mogu postojati aplikacije koje se još niste vašoj tvrtki ili ustanovi do sada upravlja.  U sklopu postupka zalihama, moguće je pronaći unsanctioned oblaka aplikacije. Potražite u članku [Pronalaženje unsanctioned oblaka aplikacije s otkrivanje aplikacije oblaka](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Vrste provjere autentičnosti
Svaka aplikacija može imati preduvjeti za različite provjeru autentičnosti. Azure AD potpisivanje certifikata može se koristiti s aplikacijama koje koriste SAML 2.0, WS Federacija ili OpenID protokoli za povezivanje kao i lozinka jedinstvenu prijavu. Dodatne informacije o aplikaciji vrste provjere autentičnosti za Azure AD potražite u članku [Upravljanje certifikata za vanjskog jedinstvenu prijavu u servisu Azure Active Directory](active-directory-sso-certs.md) i [lozinku na temelju znak](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Omogućivanje SSO s Proxy aplikacije za Azure AD
S Proxy za aplikaciju Microsoft Azure AD omogućuje pristup aplikacijama koje se nalazi unutar privatne mreže sigurno, s bilo kojeg mjesta i na svim uređajima. Nakon instalacije programa connector proxy aplikacije u okruženju sustava, to možete jednostavno konfigurirati Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Integriranje aplikacije s Azure AD
U sljedećim člancima navode različite načine aplikacije Integracija s Azure AD i neke uputiti.

- [Određivanje koji servisa Active Directory za korištenje](active-directory-administer.md)
- [Korištenje aplikacija u galeriji Azure aplikacije](active-directory-appssoaccess-whatis.md)
- [Integriranje SaaS aplikacije vodiči za popis](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Upravljanje pristupom aplikacije
U sljedećim člancima opisuju načini možete upravljati pristup aplikacijama kada se integrirati s Azure AD pomoću poveznika Azure AD i Azure AD.

- [Upravljanje pristupom pomoću Azure AD](active-directory-managing-access-to-apps.md)
- [Automatizacija s Azure AD poveznika](active-directory-saas-app-provisioning.md)
- [Dodjela korisnika u aplikaciju](active-directory-applications-guiding-developers-assigning-users.md)
- [Dodjela grupe u aplikaciju](active-directory-applications-guiding-developers-assigning-groups.md)
- [Zajedničko korištenje računa](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integriranje prilagođene aplikacije
Ako pišete, novu aplikaciju, a želite da bi razvojni inženjeri korištenje power Azure AD, potražite u članku [razvojnim inženjerima Guiding](active-directory-applications-guiding-developers-for-lob-applications.md).

Ako želite dodati prilagođene aplikacije u galeriju aplikacija Azure potražite u članku ["Premjesti vlastite aplikacije" s konfiguracijom Azure AD samoposlužnog SAML](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Vidi također

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
