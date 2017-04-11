<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Overdrive knjige | Microsoft Azure" 
    description="Saznajte kako koristiti Overdrive knjige sa servisa Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Praktični vodič: Azure Active Directory Integracija s Overdrive adresari
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i OverDrive.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na OverDrive jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili OverDrive će moći znak u aplikaciju na vašem OverDrive web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za OverDrive
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Scenarij")
##<a name="enabling-the-application-integration-for-overdrive"></a>Omogućivanje integraciju aplikacija za OverDrive
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za OverDrive.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za OverDrive, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **OverDrive**.

    ![Galerija aplikacije] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **OverDrive**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti OverDrive sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **OverDrive** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako želite korisnicima da biste se prijavili OverDrive** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **OverDrive URL za prijavu** upišite URL pomoću sljedećeg uzorka "*http://mslibrarytest.libraryreserve.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Konfiguriranje URL adresa Web App")

4.  Na **Konfiguriraj jedinstvenu prijavu na OverDrive** stranici da biste preuzeli datoteku metapodataka i poslali ga u OverDrive timu za podršku.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Konfiguriranje jedinstvenu prijavu")

    >[AZURE.NOTE]Podršku OverDrive konfigurira jedinstvene prijave za vas i šalje vam obavijest dovršetku konfiguracije.

5.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjele resursa za OverDrive.  
Kada se pokuša dodijeljenog korisnika da se prijavite u OverDrive, račun OverDrive automatski se stvara po potrebi.

>[AZURE.NOTE]Možete koristiti bilo koji drugi OverDrive korisnički račun alate za stvaranje ili API-ji nudi OverDrive dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Da biste korisnicima dodijelili OverDrive, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **OverDrive **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).