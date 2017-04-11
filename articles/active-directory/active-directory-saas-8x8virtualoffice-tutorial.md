<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija sa sustavom Office virtualne 8 x 8 | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active imeničkog i 8 x 8 virtualne sustava Office."
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


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Praktični vodič: Azure Active Directory Integracija sa sustavom Office virtualne 8 x 8

Cilj ovog praktičnog vodiča će vam pokazati kako integrirati Office virtualne 8 x 8 Azure Active Directory (Azure AD).

Integriranje 8 x 8 virtualne sustava Office s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Office virtualne 8 x 8
- Možete omogućiti svojim korisnicima da automatski se prijavili u Office virtualne 8 x 8 (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija sa sustavom Office virtualne 8 x 8, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na 8 x 8 virtualne Office jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Microsoft Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje 8 x 8 virtualne Office iz galerije
2. Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Dodavanje 8 x 8 virtualne Office iz galerije
Da biste konfigurirali integraciju sustava Office virtualne 8 x 8 u Azure AD, morate dodati 8 x 8 virtualne Office iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali 8 x 8 virtualne Office iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Active Directory**.

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **8 x 8 virtualne Office**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. U oknu s rezultatima odaberite **8 x 8 virtualne Office**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu s 8 x 8 virtualne Office utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba znati koje postoji zamjena u obliku korisnika u 8 x 8 virtualne Office korisniku u Azure AD. Drugim riječima, veza odnos između programa Azure AD korisnik i povezane u 8 x 8 virtualne Office mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničkog imena** u sustavu Office virtualne 8 x 8.

Konfiguriranje i testiranje Microsoft Azure AD jedinstvenu prijavu sa sustavom Office virtualne 8 x 8, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Microsoft Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Microsoft Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje korisnika test 8 x 8 virtualne Office](#creating-a-8x8-virtual-office-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u sustavu Office virtualne 8 x 8 koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Microsoft Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Konfiguriranje Microsoft Azure AD jedinstvene prijave

U ovom ćete odjeljku Omogućivanje Microsoft Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Office virtualne 8 x 8.

**Da biste konfigurirali Microsoft Azure AD jedinstvenu prijavu 8 x 8 virtualne Office, učinite sljedeće:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Office virtualne 8 x 8** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako želite korisnicima da biste se prijavili u Office virtualne 8 x 8** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    na. U tekstni okvir **URL odgovor** upišite:`https://sso.8x8.com/saml2`

    b. Kliknite **Dalje**

4. Na stranici **Konfiguracija jedinstvenu prijavu u uredu virtualne 8 x 8** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.

5. Prijava za klijentski sustav 8 x 8 virtualne Office kao administrator.
6. Odaberite **Voditelj virtualne Office računa** u aplikaciji.

    ![Konfiguriranje na strani aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Odaberite račun **tvrtke** za upravljanje i kliknite **Prijava** .

    ![Konfiguriranje na strani aplikacija](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Kliknite karticu **računi** popisu izbornika.

    ![Konfiguriranje na strani aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Na popisu računa kliknite **Jedinstvenu prijavu** .

    ![Konfiguriranje na strani aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. Odaberite **Signle prijave** u odjeljku način provjere autentičnosti i kliknite **SAML**.

    ![Konfiguriranje na strani aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Kopirajte **URL SAML SSO**, **jednu cvijeća Out URL servisa** i **izdavač URL** iz Azure AD se **prijaviti u URL-a**, **odjavite URL-a** i **URL izdavača** u 8 x 8 virtualne Office. 

    ![Konfiguriranje na strani aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Konfiguriranje na strani aplikacije](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Kliknite gumb za **preglednik** da biste prenijeli certifikat koji ste preuzeli s Azure AD.

13. Kliknite gumb **Spremi** .

14. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

15. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.
    
![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **ulogama** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>Stvaranje korisnika test virtualne Office 8 x 8

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u sustavu Office virtualne 8 x 8. 8 x 8 virtualne Office podržava samo-u – vrijeme dodjele resursa, koji je po zadanom omogućen.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. Novi korisnik stvorit će se tijekom pokušaja pristupiti 8 x 8 virtualne Office Ako još ne postoji. 

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnik sustava ćete morati obratiti timu za podršku za Office virtualne 8 x 8.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika probno Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju 8 x 8 virtualne Office.
    
![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona 8 x 8 virtualne Office, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Office virtualne 8 x 8**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je za testiranje sustava Microsoft Azure AD jedinstvenu prijavu konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu 8 x 8 virtualne Office na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Office virtualne 8 x 8.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
