<properties
    pageTitle="B2C Azure Active Directory: Najčešća pitanja | Microsoft Azure"
    description="Najčešća pitanja o Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: najčešća pitanja

Ova stranica navedeni odgovori na najčešća pitanja o B2C Azure Active Directory (Azure AD). Zadrži traženje natrag ažuriranja.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Mogu li koristiti značajke Azure AD B2C postojećih, zaposlenika Azure AD klijentu?

Trenutno Azure AD B2C značajke ne može se uključiti u postojećem klijentu za Azure AD. Preporučujemo vam da stvorite zaseban klijenta za upotrebu Azure AD B2C značajke za upravljanje sustava koje korisnici.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Mogu li koristiti Azure AD B2C za davanje društvenih prijavu (Facebook i Google +) u sustavu Office 365?

Azure AD B2C nije moguće koristiti sa sustavom Office 365. Općenito govoreći, nije moguće koristiti za provjeru autentičnosti da biste SaaS aplikacija (Office 365, Salesforce, Workday itd.). Omogućuje upravljanje identiteta i pristup samo za korisničke okrenutom web i mobilne aplikacije, a nije primjenjivo na scenarije zaposlenika ili partnera.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Što su lokalni računi u Azure AD B2C? Kako se mogu razlikovati od računa tvrtke ili obrazovne ustanove u Azure AD?

U klijent za Azure AD svaki korisnik u klijentu (osim korisnici s postojećeg računa za Microsoft) se pomoću adrese e-pošte obrasca `<xyz>@<tenant domain>`, pri čemu `<tenant domain>` jedan je od provjerene domene u klijentu ili na početni `<...>.onmicrosoft.com` domene. Ove vrste računa je račun tvrtke ili obrazovne ustanove.

U klijent za Azure AD B2C Većina aplikacije želite da se prijavite pomoću bilo koje adrese e-pošte proizvoljne korisnik (na primjer, joe@comcast.net, bob@gmail.com, sarah@contoso.com, ili jim@live.com). Ove vrste računa je lokalni račun. Danas, također podržavamo proizvoljne korisnička imena (samo običan nizovi) kao lokalni računi (na primjer, joe, teo, sarah ili jim). Možete odabrati neku od tih dviju vrsta lokalni račun na servisu Azure AD B2C.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Koje davatelje usluga društvenih identiteta koji podržavaju sada? Koje namjeravate podržava u budućnosti?

Trenutačno podržavamo Facebook, Google +, LinkedIn i Amazon. Dodat ćemo podrška za drugih davatelja popularne društvenih identitet koji se temelji na zahtjev za klijenta.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Ću konfigurirati dosezi prikupite dodatne informacije o koje korisnici iz različitih davatelji društvene identiteta?

Ne, ali ta je značajka na našem vodič. Zadani dosezi koji se koristi za naše podržani skup davatelji identiteta društvenih su:

- Facebook: e-pošte
- Google +: e-pošte
- Microsoftov račun: openid profila e-pošte
- Amazon: profila
- LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Moje aplikacije mora li se izvoditi na Azure za suradnike Azure AD B2C?

Ne možete hostirati aplikacije bilo kojeg mjesta (u oblaku ili na lokalnim). Sve potrebne za interakciju s Azure AD B2C je mogućnost za slanje i primanje zahtjeva za HTTP-a na javno dostupnu krajnje točke.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Mogu imati više klijenata za Azure AD B2C. Kako upravljati ih na portalu Azure?

Svaki klijent Azure AD B2C ima vlastitu plohu značajke B2C portala za Azure. U odjeljku [Azure AD B2C: registrirati aplikacija](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) da biste saznali kako možete doći do određenog klijenta B2C značajke plohu portala za Azure. Prebacivanje između Azure AD B2C direktorija portala za Azure će zadržati značajke B2C plohu otvorite na većini preglednika.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Kako prilagoditi provjere e-pošte (sadržaja i "od:" polja) Azure AD B2C je poslao?

[Brendiranje značajka tvrtke](../active-directory/active-directory-add-company-branding.md) možete koristiti da biste prilagodili sadržaj provjere e-pošte. Konkretno, te dvije elemente e-pošte moguće je prilagoditi:

