<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Qualtrics | Microsoft Azure" 
    description="Saznajte kako koristiti Qualtrics s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Praktični vodič: Azure Active Directory Integracija s Qualtrics
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Qualtrics.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Qualtrics jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Qualtrics će moći znak u aplikaciju na vašem Qualtrics web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Qualtrics
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenarij")
##<a name="enabling-the-application-integration-for-qualtrics"></a>Omogućivanje integraciju aplikacija za Qualtrics
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Qualtrics.

###<a name="to-enable-the-application-integration-for-qualtrics-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Qualtrics, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Qualtrics**.

    ![Galerija aplikacije] (./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Qualtrics**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Qualtrics] (./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Qualtrics sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Qualtrics** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Qualtrics** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Qualtrics na URL za prijavu** upišite URL (npr.: "*https://ssotest2ut1.qualtrics.com*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Qualtrics** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje u metadatafile Qualtrics timu za podršku.

    >[AZURE.NOTE]Konfiguracija jedan prijave mora izvoditi Qualtrics timu za podršku. Dobit ćete obavijest čim se dovrši konfiguraciju.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Qualtrics.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Qualtrics pomoću ploče programa access, Qualtrics provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću Qualtrics.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-qualtrics-perform-the-following-steps"></a>Da biste korisnicima dodijelili Qualtrics, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Qualtrics **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).