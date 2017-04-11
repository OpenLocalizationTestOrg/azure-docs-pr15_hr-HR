<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s xMatters zahtjev | Microsoft Azure"
    description="Saznajte kako koristiti xMatters zahtjev s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Praktični vodič: Azure Active Directory Integracija s xMatters zahtjev
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i xMatters zahtjev. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijent xMatters zahtjev
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili xMatters zahtjev će moći znak u aplikaciju na vašem xMatters zahtjev web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za xMatters zahtjev
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenarij")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Omogućivanje integraciju aplikacija za xMatters zahtjev
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za xMatters zahtjev.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za xMatters zahtjev, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **xMatters zahtjev**.

    ![Galerija aplikacije] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **XMatters zahtjev**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![xMatters zahtjev] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters zahtjev")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti zahtjev XMatters sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **XMatters zahtjev** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili zahtjev XMatters** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Konfiguriranje URL adresa web app")

    na. U tekstni okvir **XMatters zahtjev URL za prijavu** upišite URL pomoću sljedećeg uzorka:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Kliknite **Dalje**.


4.  Na stranici **Konfiguracija jedinstvenu prijavu na zahtjev XMatters** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Morate prosljeđivanje certifikata radi xMatters timu za podršku. Certifikat je potrebno prenijeti od tima za podršku xMatters prije nego što možete dovršavanje jedan konfiguraciju za prijavu.

    ![Konfiguriranje jednom prijava] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Konfiguriranje jednom prijava")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke XMatters zahtjev kao administrator.

6.  Na alatnoj traci na vrhu kliknite **administrator**, a zatim kliknite **Detalji o tvrtki** na navigacijskoj traci na lijevoj strani.

    ![Administrator] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Administrator")

7.  Na stranici **Konfiguracija SAML** , poduzmite sljedeće korake:

    ![Konfiguriranje SAML] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "Konfiguriranje SAML")

    1.  Odaberite **Omogući SAML**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na zahtjev XMatters** kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **ID davatelja identiteta** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na zahtjev XMatters** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir za **Jedan znak na URL-a** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na zahtjev XMatters** kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **URL jedan odjavite** .
    5.  Na stranici Detalji o tvrtki na vrhu, kliknite **Spremi promjene**.
        ![Detalji o tvrtki] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Detalji o tvrtki")

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jednom prijava] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Konfiguriranje jednom prijava")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u XMatters zahtjev, oni mora biti dodijeljena u XMatters zahtjev.  
Ako zahtjev za XMatters dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **XMatters zahtjev** .

2.  Kliknite karticu **korisnika** .

3.  Kliknite **Dodaj korisnika**.

    ![Korisnici] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Korisnici")

4.  Odaberite **aktivni**.

5.  U odjeljku **Dodavanje korisnika** , učinite sljedeće:

    ![Dodavanje korisnika] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Dodavanje korisnika")

    1.  Unesite **korisnički ID**, **ime**, **Prezime**, **web-mjesta** valjane AAD računa koji želite dodjele resursa.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi XMatters zahtjev korisnički račun alate za stvaranje ili API-ji nudi XMatters zahtjev za dodjeljivanje AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Da biste korisnicima dodijelili XMatters zahtjev, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **XMatters zahtjev **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).