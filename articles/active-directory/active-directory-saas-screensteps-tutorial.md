<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ScreenSteps | Microsoft Azure" 
    description="Saznajte kako koristiti ScreenSteps s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Praktični vodič: Azure Active Directory Integracija s ScreenSteps
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i ScreenSteps.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   ScreenSteps klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili ScreenSteps će moći znak u aplikaciju na vašem ScreenSteps web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za ScreenSteps
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenarij")
##<a name="enabling-the-application-integration-for-screensteps"></a>Omogućivanje integraciju aplikacija za ScreenSteps
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ScreenSteps.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za ScreenSteps, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **ScreenSteps**.

    ![Galerija aplikacije] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **ScreenSteps**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ScreenSteps sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **ScreenSteps** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ScreenSteps** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **ScreenSteps URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. ScreenSteps.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na ScreenSteps** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke ScreenSteps kao administrator.

6.  Kliknite **Upravljanje računom**.

    ![Upravljanje računima] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Upravljanje računima")

7.  Kliknite **udaljene provjere autentičnosti**.

    ![Udaljena provjere autentičnosti] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Udaljena provjere autentičnosti")

8.  Kliknite **Stvori provjere autentičnosti krajnje**.

    ![Udaljena provjere autentičnosti] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Udaljena provjere autentičnosti")

9.  U odjeljku **Stvaranje krajnje provjere autentičnosti** poduzeti sljedeće korake:

    ![Stvaranje krajnje provjere autentičnosti] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Stvaranje krajnje provjere autentičnosti")

    1.  U tekstni okvir **Naslov** upišite naslov.
    2.  S popisa u **načinu rada** , odaberite **SAML**.
    3.  Kliknite **Stvori**.

10. U odjeljku **Udaljene krajnja točka za provjeru autentičnosti** poduzeti sljedeće korake:

    ![Krajnja točka udaljene provjere autentičnosti] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Krajnja točka udaljene provjere autentičnosti")

    1.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na ScreenSteps** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL daljinskog prijava** .
    2.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na ScreenSteps** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **Odjava URL-a** .
    3.  Kliknite **Odaberite datoteku**, a zatim prijenos preuzete certifikata.
    4.  Kliknite **Ažuriraj**.

11. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u **ScreenSteps**, oni mora biti dodijeljena u **ScreenSteps**.  
U slučaju **ScreenSteps**dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Dodjela korisničkog računa radi ScreenSteps, učinite sljedeće:

1.  Prijavite se u klijent **ScreenSteps** .

2.  Kliknite **Upravljanje računom**.

    ![Upravljanje računima] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Upravljanje računima")

3.  Kliknite **korisnici**.

    ![Korisnici] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Korisnici")

4.  Kliknite **Stvori korisnika**.

    ![Svi korisnici] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Svi korisnici")

5.  Na popisu **Korisnička uloga** odaberite ulogu za korisniku.

6.  U odjeljku korisnička uloga upišite**"ime**, **Prezime**, **e-pošte**, **prijavu**, **lozinke** i **Potvrda lozinke"**valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.

    ![Novi korisnik] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Novi korisnik")

7.  U odjeljku grupe odaberite **Grupu provjere autentičnosti (SAML)**, a zatim **Create User**.

    ![Grupa] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Grupa")

>[AZURE.NOTE]Možete koristiti bilo koji drugi ScreenSteps korisnički račun alate za stvaranje ili API-ji nudi ScreenSteps dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Da biste korisnicima dodijelili ScreenSteps, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **ScreenSteps **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Dodjela korisnika] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Dodjela korisnika")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).