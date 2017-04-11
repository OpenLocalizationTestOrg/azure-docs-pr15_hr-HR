<properties 
    pageTitle="Praktični vodič: Integracija Azure Active Directory pomoću alata za izradu e | Microsoft Azure" 
    description="Saznajte kako pomoću alata za izradu e s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Praktični vodič: Integracija Azure Active Directory pomoću alata za izradu e
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i alata za izradu e.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijent za e-sastavljača
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili e Sastavljač će moći znak u aplikaciju na vaše e Sastavljač web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za e-sastavljača
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scenarij")
##<a name="enabling-the-application-integration-for-e-builder"></a>Omogućivanje integraciju aplikacija za e-sastavljača
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za e-Sastavljač.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za e-Sastavljač, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **E Sastavljač**.

    ![Galerija aplikacije] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **E-Sastavljač**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![E-sastavljača] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "E-sastavljača")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti e-Sastavljač sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici Integracija **E Sastavljač** aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili e-Sastavljač** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **alata za izradu e prijavite se u URL** unesite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>.e Builder.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Konfiguriranje URL adresa web app")

4.  Na na **Konfiguriraj jedinstvenu prijavu na e-Sastavljač** stranice da biste preuzeli metapodaci, kliknite **Preuzimanje metapodataka**, a zatim podatke Lokalno pohrani kao **c:\\e BuilderMetaData.xml**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Konfiguriranje jedinstvenu prijavu")

5.  Proslijedi tu datoteku metapodataka za e-Sastavljač službi za podršku. Tim potrebama podršku konfigurira jedinstvenu prijavu umjesto vas.

6.  Odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje e Sastavljač.  
Kada se pokuša dodijeljenog korisnika da se prijavite u e-Sastavljač pomoću ploče programa access, e Sastavljač provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću alata za izradu e.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Da biste korisnicima dodijelili e Sastavljač, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici Integracija **E Sastavljač **aplikacije, kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
