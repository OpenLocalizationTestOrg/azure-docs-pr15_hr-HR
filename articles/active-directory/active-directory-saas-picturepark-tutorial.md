<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Picturepark | Microsoft Azure" 
    description="Saznajte kako koristiti Picturepark s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Praktični vodič: Azure Active Directory Integracija s Picturepark
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Picturepark.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Picturepark klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Picturepark će moći znak u aplikaciju na vašem Picturepark web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Picturepark
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenarij")

##<a name="enabling-the-application-integration-for-picturepark"></a>Omogućivanje integraciju aplikacija za Picturepark
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Picturepark.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Picturepark, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Picturepark**.

    ![Galerija aplikacije] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Picturepark**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Picturepark sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za Picturepark potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikatu](http://youtu.be/YKQF266SAxI)...

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Picturepark** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Picturepark** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Picturepark na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*http://company.picturepark.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Picturepark** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Picturepark kao administrator.

6.  Na alatnoj traci na vrhu kliknite **stavku Administrativni alati**, a zatim **Management Console**.

    ![Management Console] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")

7.  Kliknite **Provjera autentičnosti**, a zatim kliknite **davatelji identiteta**.

    ![Provjera autentičnosti] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Provjera autentičnosti")

8.  U odjeljku **Konfiguriranje davatelja identiteta** poduzeti sljedeće korake:

    ![Konfiguriranje davatelja identiteta] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Konfiguriranje davatelja identiteta")

    1.  Kliknite **Dodaj**.
    2.  Upišite naziv za konfiguraciju.
    3.  Odaberite **Postavi kao zadano**.
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Picturepark** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **URI izdavač** .
    5.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir **Pouzdana ispis palac izdavač** .  

        >[AZURE.TIP]Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    6.  Kliknite **JoinDefaultUsersGroup**.
    7.  Da biste postavili atribut **Emailaddress** u tekstni okvir **zahtjeva** , unesite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Konfiguracija] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Konfiguracija")
    8.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Picturepark, oni mora biti dodijeljena u Picturepark.  
U slučaju Picturepark, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **Picturepark** .

2.  Na alatnoj traci na vrhu kliknite **stavku Administrativni alati**, a zatim **korisnika**.

    ![Korisnici] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Korisnici")

3.  Na kartici **Pregled korisnika** kliknite **Novo**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Upravljanje korisnicima")

4.  U dijaloškom okviru **Create User** poduzeti sljedeće korake:

    ![Stvaranje korisnika] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Stvaranje korisnika")

    1.  Vrsta na: **adresu e-pošte**, **lozinku**, **potvrdite lozinku**, **ime**, **Prezime**, **tvrtke**, **državu**, **poštanski broj**, **Grad** valjani Azure Active Directory korisnika koje želite ostale dodjele resursa u povezani tekstni okviri.
    2.  Odaberite **jezik**.
    3.  Kliknite **Stvori**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Picturepark korisnički račun alate za stvaranje ili API-ji nudi Picturepark dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Da biste korisnicima dodijelili Picturepark, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Picturepark **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).