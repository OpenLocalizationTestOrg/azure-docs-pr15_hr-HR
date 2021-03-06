<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Brightspace po Desire2Learn | Microsoft Azure" 
    description="Saznajte kako koristiti Brightspace po Desire2Learn s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Praktični vodič: Azure Active Directory Integracija s Brightspace po Desire2Learn

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Brightspace po Desire2Learn.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Brightspace po Desire2Learn jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Brightspace po Desire2Learn će moći znak u aplikaciju na vašem Brightspace Desire2Learn web-mjesto tvrtke (znak servis davatelja pokrenut na) ili korištenjem [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Brightspace po Desire2Learn
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scenarij")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Omogućivanje integraciju aplikacija za Brightspace po Desire2Learn

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Brightspace po Desire2Learn.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Brightspace po Desire2Learn, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Brightspace po Desire2Learn**.

    ![Galerija Apllication] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Galerija Apllication")

7.  U oknu s rezultatima odaberite **Brightspace po Desire2Learn**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Brightspace po Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace po Desire2Learn")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Brightspace po Desire2Learn sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Brightspace po Desire2Learn** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Brightspace po Desire2Learn** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Konfiguriranje URL adresa Web App")

    1.  U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika da biste se prijavili u **Brightspace po Desire2Learn** (npr.: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Kliknite **Dalje**

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Brightspace po Desire2Learn** da biste preuzeli metapodatka, kliknite **Preuzmi metapodataka**i metapodatke spremili na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje datoteke preuzete metapodataka za vaše Brightspace od tima za podršku Desire2Learn.

    >[AZURE.NOTE] Vaše Brightspace od tima za podršku Desire2Learn mora konfigurirati stvarni SSO.
Dobit ćete obavijest kada SSO je omogućeno za vašu pretplatu.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Brightspace po Desire2Learn, oni mora biti dodijeljena u Brightspace po Desire2Learn.  
U slučaju Brightspace po Desire2Learn korisničke račune morati stvoriti tako da vaše Brightspace od tima za podršku Desire2Learn.

>[AZURE.NOTE] Možete koristiti druge Brightspace pomoću alata za stvaranje Desire2Learn korisnički račun ili API-ji nudi Brightspace po Desire2Learn Dodjela resursa za Azure Active Directory korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Da biste korisnicima dodijelili Brightspace po Desire2Learn, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Brightspace po Desire2Learn **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
