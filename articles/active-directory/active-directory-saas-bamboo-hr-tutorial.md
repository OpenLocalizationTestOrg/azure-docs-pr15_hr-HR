<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s bambus HR | Microsoft Azure" 
    description="Saznajte kako koristiti bambus HR s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Praktični vodič: Azure Active Directory Integracija s bambus ljudskih Resursa

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i BambooHR.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na BambooHR jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili BambooHR će moći znak u aplikaciju na vašem BambooHR web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za BambooHR
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenarij")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Omogućivanje integraciju aplikacija za BambooHR

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za BambooHR.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za BambooHR, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **BambooHR**.

    ![Galerija aplikacije] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **BambooHR**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti BambooHR sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **BambooHR** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Scenarij] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenarij")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili BambooHR** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **BambooHR na URL za prijavu** upišite URL koriste vaši korisnici da biste se prijavili aplikaciju BambooHR (npr.: https://company.bamboohr.com), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na BambooHR** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke BambooHR kao administrator.

6.  Na početnoj stranici, poduzmite sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Jedinstvenu prijavu")

    1.  Kliknite **aplikacije**.
    2.  Na izborniku aplikacije na lijevoj strani kliknite **Jedinstvenu prijavu**.
    3.  Kliknite **SAML jedinstvenu prijavu**.

7.  U odjeljku **SAML jedinstvenu prijavu** poduzeti sljedeće korake:

    ![SAML jedinstvenu prijavu] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML jedinstvenu prijavu")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na BambooHR** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL SSO za prijavu** .
    2.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Certifikat X.509**
    4.  Kliknite **Spremi**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u BambooHR, oni mora biti dodijeljena u BambooHR.  
U slučaju BambooHR, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto **BambooHR** kao administrator.

2.  Na alatnoj traci na vrhu kliknite **Postavke**.

    ![Postavljanje] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Postavljanje")

3.  Kliknite **Pregled**.

4.  U lijevom navigacijskom oknu, idite na **sigurnosti \> korisnika**.

5.  Upišite korisničko ime, lozinku i e-pošte adresu valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.

6.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi BambooHR korisnički račun alate za stvaranje ili API-ji nudi BambooHR dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Da biste korisnicima dodijelili BambooHR, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **BambooHR **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
