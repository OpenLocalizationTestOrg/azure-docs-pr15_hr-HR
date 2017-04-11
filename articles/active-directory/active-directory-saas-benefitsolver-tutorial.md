<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Benefitsolver | Microsoft Azure"
    description="Saznajte kako koristiti Benefitsolver s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Praktični vodič: Azure Active Directory Integracija s Benefitsolver

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Benefitsolver.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Benefitsolver jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Benefitsolver će moći jednom prijava u aplikacije pomoću [Uvod u Access ploče](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Benefitsolver
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenarij")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Omogućivanje integraciju aplikacija za Benefitsolver

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Benefitsolver.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Benefitsolver, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Benefitsolver**.

    ![Galerija aplikacije] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Benefitsolver**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Benefitsolver sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Aplikacija Benefitsolver očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** .  
Sljedeća slika prikazuje primjer za to.

![Atributi] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atributi")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Benefitsolver** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Benefitsolver** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje postavki aplikacije] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Konfiguriranje postavki aplikacije")

    1.  U tekstni okvir **URL na za prijavu** upišite **http://azure.benefitsolver.com**.
    2.  U tekstni okvir **URL odgovor** upišite **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Kliknite **Dalje**.

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Benefitsolver** da biste preuzeli metapodatka, kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfiguriranje jedinstvenu prijavu")

5.  Slanje datoteke preuzete metapodataka Benefitsolver službi za podršku.

    >[AZURE.NOTE] Službi za podršku Benefitsolver mora konfigurirati stvarni SSO.
Dobit ćete obavijest kada SSO je omogućeno za vašu pretplatu.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfiguriranje jedinstvenu prijavu")

7.  Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** .

    ![Atributi] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Atributi")

8.  Da biste dodali mapiranja obavezan atribut, poduzmite sljedeće korake:

    ![Atributi] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atributi")

  	|Naziv atributa|Vrijednost atributa|
  	|---|---|
  	|ClientID|Morate tu vrijednost zatražite od tima za podršku Benefitsolver.|
  	|ClientKey|Morate tu vrijednost zatražite od tima za podršku Benefitsolver.|
  	|LogoutURL|Morate tu vrijednost zatražite od tima za podršku Benefitsolver.|
  	|IDZaposlenika|Morate tu vrijednost zatražite od tima za podršku Benefitsolver.|

    1.  Za svaki redak podatke iz gornje tablice, kliknite **Dodaj korisnika atribut**.
    2.  U tekstni okvir **Naziv atributa** upišite naziv atributa koji se prikazuju za tom retku.
    3.  U tekstni okvir **Atributa vrijednosti** odaberite vrijednost atributa koji se prikazuju za tom retku.
    4.  Kliknite **dovrši**.

9.  Kliknite **Primijeni promjene**.

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Benefitsolver, oni mora biti dodijeljena u Benefitsolver.  
U slučaju Benefitsolver, zaposlenika podaci u aplikaciji popunjeno pomoću statistiku datoteke iz sustava HRIS (obično nightly).  

>[AZURE.NOTE] Možete koristiti bilo koji drugi Benefitsolver korisnički račun alate za stvaranje ili API-ji nudi Benefitsolver dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Da biste korisnicima dodijelili Benefitsolver, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Benefitsolver **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
