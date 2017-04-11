<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Mindflash | Microsoft Azure" 
    description="Saznajte kako koristiti Mindflash s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Praktični vodič: Azure Active Directory Integracija s Mindflash
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Mindflash.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Mindflash jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Mindflash će moći znak u aplikaciju na vašem Mindflash web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Mindflash
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Scenarij")
##<a name="enabling-the-application-integration-for-mindflash"></a>Omogućivanje integraciju aplikacija za Mindflash
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Mindflash.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Mindflash, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Mindflash**.

    ![Galerija aplikacije] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Mindflash**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Mindflash sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Mindflash** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Mindflash** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Prijavite se na URL** unesite URL pomoću sljedećeg uzorka "*http://company.mindflash.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Mindflash** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje u metadatafile Mindflash timu za podršku.

    >[AZURE.NOTE] Konfiguracija jedan prijave mora izvoditi Mindflash timu za podršku. Dobit ćete obavijest čim se dovrši konfiguraciju.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Mindflash, oni mora biti dodijeljena u Mindflash.  
U slučaju Mindflash, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Mindflash** kao administrator.

2.  Otvorite **Upravljanje korisnicima**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Upravljanje korisnicima")

3.  Kliknite **Dodavanje korisnika**, a zatim kliknite **Novo**.

4.  U odjeljku **Dodavanje novih korisnika** poduzeti sljedeće korake:

    ![Dodavanje novih korisnika] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Dodavanje novih korisnika")

    1.  Upišite **ime**, **Prezime** i **e-pošte** valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Dodaj**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Mindflash korisnički račun alate za stvaranje ili API-ji nudi Mindflash dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Da biste korisnicima dodijelili Mindflash, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Mindflash **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).