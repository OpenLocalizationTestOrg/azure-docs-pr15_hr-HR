<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s nove Relic | Microsoft Azure" 
    description="Saznajte kako koristiti novi Relic s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Praktični vodič: Azure Active Directory Integracija s novi Relic
  
Cilj ovog praktičnog vodiča je da biste prikazali kako postaviti jedinstvenu prijavu između Azure Active Directory i novi Relic.
  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na novu Relic jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure Active Directory korisnika koje ste dodijelili novi Relic će moći jedinstvenu prijavu pomoću AAD ploče programa Access.

1.  Omogućivanje integraciju aplikacija za nove Relic
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Scenarij")
##<a name="enabling-the-application-integration-for-new-relic"></a>Omogućivanje integraciju aplikacija za nove Relic
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za nove Relic.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za nove Relic, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Novi Relic**.

    ![Galerija aplikacije] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Galerija aplikacije")

7.  U oknu s rezultatima, odaberite **Novi Relic**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Novi Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Novi Relic")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
U ovom se odjeljku opisuje kako omogućiti korisnicima za provjeru autentičnosti novi Relic s računa u Azure Active Directory, pomoću vanjskog pristupa na temelju SAML protokol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Novi Relic** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili novu Relic** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Novi Relic na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili novu Relic aplikacija, a zatim kliknite **Dalje**. 

    URL za aplikaciju je novi Relic klijentu URL-a (npr.: *https://rpm.newrelic.com*):

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na novi Relic** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a lokalno spremanje datoteka certifikata na računalo.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web prijavite se web-mjesto tvrtke **Novi Relic** kao administrator.

6.  Na izborniku na vrhu kliknite **Postavke računa**.

    ![Postavke računa] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Postavke računa")

7.  Kliknite karticu **Sigurnost i provjeru autentičnosti** , a zatim na kartici **jedine prijave** .

    ![Jedinstvenu prijavu] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Jedinstvenu prijavu")

8.  Na stranici dijaloški SAML poduzeti sljedeće korake:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Kliknite **Odabir datoteke** za prijenos preuzete certifikata Azure Active Directory.
    2.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na novi Relic** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL daljinska prijava** .
    3.  Azure klasični portalu na stranici **Konfiguracija jedinstvenu prijavu na novi Relic** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **odjavite odredišne URL-a** .
    4.  Kliknite **Spremi promjene**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure Active Directory korisnika da se prijavite u novi Relic, oni mora biti dodijeljena u novi Relic.  
U slučaju novi Relic dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Dodjela korisničkog računa radi novi Relic, učinite sljedeće:

1.  Prijavite se na web-mjesto tvrtke **Novi Relic** kao administrator.

2.  Na izborniku na vrhu kliknite **Postavke računa**.

    ![Postavke računa] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Postavke računa")

3.  U oknu s **računa** na lijevoj strani kliknite **Sažetak**, a zatim **Dodaj korisnika**.

    ![Postavke računa] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Postavke računa")

4.  U dijaloškom okviru **aktivni korisnici** poduzeti sljedeće korake:

    ![Aktivni korisnici] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktivni korisnici")

    1.  U tekstni okvir **e-pošte** upišite adresu e-pošte valjani Azure Active Directory korisnika kojem želite da dodjele resursa.
    2.  Kao **uloga** odaberite **korisnik**.
    3.  Kliknite **Dodaj korisnika**.

>[AZURE.NOTE]Možete koristiti bilo koje druge novi Relic korisnički račun alate za stvaranje ili API-ji nudi novu Relic dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Da biste korisnicima dodijelite novu Relic, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Novi Relic** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).




