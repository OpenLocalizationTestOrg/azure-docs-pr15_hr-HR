<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s zumiranje | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory zumiranje da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Praktični vodič: Azure Active Directory Integracija s zumiranje
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i zumiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijent za zumiranje
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili zumiranje će moći znak u aplikaciju na vašem Zumiranje web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md)
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za zumiranje
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenarij")

##<a name="enabling-the-application-integration-for-zoom"></a>Omogućivanje integraciju aplikacija za zumiranje
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za zumiranje.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za zumiranje, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Zumiranje**.

    ![Galerija aplikacije] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Zumiranje**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Zumiranje] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Zumiranje")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti zumiranje sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za **Zumiranje** aplikacije Integracija kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako želite korisnicima da biste se prijavili zumiranje** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Zumiranje URL za prijavu** upišite URL pomoću sljedećeg uzorka "*http://company.zoom.us*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na zumiranje** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke zumiranje kao administrator.

6.  Kliknite karticu za **Jedinstvenu prijavu** .

    ![Jedinstvenu prijavu] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Jedinstvenu prijavu")

7.  Kliknite karticu **Sigurnost kontrolu** , a zatim idite na postavke za **Jedinstvenu prijavu** .

8.  U odjeljku jedinstvenu prijavu poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Jedinstvenu prijavu")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na zumiranje** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL stranice za prijavu** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na zumiranje** kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **URL Sign-out stranice** .
    3.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **potvrdu davatelja identiteta**
    5.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na zumiranje** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **izdavač** .
    6.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u zumiranje, oni mora biti dodijeljena u zumiranje.  
U slučaju zumiranje, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Zumiranje** kao administrator.

2.  Kliknite karticu **Upravljanje računom** , a zatim **Upravljanje korisnicima**.

3.  U odjeljku Upravljanje korisnicima, kliknite **Dodavanje korisnika**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Upravljanje korisnicima")

4.  Na stranici **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Dodavanje korisnika")

    1.  Kao **Vrsta korisnika**, odaberite **osnovne**.
    2.  U tekstni okvir **poruke e-pošte** upišite adresu e-pošte valjani AAD račun koji želite dodjele resursa.
    3.  Kliknite **Dodaj**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi zumiranje korisnički račun alate za stvaranje ili API-ji nudi za zumiranje za dodjelu resursa AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Da biste korisnicima dodijelili zumiranje, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za **Zumiranje **aplikacije Integracija kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).