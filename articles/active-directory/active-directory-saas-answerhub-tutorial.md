<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s AnswerHub | Microsoft Azure" 
    description="Saznajte kako koristiti AnswerHub s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Praktični vodič: Azure Active Directory Integracija s AnswerHub

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software).  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili AnswerHub će moći znak u aplikaciju na vašem AnswerHub web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za AnswerHub
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Scenarij")

##<a name="enabling-the-application-integration-for-answerhub"></a>Omogućivanje integraciju aplikacija za AnswerHub

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za AnswerHub.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za AnswerHub, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **AnswerHub**.

    ![Galerija aplikacije] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **AnswerHub**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti AnswerHub sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **AnswerHub** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili AnswerHub** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **AnswerHub URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.answerhub.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na AnswerHub** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke AnswerHub kao administrator.
    >[AZURE.NOTE] Ako vam je potrebna pomoć prilikom konfiguriranja AnswerHub, obratite se [timu za podršku AnswerHub korisnika](mailto:success@answerhub.com. ).








6.  Idite na **Administracija**.

7.  Kliknite karticu **korisnika i grupa** .

8.  U navigacijskom oknu s lijeve strane, u odjeljku **Društvenih postavke** kliknite **Postavljanje SAML**.

9.  Kliknite karticu **IDP konfiguracije** .

10. Na kartici **IDP Config** poduzeti sljedeće korake:

    ![Postavljanje SAML] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "Postavljanje SAML")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na AnswerHub** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL IDP prijava** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na AnswerHub** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite IDP** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na AnswerHub** kopirajte vrijednost **Oblik naziva identifikator** i pa ih zalijepite u tekstni okvir **IDP naziv identifikator oblik** .
    4.  Pritisnite **tipke i certifikati**.

11. Na kartici tipke i certifikati poduzeti sljedeće korake:

    ![Tipke i certifikati] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Tipke i certifikati")

    1.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    2.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **IDP javni ključ (x 509 oblik)** .
    3.  Kliknite **Spremi**.

12. Na kartici **IDP Config** , kliknite **Spremi**.

13. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u AnswerHub, oni mora biti dodijeljena u AnswerHub.  
U slučaju AnswerHub, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **AnswerHub** kao administrator.

2.  Idite na **Administracija**.

3.  Kliknite karticu **korisnici i grupe** .

4.  U navigacijskom oknu s lijeve strane, u odjeljku **Upravljanje korisnicima** , kliknite **Stvori ili uvoz korisnika**.

    ![Korisnici i grupe] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Korisnici i grupe")

5.  Upišite **adresu e-pošte**, **korisničko ime** i **lozinku** valjani Azure Active Directory računa koji želite dodjele resursa u povezane tekstne okvire, a zatim kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi AnswerHub korisnički račun alate za stvaranje ili API-ji nudi AnswerHub dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Da biste korisnicima dodijelili AnswerHub, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **AnswerHub **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
