<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s 15Five | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory 15Five da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Praktični vodič: Azure Active Directory Integracija s 15Five

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i 15Five. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na 15Five jedinstvenu prijavu omogućeno pretplate

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili 15Five će moći znak u aplikaciju na vašem 15Five web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za 15Five
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-15five-tutorial/IC784667.png "Scenarij")
##<a name="enabling-the-application-integration-for-15five"></a>Omogućivanje integraciju aplikacija za 15Five

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za 15Five.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za 15Five, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-15five-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-15five-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-15five-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **15Five**.

    ![Galerija aplikacije] (./media/active-directory-saas-15five-tutorial/IC784668.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **15Five**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti 15Five sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **15Five** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-15five-tutorial/IC784670.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili 15Five** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-15five-tutorial/IC784671.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **15Five URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.15Five.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-15five-tutorial/IC784672.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na 15Five** kliknite **Preuzmi metapodataka**i prosljeđivanje datoteku metapodataka 15Five timu za podršku.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-15five-tutorial/IC784673.png "Konfiguriranje jedinstvenu prijavu")

    >[AZURE.NOTE] Jedinstvenu prijavu mora biti omogućen od tima za podršku 15Five.

5.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-15five-tutorial/IC784674.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u 15Five, oni mora biti dodijeljena u 15Five.  
U slučaju 15Five, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **15Five** kao administrator.

2.  Otvorite **Upravljanje tvrtke**.

    ![Upravljanje tvrtke] (./media/active-directory-saas-15five-tutorial/IC784675.png "Upravljanje tvrtke")

3.  Idite na **osobe \> dodavanje osoba**.

    ![Osobe] (./media/active-directory-saas-15five-tutorial/IC784676.png "Osobe")

4.  U odjeljku Dodavanje nove osobe poduzeti sljedeće korake:

    ![Dodavanje nove osobe] (./media/active-directory-saas-15five-tutorial/IC784677.png "Dodavanje nove osobe")

    1.  Upišite **ime**, **Prezime**, **Naslov**, **adresu e-pošte** valjani Azure Active Directory računa želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **gotovo**.

    >[AZURE.NOTE] Vlasnik računa Azure AD primit će poruku e-pošte uključujući vezu da biste potvrdili račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi 15Five korisnički račun alate za stvaranje ili API-ji nudi 15Five dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Da biste korisnicima dodijelili 15Five, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **15Five **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-15five-tutorial/IC784678.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-15five-tutorial/IC767830.png "Da")

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
