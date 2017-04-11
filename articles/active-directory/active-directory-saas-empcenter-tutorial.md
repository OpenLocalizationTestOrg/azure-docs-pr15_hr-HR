<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s EmpCenter | Microsoft Azure" 
    description="Saznajte kako koristiti EmpCenter s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Praktični vodič: Azure Active Directory Integracija s EmpCenter
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i EmpCenter.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na EmpCenter jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili EmpCenter će moći znak u aplikaciju na vašem EmpCenter web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za EmpCenter
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenarij")
##<a name="enabling-the-application-integration-for-empcenter"></a>Omogućivanje integraciju aplikacija za EmpCenter
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za EmpCenter.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za EmpCenter, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **EmpCenter**.

    ![Galerija aplikacije] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **EmpCenter**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti EmpCenter sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **EmpCenter** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili EmpCenter** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje postavki aplikacije] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Konfiguriranje postavki aplikacije")

    1.  U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju EmpCenter (npr.: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Kliknite **Dalje**

4.  Na stranici **Konfiguracija jedinstvenu prijavu na EmpCenter** da biste preuzeli metapodatka, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje datoteke preuzete metapodataka EmpCenter službi za podršku.

    >[AZURE.NOTE] Službi za podršku EmpCenter mora konfigurirati stvarni SSO.
Dobit ćete obavijest kada SSO je omogućeno za vašu pretplatu.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u EmpCenter, oni mora biti dodijeljena u EmpCenter.  
Slučaju EmpCenter, korisničke račune morati stvoriti tako da EmpCenter za podršku.

>[AZURE.NOTE] Možete koristiti bilo koji drugi EmpCenter korisnički račun alate za stvaranje ili API-ji nudi EmpCenter Dodjela resursa za Azure Active Directory korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Da biste korisnicima dodijelili EmpCenter, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **EmpCenter **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).