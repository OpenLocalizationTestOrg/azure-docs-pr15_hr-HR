<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Projectplace | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Projectplace da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Praktični vodič: Azure Active Directory Integracija s Projectplace
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Projectplace.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Projectplace jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Projectplace će moći znak u aplikaciju na vašem Projectplace web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Projectplace
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Scenarij")
##<a name="enabling-the-application-integration-for-projectplace"></a>Omogućivanje integraciju aplikacija za Projectplace
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Projectplace.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Projectplace, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Projectplace**.

    ![Galerija aplikacije] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Projectplace**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Projectplace sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Projectplace** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Konfiguriranje jedan Proba")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Projectplace** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Konfiguriranje jedan Proba")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Projectplace na URL za prijavu** upišite ProjectPlace klijentu URL (npr.: "*http://company.projectplace.com*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Projectplace** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka na vašem računalu.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Konfiguriranje jedan Proba")

5.  Slanje u metadatafile Projectplace timu za podršku.

    >[AZURE.NOTE] Konfiguracija jedan prijave mora izvoditi Projectplace timu za podršku. Dobit ćete obavijest čim se dovrši konfiguraciju.

6.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Konfiguriranje jedan Proba")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Projectplace, oni mora biti dodijeljena u Projectplace.  
U slučaju Projectplace, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Projectplace** kao administrator.

2.  Otvorite odjeljak **osobe**, a zatim kliknite **Članovi**.

    ![Osobe] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Osobe")

3.  Kliknite **Dodaj člana**.

    ![Dodavanje članova] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Dodavanje članova")

4.  U odjeljku **Dodavanje člana** , učinite sljedeće:

    ![Nove članove] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Nove članove")

    1.  U tekstni okvir **Nove članove** upišite adresu e-pošte valjani AAD račun koji želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **Pošalji**

        >[AZURE.NOTE] Vlasnik računa Azure Active Directory se šalje poruku e-pošte uključujući vezu da biste potvrdili račun prije postaje aktivna.
    
>[AZURE.NOTE]Možete koristiti bilo koji drugi Projectplace korisnički račun alate za stvaranje ili API-ji nudi Projectplace dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Da biste korisnicima dodijelili Projectplace, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Projectplace **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).