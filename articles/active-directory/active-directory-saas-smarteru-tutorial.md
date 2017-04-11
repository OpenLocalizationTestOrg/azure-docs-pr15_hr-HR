<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s SmarterU | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory SmarterU da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Praktični vodič: Azure Active Directory Integracija s SmarterU
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i SmarterU.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   SmarterU klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili SmarterU će moći znak u aplikaciju na vašem SmarterU web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za SmarterU
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenarij")

##<a name="enabling-the-application-integration-for-smarteru"></a>Omogućivanje integraciju aplikacija za SmarterU
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za SmarterU.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za SmarterU, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **SmarterU**.

    ![Fallery aplikacije] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Fallery aplikacije")

7.  U oknu s rezultatima odaberite **SmarterU**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti SmarterU sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **SmarterU** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili SmarterU** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Konfiguriranje jedinstvenu prijavu")

3.  Na na **Konfiguriraj jedinstvenu prijavu na SmarterU** stranice da biste preuzeli metapodaci, kliknite **Preuzimanje metapodataka**, a zatim podatke Lokalno pohrani kao **c:\\SmarterUMetaData.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Konfiguriranje jedinstvenu prijavu")

4.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke SmarterU kao administrator.

5.  Na alatnoj traci na vrhu kliknite **Postavke računa**.

    ![Postavke računa] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Postavke računa")

6.  Na stranici Konfiguracija računa, poduzmite sljedeće korake:

    ![Vanjski autorizacije] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Vanjski autorizacije")

    1.  Odaberite **Omogući vanjskim autorizacije**.
    2.  U odjeljku **Kontrola matrica prijave** odaberite karticu **SmarterU** .
    3.  U odjeljku **Zadani Prijava korisnika** odaberite karticu **SmarterU** .
    4.  Odaberite **Omogući Okta**.
    5.  Kopirajte sadržaj datoteke preuzete metapodataka, a zatim je zalijepite u tekstni okvir **Okta metapodataka** .
    6.  Kliknite **Spremi**.

7.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u SmarterU, oni mora biti dodijeljena u SmarterU.  
U slučaju SmarterU, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **SmarterU** .

2.  Idite na **korisnike**.

3.  U odjeljku korisnički poduzeti sljedeće korake:

    ![Novi korisnik] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Novi korisnik")

    1.  Kliknite **+ korisnika**.
    2.  Upišite vrijednosti povezanog atributa Azure AD korisničkog računa u sljedeći tekstni okviri: **primarni za e-poštu**, **ID zaposlenika**, **lozinku**, **Provjerite je li lozinka**, **dobiti naziv**, **Prezime**.
    3.  Kliknite **aktivna**.
    4.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi SmarterU korisnički račun alate za stvaranje ili API-ji nudi SmarterU dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Da biste korisnicima dodijelili SmarterU, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **SmarterU **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).