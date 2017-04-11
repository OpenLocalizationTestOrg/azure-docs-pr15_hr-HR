<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s osobama | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i osobe."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-people"></a>Praktični vodič: Azure Active Directory Integracija s osobama

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji osobe s Azure Active Directory (Azure AD).

Integriranje osobe s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup osobe
- Možete omogućiti svojim korisnicima da automatski se prijavili u osobama (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija s osobama, potrebne su vam sljedeće stavke:

- Azure pretplate
- S osobama jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje osoba iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-people-from-the-gallery"></a>Dodavanje osoba iz galerije
Da biste konfigurirali integraciju osoba u Azure AD, morate dodati osobe iz galerije popis upravljani SaaS aplikacija.

**Da biste dodali osobe iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 
 
    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **osobe**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/tutorial_people_01.png)

7. U oknu s rezultatima odaberite **osobe**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/tutorial_people_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s drugim osobama na temelju korisnika testa pod nazivom "Britta Dan Simona".

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s korisnicima, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje s osobama testiranje korisnika](#creating-a-people-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u odjeljku osobe koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciju osobe.



**Da biste konfigurirali Azure AD jedinstvenu prijavu s osobama, učinite sljedeće:**

1. Azure klasični portalu na stranici za integraciju aplikacije **osobe** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    [Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili osobe** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju osobe pomoću sljedećeg uzorka: **"https://\<naziv tvrtke\>.peoplehr.com/"**. 

    b. Ako ne znate URL klijent, obratite se službi osoba putem [customerservices@peoplehr.com](mailto:customerservices@peoplehr.com) da biste ga.  

    c. U tekstni okvir **identifikator** upišite URL klijenta. 

    d. U tekstni okvir **URL odgovor** , unesite URL sljedećeg uzorka: "**https://itgs.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx**".

    e. Kliknite **Dalje**


4. Na stranici **Konfiguracija jedinstvenu prijavu na osobe** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_05.png) 

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, morate prijave za vaš klijent osobe kao administrator.
    
    na. Na izborniku na lijevoj strani kliknite **Postavke**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_001.png) 

    b. Kliknite **"Tvrtke"**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_002.png) 

    c. Na na **"prijenos ' znak na ' SAML meta-podatkovnu datoteku"**, kliknite **Pregledaj** da biste prenijeli datoteku preuzete metapodataka.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)




6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
 
    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.
Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]


**Da biste stvorili ljudi testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-people-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-people-test-user"></a>Stvaranje korisnika test osobe

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u odjeljku osobe. Korisnici ne podržava samo-u – vrijeme dodjele resursa tako da se potrebno obratiti timu za podršku za osobe da biste ručno stvorili korisnik sustava.




### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju osobama.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona osobama, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **osobe**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-people-tutorial/tutorial_people_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
Kada kliknete pločicu osobe na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji osobe.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-people-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-people-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-people-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-people-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-people-tutorial/tutorial_general_205.png
