<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Zscaler | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Active Directory Zscaler da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Praktični vodič: Azure Active Directory Integracija s Zscaler
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Zscaler. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Klijenta u Zscaler
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Zscaler
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje postavki proxyja
4.  Konfiguriranje dodjeljivanje korisnika
5.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenarij")

##<a name="enabling-the-application-integration-for-zscaler"></a>Omogućivanje integraciju aplikacija za Zscaler
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Zscaler.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Zscaler, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Zscaler**.

    ![Galerija aplikacija] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Galerija aplikacija")

7.  U oknu s rezultatima odaberite **Zscaler**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Zscaler sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Ovaj postupak, dio su potrebne da biste prenijeli certifikat na Zscaler.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Zscaler** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Zscaler** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jednom prijava] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Konfiguriranje jednom prijava")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Zscaler URL za prijavu** upišite prijavu u URL koji ste dobili od Zscaler, a zatim kliknite **Dalje**: 

    >[AZURE.NOTE] Obratite se timu za podršku Zscaler ako ne znate što je URL za prijavu.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Konfiguriranje URL adresa web app")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Zscaler** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Konfiguriranje jedinstvenu prijavu")

    1.  Kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\Zscaler.cer**.
    2.  Kopirajte **URL zahtjev za provjeru autentičnosti** u međuspremnik.

5.  Prijavite se na vaš klijent Zscaler.

6.  Na izborniku na vrhu kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Administracija")

7.  U odjeljku **Upravljaj administratorima i uloge**, kliknite **Upravljanje korisnicima i provjeru autentičnosti**.

    ![Upravljanje administratorima i uloge] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Upravljanje administratorima i uloge")

8.  U odjeljku **Odaberite mogućnost obavezne provjere autentičnosti za tvrtku ili ustanovu** poduzeti sljedeće korake:

    ![Odabir mogućnosti provjere autentičnosti] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Odabir mogućnosti provjere autentičnosti")

    1.  Odaberite **pomoću provjere autentičnosti SAML jedinstvenu prijavu**.
    2.  Kliknite **Konfiguriranje SAML jedan prijave parametara**.

9.  Na stranici dijaloški okvir **Konfiguriranje SAML jedan prijave parametara** poduzeti sljedeće korake, a zatim **gotovo**:

    ![Prijenos certifikata] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Prijenos certifikata")

    1.  U tekstni okvir **URL portala za SAML koji se šalju korisnici za provjeru autentičnosti** zalijepite vrijednost polja **URL-a za zahtjev za provjeru autentičnosti** na portalu Azure klasični.
    2.  U tekstni okvir **korisničko ime za prijavu koja sadrži atribut** upišite **NameID**.
    3.  U polje **Prijenos SSL javno certifikat** prenijeti certifikat koji ste preuzeli s portala za Azure klasični.
    4.  Odaberite **Omogući SAML automatski-dodjele resursa**.

10. Na stranici dijaloški okvir **Konfiguriranje provjere autentičnosti korisnika** , učinite sljedeće:

    ![Konfiguriranje provjere autentičnosti korisnika] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Konfiguriranje provjere autentičnosti korisnika")

    1.  Kliknite **Spremi**.
    2.  Kliknite **sada aktiviraj**.

11. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-proxy-settings"></a>Konfiguriranje postavki proxyja

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Da biste konfigurirali postavke proxy poslužitelja u pregledniku Internet Explorer

1.  Pokrenite **Internet Explorer**.

2.  Odaberite **Internetske mogućnosti** na izborniku **Alati** da biste otvorili dijaloški okvir **Internetske mogućnosti** .

    ![Internetske mogućnosti] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internetske mogućnosti")

3.  Kliknite karticu **veze** .

    ![Veze] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Veze")

4.  Kliknite **Postavke LAN-a** da biste otvorili dijaloški okvir **Postavke LAN -a** .

5.  U odjeljku Proxy poslužitelj, učinite sljedeće:

    ![Proxy poslužitelj] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxy poslužitelj")

    1.  Odaberite koristite proxy poslužitelj za svoj LAN.
    2.  U tekstni okvir Adresa upišite **gateway.zscalertwo.net**.
    3.  U tekstni okvir za priključak unesite **80**.
    4.  Odaberite **koristi proxy poslužitelj za lokalne adrese**.
    5.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Postavke lokalne mreže (LAN -a)** .

6.  Kliknite **u redu** da biste zatvorili dijaloški okvir **Internetske mogućnosti** .

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Zscaler, oni mora biti dodijeljena u Zscaler.  
U slučaju Zscaler, dodjeljivanje jest zadatak za ručno.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Prijavite se u klijent **Zscaler** .

2.  Kliknite **Administracija**.

    ![Administracija] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Administracija")

3.  Kliknite **Upravljanje korisnicima**.

    ![Upravljanje korisnicima] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Upravljanje korisnicima")

4.  Na kartici **korisnika** , kliknite **Dodaj**.

    ![Dodavanje] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Dodavanje")

5.  U odjeljku Add User poduzeti sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Dodavanje korisnika")

    1.  Unesite **korisnički ID**, **Korisničko ime**, **lozinku**, **Potvrdite lozinku**, a zatim odaberite **grupe** i **odjela** valjani AAD račun koji želite dodjele resursa.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Zscaler korisnički račun alata za stvaranje ili API-ji nudi Zscaler dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Da biste korisnicima dodijelili Zscaler, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Zscaler** kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
