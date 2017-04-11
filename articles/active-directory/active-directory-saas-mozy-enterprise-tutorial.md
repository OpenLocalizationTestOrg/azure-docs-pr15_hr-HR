<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Mozy Enterprise | Microsoft Azure" 
    description="Saznajte kako koristiti Mozy Enterprise s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Praktični vodič: Azure Active Directory Integracija s Mozy Enterprise
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Mozy Enterprise.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Mozy Enterprise klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Mozy Enterprise će moći znak u aplikaciju na vaše tvrtke Mozy web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Mozy Enterprise
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Scenarij")
##<a name="enabling-the-application-integration-for-mozy-enterprise"></a>Omogućivanje integraciju aplikacija za Mozy Enterprise
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Mozy Enterprise.

###<a name="to-enable-the-application-integration-for-mozy-enterprise-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Mozy Enterprise, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **mozy enterprise**.

    ![Galerija aplikacije] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Mozy Enterprise**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Mozy Enterprise] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Mozy Enterprise")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Mozy Enterprise sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 klijent sustava Mozy Enterprise.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici Integracija **Mozy Enterprise** aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Mozy Enterprise** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Mozy Enterprise URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. Mozyenterprise.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Mozy Enterprise** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Mozy Enterprise kao administrator.

6.  U odjeljku **Konfiguriranje** kliknite **Pravila za provjeru autentičnosti**.

    ![Pravila za provjeru autentičnosti] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Pravila za provjeru autentičnosti")

7.  U odjeljku **Pravila za provjeru autentičnosti** poduzeti sljedeće korake:

    ![Pravila za provjeru autentičnosti] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Pravila za provjeru autentičnosti")

    1.  Odaberite **imeničkom servisu** **davatelja usluga**.
    2.  Odaberite **Koristi automatske LDAP**.
    3.  Kliknite karticu **SAML provjeru autentičnosti** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Mozy Enterprise** kopirajte vrijednost **URL zahtjev za provjeru autentičnosti** i pa ih zalijepite u tekstni okvir **URL za provjeru autentičnosti** .
    5.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Mozy Enterprise** kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **SAML krajnjoj točki** .
    6.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    7.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim zalijepite cijelu certifikat u tekstni okvir za **Potvrdu SAML** .
    8.  Odaberite **Omogući SSO za administratore da biste se prijavili pomoću vjerodajnica mreže**.
    9.  Kliknite **Spremi promjene**.

8.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Mozy Enterprise** odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Mozy Enterprise, oni mora biti dodijeljena u Mozy Enterprise.  
U slučaju Mozy Enterprise dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **Mozy Enterprise** .

2.  Kliknite **korisnici**, a zatim kliknite **Dodaj novog korisnika**.

    ![Korisnici] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Korisnici")

    >[AZURE.NOTE]Mogućnost **Dodaj novog korisnika** samo prikazuje se samo ako je **Mozy** odabran kao davatelj u odjeljku **pravila za provjeru autentičnosti**. Ako je konfiguriran za provjeru autentičnosti SAML zatim korisnika automatski se dodaju na njihove prvi Prijava putem znak na.

3.  U dijaloškom okviru za novi korisnik poduzeti sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Dodavanje korisnika")

    1.  Na popisu **Odaberite grupu** odaberite grupu.
    2.  Na popisu **vrsti korisnika** odaberite vrstu.
    3.  U tekstni okvir **korisničko ime** upišite ime korisnika za Azure AD.
    4.  U tekstni okvir **e-pošte** upišite adresu e-pošte korisnika za Azure AD.
    5.  Odaberite **Pošalji e-pošte korisnika upute**.
    6.  Kliknite **Dodavanje korisnika**.

    >[AZURE.NOTE]Nakon stvaranja korisnika, poruke e-pošte poslat će se korisniku Azure AD koja sadrži vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Mozy Enterprise korisnički račun alate za stvaranje ili API-ji nudi Mozy Enterprise dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
 
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-mozy-enterprise-perform-the-following-steps"></a>Da biste korisnicima dodijelili Mozy Enterprise, učinite sljedeće:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Mozy Enterprise **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).