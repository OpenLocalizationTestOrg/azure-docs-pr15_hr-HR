<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Clarizen | Microsoft Azure" 
    description="Saznajte kako koristiti Clarizen s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Praktični vodič: Azure Active Directory Integracija s Clarizen

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Clarizen.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Clarizen jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Clarizen će moći znak u aplikaciju na vašem Clarizen web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Clarizen
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenarij")
##<a name="enabling-the-application-integration-for-clarizen"></a>Omogućivanje integraciju aplikacija za Clarizen

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Clarizen.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Clarizen, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Clarizen**.

    ![Galerija aplikacije] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Clarizen**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Clarizen sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Clarizen** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Clarizen** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija jedinstvenu prijavu na Clarizen** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Konfiguriranje jedinstvenu prijavu")

4.  U prozoru preglednika drugoj web, prijavite se u vašem **Clarizen** web-mjesto tvrtke kao administrator (npr.: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Kliknite svoje korisničko ime, a zatim kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Postavke")

6.  Kliknite karticu **Globalne postavke** , a zatim pokraj **Vanjskog provjeru autentičnosti**, kliknite **Uređivanje**.

    ![Globalnih postavki] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globalnih postavki")

7.  U dijaloškom okviru **Vanjskog provjere autentičnosti** poduzeti sljedeće korake:

    ![Pridružene provjere autentičnosti] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Pridružene provjere autentičnosti")

    1.  Kliknite **Prijenos** da biste prenijeli preuzete certifikata.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Clarizen** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    3.  Azure klasični portalu na stranici dijaloški okvir **Konfiguracija jednim sign-out pri Clarizen** kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **Sign-out URL** .
    4.  Odaberite **koristi objavu**.
    5.  Kliknite **Spremi**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Clarizen, oni mora biti dodijeljena u Clarizen.  
U slučaju Clarizen, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Clarizen** kao administrator.

2.  Kliknite **osobe**.

    ![Osobe] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Osobe")

3.  Kliknite **Pozovi korisnika**.

    ![Pozivanje korisnika] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Pozivanje korisnika")

4.  Na stranici dijaloški okvir Pozovi sudionike, učinite sljedeće:

    ![Pozivanje osoba] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Pozivanje osoba")

    1.  U tekstni okvir **e-pošte** upišite adresu e-pošte valjani Azure Active Directory račun koji želite dodjele resursa.
    2.  Kliknite **Pozovi**.

    >[AZURE.NOTE] Vlasnik računa Azure Active Directory će primiti poruku e-pošte i slijedite vezu da biste potvrdili svoj račun prije postaje aktivna.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Da biste korisnicima dodijelili Clarizen, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Clarizen **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
