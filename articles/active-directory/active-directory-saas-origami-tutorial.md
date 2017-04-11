<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Origami | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Origami."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Praktični vodič: Azure Active Directory Integracija s Origami

U ovom ćete praktičnom vodiču saznati kako integrirati Origami Azure Active Directory (Azure AD).

Integriranje Origami s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Origami
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Origami (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Origami, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Origami jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Origami iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-origami-from-the-gallery"></a>Dodavanje Origami iz galerije
Da biste konfigurirali Integracija Origami u Azure AD, morate dodati Origami iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Origami iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Origami**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/tutorial_origami_01.png)
7. U oknu s rezultatima odaberite **Origami**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/tutorial_origami_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Origami utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Origami određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Origami Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Origami.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Origami, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na Origami testiranje korisnika](#creating-a-origami-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Origami koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Origami.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Origami, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Origami** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Origami** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Origami pomoću sljedećeg uzorka: **https://live.origamirisk.com/origami/account/login?account=\<naziv tvrtke\> **
    
    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Origami** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.



1. Prijavite se na račun Origami s administratorska prava.

1. Na izborniku na vrhu kliknite **administrator**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)
  

1. Na stranici dijaloški okvir jedan prijavite se na postavljanje poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/123.png)

    na. Odaberite **Omogući znak na**.

    b. Azure klasični portalu kopirajte **SAML SSO URL**i pa ih zalijepite u tekstni okvir **Davatelja identiteta prijavu URL stranice** .

    c. Azure klasični portalu kopirajte **JEDAN znak OUT URL SERVISA**i pa ih zalijepite u tekstni okvir **URL stranice Sign-out davatelja identiteta** .

    d. Kliknite **Pregledaj** da biste prenijeli certifikat koji ste preuzeli s portala za Azure klasični.

    e. Kliknite **Spremi promjene**.


6. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-origami-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-origami-test-user"></a>Stvaranje korisnik za testiranje sustava Origami

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Origami. 

1. Prijavite se na račun Origami s administratorska prava.

2. Na izborniku na vrhu kliknite **administrator**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. U dijaloškom okviru **korisnika i sigurnost** kliknite **korisnicima**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. Kliknite **Dodaj novog korisnika**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. U dijaloškom okviru Dodavanje novog korisnika poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    na. U tekstni okvir **Korisničko ime** upišite korisničko ime od Britta Dan Simona Azure klasični portalu.

    b. U tekstni okvir **Lozinka** upišite na passwotd.

    c. U tekstni okvir **Potvrda lozinke** ponovno upišite lozinku.

    d. U tekstni okvir **ime** upišite **Britta**.

    e. U tekstni okvir **Prezime** upišite **Dan Simona**.

    f. Kliknite **Spremi**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

1. Dodijelite **Uloge korisnika** i **Klijentski pristup** korisniku. 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Origami.

![Dodijeli korisniku][200] 

**Da biste dodijelili Origami dan Britta Simona, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Origami**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-origami-tutorial/tutorial_origami_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Origami na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Origami.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-origami-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-origami-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-origami-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-origami-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-origami-tutorial/tutorial_general_205.png
