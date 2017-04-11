<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija sa sustavom Exchange informacije Alcumus | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i sustava Exchange Alcumus informacije."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Praktični vodič: Azure Active Directory Integracija sa sustavom Exchange Alcumus informacije

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Exchange Alcumus informacije s Azure Active Directory (Azure AD).  
Integriranje sustava Exchange Alcumus informacije s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Alcumus informacije Exchange 
- Možete omogućiti svojim korisnicima da automatski se prijavili u sa sustavom Exchange Alcumus Info (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija sa sustavom Exchange Alcumus informacije, potrebne su vam sljedeće stavke:

- Pretplatu na [Azure AD](https://azure.microsoft.com/)
- Sustava [Exchange informacije Alcumus](http://www.alcumusgroup.com/) jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od tri glavna sastavni blokovi:

1. Dodavanje Exchange Alcumus informacije iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Dodavanje Exchange Alcumus informacije iz galerije
Da biste konfigurirali integraciju sustava Exchange Alcumus informacije u Azure AD, morate dodati Exchange Alcumus informacije iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Exchange Alcumus informacije iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Alcumus informacije Exchange**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **Exchange Alcumus informacije**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu sa sustavom Exchange informacije Alcumus utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u sustavu Exchange Alcumus podaci o korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u sustavu Exchange informacije Alcumus Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničkog imena** u sustavu Exchange Alcumus informacije.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu sa sustavom Exchange Alcumus informacije, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Exchange za informacije Alcumus testiranje korisnika](#creating-a-alcumus-info-exchange-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u sustavu Exchange informacije Alcumus koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciju sustava Exchange Alcumus informacije.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Alcumus informacije Exchange, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Alcumus informacije Exchange** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6]

2. Na stranici **kako biste željeli korisnika da biste se prijavili Exchange informacije Alcumus** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][7]

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake: 

    ![Azure AD jedinstvene prijave][8]
 
    na. u tekstni okvir **URL odgovor** upišite URL potrošača koja je instalacijski program za koje služba za podršku Alcumus informacije Exchange.

    > [AZURE.NOTE] Ako ne znate što je desne vrijednosti, obratite se timu za podršku Alcumus informacije Exchange putem [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Kliknite **Dalje**.
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Exchange Alcumus informacije** kliknite **Preuzimanje metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Što je Azure AD Connect][9]

5. Obratite se timu za podršku Alcumus informacije Exchange putem [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), pošaljite im datoteku metapodataka i ih joj koje treba omogućuju SSO za vas.


6. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Što je Azure AD Connect][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Što je Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.
  
    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.
  
    c. Kliknite Dalje.



6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    na. U tekstni okvir **ime** upišite **Britta**.  
  
    b. U na **Prezime** txtbox, vrsta **Dan Simona**.
  
    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.
  
    d. Na popisu **uloga** odaberite **korisnik**.
  
    e. Kliknite **Dalje**.


7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.
  
    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Stvaranje korisnika test Alcumus informacije Exchange

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u sustavu Exchange Alcumus informacije.

**Da biste stvorili korisnik naziva dan Britta Simona u sustavu Exchange Alcumus informacije, učinite sljedeće:**

1. Obratite se timu za podršku Alcumus informacije Exchange putem [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju sa sustavom Exchange Alcumus informacije.

![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona Alcumus informacije Exchange, učinite sljedeće:**

1. Na portalu Azure da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Exchange Alcumus informacije**.

    ![Dodijeli korisniku][202]

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu Exchange Alcumus informacije na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji sustava Exchange Alcumus informacije.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png