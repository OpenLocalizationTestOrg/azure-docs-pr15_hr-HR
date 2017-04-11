<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Sprinklr | Microsoft Azure" 
    description="Saznajte kako koristiti Sprinklr s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Praktični vodič: Azure Active Directory Integracija s Sprinklr
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Sprinklr.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Sprinklr klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Sprinklr će moći znak u aplikaciju na vašem Sprinklr web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Sprinklr
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Scenarij")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Omogućivanje integraciju aplikacija za Sprinklr
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Sprinklr.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Sprinklr, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Sprinklr**.

    ![Galerija aplikacije] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Sprinklr**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Sprinklr sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Sprinklr** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Sprinklr** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Sprinklr URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. sprinklr.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Sprinklr** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Sprinklr kao administrator.

6.  Idite na **Administracija \> postavke**.

    ![Administracija] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administracija")

7.  Idite na **partnera za upravljanje \> znak** na u lijevom oknu.

    ![Upravljanje partnera] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Upravljanje partnera")

8.  Kliknite **+ znak dodatke**.

    ![Jedan znak Ons] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Jedan znak Ons")

9.  Na stranici **Jedine prijave** , poduzmite sljedeće korake:

    ![Jedan znak Ons] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Jedan znak Ons")

    1.  U tekstni okvir **naziv** upišite naziv za konfiguraciju (npr.: *WAADSSOTest*).
    2.  Odaberite **omogućena**.
    3.  Odaberite **Korištenje novog SSO certifikata**.
    4.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Potvrda davatelja identiteta**
    6.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Sprinklr** kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **Entitet ID-a** .
    7.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Sprinklr** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .
    8.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Sprinklr** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL za odjavite davatelja identiteta** .
    9.  Kao **SAML korisničkog ID -a vrsta**, odaberite **pridruživanju sadrži korisnika "korisničko ime s sprinklr.com**.
    10. **Mjesto za SAML korisnički ID**, odaberite **korisnički ID je identifikator naziv elementa izjave predmet**.
    11. Zatvorite **Spremi**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Za AAD da korisnici mogu se prijaviti, oni mora biti dodijeljena za pristup unutar Sprinklr aplikacije.  
U ovom se odjeljku opisuje kako stvoriti AAD korisničke račune unutar Sprinklr.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Dodjela korisnički račun u Sprinklr, učinite sljedeće:

1.  Prijavite se u web-mjesto tvrtke Sprinklr kao administrator.

2.  Idite na **Administracija \> postavke**.

    ![Administracija] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administracija")

3.  Idite na **Upravljanje klijent \> korisnicima** u lijevom oknu.

    ![Postavke] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Postavke")

4.  Kliknite **Dodaj korisnika**.

    ![Postavke] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Postavke")

5.  U dijaloškom okviru **Uređivanje korisnika** poduzeti sljedeće korake:

    ![Uređivanje korisnika] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Uređivanje korisnika")

    1.  U **e-pošte**, **ime** i **Prezime** tekstnim okvirima, vrsta informacija u korisnički račun za Azure AD želite dodjele resursa.
    2.  Odaberite **lozinku onemogućene**.
    3.  Odaberite **jezik**.
    4.  Odaberite **vrstu korisnika**.
    5.  Kliknite **Ažuriraj**.

    >[AZURE.IMPORTANT] **Lozinka onemogućene** se mora biti odabran da biste omogućili korisnika da se prijavite se putem davatelja identiteta.

6.  Idite na **ulogama**i poduzeti sljedeće korake:

    ![Uloge partnera] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Uloge partnera")

    1.  Na popisu **globalnih** odaberite **sve\_dozvole**.
    2.  Kliknite **Ažuriraj**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Sprinklr korisnički račun alate za stvaranje ili API-ji nudi Sprinklr Dodjela Azure AD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Da biste korisnicima dodijelili Sprinklr, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Sprinklr **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).