<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Syncplicity | Microsoft Azure" 
    description="Saznajte kako koristiti Syncplicity s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Praktični vodič: Azure Active Directory Integracija s Syncplicity
  
Cilj ovog praktičnog vodiča je da biste prikazali kako postaviti jedinstvenu prijavu između Azure Active Directory (Azure AD) i Syncplicity.
  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Syncplicity klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika kojemu ste dodijelili Syncplicity pristup bit će moći jedan znak u aplikaciju na vašem Syncplicity web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću ploče Azure AD pristup.

1.  Omogućivanje integraciju aplikacija za Syncplicity
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-syncplicity-tutorial/IC769524.png "Scenarij")

##<a name="enabling-the-application-integration-for-syncplicity"></a>Omogućivanje integraciju aplikacija za Syncplicity
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Syncplicity.

###<a name="to-enable-the-application-integration-for-syncplicity-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Syncplicity, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-syncplicity-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-syncplicity-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-syncplicity-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-syncplicity-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Syncplicity**.

    ![Galerija Syncplicity aplikacije] (./media/active-directory-saas-syncplicity-tutorial/IC769532.png "Galerija Syncplicity aplikacije")

7.  U oknu s rezultatima odaberite **Syncplicity**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Syncplicity] (./media/active-directory-saas-syncplicity-tutorial/IC769533.png "Syncplicity")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
U ovom se odjeljku opisuje kako omogućiti korisnicima za provjeru autentičnosti Syncplicity s računa u Azure Active Directory, pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Syncplicity** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-syncplicity-tutorial/IC769534.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Syncplicity** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Microsoft Azure AD jedinstvene prijave] (./media/active-directory-saas-syncplicity-tutorial/IC769535.png "Microsoft Azure AD jedinstvene prijave")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Syncplicity URL za prijavu** upišite URL korisnici koriste se prijaviti u aplikaciju Syncplicity kliknite **Dalje**. 

    URL za aplikaciju je Syncplicity klijentu URL-a (npr.: *http://company.Syncplicity.com*):

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-syncplicity-tutorial/IC769536.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Syncplicity** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a lokalno spremanje datoteka certifikata na računalo.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-syncplicity-tutorial/IC769543.png "Konfiguriranje jedinstvenu prijavu")

5.  Prijavite se u klijent **Syncplicity** .

6.  Na izborniku na vrhu kliknite **administrator**, odaberite **Postavke**, a zatim kliknite **Prilagođena domena i jedinstvenu prijavu**.

    ![Syncplicity] (./media/active-directory-saas-syncplicity-tutorial/IC769545.png "Syncplicity")

7.  Na stranici dijaloški okvir **Jedan prijavu (SSO)** , poduzmite sljedeće korake:

    ![Jedan prijave \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/IC769550.png "Single Sign-On \(SSO\)")

    1.  U tekstni okvir **Prilagođena domena** upišite naziv domene.
    2.  Odaberite **omogućen** kao **jedan Status za prijavu**.
    3.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na Syncplicity** kopirajte vrijednost **ID entitet** , a pa ih zalijepite u tekstni okvir **Entitet ID-a** .
    4.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na Syncplicity** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL stranice za prijavu** .
    5.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na Syncplicity** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL stranice odjavite** .
    6.  **Certifikat za davatelja identiteta**, kliknite **Odabir datoteka**, a prijenos certifikat koji ste preuzeli s portala za Azure klasični.
    7.  Kliknite **Spremi promjene**.

8.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Potvrda] (./media/active-directory-saas-syncplicity-tutorial/IC769554.png "Potvrda")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Za AAD da korisnici mogu se prijaviti, oni mora biti dodijeljena Syncplicity aplikaciju. U ovom se odjeljku opisuje kako stvoriti AAD korisničke račune u Syncplicity.

###<a name="to-provision-a-user-account-to-syncplicity-perform-the-following-steps"></a>Dodjela korisničkog računa radi Syncplicity, učinite sljedeće:

1.  Prijavite se u klijent **Syncplicity** (npr.: *https://company.Syncplicity.com*).

2.  Kliknite **administrator** , a zatim odaberite **korisničke račune**.

3.  Kliknite **Dodaj korisnika**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-syncplicity-tutorial/IC769764.png "Upravljanje korisnicima")

4.  Unesite **adresu e-pošte** računa AAD želite Dodjela odaberite **korisnik** kao **uloga**, a zatim kliknite **Dalje**.

    ![Informacije o računu] (./media/active-directory-saas-syncplicity-tutorial/IC769765.png "Informacije o računu")

    >[AZURE.NOTE] Vlasnik računa AAD će primiti poruku e-pošte uključujući vezu da biste potvrdili i aktivirali račun.

5.  Odaberite grupu u svojoj tvrtki da novi korisnik mora postati član, a zatim kliknite **Dalje**.

    ![Članstvo u grupi] (./media/active-directory-saas-syncplicity-tutorial/IC769772.png "Članstvo u grupi")

    >[AZURE.NOTE] Ako nema grupa na popisu, samo kliknite **Dalje**.

6.  Odaberite mape koje želite smjestiti u odjeljku Syncplicity na kontrolu na korisnikovu računalu, a zatim kliknite **Dalje**.

    ![Syncplicity mape] (./media/active-directory-saas-syncplicity-tutorial/IC769773.png "Syncplicity mape")

>[AZURE.NOTE] Možete koristiti bilo koji drugi Syncplicity korisnički račun alate za stvaranje ili API-ji nudi Syncplicity dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-syncplicity-perform-the-following-steps"></a>Da biste korisnicima dodijelili Syncplicity, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Syncplicity** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-syncplicity-tutorial/IC769557.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-syncplicity-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).

