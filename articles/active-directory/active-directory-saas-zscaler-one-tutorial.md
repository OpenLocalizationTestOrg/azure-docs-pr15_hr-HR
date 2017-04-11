<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Zscaler jedan | Microsoft Azure" 
    description="Saznajte kako koristiti Zscaler jedan s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a>Praktični vodič: Azure Active Directory Integracija s Zscaler jedan

Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i ZScaler jedan.  
 Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:  

-   Valjani Azure pretplate
-   U jedan ZScaler jedinstvenu prijavu omogućeno pretplate  

Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili ZScaler jedan će moći znak u aplikaciju na vaše ZScaler jednog web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).  

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:  

1.  Omogućivanje integraciju aplikacija za jednu ZScaler
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje postavki proxyja
4.  Konfiguriranje dodjeljivanje korisnika
5.  Dodjela korisnika  

![Scenarij] (./media/active-directory-saas-zscaler-one-tutorial/IC800214.png "Scenarij")  

##<a name="enabling-the-application-integration-for-zscaler-one"></a>Omogućivanje integraciju aplikacija za jednu ZScaler

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ZScaler jedan.  

###<a name="to-enable-the-application-integration-for-zscaler-one-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za jednu ZScaler, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.  

    ![Active Directory] (./media/active-directory-saas-zscaler-one-tutorial/IC700993.png "Active Directory")  

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.  

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .  

    ![Aplikacija] (./media/active-directory-saas-zscaler-one-tutorial/IC700994.png "Aplikacija")  

4.  Kliknite **Dodaj** pri dnu stranice.  

    ![Dodavanje aplikacije] (./media/active-directory-saas-zscaler-one-tutorial/IC749321.png "Dodavanje aplikacije")  

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.  

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-zscaler-one-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")  

6.  U **okvir za pretraživanje**upišite **Jedan ZScaler**.  

    ![Galerija aplikacije] (./media/active-directory-saas-zscaler-one-tutorial/IC800215.png "Galerija aplikacije")  

7.  U oknu s rezultatima, odaberite **Jedan ZScaler**, a zatim **Dovršeno** da biste dodali aplikaciju.  

    ![ZScaler jedan] (./media/active-directory-saas-zscaler-one-tutorial/IC800216.png "ZScaler jedan")  

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ZScaler jedan sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za prijenos certifikata kodirana osnovni 64 u jednom ZScaler klijent.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)  

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na **Jedan ZScaler** Integracija stranici aplikacije kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .  

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-one-tutorial/IC800217.png "Konfiguriranje jedinstvenu prijavu")  

2.  Na stranici **kako biste željeli korisnika da biste se prijavili jedan ZScaler** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.  

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-one-tutorial/IC800218.png "Konfiguriranje jedinstvenu prijavu")  

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **ZScaler jedan znak na URL** upišite URL koji se koristi korisnicima prijave u ZScaler jednu aplikaciju, a zatim kliknite **Dalje**.  

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-zscaler-one-tutorial/IC800219.png "Konfiguriranje URL adresa Web App")  

    >[AZURE.NOTE]Stvarna vrijednost možete dobiti okruženju sustava ZScaler jedan podršku ako vam je potrebna.  

4.  Na stranici **Konfiguracija jedinstvenu prijavu na jedan ZScaler** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata na vašem računalu.  

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-one-tutorial/IC800220.png "Konfiguriranje jedinstvenu prijavu")  

5.  U prozoru preglednika drugoj web, prijavite se u jednu ZScaler tvrtke web-mjesta kao administrator.  

6.  Na izborniku na vrhu kliknite **Administracija**.  

    ![Administracija] (./media/active-directory-saas-zscaler-one-tutorial/IC800206.png "Administracija")  

7.  U odjeljku **Upravljaj administratorima i uloge**, kliknite **Upravljanje korisnicima i provjeru autentičnosti**.  

    ![Upravljanje korisnicima i provjeru autentičnosti] (./media/active-directory-saas-zscaler-one-tutorial/IC800207.png "Upravljanje korisnicima i provjeru autentičnosti")  

8.  U odjeljku **Odabir mogućnosti provjere autentičnosti za tvrtku ili ustanovu** poduzeti sljedeće korake:  

    ![Provjera autentičnosti] (./media/active-directory-saas-zscaler-one-tutorial/IC800208.png "Provjera autentičnosti")  

    1.  Odaberite **pomoću provjere autentičnosti SAML jedinstvenu prijavu**.  
    2.  Kliknite **Konfiguriranje SAML jedan prijave parametara**.  

