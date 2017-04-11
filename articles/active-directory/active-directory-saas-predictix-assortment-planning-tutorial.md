<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Predictix izbor planiranje | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i planiranje izbor Predictix."
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
    ms.date="10/14/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Praktični vodič: Azure Active Directory Integracija s Predictix izbor planiranje

U ovom ćete praktičnom vodiču saznati kako integrirati Predictix izbor planiranje Azure Active Directory (Azure AD).

Integriranje Predictix izbor planiranja s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Predictix izbor planiranje
- Možete omogućiti svojim korisnicima da automatski se prijavili u Predictix izbor planiranja (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Predictix izbor planiranje, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Predictix izbor planiranje jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Microsoft Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Predictix izbor planiranja iz galerije
2. Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu


## <a name="adding-predictix-assortment-planning-from-the-gallery"></a>Dodavanje Predictix izbor planiranja iz galerije
Da biste konfigurirali integraciju Predictix izbor planiranja u Azure AD, morate dodati Predictix izbor planiranja iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Predictix izbor planiranja iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Predictix izbor planiranja**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_01.png)

7. U oknu s rezultatima odaberite **Predictix izbor planiranje**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_02.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu
U ovom ćete odjeljku Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu Predictix izbor planiranje utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku Predictix izbor planiranja određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Predictix izbor planiranje Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Predictix izbor planiranja.

Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu Predictix izbor planiranje, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Microsoft Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Microsoft Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje testnih korisnika Predictix izbor planiranja](#creating-a-predictix-price-reporting-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Predictix izbor planiranje koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Microsoft Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Konfiguriranje Microsoft Azure AD jedinstvene prijave

U ovom ćete odjeljku Omogućivanje Microsoft Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Predictix izbor planiranja.


**Da biste konfigurirali Microsoft Azure AD jedinstvenu prijavu Predictix izbor planiranje, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za **Planiranje izbor Predictix** aplikacije Integracija kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Predictix izbor planiranja** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Predictix izbor planiranje pomoću sljedećeg uzorka: **https://\<tvrtke naziv cijene\>.ap.predictix.com/sso/request**.
    
    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Predictix izbor planiranje** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se timu za podršku za planiranje izbor Predictix i dostavite im sljedeće:

    • Preuzete certifikata

    • **Entitet ID-a**

    • **SAML SSO URL-a**

    • **Jedan Odjava URL servisa**

6. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-predictix-assortment-planning-test-user"></a>Stvaranje test korisnik sustava Predictix izbor planiranje

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona Predictix izbor planiranja. Raditi Predictix izbor planiranje timu za podršku da biste dodali korisnike u platformu Predictix izbor planiranja.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Predictix izbor planiranja.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Predictix izbor planiranje, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Predictix izbor planiranja**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom se odjeljku testiranje sustava Microsoft Azure AD jedinstvenu prijavu konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Predictix izbor planiranja na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Predictix izbor planiranja.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_205.png
