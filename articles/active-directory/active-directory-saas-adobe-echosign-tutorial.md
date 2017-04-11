<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Adobe EchoSign | Microsoft Azure" 
    description="Saznajte kako koristiti Adobe EchoSign s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Praktični vodič: Azure Active Directory Integracija s Adobe EchoSign

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Adobe EchoSign.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Adobe EchoSign jednom prijava omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Adobe EchoSign će moći znak u aplikaciju na vašem Adobe EchoSign web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Adobe EchoSign
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scenarij")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Omogućivanje integraciju aplikacija za Adobe EchoSign

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Adobe EchoSign.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Adobe EchoSign, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Adobe EchoSign**.

    ![Galerija aplikacije] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Adobe EchoSign**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Adobe EchoSign sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Adobe EchoSign** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Adobe EchoSign** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Adobe EchoSign na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.echosign.com/*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Adobe EchoSign** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Adobe EchoSign kao administrator.

6.  Na izborniku u gornjem kliknite **poslovni subjekt**, a zatim u navigacijskom oknu na lijevoj die kliknite **SAML postavke** u odjeljku **Postavke računa**.

    ![Računa] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Računa")

7.  U odjeljku postavke SAML poduzeti sljedeće korake:

    ![Postavke SAML] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "Postavke SAML")

    1.  Kao **SAML način**, odaberite **SAML obavezna**.
    2.  Odaberite **Dopusti administratori EchoSign računa da biste se prijavili pomoću vjerodajnica EchoSign**.
    3.  Kao **Stvaranja korisnika**, odaberite **automatski dodati korisnike provjeriti preko SAML**.

8.  Premještanje na izvođenje sljedećih koraka:

    ![Postavke SAML] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "Postavke SAML")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Adobe EchoSign** kopirajte vrijednost **ID entitet** , a pa ih zalijepite u tekstni okvir **ID entitet IdP** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Adobe EchoSign** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL IdP prijava** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Adobe EchoSign** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite IdP** .
    4.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **Potvrdu IdP** 6.  Kliknite **Spremi promjene**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u programu Adobe EchoSign, oni mora biti dodijeljena u programu Adobe EchoSign.  
U slučaju Adobe EchoSign dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Adobe EchoSign** kao administrator.

2.  Na izborniku na vrhu kliknite **račun**, zatim u navigacijskom oknu na lijevoj die kliknite **korisnici i grupe**, i pa kliknite **Stvori novi korisnik**.

    ![Računa] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Računa")

3.  U odjeljku **Stvaranje novog korisnika** poduzeti sljedeće korake:

    ![Stvaranje korisnika] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Stvaranje korisnika")

    1.  Unesite **Adresu e-pošte**, **ime** i **Prezime** valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Stvori korisnika**.

        >[AZURE.NOTE] Vlasnik računa Azure Active Directory primit će poruku e-pošte koja sadrži vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Adobe EchoSign korisnički račun alate za stvaranje ili API-ji nudi Adobe EchoSign dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Da biste korisnicima dodijelili Adobe EchoSign, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Adobe EchoSign **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