9.  Na stranici dijaloški okvir **Konfiguriranje SAML jedan prijave parametara** poduzeti sljedeće korake, a zatim **gotovo**:  

    ![Jedinstvenu prijavu] (./media/active-directory-saas-zscaler-one-tutorial/IC800209.png "Jedinstvenu prijavu")  

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na jedan ZScaler** kopirajte vrijednost **URL zahtjev za provjeru autentičnosti** i pa ih zalijepite u tekstni okvir **URL portala za SAML koji se šalju korisnici za provjeru autentičnosti** .  
    2.  U tekstni okvir **korisničko ime za prijavu koja sadrži atribut** upišite **NameID**.  
    3.  Da biste prenijeli preuzete certifikata, kliknite **Zscaler pem**.  
    4.  Odaberite **Omogući SAML automatski-dodjele resursa**.  

10. Na stranici dijaloški okvir **Konfiguriranje provjere autentičnosti korisnika** , učinite sljedeće:  

    ![Administracija] (./media/active-directory-saas-zscaler-one-tutorial/IC800210.png "Administracija")  

    1.  Kliknite **Spremi**.  
    2.  Kliknite **sada aktiviraj**.  

11. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na jedan ZScaler** odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno**.  

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-one-tutorial/IC800221.png "Konfiguriranje jedinstvenu prijavu")  

##<a name="configuring-proxy-settings"></a>Konfiguriranje postavki proxyja

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Da biste konfigurirali postavke proxy poslužitelja u pregledniku Internet Explorer

1.  Pokrenite **Internet Explorer**.  

2.  Odaberite **Internetske mogućnosti** na izborniku **Alati** da biste otvorili dijaloški okvir **Internetske mogućnosti** .  

    ![Internetske mogućnosti] (./media/active-directory-saas-zscaler-one-tutorial/IC769492.png "Internetske mogućnosti")  

3.  Kliknite karticu **veze** .  

    ![Veze] (./media/active-directory-saas-zscaler-one-tutorial/IC769493.png "Veze")  

4.  Kliknite **Postavke LAN-a** da biste otvorili dijaloški okvir **Postavke LAN -a** .  

5.  U odjeljku Proxy poslužitelj, učinite sljedeće:  

    ![Proxy poslužitelj] (./media/active-directory-saas-zscaler-one-tutorial/IC769494.png "Proxy poslužitelj")  

    1.  Odaberite koristite proxy poslužitelj za svoj LAN.  
    2.  U tekstni okvir Adresa upišite **gateway.zscalerone.net**.  
    3.  U tekstni okvir za priključak unesite **80**.  
    4.  Odaberite **koristi proxy poslužitelj za lokalne adrese**.  
    5.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Postavke lokalne mreže (LAN -a)** .  

6.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Internetske mogućnosti** .  

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u jedan ZScaler, oni mora biti dodijeljena ZScaler s jednom.  
 U slučaju ZScaler jedan dodjeljivanje jest zadatak za ručno.  

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se na **Zscaler jednog** klijenta.  

2.  Kliknite **Administracija**.  

    ![Administracija] (./media/active-directory-saas-zscaler-one-tutorial/IC781035.png "Administracija")  

3.  Kliknite **Upravljanje korisnicima**.  

    ![Dodavanje] (./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Dodavanje")  

4.  Na kartici **korisnika** , kliknite **Dodaj**.  

    ![Dodavanje] (./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Dodavanje")  

5.  U odjeljku Add User poduzeti sljedeće korake:  

    ![Dodavanje korisnika] (./media/active-directory-saas-zscaler-one-tutorial/IC781038.png "Dodavanje korisnika")  

    1.  Unesite **korisnički ID**, **Korisničko ime**, **lozinku**, **Potvrdite lozinku**, a zatim odaberite **grupe** i **odjela** valjani AAD račun koji želite dodjele resursa.  
    2.  Kliknite **Spremi**.  

>[AZURE.NOTE]Možete koristiti bilo koje druge ZScaler jednog korisničkog računa alate za stvaranje ili API-ji nudi ZScaler jedan dodjele resursa AAD korisničkih računa.  

##<a name="assigning-users"></a>Dodjela korisnika

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.  

###<a name="to-assign-users-to-zscaler-one-perform-the-following-steps"></a>Da biste dodijelili korisnici ZScaler jedan, učinite sljedeće:

1.  Azure klasični portalu stvorite testnom računu.  

2.  Na **ZScaler jednu** stranicu Integracija za aplikaciju, kliknite **dodijeliti korisnicima**.  

    ![Dodjela korisnika] (./media/active-directory-saas-zscaler-one-tutorial/IC800222.png "Dodjela korisnika")  

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.  

    ![Da] (./media/active-directory-saas-zscaler-one-tutorial/IC767830.png "Da")  

Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).  
