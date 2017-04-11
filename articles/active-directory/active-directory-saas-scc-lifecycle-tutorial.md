<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s životni ciklus SCC | Microsoft Azure" 
    description="Saznajte kako koristiti SCC životni ciklus s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Praktični vodič: Azure Active Directory Integracija s SCC životni ciklus
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i SCC životni ciklus.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na SCC životni ciklus jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili životni ciklus SCC će moći znak u aplikaciju na vašem SCC životni ciklus web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za životni ciklus SCC
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenarij")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Omogućivanje integraciju aplikacija za životni ciklus SCC
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za životni ciklus SCC.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za životni ciklus SCC, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **SCC životni ciklus**.

    ![Galerija aplikacije] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Životni ciklus SCC**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Životni ciklus SCC] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "Životni ciklus SCC")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti životni ciklus SCC sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Životni ciklus SCC** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili SCC životni ciklus** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **URL na za prijavu** upišite URL koji se koristi korisnika da biste se prijavili vaše aplikacije životni ciklus SCC pomoću sljedećeg uzorka "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na SCC životni ciklus** kliknite **Preuzimanje metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Konfiguriranje jedinstvenu prijavu")

5.  Prosljeđivanje tu datoteku metapodataka službi za podršku za životni ciklus SCC.

    >[AZURE.NOTE]Jedinstvenu prijavu mora biti omogućen od tima za podršku SCC životni ciklus.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u SCC životni ciklus, oni mora biti dodijeljena u SCC životni ciklus.
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjele resursa za životni ciklus SCC.  
Kada se pokuša dodijeljenog korisnika da se prijavite u SCC životni ciklus, račun za životni ciklus SCC automatski se stvara po potrebi.

>[AZURE.NOTE]Možete koristiti bilo koji drugi SCC životni ciklus korisnički račun alate za stvaranje ili API-ji nudi životni ciklus SCC dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Da biste korisnicima dodijelite životni ciklus SCC, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Životni ciklus SCC **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).