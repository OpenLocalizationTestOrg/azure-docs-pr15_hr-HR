<properties 
    pageTitle="Praktični vodič: Azure Active Directory integracije sa softverom okupljanje | Microsoft Azure" 
    description="Saznajte kako koristiti softver Rally s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Praktični vodič: Azure Active Directory integracije sa softverom okupljanje
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Rally softver.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Rally softver klijenta
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Rally softver
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scenarij")
##<a name="enabling-the-application-integration-for-rally-software"></a>Omogućivanje integraciju aplikacija za Rally softver
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Rally softvera.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Rally softver, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **okupljanje**.

    ![Galerija aplikacije] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Rally softver**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Rally softver] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally softver")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Rally softver sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Ovaj postupak, dio su potrebne da biste prenijeli certifikat na Rally softver.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Rally softver **kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako želite korisnicima da biste se prijavili okupljanje** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Microsoft Azure AD jedinstvene prijave] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD jedinstvene prijave")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **Rally softver klijentu URL-a** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. rally.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na okupljanje** kliknite preuzimanje metapodataka i spremite je na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Konfiguriranje jedinstvenu prijavu")

5.  Prijavite se u klijent **Rally softvera** .

6.  Na alatnoj traci na vrhu kliknite **Postavljanje**, a zatim **Pretplata**.

    ![Pretplate] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Pretplate")

7.  Kliknite gumb **Akcije** na alatnoj traci na vrhu na desnoj strani, a zatim odaberite **Uređivanje pretplate**.

8.  Na stranici dijaloški okvir **pretplati** poduzeti sljedeće korake, a zatim kliknite **Spremi i Zatvori**:

    ![Provjera autentičnosti] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Provjera autentičnosti")

    1.  Odaberite **okupljanje ili SSO provjere autentičnosti** padajućem provjere autentičnosti
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Rally softver** kopirajte vrijednost **ID davatelja identiteta** , a zatim je zalijepite u tekstni okvir **URL davatelja identiteta**
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Rally softver** kopirajte **URL daljinskog odjavite** vrijednost.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Za AAD da korisnici mogu se prijaviti, oni mora biti dodijeljena da aplikacije Rally softver pomoću njihova korisničkog imena Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u klijent Rally softvera.

2.  Idite na **Postavljanje \> korisnika**, a zatim kliknite **+ Dodaj novi**.

    ![Korisnici] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Korisnici")

3.  Unesite naziv u tekstni okvir novog korisnika, a zatim kliknite **Dodaj s detaljima**.

4.  U odjeljku **Create User** poduzeti sljedeće korake:

    ![Stvaranje korisnika] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Stvaranje korisnika")

    1.  U tekstni okvir **Korisničko ime** upišite naziv za Azure AD korisnika kojeg želite dodjele resursa.
    2.  U tekstni okvir **Adresa e-pošte** upišite adresu e-pošte korisnika Azure AD želite dodjele resursa.
    3.  Kliknite **Spremi i Zatvori**.

>[AZURE.NOTE]Možete koristiti bilo koji drugi Rally softver korisnički račun alate za stvaranje ili API-ji nudi Rally softver za dodjeljivanje AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Da biste korisnicima dodijelili Rally softver, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Rally softver** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).




