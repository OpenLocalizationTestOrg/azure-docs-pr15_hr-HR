<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Boomi | Microsoft Azure" 
    description="Saznajte kako koristiti Boomi s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Praktični vodič: Azure Active Directory Integracija s Boomi

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Boomi.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Boomi jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Boomi će moći znak u aplikaciju na vašem Boomi web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Boomi
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenarij")
##<a name="enabling-the-application-integration-for-boomi"></a>Omogućivanje integraciju aplikacija za Boomi

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Boomi.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Boomi, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Boomi**.

    ![Galerija aplikacije] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Boomi**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Boomi sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Boomi** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Boomi** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **URL odgovor Boomi** upišite **Boomi AtomSphere prijava URL** (npr.: "*https://platform.boomi.com/sso/AccountName/saml*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Boomi** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Boomi kao administrator.

6.  Na alatnoj traci na vrhu kliknite na naziv tvrtke, a zatim **Postavljanje**.

    ![Postavljanje] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Postavljanje")

7.  Kliknite **Mogućnosti SSO**.

    ![Mogućnosti jedinstvene Prijave] (./media/active-directory-saas-boomi-tutorial/IC790829.png "Mogućnosti jedinstvene Prijave")

8.  U odjeljku **Jedan mogućnosti za prijavu** učinite sljedeće:

    ![Jedan mogućnosti za prijavu] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Jedan mogućnosti za prijavu")

    1.  Odaberite **Omogući SAML jedinstvenu prijavu**.
    2.  Kliknite **Uvezi**, da biste prenijeli preuzete certifikata.
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Boomi** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .
    4.  **Vanjski pristup Id mjesto**, odaberite **vanjski pristup Id se element NameID predmeta**.
    5.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Boomi, oni mora biti dodijeljena u Boomi.  
U slučaju Boomi, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Boomi** kao administrator.

2.  Idite na **Upravljanje korisnicima \> korisnika**.

    ![Korisnici] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Korisnici")

3.  Kliknite **Dodaj korisnika**.

    ![Dodavanje korisnika] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Dodavanje korisnika")

4.  Na stranici dijaloški okvir **Dodavanje uloge korisnika** , poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Dodavanje korisnika")

    1.  Upišite **ime**, **Prezime** i **e-pošte** valjan račun za AAD želite Dodjela resursa u povezani tekstni okviri.
    2.  Kliknite u redu.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Boomi korisnički račun alate za stvaranje ili API-ji nudi Boomi dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Da biste korisnicima dodijelili Boomi, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Boomi **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
