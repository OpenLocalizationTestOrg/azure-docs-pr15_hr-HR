<properties
    pageTitle="Uvjetno pristup za aplikacije koje se objavljuju s proxy poslužitelj za Azure AD aplikacije"
    description="Opisuje kako postaviti uvjetno pristup aplikacijama objavite moguće pristupiti daljinski pomoću Proxy aplikacije za Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Rad s programom access uvjetno

Možete konfigurirati pravila za pristup da biste dodijelili uvjetno pristup aplikacijama koje su objavljene pomoću Proxy aplikacije. To vam omogućuje da:

- Obavezan višestruka provjera autentičnosti po aplikacije
- Obavezan višestruka provjera autentičnosti samo kada korisnici nisu na poslu
- Onemogućite korisnicima pristupa aplikacije kada se ne nalaze na poslu

Ta pravila se mogu primijeniti na sve korisnike i grupe ili samo određenim korisnicima i grupama. Prema zadanim postavkama pravila primjenjuju se na sve korisnike koji imaju pristup aplikaciji. No mogu pravilo biti ograničene i korisnicima koji su članovi navedeni sigurnosne grupe.  

Pravila za pristup vrednuju se kad korisnik pristupa vanjskim aplikaciji koja koristi OAuth 2.0, povezivanje OpenID, SAML ili WS Federacija. Osim toga, pravila za pristup vrednuju se s OAuth 2.0 i povezivanje OpenID prilikom osvježavanja token se koristi za dohvaćanje token za pristup.

## <a name="conditional-access-prerequisites"></a>Preduvjeti za uvjetno programa access

- Pretplatu na Premium Azure Active Directory
- Pridruženi ili upravljanih Azure Active Directory klijenta
- Obavezan vanjskim korisnicima omogućiti višestruke provjere autentičnosti (MFA)  
    ![Konfiguriranje pravila za pristup - zahtijevaju višestruka provjera autentičnosti](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Konfiguriranje po aplikacije višestruka provjera autentičnosti
1. Prijavite se kao administrator na portalu za Azure klasični.
2. Idite na servisu Active Directory i odaberite direktoriju u kojem želite omogućiti Proxy aplikacije.
3. Kliknite **aplikacije** i pomaknite se prema dolje do odjeljka **Pravila za pristup** . U odjeljku pravila za pristup prikazuje se samo za aplikacije objavljeno korištenjem Proxy aplikacije koje koriste pridruženim provjere autentičnosti.
4. Omogućite pravilo tako da odaberete **Omogućiti pristup pravila** **na**.
5. Navedite korisnike i grupe na kojoj će se primjenjivati pravila. Pomoću gumba **Dodaj grupu** odaberite jednu ili više grupa na koji će se primjenjivati pravila programa access. U ovom dijaloškom okviru mogu se koristiti i da biste uklonili odabrane grupe.  Nakon odabira pravila da biste primijenili grupe, pravila za pristup samo provest će se za korisnike koji pripadaju nešto od navedenog sigurnosne grupe.  

  - Da biste isključili izričito sigurnosne grupe iz pravila, provjerite **osim** i navedite jednu ili više grupa. Korisnike koji su članovi grupe na popisu Except će se tražiti da biste izvršili višestruke provjere autentičnosti.  

  - Ako korisnik nije konfiguriran pomoću značajke višestruka provjera autentičnosti po korisniku, tu postavku će nadjačavaju pravila višestruke provjere autentičnosti za aplikaciju. To znači da korisnik koji je konfiguriran za višestruke provjere autentičnosti po korisniku će se zatražiti da izvođenje višestruka provjera autentičnosti čak i ako ih izuzeti iz aplikacije višestruka provjera autentičnosti pravila. Saznajte više o [višestruke provjere autentičnosti i postavke po korisniku](../multi-factor-authentication/multi-factor-authentication.md).

6. Odaberite pravilo access koju želite postaviti:
    - **Obavezan višestruke provjere autentičnosti**: korisnicima kojima Primjena pravila za pristup će se zatražiti da dovršeno višestruka provjera autentičnosti prije pristupanja aplikacije na koju se odnosi pravilo.
    - **Obavezan višestruka provjera autentičnosti kada nisu na poslu**: korisnici pokušaju pristupa aplikacije iz pouzdanog IP adrese nisu potrebni za izvođenje višestruke provjere autentičnosti. Na stranici Postavke višestruka provjera autentičnosti moguće je konfigurirati pouzdanih rasponi IP adresa.
    - **Blokiranje pristupa kada nisu na poslu**: korisnici pokušaju pristupa aplikacije nalazi izvan mreže vaše tvrtke nećete moći pristupiti aplikacije.


## <a name="configuring-mfa-for-federation-services"></a>Konfiguriranje MFA za federation services
Za vanjskim korisnicima, može se izvesti višestruke provjere autentičnosti (MFA) tako da Azure Active Directory ili s lokalnog poslužitelja za AD FS. Prema zadanim postavkama, MFA će se pojaviti na bilo kojoj stranici hostira tvrtka Azure Active Directory. Da biste konfigurirali MFA lokalnog, pokrenite Windows PowerShell i koristiti ovo svojstvo – SupportsMFA da biste postavili modul Azure AD.

Sljedeći primjer pokazuje kako omogućiti lokalnog MFA pomoću [cmdleta skup MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) na klijentu s domenom contoso.com:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Uz Postavljanje se oznaka instancu vanjskog klijentu za AD FS mora biti konfiguriran za izvršavanje višestruke provjere autentičnosti. Slijedite upute za [implementaciju sustava Microsoft Azure višestruka provjera autentičnosti lokalnog](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Vidi također

- [Rad s programima koji su svjesni zahtjevima](active-directory-application-proxy-claims-aware-apps.md)
- [Objavljivanje aplikacija pomoću Proxy aplikacije](active-directory-application-proxy-publish.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)
- [Objavljivanje aplikacije koje koriste naziv domene](active-directory-application-proxy-custom-domains.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
