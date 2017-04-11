<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s InsideView | Microsoft Azure" 
    description="Saznajte kako koristiti InsideView s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Praktični vodič: Azure Active Directory Integracija s InsideView
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i InsideView.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   InsideView klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili InsideView će moći znak u aplikaciju na vašem InsideView web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za InsideView
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenarij")
##<a name="enabling-the-application-integration-for-insideview"></a>Omogućivanje integraciju aplikacija za InsideView
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za InsideView.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za InsideView, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **InsideView**.

    ![Galerija aplikacije] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **InsideView**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti InsideView sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **InsideView** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Konfiguriranje jedan Proba")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili InsideView** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Konfiguriranje jedan Proba")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **URL odgovor InsideView** upišite InsideView SSO URL (npr.: `https://my.insideview.com/iv/<STS Name>/login.iv`), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na InsideView** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Konfiguriranje jedan Proba")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke InsideView kao administrator.

6.  Na alatnoj traci na vrhu kliknite **administrator**, **SingleSignOn postavke**, a zatim kliknite **Dodaj SAML**.

    ![SAML jedine prijave postavke] (./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML jedine prijave postavke")

7.  U odjeljku **Dodaj novi SAML** poduzeti sljedeće korake:

    ![Dodavanje nove SAML] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Dodavanje nove SAML")

    1.  U tekstni okvir **STS naziv** upišite naziv za konfiguraciju.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na InsideView** kopirajte vrijednost **Pokrenut davatelja usluge (SP) krajnjoj točki** i pa ih zalijepite u tekstni okvir **WS/SamlP-Fed Unsolicated krajnje** .
    3.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.
        
        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **STS certifikat**
    5.  U tekstni okvir **Crm korisnički Id mapiranja** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  U tekstni okvir **Mapiranja za Crm e-pošte** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  U tekstni okvir **Crm ime mapiranja** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  U tekstni okvir **mapiranja Crm Prezime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Kliknite **Spremi**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedan Proba] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Konfiguriranje jedan Proba")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u InsideView, oni mora biti dodijeljena u InsideView.  
U slučaju InsideView, dodjeljivanje jest zadatak za ručno.
  
Da biste korisnika ili kontakata stvorenih u InsideView, obratite se upravitelju uspjeh klijenta ili slanje poruke e-pošte**support@insideview.com**

>[AZURE.NOTE] Možete koristiti bilo koji drugi InsideView korisnički račun alate za stvaranje ili API-ji nudi InsideView Dodjela Azure AD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Da biste korisnicima dodijelili InsideView, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **InsideView **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).