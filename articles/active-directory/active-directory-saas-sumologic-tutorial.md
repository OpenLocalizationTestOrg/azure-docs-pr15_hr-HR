<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s SumoLogic | Microsoft Azure" 
    description="Saznajte kako koristiti SumoLogic s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Praktični vodič: Azure Active Directory Integracija s SumoLogic
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i SumoLogic.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   SumoLogic klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili SumoLogicwill se moći jednom prijava u aplikaciju na vašem SumoLogic tvrtke web-mjesta (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za SumoLogic
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Scenarij")

##<a name="enabling-the-application-integration-for-sumologic"></a>Omogućivanje integraciju aplikacija za SumoLogic
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za SumoLogic.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za SumoLogic, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **sumologic**.

    ![Galerija aplikacije] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **SumoLogic**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti SumoLogic sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 vaše SumoLogictenant.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **SumoLogic** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili SumoLogic** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **SumoLogic URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. SumoLogic.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje aoo URL-a] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Konfiguriranje aoo URL-a")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na SumoLogic** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke SumoLogic kao administrator.

6.  Idite na **Upravljanje \> sigurnosti**.

    ![Upravljanje] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Upravljanje")

7.  Kliknite **SAML**.

    ![Postavke globalne sigurnosti] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globalni sigurnosne postavke")

8.  S popisa **Odaberite konfiguraciju ili stvorite novi** odaberite **Azure AD**pa kliknite **Konfiguriraj**.

    ![Konfiguriranje SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Konfiguriranje SAML 2.0")

9.  U dijaloškom okviru **Konfiguriranje SAML 2.0** poduzeti sljedeće korake:

    ![Konfiguriranje SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Konfiguriranje SAML 2.0")

    1.  U tekstni okvir **Naziv konfiguracije** upišite **Azure AD**.
    2.  Odaberite **način rada za ispravljanje pogrešaka**.
    3.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na SumoLogic** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    4.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na SumoLogic** kopirajte vrijednost **URL zahtjev za provjeru autentičnosti** i pa ih zalijepite u tekstni okvir **Authn zahtjev za URL-a** .
    5.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otvorite certifikata kodirana base-64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim zalijepite cijelu certifikata u **Certifikat X.509** tekstni okvir.
    7.  Kao **Atribut e-pošte**, odaberite **koristi SAML predmet**.
    8.  Odaberite **SP pokrenut konfiguracije prijava**.
    9.  U tekstni okvir **Put prijavite** , upišite **Azure**.
    10. Kliknite **Spremi**.

10. Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na SumoLogic** odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u SumoLogic, oni mora biti dodijeljena da SumoLogic.  
U slučaju SumoLogic, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **SumoLogic** .

2.  Idite na **Upravljanje \> korisnika**.

    ![Korisnici] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Korisnici")

3.  Kliknite **Dodaj**.

    ![Korisnici] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Korisnici")

4.  U dijaloškom okviru **Novi korisnik** poduzeti sljedeće korake:

    ![Novi korisnik] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Novi korisnik")

    1.  Upišite povezanih informacija Azure AD računa za dodjelu resursa u tekstnim okvirima **imena**, **Prezimena** i **e-pošte** .
    2.  Odaberite ulogu.
    3.  Kao **Status**odaberite **aktivni**.
    4.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi SumoLogic korisnički račun alate za stvaranje ili API-ji nudi SumoLogic dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Da biste korisnicima dodijelili SumoLogic, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **SumoLogic** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).