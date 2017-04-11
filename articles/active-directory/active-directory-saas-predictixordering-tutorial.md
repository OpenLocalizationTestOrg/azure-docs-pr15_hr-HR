<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s redoslijedom Predictix | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Predictix redoslijed."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a>Praktični vodič: Azure Active Directory Integracija s Predictix redoslijedom

U ovom ćete praktičnom vodiču saznati kako integrirati Predictix redoslijed Azure Active Directory (Azure AD).

Integriranje Predictix redoslijed s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Predictix redoslijedom
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Predictix redoslijed (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija s Predictix redoslijedom, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Predictix redoslijed jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Predictix redoslijed iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-predictix-ordering-from-the-gallery"></a>Dodavanje Predictix redoslijed iz galerije
Da biste konfigurirali integraciju Predictix redoslijedom u Azure AD, morate dodati Predictix redoslijed iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Predictix redoslijed iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Predictix redoslijed**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_01.png)

7. U oknu s rezultatima odaberite **Predictix redoslijedom**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Predictix redoslijedom utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Predictix redoslijed određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Predictix redoslijed Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Predictix redoslijed.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Predictix redoslijedom, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje testnih korisnika Predictix redoslijed](#creating-a-predictix-price-reporting-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Predictix redoslijed koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Predictix redoslijed.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Predictix redoslijed, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za **Naručivanje Predictix** aplikacije Integracija kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Predictix redoslijed** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Predictix redoslijed pomoću sljedećeg uzorka: **https://\<tvrtke naziv cijene\>.ordering.predictix.com/sso/request**.
    
    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Predictix redoslijed** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se timu za podršku Predictix redoslijed i dostavite im sljedeće:

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

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-predictix-ordering-test-user"></a>Stvaranje test korisnik sustava Predictix redoslijedom

U ovom ćete odjeljku stvoriti korisnik naziva dan Britta Simona u Predictix redoslijed. Raditi Predictix redoslijed timu za podršku da biste dodali korisnike u platformu Predictix redoslijed.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da dodjelu pristup Predictix redoslijed.

![Dodijeli korisniku][200] 

**Da biste dodijelili redoslijed Predictix dan Britta Simona, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Predictix redoslijed**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Predictix redoslijed na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Predictix redoslijed.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_205.png