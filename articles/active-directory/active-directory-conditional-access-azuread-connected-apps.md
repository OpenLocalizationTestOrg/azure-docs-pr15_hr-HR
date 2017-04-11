<properties
    pageTitle="Azure uvjetno pristup aplikacijama SaaS | Microsoft Azure"
    description="Uvjetno pristupa u Azure AD omogućuje konfiguriranje pravila za pristup po aplikacije višestruke provjere autentičnosti i mogućnost da biste blokirali pristup za korisnike ne pouzdani mreže. "
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Uvod u Azure Active Directory uvjetno programa Access

Azure Active Directory uvjetno pristupa za [SaaS](https://azure.microsoft.com/overview/what-is-saas/) aplikacije i Azure AD povezani aplikacija omogućuje konfiguriranje uvjetno pristupa na temelju grupa, mjesto i osjetljivost aplikacije. 

Pomoću uvjetnog pristupa na temelju osjetljivost aplikacije, možete postaviti pravila za pristup višestruke provjere autentičnosti (MFA) po aplikacije. MFA svaku aplikaciju omogućuje blokiranje pristupa za korisnike koji nisu na mreži kao pouzdanih pouzdanih. Pravila MFA možete primijeniti na sve korisnike koji su vam dodijeljeni aplikaciji ili samo za korisnike u navedenom sigurnosne grupe.  Korisnici možda izuzeti iz obavezne MFA ako su pristup aplikacije iz IP adresu koja je u mrežu na tvrtke ili ustanove.

Ove mogućnosti će biti dostupna korisnicima koji su kupili licencu za Azure Active Directory Premium.

## <a name="scenario-prerequisites"></a>Scenarij preduvjeti
* Licence za Premium Azure Active Directory

* Pridruženi ili upravljanih klijentu Azure Active Directory

* Pridruženi klijenata potrebno je omogućiti višestruke provjere autentičnosti.

## <a name="configure-per-application-access-rules"></a>Konfiguriranje pravila-aplikacijama programa access

U ovom se odjeljku opisuje kako konfigurirati pravila-aplikacijama programa access.

1. Prijavite se na portal za Azure klasični pomoću računa koji je globalni administrator za Azure AD.
2. U lijevom oknu odaberite **Servisa Active Directory**.
3. Na kartici direktorija odaberite direktorija.
4. Odaberite karticu **aplikacije** .
5. Odaberite aplikaciju koju pravilo postavit će se.
6. Odaberite karticu **Konfiguracija** .
7. Pomaknite se prema dolje do odjeljka pravila programa access. Odaberite željeni pristup pravilo.
8. Navedite pravilo će se primijeniti na korisnike.
9. Da biste omogućili pravilo tako da odaberete **omogućeno na**.

##<a name="understanding-access-rules"></a>Razumijevanje pravila za pristup

U ovom se odjeljku daje detaljan opis pravila za pristup podržane u programu Access uvjetno aplikacije Azure.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Određivanje korisnika pravila za pristup primijeniti

Prema zadanim postavkama pravila primjenjuju se na sve korisnike koji imaju pristup aplikaciji. Međutim, pravila možete ograničiti korisnicima koji su navedeni sigurnosne grupe. Gumb **Dodaj grupu** koristi se za odaberite jednu ili više grupa u dijaloškom okviru Odabir grupe koji će se primjenjivati pravila programa access. U ovom dijaloškom okviru mogu se koristiti i da biste uklonili odabrane grupe. Nakon odabira pravila da biste primijenili grupe, pravila za pristup samo provest će se za korisnike koji pripadaju nešto od navedenog sigurnosne grupe.

Sigurnosne grupe mogu također se izričito izuzeti iz pravila odabirom mogućnosti **osim** i određivanjem jednu ili više grupa. Korisnici koji su članovi grupe na popisu **osim** neće biti predmet zahtjeva višestruka provjera autentičnosti čak i ako su član grupe kojoj pripada pravila za pristup.
Pravilo za pristup prikazano u nastavku potrebno svi korisnici u grupu upravitelja koristiti višestruke provjere autentičnosti prilikom pristupanja aplikacije.

![Postavljanje pravila za uvjetno pristup s MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Pravila za uvjetno pristup s MFA
Ako korisnik konfiguriran pomoću značajke višestruka provjera autentičnosti po korisniku, ova postavka korisniku će se kombinirati s pravilima višestruke provjere autentičnosti za aplikaciju. To znači da korisnik koji je konfiguriran za višestruke provjere autentičnosti po korisniku će se tražiti da biste izvršili višestruke provjere autentičnosti, čak i ako ih izuzeti iz pravila višestruke provjere autentičnosti za aplikaciju. Dodatne informacije o postavkama višestruke provjere autentičnosti i po korisniku.

### <a name="access-rule-options"></a>Mogućnosti programa Access pravila
Podržane su sljedeće mogućnosti:

* **Obavezan višestruke provjere autentičnosti**: korisnicima kojima se pristupa pravila primjenjuju na, će se zatražiti da dovršeno višestruka provjera autentičnosti prije pristupanja aplikacije koja se pravilo primjenjuje.

* **Obavezan višestruka provjera autentičnosti kada nisu na poslu**: korisnik koji dolazi iz pouzdanog IP adrese nisu potrebni za izvođenje višestruke provjere autentičnosti. Na stranici Postavke višestruka provjera autentičnosti moguće je konfigurirati pouzdano rasponi IP adresa.

* **Blokiranje pristupa kada nisu na poslu**: bit će blokirane korisnika koji ne dolaze iz pouzdanih IP adresa. Na stranici Postavke višestruka provjera autentičnosti moguće je konfigurirati pouzdanih rasponi IP adresa.

### <a name="setting-rule-status"></a>Postavljanje statusa pravila
Pristup pravilo stanja omogućuje uključivanje pravila ili isključivanje. Kad su pravila za pristup isključivanje, obavezne višestruka provjera autentičnosti ne nameće.

### <a name="access-rule-evaluation"></a>Primjenu pravila programa Access

Pravila za pristup vrednuju se kad korisnik pristupa vanjskim aplikaciji koja koristi OAuth 2.0, povezivanje OpenID, SAML ili WS Federacija. Osim toga, pravila za pristup vrednuju se kada OAuth 2.0 i OpenID povezati pomoću tokena osvježavanja dobiti token za pristup. Ako procjene pravila ne uspije kada se koristi token osvježavanja, vratit će se pogreška **invalid_grant** , to znači da korisnik mora ponovno autentičnost klijentu.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Konfiguriranje servisa za vanjski pristup omogućuje višestruka provjera autentičnosti

Za vanjskim korisnicima, može se izvesti MFA Azure Active Directory ili s lokalnog poslužitelja za AD FS.

Prema zadanim postavkama MFA pojavit će se na stranici hostira tvrtka Azure Active Directory. Da biste konfigurirali MFA lokalnog, svojstvo **– SupportsMFA** mora biti postavljeno na **true** servisa Azure Active Directory, pomoću modula Azure AD za Windows PowerShell.

Sljedeći primjer pokazuje kako omogućiti lokalnog MFA pomoću [cmdleta postavljanje MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) na klijentu s domenom contoso.com:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Uz Postavljanje se oznaka instancu vanjskog klijentu za AD FS mora biti konfiguriran za izvršavanje višestruke provjere autentičnosti. Slijedite upute za [implementaciju Azure višestruka provjera autentičnosti lokalnog](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Povezani članci

- [Zaštita pristup sustavu Office 365 i drugih aplikacija s Azure Active Directory](active-directory-conditional-access.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
