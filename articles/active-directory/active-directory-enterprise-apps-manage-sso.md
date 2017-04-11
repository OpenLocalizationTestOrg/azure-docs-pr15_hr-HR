<properties
    pageTitle="Upravljanje prijave za enterprise aplikacije u pretpregledu Azure Active Directory s jednom | Microsoft Azure"
    description="Informirajte se o upravljanju znak na za enterprise aplikacije pomoću Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Pregled: Upravljanje jedinstvene prijave za enterprise aplikacije na novom portalu Azure

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-enterprise-apps-manage-sso.md)
- [Azure klasični portal](active-directory-sso-integrate-saas-apps.md)

U ovom se članku opisuje kako koristiti [Azure portal](https://portal.azure.com) za upravljanje jedan prijave postavkama za programe, osobito one koji su dodani u [galeriju aplikacija Azure Active Directory (Azure AD)](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Sučelje za upravljanje Azure AD za jedinstvenu prijavu trenutno javno pretpregled, a u ovom se članku opisuju nove značajke, kao i nekoliko privremene ograničenja koja će se na mjestu samo tijekom razdoblja za pretpregled. [Novosti u pretpregledu?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Pronalaženje aplikacija na novom portalu

Od 2016 rujan sve aplikacije podešena za jednu prijave u imeniku administrator directory pomoću [galerije aplikacije Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) unutar [Azure klasični portal](https://manage.windowsazure.com), sada može pregledavati i upravlja na portalu za Azure.

Te aplikacije pronaći ćete u odjeljku **Aplikacijama** Azure portala vezu na koje možete pronaći u popisu **Više servisa** na [portalu](https://portal.azure.com). Enterprise aplikacije sadrži aplikacije koje su u uveden i koriste korisnicima unutar tvrtke ili ustanove.

![Plohu aplikacijama][1]

Odaberite **sve aplikacije** da biste pogledali popis svih aplikacija koje su konfigurirana, uključujući aplikacije koje su dodane iz galerije. Odabir aplikacije učitava plohu resursa za tu aplikaciju, gdje se izvješća mogu pregledavati za tu aplikaciju i upravlja različite postavke.

Da biste upravljali postavkama jedan prijave, odaberite **jedinstvenu prijavu**.

![Plohu resursa aplikacije][2]


##<a name="single-sign-on-modes"></a>Jedan načina za prijavu

Plohu za **jedinstvenu prijavu** započinje s izbornikom za **način rada** , koji omogućuje jedan način za prijavu mora biti konfigurirana tako. Dostupne mogućnosti obuhvaćaju sljedeće:

* **Utemeljene na SAML prijava** - Ova mogućnost je dostupno samo ako je aplikacija podržava punu pridruženim jedinstvenu prijavu s Azure Active Directory pomoću protokola SAML 2.0.

* **Utemeljen na lozinku za prijavu** – Ova mogućnost dostupna ako Azure AD podržava ispunjavanja za ovu aplikaciju obrasca za lozinku.

* **Povezani prijava** - prijašnji "Postojeći jedinstvenu prijavu", tu mogućnost administratorima omogućuje da biste postavili vezu na ovu aplikaciju u svoje korisničke Azure AD teksta ili Office 365 aplikacije pokretač.

Dodatne informacije o ovih načina rada potražite u članku [kako jedan prijave pomoću servisa Azure Active Directory tvrtke](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>Utemeljene na SAML prijava

Mogućnost **utemeljene na SAML prijave** prikazuje plohu koji je podijeljen u četiri sekcije:

###<a name="domains-and-urls"></a>Domene i URL-ovi
Ovo je gdje se sve pojedinosti o domeni i URL-ove aplikacije dodaju se Azure AD direktorija. Sve unose potrebne da bi jedan radni za prijavu aplikacija prikazuju se izravno na zaslonu dok sve unose neobavezni moguće je prikazati tako da odaberete potvrdni okvir **Pokaži napredne postavke URL-a** . Čitavog popisa podržanih unosa obuhvaća:

* **Prijavite se na URL** – gdje dolazi korisnika da se prijavite u ovoj aplikaciji. Ako aplikacija je konfiguriran za izvođenje servisa davatelja pokrenut znak na, zatim kada korisnik vodi na ovom URL-u, davatelj usluga ne potrebne preusmjeravanje za Azure AD za provjeru autentičnosti i Prijava korisnika. Ako polje se popunjava, pa Azure AD koristi ovaj URL za pokretanje programa iz sustava Office 365 i Azure AD pristup ploča. Ako je ispušten ovo polje, a zatim Azure AD umjesto toga izvodi davatelja identiteta-pokrenuti prijaviti prilikom pokretanja aplikacije iz sustava Office 365, na ploči za Azure AD Access ili Azure AD jedan URL za prijavu.

* **Identifikator** - ova URI treba identificirati samo aplikacija za konfiguriran koje jednom prijava. To je vrijednost koja Azure AD šalje natrag aplikacije kao parametar publike tokena SAML i aplikacije očekuje da biste provjerili je. Ta vrijednost pojavit će se kao ID entitet u bilo kojem metapodacima SAML nudi aplikacije

* **Odgovori URL** - odgovor je URL gdje se aplikacija očekuje prima SAML token. To se naziva URL pridruživanju potrošača servisa (ACS). Nakon što ih unesena, kliknite Dalje da biste prešli na sljedećem zaslonu. Ovaj zaslon daje informacije o što potrebno je konfigurirati na strani aplikacije da biste omogućili da biste prihvatili SAML tokena iz Azure AD.

* **Stanje prijenosa** - stanje prijenosa nije neobavezan parametar koji mogu pomoći utvrditi aplikacije gdje želite preusmjeriti korisnika po dovršetku provjere autentičnosti. Obično je vrijednost valjani URL pri aplikacije, ali neke aplikacije koriste to polje različito (pogledajte aplikacije znak u dokumentaciji). Mogućnost da biste postavili stanje prijenosa je nova značajka koja je jedinstvena na novom portalu Azure.

###<a name="user-attributes"></a>Korisničke atribute
Ovo je gdje možete administratori prikaz i uređivanje atributa koji su poslani token SAML koji Azure AD problemi s aplikacijom svaki put kada korisnici prijavite se u.

Za prvo izdanje pretpregled atribut samo koji se može uređivati podržano je atribut **Identifikator korisnika** . Vrijednost toga atributa jest polje u Azure AD jedinstveno označava svaki korisnik u aplikaciji. Na primjer, ako aplikacija je uveden Pomoć "adresu e-pošte" kao korisničko ime i Jedinstveni identifikator, zatim vrijednost će se postaviti u polje "user.mail" u Azure AD.

Uređivanje atribute će biti podržane u sljedećim pretpregled.

###<a name="saml-signing-certificate"></a>Certifikat za potpisivanje SAML
U ovom se odjeljku prikazuje detalje certifikata koji koristi Azure AD za potpisivanje SAML tokena koje izdaje aplikaciju potvrđuje za svakog korisnika. Omogućuje svojstva trenutne potvrde da pregledavaju, uključujući datum isteka.

Mogućnost prelaska certifikat i upravljanje dodatne certifikat mogućnosti će biti podržane u narednih pretpregled izdanja. Imajte na umu da puni upravljanje potvrde i dalje može izvršiti [Azure klasični portal](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Konfiguracija aplikacije
Konačni će se odjeljku nalaze dokumentaciju i/ili kontrole koje su potrebne za konfiguriranje same aplikacije da biste upotrijebili Azure Active Directory davateljem identiteta.

Na izborniku potpaletama **Konfiguriranje aplikacije** sadrži nove kratka, ugrađeni upute za konfiguriranje aplikacije. To je drugi nova značajka jedinstvena na novom portalu Azure.

> [AZURE.NOTE] Primjeru ugrađenu dokumentacije potražite u članku Salesforce.com aplikacije. Dodatne aplikacije u dokumentaciji se neprestano dodaje tijekom pretpregleda.

![Ugrađeni dokumenti][3]

##<a name="password-based-sign-on"></a>Utemeljen na lozinku za prijavu
Ako je podržana za aplikaciju, odaberete SSO način-poštu, a zatim trenutačno odaberete **Spremanje** konfigurira ga ne operacijskim sustavom SSO. Dodatne informacije o implementaciji SSO-poštu potražite u članku [kako jedan prijave pomoću servisa Azure Active Directory tvrtke](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Utemeljen na lozinku za prijavu][4]


##<a name="linked-sign-on"></a>Povezane prijava
Ako je podržana za aplikaciju, odaberete povezane način SSO omogućuje unesite URL koji želite da se ploča za Azure AD Access ili Office 365 za preusmjeravanje kad korisnici kliknu na ovu aplikaciju. Dodatne informacije o povezane SSO (prijašnjeg "postojeće SSO") potražite u članku [kako jedan prijave pomoću servisa Azure Active Directory tvrtke](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Povezane prijave][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
