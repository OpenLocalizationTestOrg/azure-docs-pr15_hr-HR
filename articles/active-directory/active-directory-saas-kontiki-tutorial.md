<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Kontiki | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Kontiki da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Praktični vodič: Azure Active Directory Integracija s Kontiki
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Kontiki.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Kontiki jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Kontiki će moći znak u aplikaciju na vašem Kontiki web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Kontiki
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Scenarij")
##<a name="enabling-the-application-integration-for-kontiki"></a>Omogućivanje integraciju aplikacija za Kontiki
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Kontiki.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Kontiki, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Kontiki**.

    ![Galerija aplikacije] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Kontiki**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Kontiki sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Kontiki** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Konfiguriranje jedan Proba")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Kontiki** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Konfiguriranje jedan Proba")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Kontiki na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili Kontiki (npr.: "*https://company.mc.eval.kontiki.com/*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Kontiki** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka na vašem računalu.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Konfiguriranje jedan Proba")

5.  Slanje u metadatafile Kontiki timu za podršku.

    >[AZURE.NOTE] Konfiguracija jedan prijave mora izvoditi Kontiki timu za podršku. Dobit ćete obavijest čim se dovrši konfiguraciju.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Konfiguriranje jedan Proba")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Kontiki.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Kontiki pomoću ploče programa access, Kontiki provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću Kontiki.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Da biste korisnicima dodijelili Kontiki, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Kontiki **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).