- **Natpis logotip**: prikazana dolje desno.
- **Boja pozadine**: na vrhu.

    ![Snimka zaslona s prilagođene provjere e-pošte](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Potpis e-pošte sadrži naziv klijentu B2C koju ste naveli prilikom stvaranja B2C klijentu. Možete promijeniti naziv pomoću ove upute:

- Prijavite se za na [Azure klasični portala](https://manage.windowsazure.com/) kao Administrator pretplate.
- Dođite do B2C klijentu.
- Kliknite karticu **Konfiguracija** .
- Promijenite **naziv** polja u odjeljku **Svojstva direktorija** .
- Kliknite **Spremi** pri dnu stranice.

Trenutno ne postoji način da biste promijenili u "od:" na e-pošte. Ako vas zanimaju u tu mogućnost i potpuno prilagodbu tijelo provjere e-pošte, glasati za značajku na [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails).

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Kako mogu li se pri migraciji Moje postojeće korisnička imena, lozinke i profili iz moje baze podataka za Azure AD B2C?

API Azure AD grafikona možete koristiti za pisanje alata za migraciju. [Uzorak grafikonu API](active-directory-b2c-devquickstarts-graph-dotnet.md) detalje potražite u članku. Ćemo vam ponuditi različite mogućnosti migracije i alata u novom alatu-tvorničke u budućnosti.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Pravilnik za lozinke koje se koristi za lokalni računi u Azure AD B2C?

Azure AD B2C pravila lozinke za lokalni računi temelji se na pravila za Azure AD. Azure AD B2C je prijave, registracije ili prijave i lozinku ponovno postavljanje koristi pravila snage "istaknuti" lozinke i ne ističe lozinke. Pročitajte [pravilnik za lozinke Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) više pojedinosti.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Mogu li koristiti Azure AD Connect će se migrirati potrošača identiteta koji se nalaze na moj lokalnog servisa Active Directory za Azure AD B2C?

Ne, Azure AD Connect nije namijenjena za rad s Azure AD B2C. Ćemo vam ponuditi različite mogućnosti migracije i alata u novom alatu-tvorničke u budućnosti.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C funkcionira sustavima kao što je Microsoft Dynamics CRM?

Trenutno ne. Integriranje te sustavima nalazi se na našem vodič.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Ne Azure AD B2C sa lokalni sustav SharePoint 2016 ili starijim verzijama?

Trenutno ne. Azure AD B2C nema podrške za SAML 1.1 tokena koje portalima i programi za e-trgovine utemeljena na potrebe lokalnog sustava SharePoint. Imajte na umu da Azure AD B2C nisu Predviđeni SharePoint vanjskog zajedničkog korištenja partnera scenarija; Umjesto toga pogledajte [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) .

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Trebam li koristiti Azure AD B2C ili B2B za upravljanje vanjskim identiteta?

Ovaj članak o [vanjskim identiteta](../active-directory/active-directory-b2b-compare-external-identities.md) da biste saznali više o primjeni odgovarajućih značajki scenariji za vanjske identitet.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Što izvješćivanje i nadzor značajke ne Azure AD B2C pružaju? Da su jednaki onima Azure AD Premium?

Ne, Azure AD B2C ne podržava isti skup izvješća kao Azure AD Premium. Azure AD B2C će biti otpustite osnovni izvješćivanje i nadzor API-ji uskoro.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Mogu li localize korisničko Sučelje stranica posluživanje sustavom Azure AD B2C? Koje jezike podržava?

Trenutno Azure AD B2C optimiziran je za engleski samo. Plan čim uvođenju značajke lokalizaciju.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Mogu li koristiti vlastitu URL-ovi na stranicama moju prijavu i prijavu koja su služila po Azure AD B2C? Ako, primjerice, mogu li promijeniti URL iz login.microsoftonline.com za login.contoso.com?

Trenutno ne. Ta je značajka na našem vodič. Imajte na umu da potvrđivanje domene na kartici **Domains** vaš klijent Azure portala za klasični će ne to učiniti.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Kako izbrisati Azure AD B2C klijentu?

Slijedite ove korake da biste izbrisali klijentu za Azure AD B2C:

- Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
- Dođite do blades **aplikacije**, **davatelji identiteta** i **sva pravila** i brisati sve stavke u svaku od njih.
- Sada se prijavite na [portal za Azure klasični](https://manage.windowsazure.com/) kao Administrator pretplate. (To je iste tvrtke ili obrazovne ustanove ili isti Microsoftov račun koji ste koristili za registraciju za Azure.)
- Dođite do servisa Active Directory nastavak na lijevoj strani, a zatim kliknite B2C klijentu.
- Kliknite karticu **korisnika** .
- Odaberite svakog korisnika u uključivanje (Isključi korisnika koji trenutno prijavljeni kao, odnosno pretplate Administrator). Kliknite **Izbriši** pri dnu stranice, a zatim kliknite **da** kada se to od vas zatraži.
- Kliknite karticu **aplikacije** .
- Odaberite **aplikacije koje je vlasnik Moja tvrtka** u polju **Prikaži** padajućem i kliknite kvačicu.
- Vidjet ćete aplikaciju naziva **b2c proširenja aplikacije** navedene. Kliknite **Izbriši** pri dnu stranice, a zatim kliknite **da** kada se to od vas zatraži.
- Ponovno otvorite proširenja za Active Directory, a zatim odaberite B2C klijentu.
- Kliknite **Izbriši** pri dnu stranice. Slijedite upute na zaslonu da biste dovršili postupak.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Mogu li dobiti Azure AD B2C kao dio paketa mobilnost za tvrtke?

Ne, Azure AD B2C je pay-as-you-go servisa Azure i nije dio paketa mobilnost za tvrtke.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Kako se prijaviti probleme s Azure AD B2C?

Potražite [datoteku podržava zahtjeva za Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="more-information"></a>Dodatne informacije

Možda želite pregledati trenutni [servisna ograničenja, ograničenja i ograničenjima](active-directory-b2c-limitations.md).
