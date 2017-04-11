<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Egnyte | Microsoft Azure" 
    description="Saznajte kako koristiti Egnyte s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Praktični vodič: Azure Active Directory Integracija s Egnyte
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Egnyte.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Egnyte jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Egnyte će moći znak u aplikaciju na vašem Egnyte web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md)
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Egnyte
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-egnyte-tutorial/IC787812.png "Scenarij")
##<a name="enabling-the-application-integration-for-egnyte"></a>Omogućivanje integraciju aplikacija za Egnyte
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Egnyte.

###<a name="to-enable-the-application-integration-for-egnyte-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Egnyte, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-egnyte-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-egnyte-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-egnyte-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **egnyte**.

    ![Galerija aplikacije] (./media/active-directory-saas-egnyte-tutorial/IC787813.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Egnyte**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Egnyte] (./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Egnyte sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Egnyte** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-egnyte-tutorial/IC787815.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Egnyte** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-egnyte-tutorial/IC787816.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Egnyte URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.egnyte.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-egnyte-tutorial/IC787817.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Egnyte** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-egnyte-tutorial/IC787818.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Egnyte kao administrator.

6.  Kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-egnyte-tutorial/IC787819.png "Postavke")

7.  Na izborniku kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-egnyte-tutorial/IC787820.png "Postavke")

8.  Kliknite karticu **Konfiguracija** , a zatim kliknite **Sigurnost**.

    ![Sigurnost] (./media/active-directory-saas-egnyte-tutorial/IC787821.png "Sigurnost")

9.  U odjeljku **Jedan prijave provjere autentičnosti** poduzeti sljedeće korake:

    ![Jedine prijave provjere autentičnosti] (./media/active-directory-saas-egnyte-tutorial/IC787822.png "Jedine prijave provjere autentičnosti")

    1.  Kao **jedan prijave provjeru autentičnosti**, odaberite **SAML 2.0**.
    2.  Kao **davatelja identiteta**, odaberite **AzureAD**.
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Egnyte** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Egnyte** kopirajte vrijednost **ID entitet** , a pa ih zalijepite u tekstni okvir **ID za entitet davatelja identiteta** .
    5.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **potvrdu davatelja identiteta** .
    7.  Kao **zadani korisničkog mapiranja**, odaberite **adresu e-pošte**.
    8.  Kao **vrijednost koristi izdavač specifične za domene**, odaberite **onemogućene**.
    9.  Kliknite **Spremi**.

10. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-egnyte-tutorial/IC787823.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Egnyte, oni mora biti dodijeljena u Egnyte.  
U slučaju Egnyte, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke Egnyte **Egnyte** kao administrator.

2.  Idite na **Postavke \> korisnici i grupe**.

3.  Kliknite **Dodaj novog korisnika**, a zatim odaberite željenu vrstu korisnika kojeg želite dodati.

    ![Korisnici] (./media/active-directory-saas-egnyte-tutorial/IC787824.png "Korisnici")

4.  U odjeljku **Novi korisnik standardni** poduzeti sljedeće korake:

    ![Novi korisnik Standard] (./media/active-directory-saas-egnyte-tutorial/IC787825.png "Novi korisnik Standard")

    1.  Upišite **e-pošte**, **korisničko ime** i druge detalje valjani Azure Active Directory račun koji želite dodjele resursa.
    2.  Kliknite **Spremi**.

    >[AZURE.NOTE] Vlasnik računa Azure Active Directory će primiti obavijest e-pošte.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Egnyte korisnički račun alate za stvaranje ili API-ji nudi Egnyte dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-egnyte-perform-the-following-steps"></a>Da biste korisnicima dodijelili Egnyte, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Egnyte **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-egnyte-tutorial/IC787826.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-egnyte-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).