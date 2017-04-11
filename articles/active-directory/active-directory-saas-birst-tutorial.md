<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Birst Agilno Business Analytics | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Birst Agilno Business Analytics."
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


# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a>Praktični vodič: Azure Active Directory Integracija s Birst Agilno Business Analytics

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Birst Agilno Business Analytics s Azure Active Directory (Azure AD).

Integriranje Birst Agilno Business Analytics s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Birst Agilno Business Analytics
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Birst Agilno Business Analytics (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Birst Agilno Business Analytics, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Birst Agilno Business Analytics jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Birst Agilno Business Analytics iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-birst-agile-business-analytics-from-the-gallery"></a>Dodavanje Birst Agilno Business Analytics iz galerije
Da biste konfigurirali Integracija Birst Agilno Business Analytics u Azure AD, morate dodati Birst Agilno Business Analytics iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Birst Agilno Business Analytics iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Birst Agilno Business Analytics**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/tutorial_birst_01.png)

7. U oknu s rezultatima odaberite **Birst Agilno Business Analytics**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/tutorial_birst_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Birst Agilno Business Analytics utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Birst Agilno Business Analytics korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Birst Agilno Business Analytics Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Birst Agilno Business Analytics.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Birst Agilno Business Analytics, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje analize tvrtke Agilno Birst testiranje korisnika](#creating-a-birst-agile-business-analytics-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Birst Agilno Business Analytics koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Birst Agilno Business Analytics.



**Da biste konfigurirali Azure AD jedinstvenu prijavu Birst Agilno Business Analytics, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Birst Agilno Business Analytics** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Birst Agilno Business Analytics** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-birst-tutorial/tutorial_birst_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-birst-tutorial/tutorial_birst_04.png) 


    na. U tekstni okvir prijavite se na URL unesite URL koji se koristi korisnika za prijavu u aplikaciju Birst Agilno Business Analytics pomoću sljedećeg uzorka: **"https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID"**.
    URL je ovisna o podatkovnim centrom računa Birst nalazi. Za podatkovnog centra sad koristiti **"https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID"** , a za podatkovnog centra Europe **"https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID"**

    b. Kliknite **Dalje**.


4. Na stranici **Konfiguracija jedinstvenu prijavu na Birst Agilno Business Analytics** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-birst-tutorial/tutorial_birst_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se službi za podršku Birst Agilno Business Analytics putem [info@birst.com](emailTo:info@birst.com) i priložili datoteku preuzete certifikata e-pošte. I navedite SAML SSO URL, URL izgleda za prijavu i izdavatelj URL tako da je konfigurirati za integraciju SSO.


> [AZURE.NOTE] Provjerite spomenuti Birst timu da ta Integracija moraju algoritam SHA256 (SHA1 neće biti podržane) tako da ih možete postaviti na SSO na odgovarajući poslužitelj poput **app2101** itd.



6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
  
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-birst-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-birst-agile-business-analytics-test-user"></a>Stvaranje korisnika test Birst Agilno Business Analytics

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u Birst Agilno Business Analytics korisnika. Raditi Birst Agilno Business Analytics za podršku da biste dodali korisnike u Birst računa. 


> [AZURE.NOTE]Ako je potrebno ručno stvoriti korisnik sustava ćete morati obratite se timu za podršku Birst Agilno Business Analytics.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Birst Agilno Business Analytics.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Birst Agilno Business Analytics, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Birst Agilno Business Analytics**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-birst-tutorial/tutorial_birst_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Birst Agilno Business Analytics na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Birst Agilno Business Analytics.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-birst-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-birst-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-birst-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-birst-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-birst-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-birst-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-birst-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-birst-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-birst-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-birst-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-birst-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-birst-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-birst-tutorial/tutorial_general_205.png
