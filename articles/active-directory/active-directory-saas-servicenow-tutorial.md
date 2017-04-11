<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ServiceNow i ServiceNow Express | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i ServiceNow ServiceNow Express."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Praktični vodič: Azure Active Directory Integracija s ServiceNow i ServiceNow Express.


U ovom ćete praktičnom vodiču saznati kako ServiceNow i ServiceNow Express integrirati Azure Active Directory (Azure AD).

Integriranje ServiceNow i ServiceNow Express sa Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup ServiceNow i ServiceNow Express
- Možete omogućiti svojim korisnicima da automatski se prijavili u ServiceNow i ServiceNow Express (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija s ServiceNow i ServiceNow Express, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Za ServiceNow, instanci ili klijent ServiceNow, Calgary verzija ili noviji
- Za ServiceNow Express, instance komponente ServiceNow Express, Helsinki verzija ili noviji
- ServiceNow klijenta mora imati [Više davatelja jedan znak na dodatak](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) omogućen. To možete učiniti slanjem zahtjeva za uslugu pri <https://hi.service-now.com/> 


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje. Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje ServiceNow iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave za ServiceNow ili ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>Dodavanje ServiceNow iz galerije
Da biste konfigurirali integraciju ServiceNow ili ServiceNow Express u Azure AD, morate dodati ServiceNow iz galerije popis upravljani SaaS aplikacija. 

**Da biste dodali ServiceNow iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **ServiceNow**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. U oknu s rezultatima odaberite **ServiceNow**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom odjeljku možete konfigurirati i testiranje Azure AD jedinstvenu prijavu ServiceNow ili ServiceNow Express utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u ServiceNow određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u ServiceNow Azure AD korisnik mora uspostaviti.
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u ServiceNow. Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s ServiceNow, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvene prijave za ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Konfiguriranje Azure AD jedinstvene prijave za ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** – da biste omogućili korisnicima da biste koristili ovu značajku.
3. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na ServiceNow testiranje korisnika](#creating-a-servicenow-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u ServiceNow koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
6. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

> [AZURE.NOTE] Ako želite konfigurirati ServiceNow izostaviti korak 2. Isto tako, ako želite konfigurirati ServiceNow Express izostaviti korak 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfiguriranje Azure AD jedinstvene prijave za ServiceNow

1.  Azure AD klasični portalu na stranici za integraciju aplikacije **ServiceNow** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ServiceNow** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Konfiguriranje URL adresa web app")

    na. u tekstni okvir **ServiceNow na URL za prijavu** upišite URL koristi za korisnike za prijavu u aplikaciju ServiceNow pratiti uzorak: `https://<instance-name>.service-now.com`.

    b. U tekstni okvir **identifikator** upišite URL koriste vaši korisnici za prijavu u aplikaciju ServiceNow pratiti uzorak: `https://<instance-name>.service-now.com`.

    c. Kliknite **Dalje**

4.  Da bi se Azure AD automatski konfigurirao ServiceNow za provjere autentičnosti utemeljene na SAML, unesite ServiceNow naziv instance, administratorskog korisničkog imena i lozinke za administratore u obrascu **Automatsko konfiguriranje jedinstvenu prijavu** , a zatim kliknite *Konfiguriraj*. Imajte na umu da dobili administrator korisničko ime mora imati ulogu **security_admin** koja je dodijeljena u ServiceNow za rad. U suprotnom, da biste ručno konfiguriranje ServiceNow da biste upotrijebili Azure AD davatelja identiteta SAML, kliknite **ručno konfiguriranje aplikacija za jedinstvenu prijavu**, a zatim kliknite **Dalje** i poduzeti sljedeće korake.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Konfiguriranje URL adresa web app")



5.  Na stranici **Konfiguracija jedinstvenu prijavu na ServiceNow** kliknite **Preuzimanje certifikata**, spremanja datoteka certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfiguriranje jedinstvenu prijavu")

1. Prijava u aplikaciju ServiceNow kao administrator.
2. Aktivirati dodatak za _integraciju - više davatelja jedan prijave Installer_ prateći sljedeće korake:

    na. U navigacijskom oknu s lijeve strane, idite na odjeljak **Definicija sustava** , a zatim kliknite **Dodaci**.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Dodatak za aktiviranje")
    
    b. Traženje _Integracija - više davatelja jedan prijave instalacijski program_.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Dodatak za aktiviranje")

    c. Odaberite dodatak. Rigth kliknite i odaberite **Aktiviraj/nadogradnje**.
    
    d. Kliknite gumb **Aktiviraj** .

2. U navigacijskom oknu s lijeve strane kliknite **Svojstva**.  

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Konfiguriranje URL adresa web app")


3. U dijaloškom okviru **Svojstva SSO za više davatelja** poduzeti sljedeće korake:

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Konfiguriranje URL adresa web app")

    na. Kao **omogućiti više davatelja jedinstvene Prijave**, odaberite **da**.

    b. Kao **Omogući zapisivanje pogrešaka imate više davatelja SSO integracije**, odaberite **da**.

    c. U tekstni okvir koji se **polje na korisnike tablice koje...** upišite **korisničko_ime**.

    d. Kliknite **Spremi**.



1. U navigacijskom oknu s lijeve strane kliknite **x509 certifikata**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Konfiguriranje jedinstvenu prijavu")


1. U dijaloškom okviru **X.509 certifikate** kliknite **Novo**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Konfiguriranje jedinstvenu prijavu")


1. U dijaloškom okviru **X.509 certifikate** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfiguriranje jedinstvenu prijavu")

    na. Kliknite **Novo**.

    b. U tekstni okvir **naziv** upišite naziv za konfiguraciju (npr.: **TestSAML2.0**).

    c. Odaberite **aktivni**.

    d. **Oblikovanje**odaberite **PEM**.

    e. Kao **vrstu**odaberite **Pouzdanosti certifikata trgovine**.
    
    f. Otvaranje Base64 kodirani certifikat u Bloku za pisanje, kopirajte na sadržaj u međuspremnik i pa ih zalijepite u tekstni okvir **PEM certifikata** .

    g. Kliknite **Ažuriraj**.


1. U navigacijskom oknu s lijeve strane kliknite **Davatelji identiteta**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Konfiguriranje jedinstvenu prijavu")

1. U dijaloškom okviru **Davatelji identiteta** kliknite **Novo**:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Konfiguriranje jedinstvenu prijavu")


1. U dijaloškom okviru **Davatelji identiteta** kliknite **SAML2 Update1?**:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Konfiguriranje jedinstvenu prijavu")


1. U dijaloškom okviru svojstva Update1 SAML2 poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Konfiguriranje jedinstvenu prijavu")


    na. u tekstni okvir **naziv** upišite naziv za konfiguraciju (npr.: **SAML 2.0**).

    b. U **Polje korisničkom** tekstni okvir, vrsta **e-pošte** ili **user_id**, ovisno o tome koje polje koristi identificirati samo korisnici u implementaciji sustava ServiceNow. 
    
    > [AZURE.NOTE] Možete configue Azure AD slanje Azure AD korisničkog ID-a (korisnikovo Glavno ime) ili adresu e-pošte kao jedinstveni identifikator u SAML token tako da na **ServiceNow > atribute > jedinstvenu prijavu** dio Azure klasični portal i mapiranje željeno polje atribut **nameidentifier** . Vrijednosti pohranjene za odabrani atribut u Azure AD (npr. korisnikovo Glavno ime) moraju se podudarati vrijednosti pohranjene u ServiceNow za uneseno polje (npr. user_id)

    c. Na portalu klasični Azure AD kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **URL davatelja identiteta** .

    d. Na portalu klasični Azure AD kopirajte vrijednost **URL zahtjev za provjeru autentičnosti** i pa ih zalijepite u tekstni okvir **AuthnRequest davatelja identiteta** .

    e. Na portalu klasični Azure AD kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **SingleLogoutRequest davatelja identiteta** .

    f. U tekstni okvir **Početnu stranicu ServiceNow** upišite URL ServiceNow instancu početne stranice.

    > [AZURE.NOTE] Na početnu stranicu instancu ServiceNow je Ulančavanje **ServieNow klijentu URL** i **/navpage.do** (npr.:`https://fabrikam.service-now.com/navpage.do`).
 

    g. U na **ID entitet / izdavač** tekstni okvir, unesite URL ServiceNow klijentu.

    h. U tekstni okvir **URL publici** upišite URL ServiceNow klijentu. 

    li. U tekstni okvir **Protokol za povezivanje za SingleLogoutRequest u IDP** upišite **urn: organizacija: imena: tc: SAML:2.0:bindings:HTTP-preusmjeravanje**.

    j. U tekstni okvir NameID pravila upišite **urn: organizacija: imena: tc: SAML:1.1:nameid-oblik: neodređenim**.

    k. Poništite mogućnost **Stvaranje programa AuthnContextClass**.

    l. U **AuthnContextClassRef način**, upišite `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. To je samo potrebno ako ste oblaka samo tvrtke ili ustanove. Ako koristite u lokalnu ADFS ili MFA za provjeru autentičnosti, a trebali biste konfigurirati tu vrijednost. 

    m. U tekstni okvir koji se **Sat Skew** upišite **60**.

    n. Kao **Jedan znak na skripte**, odaberite **MultiSSO_SAML2_Update1**.

    o. Kao **x509 certifikat**, odaberite certifikat koji ste stvorili u prethodnom koraku.

    p. Kliknite **Pošalji**. 



6. Na portalu klasični Azure AD odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfiguriranje jedinstvenu prijavu")

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.
 
    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfiguriranje jedinstvenu prijavu")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfiguriranje Azure AD jedinstvene prijave za ServiceNow Express

1.  Azure AD klasični portalu na stranici za integraciju aplikacije **ServiceNow** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ServiceNow** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguracija postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Konfiguriranje URL adresa web app")

    na. u tekstni okvir **ServiceNow na URL za prijavu** upišite URL koristi za korisnike za prijavu u aplikaciju ServiceNow pratiti uzorak: `https://<instance-name>.service-now.com`.

    b. U tekstni okvir **Izdavač URL** unesite URL koriste vaši korisnici za prijavu u aplikaciju ServiceNow pratiti uzorak `https://<instance-name>.service-now.com`.

    c. Kliknite **Dalje**

4.  Kliknite **ručno konfiguriranje aplikacija za jedinstvenu prijavu**, a zatim kliknite **Dalje** , a poduzeti sljedeće korake.

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Konfiguriranje URL adresa web app")

5.  Na stranici **Konfiguracija jedinstvenu prijavu na ServiceNow** kliknite **Preuzmite certifikat**, spremanje datoteka certifikata lokalno na vašem računalu i zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfiguriranje jedinstvenu prijavu")

6. Prijava u aplikaciju ServiceNow Express kao administrator.

7. U navigacijskom oknu s lijeve strane kliknite **Jedinstvenu prijavu**.  

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Konfiguriranje URL adresa web app")


8. U dijaloškom okviru **Jedinstvenu prijavu** konfiguracije na kliknite ikonu u gornjem desnom kutu i postaviti sljedeća svojstva:

    ![Konfiguriranje URL adresa web app] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Konfiguriranje URL adresa web app")

    na. Uključivanje ili isključivanje **omogućiti više davatelja SSO** udesno.

    b. Uključivanje ili isključivanje **omogućiti ispravljanje pogrešaka zapisivanja za više davatelja SSO integraciju sa servisom** udesno.

    c. U tekstni okvir koji se **polje na korisnike tablice koje...** upišite **korisničko_ime**.


9. U dijaloškom okviru **Jedinstvenu prijavu** kliknite **Dodaj novi certifikat**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Konfiguriranje jedinstvenu prijavu")



10. U dijaloškom okviru **X.509 certifikate** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfiguriranje jedinstvenu prijavu")

    na. U tekstni okvir **naziv** upišite naziv za konfiguraciju (npr.: **TestSAML2.0**).

    b. Odaberite **aktivni**.

    c. **Oblikovanje**odaberite **PEM**.

    d. Kao **vrstu**odaberite **Pouzdanosti certifikata trgovine**.

    e. Stvaranje datoteke za Base64 kodirani iz preuzete certifikata.
   
    > [AZURE.NOTE] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Otvaranje Base64 kodirani certifikat u Bloku za pisanje, kopirajte na sadržaj u međuspremnik i pa ih zalijepite u tekstni okvir **PEM certifikata** .

    g. Kliknite **Ažuriraj**.


11. U dijaloškom okviru **Jedinstvenu prijavu** kliknite **Dodaj novi IdP**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Konfiguriranje jedinstvenu prijavu")


12. U dijaloškom okviru **Dodavanje novog davatelja identiteta** u odjeljku **Konfiguriranje davatelja identiteta**, poduzmite sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Konfiguriranje jedinstvenu prijavu")


    na. U tekstni okvir **naziv** upišite naziv za konfiguraciju (npr.: **SAML 2.0**).

    b. Na portalu klasični Azure AD kopirajte vrijednost **ID davatelja identiteta** i pa ih zalijepite u tekstni okvir **URL davatelja identiteta** .

    c. Na portalu klasični Azure AD kopirajte vrijednost **URL zahtjev za provjeru autentičnosti** i pa ih zalijepite u tekstni okvir **AuthnRequest davatelja identiteta** .

    d. Na portalu klasični Azure AD kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **SingleLogoutRequest davatelja identiteta** .

    e. Kao **Certifikat davatelja identiteta**, odaberite certifikat koji ste stvorili u prethodnom koraku.


13. Kliknite **Napredne postavke**, a zatim u odjeljku **Dodatna svojstva davatelja identiteta**, učinite sljedeće:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Konfiguriranje jedinstvenu prijavu")

    na. U tekstni okvir **Protokol za povezivanje za SingleLogoutRequest u IDP** upišite **urn: organizacija: imena: tc: SAML:2.0:bindings:HTTP-preusmjeravanje**.

    b. U tekstni okvir **NameID pravila** upišite **urn: organizacija: imena: tc: SAML:1.1:nameid-oblik: neodređenim**.    

    c. U **AuthnContextClassRef način**, upišite **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. Poništite mogućnost **Stvaranje programa AuthnContextClass**.

14. U odjeljku **Dodatna svojstva davatelja usluge**, učinite sljedeće:

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Konfiguriranje jedinstvenu prijavu")

    na. U tekstni okvir **Početnu stranicu ServiceNow** upišite URL ServiceNow instancu početne stranice.

    > [AZURE.NOTE] Na početnu stranicu instancu ServiceNow je Ulančavanje **ServieNow klijentu URL** i **/navpage.do** (npr.: `https://fabrikam.service-now.com/navpage.do`).

    b. U na **ID entitet / izdavač** tekstni okvir, unesite URL ServiceNow klijentu.

    c. U tekstni okvir **URI ciljne Skupine** upišite URL ServiceNow klijentu. 

    d. U tekstni okvir koji se **Sat Skew** upišite **60**.

    e. U **Polje korisničkom** tekstni okvir, vrsta **e-pošte** ili **user_id**, ovisno o tome koje polje koristi identificirati samo korisnici u implementaciji sustava ServiceNow.
    
    > [AZURE.NOTE] Možete configue Azure AD slanje Azure AD korisničkog ID-a (korisnikovo Glavno ime) ili adresu e-pošte kao jedinstveni identifikator u SAML token tako da na **ServiceNow > atribute > jedinstvenu prijavu** dio Azure klasični portal i mapiranje željeno polje atribut **nameidentifier** . Vrijednosti pohranjene za odabrani atribut u Azure AD (npr. korisnikovo Glavno ime) moraju se podudarati vrijednosti pohranjene u ServiceNow za uneseno polje (npr. user_id)

    f. Kliknite **Spremi**. 


15. Na portalu klasični Azure AD odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfiguriranje jedinstvenu prijavu")

16. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.
 
    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisniku dodjeljivanje korisničkih računa servisa Active Directory za ServiceNow.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1. U Azure klasični portala za upravljanje, na stranici za integraciju aplikacije **ServiceNow** kliknite **Konfiguriraj korisnika dodjele resursa**. 

    ![Dodjela resursa za korisnika] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Dodjela resursa za korisnika")


2. Na stranici **Unesite vjerodajnice ServiceNow da biste omogućili automatsko korisnika dodjele resursa** , navedite sljedeće postavke konfiguracije: Konfiguriranje dodjele resursa korisnika 

     na. U tekstni okvir **Naziv Instance ServiceNow** upišite naziv instance ServiceNow.

     b. U tekstni okvir **ServiceNow administrator korisničko ime** upišite naziv ServiceNow administratorskog računa.

     c. U tekstni okvir **ServiceNow administratorsku lozinku** upišite lozinku za taj račun.

     d. Kliknite **Provjera valjanosti** da biste provjerili konfiguraciju.

     e. Kliknite gumb **Dalje** da biste otvorili stranicu **sljedeće korake** .

     f. Ako želite Dodjela resursa za sve korisnike u ovu aplikaciju odaberite "**automatski Dodjela resursa za sve korisničke račune u imeniku za ovu aplikaciju**". 

    ![Daljnji koraci] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Daljnji koraci")

     g. Na stranici **daljnji koraci** kliknite **dovrši** da biste spremili konfiguraciju.

### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   


### <a name="creating-a-servicenow-test-user"></a>Stvaranje ServiceNow testnih korisnika

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u ServiceNow. U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u ServiceNow. Ako ne znate kako dodati korisnika na vašem računu ServiceNow ili ServiceNow Express, obratite se timu za podršku ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju ServiceNow.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona ServiceNow, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **ServiceNow**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu svi korisnici odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu ServiceNow na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji ServiceNow.

## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
