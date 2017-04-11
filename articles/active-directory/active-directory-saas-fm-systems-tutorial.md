<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s FM: sustavi | Microsoft Azure" 
    description="Saznajte kako koristiti FM: sustavima s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Praktični vodič: Azure Active Directory Integracija s FM: sustavi
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i FM:Systems.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na FM:Systems jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili FM:Systems će moći znak u aplikaciju na vašem FM:Systems web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za FM:Systems
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Scenarij")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Omogućivanje integraciju aplikacija za FM:Systems
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za FM:Systems.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za FM:Systems, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **FM:Systems**.

    ![Galerija aplikacije] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **FM:Systems**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![FM: sustavi] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: sustavi")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti FM:Systems sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **FM:Systems** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili FM:Systems** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Konfiguriranje URL adresa Web App")

    1.  U tekstni okvir **FM:Systems na URL za prijavu** upišite FM:Systems **Odgovor URL** (npr.: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Tu vrijednost možete pristupiti iz vaše FM: sustavi službi za podršku.

    2.  Kliknite **Dalje**

4.  Na stranici **Konfiguracija jedinstvenu prijavu na FM:Systems** da biste preuzeli metapodatka, kliknite **Preuzmi metapodataka**i metapodatke spremili na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje datoteke preuzete metapodataka za vaše FM: sustavi službi za podršku.

    >[AZURE.NOTE] FM: Mora konfigurirati stvarni SSO sustavi za podršku.
Dobit ćete obavijest kada SSO je omogućeno za vašu pretplatu.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u FM:Systems, oni mora biti dodijeljena u FM:Systems.  
U slučaju FM:Systems, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  U prozoru web-preglednika, prijavite se u web-mjesto tvrtke FM:Systems kao administrator.

2.  Idite na **administracijom sustava \> upravljanje sigurnosnim \> korisnika \> popis korisnika**.

    ![Administriranje sustava] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Administriranje sustava")

3.  Kliknite **Stvaranje novog korisnika**.

    ![Stvaranje novog korisnika] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Stvaranje novog korisnika")

4.  U odjeljku **Create User** poduzeti sljedeće korake:

    ![Stvaranje korisnika] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Stvaranje korisnika")

    1.  Unesite korisničko ime, lozinku i njegov potvrdu, adresu e-pošte i ID zaposlenika valjani Azure Active Directory računa koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Dalje**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi FM:Systems korisnički račun alate za stvaranje ili API-ji nudi FM:Systems dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Da biste korisnicima dodijelili FM:Systems, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **FM:Systems **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).