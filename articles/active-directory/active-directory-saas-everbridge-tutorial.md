<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Everbridge | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Everbridge."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Praktični vodič: Azure Active Directory Integracija s Everbridge

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Everbridge s Azure Active Directory (Azure AD).

Integriranje Everbridge s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Everbridge
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Everbridge (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Everbridge, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Everbridge jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Everbridge iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-everbridge-from-the-gallery"></a>Dodavanje Everbridge iz galerije
Da biste konfigurirali Integracija Everbridge u Azure AD, morate dodati Everbridge iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Everbridge iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Everbridge**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_01.png)
7. U oknu s rezultatima odaberite **Everbridge**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Everbridge utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Everbridge korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Everbridge Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Everbridge.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Everbridge, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na Everbridge testiranje korisnika](#creating-a-everbridge-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Everbridge koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Everbridge.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Everbridge, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Everbridge** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Everbridge** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_04.png)

    na. U tekstni okvir **identifikator** upišite URL-a pomoću sljedećeg uzorka:`https://sso.everbridge.net/{<company name>}`

    b. U tekstni okvir **URL odgovor** upišite URL-a pomoću sljedećeg uzorka:`https://manager.everbridge.net/saml/SSO/{<company name>}/alias/defaultAlias`

    c. Kliknite **Dalje**

4. Na stranici **Konfiguracija jedinstvenu prijavu na Everbridge** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_05.png)

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.

5. Da biste dobili SSO konfiguriran za aplikaciju, morate prijave za vaš klijent Everbridge kao administrator.

6. Na izborniku na vrhu, kliknite karticu **Postavke** i odaberite **Jedinstvenu prijavu** u odjeljku **Sigurnost**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)

    na. U tekstni okvir **naziv** upišite naziv davatelja identifikator (npr.: naziv tvrtke).

    b. U tekstni okvir **API naziv** upišite naziv API-JA.

    C. Kliknite gumb **Odabir datoteke** za prijenos datoteke metapodataka koji ste preuzeli u **4**.

    d. **Mjesto identiteta SAML**, odaberite "Identiteta je u elementu NameIdentifier izjava predmet".

    e. Kopirajte URL SAML SSO Azure AD **URL za prijavu davatelja identiteta** u Everbridge.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)

    f. Kao **Servisa davatelja pokrenut zahtjev za povezivanje**, odaberite HTTP preusmjeravanje.

7. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

Na popisu korisnika odaberite **Dan Britta Simona**.
    
![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-everbridge-test-user"></a>Stvaranje Everbridge testnih korisnika

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Everbridge. Raditi Everbridge timu za podršku putem <mailto:support@everbridge.com> da biste dodali korisnike u Everbridge platforme.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Everbridge.
    
![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona Everbridge, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Everbridge**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_50.png)

3. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Everbridge na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Everbridge.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_205.png
