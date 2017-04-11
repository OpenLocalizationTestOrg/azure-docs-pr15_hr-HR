<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s BlueJeans | Microsoft Azure" 
    description="Saznajte kako koristiti BlueJeans s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Praktični vodič: Azure AD Integracija s BlueJeans

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i BlueJeans.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na BlueJeans jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili BlueJeans će moći znak u aplikaciju na vašem BlueJeans web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za BlueJeans
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Scenarij")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Omogućivanje integraciju aplikacija za BlueJeans

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za BlueJeans.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za BlueJeans, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **BlueJeans**.

    ![Galerija aplikacije] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **BlueJeans**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti BlueJeans sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **BlueJeans** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili BlueJeans** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **BlueJeans na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.BlueJeans.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na BlueJeans** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke **BlueJeans** kao administrator.

6.  Idite na **ADMINISTRATOR \> postavke grupe \> sigurnost**.

    ![Administrator] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Administrator")

7.  U odjeljku **Sigurnost** poduzeti sljedeće korake:

    ![SAML jedine prijave] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML jedine prijave")

    1.  Odaberite **SAML jedine prijave**.
    2.  Odaberite **Omogući automatsko dodjele resursa**.

8.  Premještanje sljedeće korake:

    ![Put certifikata] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Put certifikata")

    1.  Kliknite **Odaberite datoteku**, a zatim prijenos preuzete certifikata.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na BlueJeans** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na BlueJeans** kopirajte vrijednost **URL Promijeni lozinku** i pa ih zalijepite u tekstni okvir **URL-a za promjenu lozinke** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na BlueJeans** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL odjavite** .

9.  Premještanje sljedeće korake:

    ![Spremanje promjena] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Spremanje promjena")

    1.  U tekstni okvir **id korisnika** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  U tekstni okvir **e-pošte** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Kliknite **Spremi promjene**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u BlueJeans, oni mora biti dodijeljena u BlueJeans.  
U slučaju BlueJeans, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **BlueJeans** kao administrator.

2.  Idite na **ADMINISTRATOR \> Upravljanje korisnicima \> Dodavanje korisnika**.

    ![Administrator] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Administrator")

    >[AZURE.IMPORTANT] Na kartici **Dodaj korisnika** dostupna samo ako je u **karticu sigurnost** **Omogući automatsko dodjeljivanje** poništen potvrdni okvir.

3.  U odjeljku **Add User** poduzeti sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Dodavanje korisnika")

    1.  Upišite **BlueJeans korisničko ime**, **adresu e-pošte**, **BlueJeans ID sastanka**, **Pristupni izraz moderatora**, **Ime i prezime**, **tvrtke** valjan račun za AAD želite Dodjela resursa u povezani tekstni okviri.
    2.  Kliknite **Dodaj korisnika**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi BlueJeans korisnički račun alate za stvaranje ili API-ji nudi BlueJeans dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Da biste korisnicima dodijelili BlueJeans, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **BlueJeans **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
