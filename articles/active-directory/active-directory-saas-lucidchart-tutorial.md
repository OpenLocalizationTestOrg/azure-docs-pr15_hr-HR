<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Lucidchart | Microsoft Azure" 
    description="Saznajte kako koristiti Lucidchart s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Praktični vodič: Azure Active Directory Integracija s Lucidchart
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Lucidchart.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na Lucidchart jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Lucidchart će moći znak u aplikaciju na vašem Lucidchart web-mjesto tvrtke (znak servis davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Lucidchart
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Scenarij")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Omogućivanje integraciju aplikacija za Lucidchart
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Lucidchart.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Lucidchart, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Lucidchart**.

    ![Galerija aplikacije] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Lucidchart**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Lucidchart sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Lucidchart** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Lucidchart** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Lucidchart na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili aplikaciju Lucidchart (npr.: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Lucidchart** da biste preuzeli metapodatka, kliknite **Preuzmi metapodataka**, a zatim spremite datoteku podataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Lucidchart kao administrator.

6.  Na izborniku na vrhu kliknite **tima**.

    ![Tim] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Tim")

7.  Kliknite **aplikacije \> upravljanje SAML**.

    ![Upravljanje SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "Upravljanje SAML")

8.  Na stranici dijaloški okvir **Postavke provjere autentičnosti SAML** poduzeti sljedeće korake:

    1.  Odaberite **Omogući provjeru autentičnosti SAML**, a zatim kliknite **neobavezno**.
        ![Postavke provjere autentičnosti SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "Postavke provjere autentičnosti SAML")
    2.  U tekstni okvir **domenu** upišite domenu, a zatim **Promjena certifikata**.
        ![Promjena certifikata] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Promjena certifikata")
    3.  Otvorite metapodataka preuzetu datoteku, kopirajte sadržaj i pa ih zalijepite u tekstni okvir **Prijenos metapodataka** .
        ![Prijenos metapodataka] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Prijenos metapodataka")
    4.  Odaberite **automatski Dodaj novog korisnika u tim**, a zatim kliknite **Spremi promjene**.
        ![Spremanje promjena] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Spremanje promjena")

9.  Odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Postoji nijedna stavka akcija za konfiguriranje korisnika dodjeljivanje Lucidchart.  
Kada se pokuša dodijeljenog korisnika da se prijavite u Lucidchart pomoću ploče programa access, Lucidchart provjerava postoji li korisnik.  
Ako je dostupno korisnički račun još, automatski stvorena je pomoću Lucidchart.
##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Da biste korisnicima dodijelili Lucidchart, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Lucidchart **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).