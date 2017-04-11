<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Kintone | Microsoft Azure" 
    description="Saznajte kako koristiti Kintone s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Praktični vodič: Azure Active Directory Integracija s Kintone
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Kintone.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Kintone jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Kintone će moći znak u aplikaciju na vašem Kintone web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Kintone
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Scenarij")
##<a name="enabling-the-application-integration-for-kintone"></a>Omogućivanje integraciju aplikacija za Kintone
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Kintone.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Kintone, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Kintone**.

    ![Galerija aplikacije] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Kintone**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Kintone sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Kintone** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Kintone** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Kintone na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://company.kintone.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Kintone** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke **Kintone** kao administrator.

6.  Kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Postavke")

7.  Kliknite **korisnika i administratora sustava**.

    ![Korisnici i administracijom sustava] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Korisnici i administracijom sustava")

8.  U odjeljku **administracijom sustava \> sigurnost** kliknite **Prijava**.

    ![Prijava] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Prijava")

9.  Kliknite **Omogući SAML provjeru autentičnosti**.

    ![Provjera autentičnosti SAML] (./media/active-directory-saas-kintone-tutorial/IC785882.png "Provjera autentičnosti SAML")

10. U odjeljku Provjera autentičnosti SAML poduzeti sljedeće korake:

    ![Provjera autentičnosti SAML] (./media/active-directory-saas-kintone-tutorial/IC785883.png "Provjera autentičnosti SAML")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Kintone** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Kintone** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL odjavite** .
    3.  Kliknite **Pregledaj** da biste prenijeli preuzete certifikata.
    4.  Kliknite **Spremi**.

11. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Kintone, oni mora biti dodijeljena u Kintone.  
U slučaju Kintone, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Kintone** kao administrator.

2.  Kliknite **Postavke**.

    ![Postavke] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Postavke")

3.  Kliknite **korisnika i administratora sustava**.

    ![Administriranje sustava & korisnika] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Administriranje sustava & korisnika")

4.  U odjeljku **Administracija korisnika**kliknite **odjela i korisnicima**.

    ![Odjela i korisnika] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Odjela i korisnika")

5.  Kliknite **novi korisnik**.

    ![Novi korisnici] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Novi korisnici")

6.  U odjeljku **Novi korisnik** poduzeti sljedeće korake:

    ![Novi korisnici] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Novi korisnici")

    1.  Upišite **Zaslonsko ime**, **Korisničko ime za prijavu**, **Novu lozinku**, **Potvrdite lozinku**, **Adresu e-pošte** i druge detalje valjani AAD račun koji želite dodjele resursa u povezanim texboxes.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Kintone korisnički račun alate za stvaranje ili API-ji nudi Kintone dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Da biste korisnicima dodijelili Kintone, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Kintone **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).