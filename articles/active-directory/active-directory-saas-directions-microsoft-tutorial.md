<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s uputama na Microsoft | Microsoft Azure" 
    description="Saznajte kako pomoću uputa na Microsofta Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Praktični vodič: Azure Active Directory Integracija s uputama na Microsoft

Cilj ovog praktičnog vodiča je da biste prikazali integraciju sa servisom Azure Active Directory i uputa na Microsoft.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Kretanje u programu Microsoft pretplate

Ako još nemate pridruženim smjera još Microsoft pretplate na zahtjev za e-pošte "*service@DirectionsOnMicrosoft.com*".

Po dovršetku ovog praktičnog vodiča Azure Active Directory korisnika koje ste dodijelili upute na Microsoft će moći jednom prijava u aplikacije pomoću jedinstvene prijave.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija Microsoft ćete do uputa
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scenarij")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Omogućivanje integraciju aplikacija Microsoft ćete do uputa

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija Microsoft ćete do uputa.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija Microsoft ćete do uputa, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **upute na Microsoft**.

    ![Galerija aplikacije] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **upute na Microsoft**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Scenarij] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scenarij")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti upute na Microsoft sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **upute na Microsoft** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili upute na Microsoft** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Microsoft Azure AD Singel prijave] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel prijave")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir URL na za prijavu upišite **https://www.directionsonmicrosoft.com/user/login**, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Microsoft ćete do uputa** kliknite **Preuzimanje metapodataka**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Konfiguriranje jedinstvenu prijavu")

5.  Poslati datoteku metapodataka upute na podršku za Microsoft (*service@DirectionsOnMicrosoft.com*). Da biste omogućili upute na Microsoft timu za podršku da biste pronašli vaše članstvo vanjsko web-mjesta, uvrstite podatke o svojoj tvrtki u e-pošte.

    >[AZURE.NOTE] Jedinstvenu prijavu na Microsoft ćete do uputa mora biti omogućen prema uputama na podršku za Microsoft.
Primit ćete obavijest kada jedinstvenu prijavu omogućena.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje upute na Microsoft.  
Kada se pokuša dodijeljenog korisnika da se prijavite u upute na Microsoft pomoću ploče programa access, upute na Microsoft provjerava postoji li korisnik. Ako je dostupno korisnički račun još, automatski stvara prema uputama na Microsoft.
##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Da biste korisnicima dodijelili upute na Microsoft, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **upute na Microsoft **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Da")
