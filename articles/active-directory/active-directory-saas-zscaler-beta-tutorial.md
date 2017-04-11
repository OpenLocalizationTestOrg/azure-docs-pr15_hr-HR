<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Zscaler Beta | Microsoft Azure" 
    description="Saznajte kako koristiti Zscaler Beta s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Praktični vodič: Azure Active Directory Integracija s Zscaler Beta
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i ZScaler Beta.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na ZScaler Beta jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili ZScaler Beta će moći znak u aplikaciju na vašem ZScaler Beta web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za ZScaler Beta
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje postavki proxyja
4.  Konfiguriranje dodjeljivanje korisnika
5.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-zscaler-beta-tutorial/IC800223.png "Scenarij")

##<a name="enabling-the-application-integration-for-zscaler-beta"></a>Omogućivanje integraciju aplikacija za ZScaler Beta
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ZScaler Beta.

###<a name="to-enable-the-application-integration-for-zscaler-beta-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za ZScaler Beta, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-beta-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-zscaler-beta-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-zscaler-beta-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-zscaler-beta-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **ZScaler Beta**.

    ![Galerija aplikacije] (./media/active-directory-saas-zscaler-beta-tutorial/IC800224.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **ZScaler Beta**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ZScaler jedan] (./media/active-directory-saas-zscaler-beta-tutorial/IC800216.png "ZScaler jedan")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ZScaler Beta sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 ZScaler Beta klijentu.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **ZScaler Beta** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-beta-tutorial/IC800225.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ZScaler Beta** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-beta-tutorial/IC800226.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **ZScaler Beta na URL za prijavu** upišite URL koji se koristi korisnika za prijavu u aplikaciju ZScaler Beta, a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-zscaler-beta-tutorial/IC800227.png "Konfiguriranje URL adresa Web App")

    >[AZURE.NOTE] Stvarna vrijednost možete doći okruženju sustava s tim za podršku ZScaler Beta ako vam je potrebna.

4.  Na stranici **Konfiguracija jedinstvenu prijavu na ZScaler Beta** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-beta-tutorial/IC800228.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke ZScaler Beta kao administrator.

6.  Na izborniku na vrhu kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-zscaler-beta-tutorial/IC800206.png "Administracija")

7.  U odjeljku **Upravljaj administratorima i uloge**, kliknite **Upravljanje korisnicima i provjeru autentičnosti**.

    ![Upravljanje korisnicima i provjeru autentičnosti] (./media/active-directory-saas-zscaler-beta-tutorial/IC800207.png "Upravljanje korisnicima i provjeru autentičnosti")

8.  U odjeljku **Odabir mogućnosti provjere autentičnosti za tvrtku ili ustanovu** poduzeti sljedeće korake:

    ![Provjera autentičnosti] (./media/active-directory-saas-zscaler-beta-tutorial/IC800208.png "Provjera autentičnosti")

    1.  Odaberite **pomoću provjere autentičnosti SAML jedinstvenu prijavu**.
    2.  Kliknite **Konfiguriranje SAML jedan prijave parametara**.

9.  Na stranici dijaloški okvir **Konfiguriranje SAML jedan prijave parametara** poduzeti sljedeće korake, a zatim **gotovo**:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-zscaler-beta-tutorial/IC800209.png "Jedinstvenu prijavu")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ZScaler Beta** kopirajte vrijednost **URL zahtjev za provjeru autentičnosti** i pa ih zalijepite u tekstni okvir **URL portala za SAML koji se šalju korisnici za provjeru autentičnosti** .
    2.  U tekstni okvir **korisničko ime za prijavu koja sadrži atribut** upišite **NameID**.
    3.  Da biste prenijeli preuzete certifikata, kliknite **Zscaler pem**.
    4.  Odaberite **Omogući SAML automatski-dodjele resursa**.

10. Na stranici dijaloški okvir **Konfiguriranje provjere autentičnosti korisnika** , učinite sljedeće:

    ![Administracija] (./media/active-directory-saas-zscaler-beta-tutorial/IC800210.png "Administracija")

    1.  Kliknite **Spremi**.
    2.  Kliknite **sada aktiviraj**.

11. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ZScaler Beta** odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-beta-tutorial/IC800229.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-proxy-settings"></a>Konfiguriranje postavki proxyja

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Da biste konfigurirali postavke proxy poslužitelja u pregledniku Internet Explorer

1.  Pokrenite **Internet Explorer**.

2.  Odaberite **Internetske mogućnosti** na izborniku **Alati** da biste otvorili dijaloški okvir **Internetske mogućnosti** .

    ![Internetske mogućnosti] (./media/active-directory-saas-zscaler-beta-tutorial/IC769492.png "Internetske mogućnosti")

3.  Kliknite karticu **veze** .

    ![Veze] (./media/active-directory-saas-zscaler-beta-tutorial/IC769493.png "Veze")

4.  Kliknite **Postavke LAN-a** da biste otvorili dijaloški okvir **Postavke LAN -a** .

5.  U odjeljku Proxy poslužitelj, učinite sljedeće:

    ![Proxy poslužitelj] (./media/active-directory-saas-zscaler-beta-tutorial/IC769494.png "Proxy poslužitelj")

    1.  Odaberite koristite proxy poslužitelj za svoj LAN.
    2.  U tekstni okvir Adresa upišite **gateway.zscalerBeta.net**.
    3.  U tekstni okvir za priključak unesite **80**.
    4.  Odaberite **koristi proxy poslužitelj za lokalne adrese**.
    5.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Postavke lokalne mreže (LAN -a)** .

6.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Internetske mogućnosti** .

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u ZScaler Beta, oni mora biti dodijeljena da ZScaler Beta.  
U slučaju ZScaler Beta dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u klijent **Zscaler** .

2.  Kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-zscaler-beta-tutorial/IC781035.png "Administracija")

3.  Kliknite **Upravljanje korisnicima**.

    ![Dodavanje] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Dodavanje")

4.  Na kartici **korisnika** , kliknite **Dodaj**.

    ![Dodavanje] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Dodavanje")

5.  U odjeljku Add User poduzeti sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-zscaler-beta-tutorial/IC781038.png "Dodavanje korisnika")

    1.  Unesite **korisnički ID**, **Korisničko ime**, **lozinku**, **Potvrdite lozinku**, a zatim odaberite **grupe** i **odjela** valjani AAD račun koji želite dodjele resursa.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi ZScaler Beta korisnički račun alate za stvaranje ili API-ji nudi ZScaler Beta dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-zscaler-beta-perform-the-following-steps"></a>Da biste korisnicima dodijelili ZScaler Beta, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **ZScaler Beta** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-zscaler-beta-tutorial/IC800230.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-zscaler-beta-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).