<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Pagerduty | Microsoft Azure" 
    description="Saznajte kako koristiti Pagerduty s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Praktični vodič: Azure Active Directory Integracija s Pagerduty
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Pagerduty.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Pagerduty klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Pagerduty će moći znak u aplikaciju na vašem Pagerduty web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Pagerduty
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scenarij")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Omogućivanje integraciju aplikacija za Pagerduty
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Pagerduty.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Pagerduty, poduzmite sljedeće korake:

1.  Na portalu za upravljanje Azure, u lijevom navigacijskom oknu kliknite **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Pagerduty**.

    ![Galerija aplikacije] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Pagerduty**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Pagerduty sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Pagerduty** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Pagerduty** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Pagerduty URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Pagerduty.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Pagerduty** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Pagerduty kao administrator.

6.  Na izborniku na vrhu kliknite **Postavke računa**.

    ![Postavke računa] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Postavke računa")

7.  Kliknite **jedinstvenu prijavu**.

    ![Jedinstvenu prijavu] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Jedinstvenu prijavu")

8.  Na stranici omogućili jedinstvenu prijavu (SSO), poduzmite sljedeće korake:

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Omogućivanje jedinstvenu prijavu")

    1.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    2.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Certifikat X.509**
    3.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na Pagerduty** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    4.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na Pagerduty** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL odjavite** .
    5.  Odaberite **Uključi jedinstvenu prijavu**.
    6.  Kliknite **Spremi promjene**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Pagerduty, oni mora biti dodijeljena u Pagerduty.  
U slučaju Pagerduty, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **Pagerduty** .

2.  Na izborniku na vrhu kliknite **korisnicima**.

3.  Kliknite **Dodavanje korisnika**.

    ![Dodavanje korisnika] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Dodavanje korisnika")

4.  U dijaloškom okviru **pozivanje timom** upišite na **prvi i prezime** i adresa **e-pošte** korisnika Azure AD želite Dodjela resursa, kliknite **Dodaj**, a zatim kliknite **Pošalji poziva**.

    ![Pozivanje vaš tim] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Pozivanje vaš tim")

    >[AZURE.NOTE] Svi korisnici dodani će primiti pozivnicu za programa da biste stvorili PagerDuty račun.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Pagerduty korisnički račun alate za stvaranje ili API-ji nudi Pagerduty dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Da biste korisnicima dodijelili Pagerduty, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Pagerduty **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).