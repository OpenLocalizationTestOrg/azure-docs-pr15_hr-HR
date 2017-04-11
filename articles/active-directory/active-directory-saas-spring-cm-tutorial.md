<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Proljetnim CM | Microsoft Azure" 
    description="Saznajte kako koristiti Proljetna CM s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Praktični vodič: Azure Active Directory Integracija s Proljetnim CM
  
Cilj ovog praktičnog vodiča je da biste prikazali kako postaviti jedinstvenu prijavu između Azure Active Directory i SpringCM.
  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na SpringCM jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure Active Directory korisnika koje ste dodijelili SpringCM će moći jedinstvenu prijavu pomoću AAD ploče programa Access.

1.  Omogućivanje integraciju aplikacija za SpringCM
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenarij")

##<a name="enabling-the-application-integration-for-springcm"></a>Omogućivanje integraciju aplikacija za SpringCM
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za SpringCM.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za SpringCM, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **SpringCM**.

    ![Galerija aplikacije] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **SpringCM**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
U ovom se odjeljku opisuje kako omogućiti korisnicima za provjeru autentičnosti SpringCM s računa u Azure Active Directory, pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **SpringCM** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili SpringCM** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **URL SpringCM prijavu na** upišite URL koji se koristi korisnika da biste se prijavili SpringCM aplikacije, a zatim kliknite **Dalje**. 

    URL za aplikaciju je SpringCM klijentu URL-a (npr.: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na SpringCM** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a lokalno spremanje datoteka certifikata na računalo.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Konfiguriranje jedan Proba")

5.  U prozoru preglednika drugoj web, prijavite se web-mjesto tvrtke **SpringCM** kao administrator.

6.  Na izborniku na vrhu kliknite **IDI na**, kliknite **Postavke**, a zatim **SAML SSO**u u odjeljku **Preference računa** .

    ![SAML SSO] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  U odjeljku Konfiguriranje davatelja identiteta poduzeti sljedeće korake:

    ![Konfiguriranje davatelja identiteta] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Konfiguriranje davatelja identiteta")

    1.  Da biste prenijeli preuzete Azure Active Directory certifikata, kliknite **Odabir izdavatelj certifikata** ili **Promjena izdavatelj certifikata**.
    2.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na SpringCM** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    3.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na SpringCM** kopirajte vrijednost **Singel prijave URL servisa** i pa ih zalijepite u tekstni okvir **Pokrenut davatelja usluge (SP) krajnjoj točki** .
    4.  Kao **SAML omogućena**, odaberite **Omogući**.
    5.  Kliknite **Spremi**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Konfiguriranje jedan Proba")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure Active Directory korisnika da se prijavite u SpringCM, oni mora biti dodijeljena u SpringCM.  
U slučaju SpringCM, dodjeljivanje jest zadatak za ručno.

>[AZURE.NOTE] Dodatne informacije potražite u članku [Stvaranje i uređivanje korisnika SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Dodjela korisničkog računa radi SpringCM, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **SpringCM** kao administrator.

2.  Kliknite **Idi na**, a zatim kliknite **Adresar**.

    ![Stvaranje korisnika] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Stvaranje korisnika")

3.  Kliknite **Stvori korisnika**.

4.  Odaberite **korisnička uloga**.

5.  Odaberite **Pošalji e-pošte za aktivaciju**.

6.  Upišite ime, prezime i adresa e-pošte Azure Active Directory korisnički račun koji želite dodjele resursa u povezane tekstne okvire.

7.  Dodavanje korisnika u **sigurnosnoj grupi**.

8.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi SpringCM korisnički račun alate za stvaranje ili API-ji nudi SpringCM dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Da biste korisnicima dodijelili SpringCM, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **SpringCM** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).




