<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s temelja zahtjev | Microsoft Azure" 
    description="Saznajte kako koristiti temelja zahtjev s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Praktični vodič: Azure Active Directory Integracija s zahtjev temelja

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i temelja zahtjev.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Zahtjev za temelja klijenta

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili temelja zahtjev će moći znak u aplikaciju na vaš zahtjev temelja web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za zahtjev temelja
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Scenarij")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Omogućivanje integraciju aplikacija za zahtjev temelja

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za temelja zahtjev.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za temelja zahtjev, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **temelja zahtjev**.

    ![Galerija aplikacije] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Temelja zahtjev**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Zahtjev za temelja] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Zahtjev za temelja")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti temelja zahtjev sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Temelja zahtjev** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili zahtjev temelja** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Microsoft Azure AD jedinstvene prijave] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD jedinstvene prijave")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Temelja zahtjev URL za prijavu** upišite URL pomoću sljedećeg uzorka "*http://company.csod.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na zahtjev temelja** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje sljedeće stavke zahtjev temelja službi za podršku:

    1.  Preuzeta certifikata
    2.  Vrijednost **URL daljinskog prijava**
    3.  **URL daljinskog odjavite** vrijednost.

    >[AZURE.NOTE] Jedinstvenu prijavu potrebno je konfigurirati od tima za podršku temelja zahtjev.
Dobit ćete obavijest podršku dovršetku konfiguracije.

6.  Odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u temelja zahtjev, oni mora biti dodijeljena u temelja zahtjev.  
U slučaju zahtjev temelja dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Slanje podataka o (npr.: naziv, Emial) o korisniku Azure AD želite dodjele resursa za zahtjev temelja službi za podršku.

>[AZURE.NOTE] Možete koristiti bilo koji drugi temelja zahtjev korisnički račun alate za stvaranje ili API-ji nudi temelja zahtjev za dodjeljivanje AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Da biste korisnicima dodijelili temelja zahtjev, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Temelja zahtjev **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Dodjela korisnika] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Dodjela korisnika")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
