<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Workday | Microsoft Azure" 
    description="Saznajte kako koristiti Workday s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Praktični vodič: Azure Active Directory Integracija s Workday
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i dana. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijenta u Workday
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Workday
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Konfiguriranje dodjeljivanje korisnika

![Scenarij] (./media/active-directory-saas-workday-tutorial/IC782919.png "Scenarij")

##<a name="enabling-the-application-integration-for-workday"></a>Omogućivanje integraciju aplikacija za Workday
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Salesforce.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Workday, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-workday-tutorial/IC700994.png "Aplikacija")

4.  Da biste otvorili **Galeriju aplikaciju**, kliknite **Dodaj programa aplikacija**, a zatim **Dodavanje aplikacije za tvrtke ili ustanove da biste koristili**.

    ![Što želite učiniti?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Što želite učiniti?")

5.  U **okvir za pretraživanje**upišite **dana**.

    ![WORKDAY] (./media/active-directory-saas-workday-tutorial/IC701021.png "WORKDAY")

6.  U oknu s rezultatima odaberite **dana**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![WORKDAY] (./media/active-directory-saas-workday-tutorial/IC701022.png "WORKDAY")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Workday sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje certifikata kodirana osnovni 64.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Na stranici za integraciju aplikacije **Workday** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-workday-tutorial/IC782920.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Workday** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-workday-tutorial/IC782921.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** poduzeti sljedeće korake, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-workday-tutorial/IC782957.png "Konfiguriranje URL adresa Web App")

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika da biste se prijavili Workday pomoću sljedećeg uzorka:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  U tekstni okvir **URL odgovor Workday** upišite URL odgovor Workday pomoću sljedećeg uzorka:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Odgovori URL-a, morate imati podređene domene (npr.: "www", wd2, wd3, wd3 impl, wd5, wd5 impl). 
    >Korištenje nešto poput "*http://www.myworkday.com*" funkcionira, ali "*http://myworkday.com*" ne. 
 
4.  Na stranici **Konfiguracija jedinstvenu prijavu na Workday** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-workday-tutorial/IC782922.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Workday kao administrator.

6.  Idite na **izbornika \> Workbench**.

    ![Workbench] (./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  Idite na **Administracija računa**.

    ![Administracija računa] (./media/active-directory-saas-workday-tutorial/IC782924.png "Administracija računa")

8.  Idite na **uredite klijenta postavljanje – sigurnost**.

    ![Uređivanje sigurnosne klijenta] (./media/active-directory-saas-workday-tutorial/IC782925.png "Uređivanje sigurnosne klijenta")

9.  U odjeljku **Preusmjeravanja URL-ovi** poduzeti sljedeće korake:

    ![Preusmjeravanje URL-ova] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Preusmjeravanje URL-ova")

    na. Kliknite **Dodaj redak**.

    b. U tekstni okvir za **Prijavu preusmjeravanje URL-a** i **Mobile preusmjeravanje URL-a** tekstni okvir, unesite **URL Workday klijenta** na koji ste unijeli na stranici **Konfiguriranje aplikacije URL** portala za Azure klasični.
    
    c. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Workday** kopirajte **URL servisa za jednu Sign-Out**, a zatim je zalijepite u tekstni okvir **Odjavite preusmjeravanje URL-a** .

    d.  U tekstni okvir **okruženja** , upišite naziv okruženju.  


    >[AZURE.NOTE] Vrijednost atributa okruženje je vezan uz vrijednost URL klijentu:
    >
    >-   Ako naziv domene klijentu Workday URL započinje impl (npr.: *https://impl.workday.com/\<klijentu\>/login-saml2.htmld*), **okruženje** atributa mora biti postavljeno na implementaciju.
    >-   Ako naziv domene počinje nešto drugo, morate je obratite Workday da biste dohvatili odgovarajuću vrijednost **okruženje** .

10. U odjeljku **Postavljanje SAML** poduzeti sljedeće korake:

    ![Postavljanje SAML] (./media/active-directory-saas-workday-tutorial/IC782926.png "Postavljanje SAML")

    na.  Odaberite **Omogući provjeru autentičnosti SAML**.

    b.  Kliknite **Dodaj redak**.

11. U odjeljku davatelji identiteta SAML poduzeti sljedeće korake:

    ![Davatelji identiteta SAML] (./media/active-directory-saas-workday-tutorial/IC7829271.png "Davatelji identiteta SAML")

    na. U tekstni okvir Naziv davatelja identiteta upišite naziv davatelja (npr.: *SPInitiatedSSO*).

    b. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Workday** kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **izdavač** .

    c. Odaberite **Omogući Workday odjavite Initialted**.

    d. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Workday** kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **Odjavite zahtjev za URL-a** .


    e. Kliknite **Identitet davatelja javni ključ certifikat**, a zatim kliknite **Stvori**. 

    ![Stvaranje] (./media/active-directory-saas-workday-tutorial/IC782928.png "Stvaranje")

    f. Kliknite **Stvaranje x509 javni ključ**. 
        
    ![Stvaranje] (./media/active-directory-saas-workday-tutorial/IC782929.png "Stvaranje")


1. U odjeljku **Prikaz x509 javni ključ** poduzeti sljedeće korake: 

    ![Prikaz x509 javni ključ] (./media/active-directory-saas-workday-tutorial/IC782930.png "Prikaz x509 javni ključ") 

    na. U tekstni okvir **naziv** upišite naziv certifikata (npr.: *PPE\_SP*).
        
    b. U **Valjani iz** tekstni okvir upišite vrijedi od vrijednost atributa svog certifikata.
    
    c.  U **Valjani u** tekstni okvir upišite valjanu vrijednost atributa certifikata.
        
    >[AZURE.NOTE] Možete dobiti valjanog od datuma i valjanog do datuma iz preuzete certifikat dvoklikom. Datume navedene su na kartici **pojedinosti** .

    d. Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

    >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    e.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, a zatim kopirajte na sadržaj.
    
    f.  U tekstni okvir **Potvrda** zalijepiti sadržaj međuspremnik.
    
    g.  Kliknite **u redu**.

12.  Poduzmite sljedeće korake: 

    ![Konfiguriranje SSO] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "Konfiguriranje SSO")

    na.  Omogućivanje na **x509 privatni ključ par**.

    b.  U tekstni okvir **ID davatelja usluge** unesite **http://www.workday.com**.

    c.  Odaberite **Omogući RD pokrenut SAML provjeru autentičnosti**.

    d.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Workday** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL servisa SSO IdP** .
     
    e. Odaberite **Umanji zahtjev za SP pokrenut provjeru autentičnosti**.

    f. Kao **Način potpis zahtjev za provjeru autentičnosti**, odaberite **SHA256**. 
        
    ![Način potpis zahtjev za provjeru autentičnosti] (./media/active-directory-saas-workday-tutorial/IC782932.png "Način potpis zahtjev za provjeru autentičnosti") 
 
    g. Kliknite **u redu**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na Workday** kliknite **Dalje**. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-workday-tutorial/IC782934.png "Konfiguriranje jedinstvenu prijavu")

13. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Konfiguriranje jedinstvenu prijavu")



##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste dobili testnih korisnika dodjeli u Workday, morate obratite se timu za podršku dana.  
Workday podršku će stvoriti korisnika.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Da biste korisnicima dodijelili Workday, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Workday **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-workday-tutorial/IC782935.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-workday-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).