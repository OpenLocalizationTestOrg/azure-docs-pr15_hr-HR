<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s SimpleNexus | Microsoft Azure" 
    description="Saznajte kako koristiti SimpleNexus s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Praktični vodič: Azure Active Directory Integracija s SimpleNexus
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i SimpleNexus.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na SimpleNexus jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili SimpleNexus će moći znak u aplikaciju na vašem SimpleNexus web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za SimpleNexus
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scenarij")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Omogućivanje integraciju aplikacija za SimpleNexus
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za SimpleNexus.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za SimpleNexus, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **jednostavne nexus**.

    ![Galerija aplikacije] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **SimpleNexus**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Jednostavni Nexus] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Jednostavni Nexus")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti SimpleNexus sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **SimpleNexus** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili SimpleNexus** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **SimpleNexus URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://simplenexus.com/CompanyName\_prijava*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na SimpleNexus** kliknite **Preuzmi metapodataka**i prosljeđivanje datoteku metapodataka na SimpleNexus službi za podršku.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Konfiguriranje jedinstvenu prijavu")

    >[AZURE.NOTE] Jedinstvenu prijavu mora biti omogućen od tima za podršku SimpleNexus.

5.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u SimpleNexus, oni mora biti dodijeljena u SimpleNexus.  
U slučaju SimpleNexus, dodjeljivanje je ručno zadatak obavlja administrator klijenta.

>[AZURE.NOTE] Možete koristiti bilo koji drugi SimpleNexus korisnički račun alate za stvaranje ili API-ji nudi SimpleNexus dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Da biste korisnicima dodijelili SimpleNexus, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **SimpleNexus **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).