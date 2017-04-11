<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Showpad | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Showpad."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-showpad"></a>Praktični vodič: Azure Active Directory Integracija s Showpad

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Showpad s Azure Active Directory (Azure AD).

Integriranje Showpad s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Showpad
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Showpad (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure Active Directory Portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Showpad, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Showpad pretplate


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Showpad iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-showpad-from-the-gallery"></a>Dodavanje Showpad iz galerije
Da biste konfigurirali Integracija Showpad u Azure AD, morate dodati Showpad iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Showpad iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Aplikacija][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.
 
    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Showpad**.

    ![Aplikacija](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. U oknu s rezultatima odaberite **Showpad**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Showpad utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Showpad korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Showpad Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Showpad.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Showpad, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na Showpad testiranje korisnika](#creating-a-showpad-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Showpad koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Showpad.



**Da biste konfigurirali Azure AD jedinstvenu prijavu Showpad, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Showpad** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Showpad** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png) 


    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Showpad pomoću sljedećeg uzorka:`https://<company name>.showpad.biz/login`

    b. U tekstni okvir **identifikator** upišite URL-a pomoću sljedećeg uzorka:`https://<company name>.showpad.biz`

    c. Kliknite **Dalje**


4. Na stranici **Konfiguracija jedinstvenu prijavu na Showpad** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Prijava za vaš klijent Showpad kao administrator.

6. Na izborniku na vrhu kliknite **Postavke**.

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacije](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

7. Dođite do "**Jedinstvenu prijavu**" i kliknite "**Omogući**".
 
    ![Konfiguriranje jedinstvenu prijavu na strani aplikacija](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. U dijaloškom okviru **Dodavanje servisa SAML 2.0** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacije](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 

    na. U tekstni okvir **naziv** upišite naziv davatelja identifikator (npr.: naziv tvrtke).

    b. Kao **Izvor metapodatke**, odaberite **XML**.

    c. Kopirajte sadržaj XML datoteke preuzete metapodataka, a zatim je zalijepite u tekstni okvir **XML metapodatke** .

    d. Odaberite **Automatsko dodjele račune za nove korisnike prilikom prijave**.

    e. Kliknite **Pošalji**.


10. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]


11. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
  
    ![Azure AD jedinstvene prijave][11]






### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili Showpad testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png) 

    na. U tekstni okvir **Korisničko ime** upišite **BrittaSimon**.

    b. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Kao **uloga**, odaberite **korisnika**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.


### <a name="creating-a-showpad-test-user"></a>Stvaranje Showpad testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u Showpad korisnika. 

Showpad podržava samo-u – vrijeme dodjele resursa. Omogućeno je Dodjela resursa u **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)**. 

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. 




### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Showpad.

![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona Showpad, učinite sljedeće:**

1. Azure klasični portala na izborniku na vrhu kliknite **aplikacije**.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija kliknite **Showpad**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]




### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu **Showpad** na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Showpad.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png
