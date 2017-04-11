<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s AppDynamics | Microsoft Azure" 
    description="Saznajte kako koristiti AppDynamics s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Praktični vodič: Azure Active Directory Integracija s AppDynamics

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i AppDynamics. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na AppDynamics jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili AppDynamics će moći znak u aplikaciju na vašem AppDynamics web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za AppDynamics
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Scenarij")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Omogućivanje integraciju aplikacija za AppDynamics

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za AppDynamics.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za AppDynamics, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **AppDynamics**.

    ![Galerija aplikacije] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **AppDynamics**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti AppDynamics sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **AppDynamics** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Konfiguriranje jedan Proba")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili AppDynamics** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Konfiguriranje jedan Proba")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **AppDynamics na URL za prijavu** upišite URL koriste vaši korisnici prijave za AppDynamics ("*https://companyname.saas.appdynamics.com*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na AppDynamics** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Konfiguriranje jedan Proba")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke AppDynamics kao administrator.

6.  Na alatnoj traci na vrhu kliknite **Postavke**, a zatim kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Administracija")

7.  Kliknite karticu **Davatelja usluge provjere autentičnosti** .

    ![Davatelja usluge provjere autentičnosti] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Davatelja usluge provjere autentičnosti")

8.  U odjeljku **Davatelja usluge provjere autentičnosti** poduzeti sljedeće korake:

    ![Konfiguriranje SAML] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "Konfiguriranje SAML")

    1.  Kao **Davatelja usluge provjere autentičnosti**, odaberite **SAML**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na AppDynamics** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na AppDynamics** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL odjavite** .
    4.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **potvrdu**
    6.  Kliknite **Spremi**.
        ![Spremanje] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Spremanje")

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Konfiguriranje jedan Proba")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u AppDynamics, oni mora biti dodijeljena u AppDynamics.  
U slučaju AppDynamics, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u web-mjesto tvrtke AppDynamics kao administrator.

2.  Idite na **korisnike**, a zatim kliknite **+** da biste otvorili dijaloški okvir **Stvaranje korisnika** .

    ![Korisnici] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Korisnici")

3.  U odjeljku **Create User** poduzeti sljedeće korake:

    ![Stvaranje korisnika] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Stvaranje korisnika")

    1.  Unesite **korisničko ime**, **naziv**, **e-pošte**, **Novu lozinku**, **Ponovite novu lozinku** valjani AAD računa želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi AppDynamics korisnički račun alate za stvaranje ili API-ji nudi AppDynamics Dodjela Azure AD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Da biste korisnicima dodijelili AppDynamics, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **AppDynamics **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
