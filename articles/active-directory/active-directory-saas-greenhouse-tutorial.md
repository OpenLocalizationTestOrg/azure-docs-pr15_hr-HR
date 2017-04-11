<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Greenhouse | Microsoft Azure" 
    description="Saznajte kako koristiti Greenhouse s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Praktični vodič: Azure Active Directory Integracija s Greenhouse
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Greenhouse.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Greenhouse jedan prijave pretplatu
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Greenhouse će moći znak u aplikaciju na vašem Greenhouse web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Greenhouse
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scenarij")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Omogućivanje integraciju aplikacija za Greenhouse
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Greenhouse.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Greenhouse, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **greenhouse**.

    ![Galerija aplikacije] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Greenhouse**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Greenhouse] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Greenhouse")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Greenhouse sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Greenhouse** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Greenhouse** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Prijavite se na URL** unesite URL pomoću sljedećeg uzorka "*https://company.greenhouse.io*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Greenhouse** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Konfiguriranje jedinstvenu prijavu")

5.  Prosljeđivanje tu datoteku metapodataka službi za podršku Greenhouse.

    >[AZURE.NOTE] Jedinstvenu prijavu mora biti omogućen od tima za podršku Greenhouse.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Greenhouse, oni mora biti dodijeljena u Greenhouse.  
U slučaju Greenhouse, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Greenhouse** kao administrator.

2.  Na izborniku na vrhu kliknite **Konfiguriraj**, a zatim **korisnika**.

    ![Korisnici] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Korisnici")

3.  Kliknite **novi korisnici**.

    ![Novi korisnik] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Novi korisnik")

4.  U odjeljku **Dodavanje novog korisnika** poduzeti sljedeće korake:

    ![Dodavanje novog korisnika] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Dodavanje novog korisnika")

    1.  U tekstni okvir **Enter e-pošte korisnika** upišite adresu e-pošte valjani Azure Active Directory račun koji želite dodjele resursa.
    2.  Kliknite **Spremi**.
        
        >[AZURE.NOTE] Račun za Azure Active Directory slike primit će poruku e-pošte uključujući vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Greenhouse korisnički račun alate za stvaranje ili API-ji nudi Greenhouse dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Da biste korisnicima dodijelili Greenhouse, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Greenhouse **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).