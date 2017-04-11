<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Panopto | Microsoft Azure" 
    description="Saznajte kako koristiti Panopto s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Praktični vodič: Azure Active Directory Integracija s Panopto
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Panopto.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Panopto klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Panopto će moći znak u aplikaciju na vašem Panopto web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Panopto
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenarij")
##<a name="enabling-the-application-integration-for-panopto"></a>Omogućivanje integraciju aplikacija za Panopto
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Panopto.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Panopto, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Panopto**.

    ![Galerija Appkication] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Galerija Appkication")

7.  U oknu s rezultatima odaberite **Panopto**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Panopto sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Panopto** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Panopto** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Panopto URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Panopto.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Panopto** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Panopto kao administrator.

6.  Na alatnoj traci na lijevoj strani kliknite **sustav**, a zatim **Davatelji identiteta**.

    ![Sustav] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Sustav")

7.  Kliknite **Dodavanje davatelja usluga**.

    ![Davatelji identiteta] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Davatelji identiteta")

8.  U odjeljku davatelj usluga SAML poduzeti sljedeće korake:

    ![Konfiguriranje SaaS] (./media/active-directory-saas-panopto-tutorial/IC777672.png "Konfiguriranje SaaS")

    1.  Na popisu **Vrsta davatelja** odaberite **SAML20**
    2.  U tekstni okvir **Naziv Instance** upišite naziv instance.
    3.  U tekstni okvir **Neslužbeni opis** Upišite neslužbeni opis.
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Panopto** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    5.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Panopto** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **Url stranice za Uskoči** .
    6.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    7.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Javni ključ**
    8.  Kliknite **Spremi**.
        ![Spremanje] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Spremanje")

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Panopto.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Panopto pomoću ploče programa access, Panopto provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću Panopto.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Panopto korisnički račun alate za stvaranje ili API-ji nudi Panopto Dodjela Azure AD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Da biste korisnicima dodijelili Panopto, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Panopto **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).