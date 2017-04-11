<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija sa službom za korisnike Jitbit | Microsoft Azure" 
    description="Saznajte kako koristiti službom za korisnike Jitbit s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Praktični vodič: Azure Active Directory Integracija sa službom za korisnike Jitbit
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Jitbit službom za korisnike.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Služba za Jitbit klijenta
  
Po dovršetku ovog praktičnog vodiča, koje ste dodijelili Jitbit služba za korisnike Azure AD će moći znak u aplikaciju na vašem Jitbit službom za korisnike web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Jitbit službom za korisnike
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scenarij")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Omogućivanje integraciju aplikacija za Jitbit službom za korisnike
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Jitbit službom za korisnike.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za službom za korisnike Jitbit, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Jitbit službom za korisnike**.

    ![Galerija aplikacije] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Jitbit službom za korisnike**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti službom za korisnike Jitbit sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol. .  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Da biste mogli konfigurirati jedinstvenu prijavu na klijentu službom za korisnike Jitbit, morate najprije obratite Jitbit služba za tehničku podršku da biste dobili Ova funkcija nije omogućena.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Službom za korisnike Jitbit** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili službom za korisnike Jitbit** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Jitbit služba za URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Jitbit.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na službom za korisnike Jitbit** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\Jitbit Helpdesk.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke službom za korisnike Jitbit kao administrator.

6.  Na alatnoj traci na vrhu kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administracija")

7.  Kliknite **opće postavke**.

    ![Korisnici, tvrtke i dozvole] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Korisnici, tvrtke i dozvole")

8.  U odjeljku Konfiguriranje **Postavke provjere autentičnosti** poduzeti sljedeće korake:

    ![Postavke provjere autentičnosti] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Postavke provjere autentičnosti")

    1.  Odaberite **Omogući SAML 2.0 jedan se prijaviti na** prijavu pomoću jedne prijavu (SSO) **OneLogin**.
    2.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na službom za korisnike Jitbit** kopirajte vrijednost **davatelja usluga (SP) pokrenut krajnjoj točki** i pa ih zalijepite u tekstni okvir **URL ADRESU krajnje točke** .
    3.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.
        
        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otvorite certifikata kodirana osnovni 64, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Certifikat X.509**
    5.  Kliknite **Spremi promjene**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Jitbit službom za korisnike, oni mora biti dodijeljena u Jitbit službom za korisnike.  
U slučaju Jitbit službom za korisnike, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **Jitbit službom za korisnike** .

2.  Na izborniku na vrhu kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administracija")

3.  Kliknite **korisnici, tvrtke i dozvole**.

    ![Korisnici, tvrtke i dozvole] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Korisnici, tvrtke i dozvole")

4.  Kliknite **Dodaj korisnika**.

    ![Dodavanje korisnika] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Dodavanje korisnika")

5.  U odjeljku Stvaranje upišite podatke o Azure AD računa za dodjelu resursa u sljedeći tekstni okviri: **korisničko ime**, **e-pošte**, **ime**, **Prezime**

    ![Stvaranje] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Stvaranje")

6.  Kliknite **Stvori**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Jitbit služba za korisnički račun alate za stvaranje ili API-ji nudi Jitbit služba za dodjeljivanje AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Da biste dodijelili Jitbit služba za korisnike, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Službom za korisnike Jitbit **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).