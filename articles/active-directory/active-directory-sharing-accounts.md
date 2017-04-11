<properties
    pageTitle="Zajedničko korištenje putem Azure AD |  Microsoft Azure"
    description="U članku se opisuje kako Azure Active Directory omogućuje tvrtki ili ustanova te sigurno zajedničko korištenje račune za lokalni aplikacije i servise u oblaku potrošača."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Zajedničko korištenje računa servisu Azure AD

## <a name="overview"></a>Pregled
Ponekad tvrtkama ili ustanovama morate koristiti jedinstveno korisničko ime i lozinku za većem broju osoba. To se obično događa u dva slučaja:

- Prilikom pristupanja aplikacije koje su potrebne jedinstvene prijave i lozinku za svakog korisnika, hoće li lokalnog aplikacije ili potrošača cloud services (npr. računi društvenih mreža tvrtke).
- Prilikom stvaranja okruženja za više korisnika. Možda ćete jednu, lokalni račun koji višom ovlasti i može se koristiti za osnovni postavljanje, Administracija i oporavak aktivnosti (npr. na lokalni "globalni administrator" račun za Office 365) ili korijenski računa u Salesforce.

Najčešći te račune želite zajednički koristiti distribucija vjerodajnica (korisničko ime i lozinka) desno pojedincima ili pohranite na zajedničkom mjestu gdje više pouzdanih agenata im možete pristupiti.

Tradicionalni zajedničkog korištenja model sadrži nekoliko Nedostaci:

- Omogućivanje pristupa nove aplikacije potreban je distribuiranje vjerodajnice svima koji je potreban pristup.
- Sve zajedničke aplikacije možda ćete morati vlastiti Jedinstveni skup zajedničke vjerodajnice zahtijevate od korisnika da ne zaboravite više skupova vjerodajnice. Kada korisnici imaju Zapamti mnogo vjerodajnice, rizika povećava će spremali za opasan prakse. (npr. napisati lozinke).
- Ne može odrediti tko ima pristup aplikaciji.
- Ne može odrediti tko ima *pristupiti* aplikacija.
- Kada je potrebno da biste uklonili pristup aplikaciju, morate ažurirati vjerodajnice i ponovno ih distribucija svima koji je potreban pristup tom programu.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory račun zajedničko korištenje

Azure AD nudi novi pristup za korištenje zajedničke račune koji uklanja te Nedostaci.

Azure AD administrator konfigurira koje aplikacije korisnik može pristupiti tako da pomoću ploče programa Access, a zatim odaberete vrstu jedan najbolje za prijavu najprikladniji za tu aplikaciju. Jedan od te vrste *operacijskim sustavom jedne-prijava*omogućuje Azure AD poslužiti kao vrstu "broker" tijekom postupka prijave za tu aplikaciju.

Korisnicima se prijaviti u jednom pomoću računa tvrtke ili ustanove. To je isti račun koje redovito koristite da biste pristupili svoje radne površine i e-pošte. Možete otkriti i pristup samo aplikacije koje su dodijeljene. Zajedničke računa ovaj popis aplikacija mogu sadržavati bilo koji broj zajedničke vjerodajnice. Krajnji korisnik nije potrebno Imajte na umu i zapišite različite račune koje se koristi.

Zajedničke računa ne samo povećati nadzor i poboljšanja upotrebljivosti, oni i poboljšanje sigurnosti. Korisnici s dozvolama za korištenje vjerodajnice ne vidite zajedničke lozinku, ali umjesto dobivaju dozvole da biste koristili lozinku kao dio sustava tijek orchestrated provjeru autentičnosti. Nadalje, s nekim aplikacijama SSO lozinku imate mogućnost da bi Azure AD povremeno dinamične (Ažuriraj) lozinku pomoću velikih, složenih lozinke povećava sigurnost računa. Administrator može jednostavno dodijeliti ili opozvati pristup aplikaciji i i znate tko ima pristup računu i tko je pristupiti u prošlosti.

Azure AD podržava zajedničke račune za sve Enterprise mobilnost paket (EMS), Premium ili osnovni licencirani korisnici preko svih vrsta lozinke jednog prijavite se u aplikacijama. Možete omogućiti zajedničko korištenje za račune za bilo koju od tisuće unaprijed integrirane aplikacije u galeriju aplikacija, a možete dodati vlastitu provjere autentičnosti lozinke aplikacijom [prilagođene SSO aplikacije](active-directory-sso-integrate-saas-apps.md).

Azure AD značajke koje omogućuju zajedničko korištenje računa obuhvaćaju sljedeće:

- [Lozinka jedinstvenu prijavu](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Lozinka jedan prijave agent
- [E-pošta](active-directory-accessmanagement-self-service-group-management.md)
- Lozinka prilagođene aplikacije
- [Korištenje aplikacije nadzorne ploče i izvješća](active-directory-passwords-get-insights.md)
- Portalima krajnji korisnik programa access
- [Proxy aplikacije](active-directory-application-proxy-get-started.md)
- [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Zajedničko korištenje računa
Da biste koristili Azure AD za zajedničko korištenje računa, morat ćete:

- Dodavanje aplikacije [galeriju aplikacija](https://azure.microsoft.com/marketplace/active-directory/) ili [prilagođene aplikacije](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Konfiguriranje aplikacija za lozinke jednog prijavu (SSO)
- [Grupe na temelju dodjela](active-directory-accessmanagement-group-saasapps.md) koristiti, a zatim odaberite mogućnost da biste unijeli zajedničke vjerodajnica
- Neobavezno: u nekim ćete aplikacijama, kao što su Facebook, Twitter i LinkedIn, možete omogućiti mogućnost za [snimljene fotografije pokazivač Azure AD automatiziranog lozinke](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Možete promijeniti račun zajedničke sigurnije s višestruke provjere autentičnosti (MFA) (Dodatne informacije o [zaštiti aplikacije s Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) i možete delegirati ograničenu mogućnost upravljanja tko ima pristup aplikacije pomoću [Azure AD samoposlužnog](active-directory-accessmanagement-self-service-group-management.md) upravljanje grupe.

## <a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Zaštita aplikacije pomoću uvjetnog programa access](active-directory-conditional-access.md)
- [Upravljanje SSAA samostalno grupe](active-directory-accessmanagement-self-service-group-management.md)
