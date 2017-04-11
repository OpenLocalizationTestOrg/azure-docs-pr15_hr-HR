<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s prilagodljivo paket | Microsoft Azure"
    description="Saznajte kako koristiti prilagodljivo paket s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Praktični vodič: Azure Active Directory Integracija s prilagodljivo paketa

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i prilagodljivo paket.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijent prilagodljiva paketa

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili prilagodljivo paketa će moći znak u aplikaciju na vašem prilagodljivo paketa web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija prilagodljivo paket
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenarij")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Omogućivanje integraciju aplikacija prilagodljivo paket

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija prilagodljivo paket.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija prilagodljivo paket, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Prilagodljivo paket**.

    ![Galerija aplikacije] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Prilagodljivo paket**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Prilagodljivo paketa] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Prilagodljivo paketa")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti prilagodljivo paket sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvenu prijavu prilagodljivo paket potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici Integracija **Prilagodljivo paketa** aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili prilagodljivo paket** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija postavki aplikacije** u tekstni okvir **URL odgovor** upišite URL pomoću sljedećeg uzorka "*samlsso/https://login.adaptiveinsights.com:443/RlJFRVRSSUFMMTI3MTE =*", a zatim kliknite **Dalje**.

    >[AZURE.NOTE] Tu vrijednost možete pristupiti iz prilagodljivo paket **SAML SSO postavke** stranice.

    ![Konfiguriranje postavki aplikacije] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Konfiguriranje postavki aplikacije")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na prilagodljivo paket** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke prilagodljivo paketa kao administrator.

6.  Idite na **administrator**.

    ![Administrator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrator")

7.  U odjeljku **korisnici i uloge** kliknite **Upravljanje postavkama SSO SAML**.

    ![Upravljanje postavkama SAML SSO] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Upravljanje postavkama SAML SSO")

8.  Na stranici s **Postavkama SSO SAML** poduzeti sljedeće korake:

    ![Postavke SAML SSO] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "Postavke SAML SSO")

    1.  U tekstni okvir **naziv davatelja identiteta** upišite naziv za konfiguraciju.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na prilagodljivo paket** kopirajte vrijednost **ID entitet** , a pa ih zalijepite u tekstni okvir **davatelja identiteta entitet ID -a** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na prilagodljivo paket** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **davatelja identiteta SSO URL-a** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na prilagodljivo paket** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **URL odjavite Prilagođeno** .
    5.  Da biste prenijeli preuzete certifikata, kliknite **Odabir datoteke**.
    6.  Kao **SAML korisnički id**, odaberite **korisnikovo prilagodljivo uvida korisničko ime**.
    7.  **Mjesto za SAML korisnički id**, odaberite **id korisnika u NameID predmet**.
    8.  Kao **SAML NameID oblik**, odaberite **adresu e-pošte**.
    9.  Kao **Omogućiti SAML**, odaberite **Dopusti SSO SAML i izravno prilagodljivo uvida prijava**.
    10. Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u prilagodljivo paket, oni mora biti dodijeljena u prilagodljivo paket.  
U slučaju prilagodljivo paket dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Prilagodljivo paket** kao administrator.

2.  Idite na **administrator**.

    ![Administrator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrator")

3.  U odjeljku **korisnici i uloge** kliknite **Dodaj korisnika**.

    ![Dodavanje korisnika] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Dodavanje korisnika")

4.  U odjeljku **Novi korisnik** poduzeti sljedeće korake:

    ![Slanje] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Slanje")

    1.  Upišite **naziv**, **prijavu**, **e-pošte**, **lozinku** za valjani Azure Active Directory korisnika želite dodjele resursa u povezani tekstni okviri.
    2.  Odaberite **ulogu**.
    3.  Kliknite **Pošalji**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi prilagodljivo paket korisničkog računa alate za stvaranje ili API-ji nudi prilagodljivo paket dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Da biste korisnicima dodijelili prilagodljivo paket, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici Integracija **Prilagodljivo paketa **aplikacije, kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
