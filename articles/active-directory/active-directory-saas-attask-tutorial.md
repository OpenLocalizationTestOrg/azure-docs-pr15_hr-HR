<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s @Task| Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i @Task."
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


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Praktični vodič: Azure Active Directory Integracija s@Task

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji @Task s Azure Active Directory (Azure AD).  
Integriranje @Task s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup@Task
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste @Task (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični Portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija s @Task, morate sljedećih stavki:

- Pretplatu na Azure AD
- Na @Task jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od tri glavna sastavni blokovi:

1. Dodavanje @Task iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-task-from-the-gallery"></a>Dodavanje @Task iz galerije
Da biste konfigurirali integracije s @Task u Azure AD, najprije morate dodati @Task iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali @Task iz galerije, učinite sljedeće:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1] 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2] 

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3] 

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4] 

6. U okvir za pretraživanje upišite **@Task**.

    ![Aplikacija][5] 

7. U oknu s rezultatima odaberite **@Task**, a zatim kliknite **dovrši** da biste dodali aplikaciju.

    ![Aplikacija][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave

Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s @Task utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba znati koje postoji zamjena u obliku korisnika u @Task korisniku u Azure AD je. Drugim riječima, veza odnos između Azure AD korisnik i povezane korisnika u @Task treba uspostaviti.   
Taj odnos vezu uspostavljanja dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u @Task.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s @Task, koje morate poduzeti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na @Tasktest korisnika](#creating-a-halogen-software-test-user) ** – da bi se na postoji zamjena u obliku od Dan Simona Britta u @Taskthat predstavljanje Azure AD njom povezana.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u vašem @Task aplikacije.

**Da biste konfigurirali Azure AD jedinstvenu prijavu s @Task, poduzeti sljedeće korake:**

1. Azure klasični portalu na na **@Task** integraciju aplikacija kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na na **kako biste željeli korisnika da biste se prijavili na @Task ** stranice odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][7] 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje postavki aplikacije][8] 
 
     na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnicima prijave za vaše @Task aplikacije (npr.:*https://<Tenant name>.attask ondemand.com*).

     b. Kliknite **Dalje**.

4. Na na **konfigurirati jedinstvenu prijavu na @Task ** stranice, kliknite **Preuzmi metapodataka**, spremite datoteku metapodataka lokalno na vašem računalu, a zatim kliknite **Dalje**.

    ![Što je Azure AD Connect][9] 



1. Prijava na vašem @Task web-mjesto tvrtke kao administrator.

2. Idite na **jedine prijave konfiguracije**.


1. U dijaloškom okviru **Jedinstvenu prijavu** poduzeti sljedeće korake

    ![Konfiguriranje jedinstvenu prijavu][23]

    na. Kao **vrstu**odaberite **SAML 2.0**.

    b. Odaberite **ID davatelja usluga**.

    c. Azure portala za klasični kopirajte **URL daljinskog prijava**, a pa ih zalijepite u tekstni okvir **URL portala za prijavu** .

    d. Azure portala za klasični kopirajte **URL servisa za jednu Sign-Out**i pa ih zalijepite u tekstni okvir **Sign-Out URL** .

    e. Azure portala za klasični kopirajte **URL Promijeni lozinku**, a pa ih zalijepite u tekstni okvir **URL Promijeni lozinku** .

    f. Kliknite **Spremi**.

6. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Što je Azure AD Connect][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Što je Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-an-task-test-user"></a>Stvaranje programa @Task testnih korisnika

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u @Task.


**Stvaranje korisnika naziva dan Britta Simona u @Task, poduzeti sljedeće korake:**

1. Prijavite se na vaše @Task web-mjesto tvrtke kao administrator.

2. Na izborniku na vrhu kliknite **osobe**.

3. Kliknite **novu osobu**. 

4. U dijaloškom okviru Nova osoba poduzeti sljedeće korake:

    ![Stvaranje programa @Task testnih korisnika][21] 

    na. U tekstni okvir **ime** upišite "Britta".

    b. U tekstni okvir **Prezime** upišite "dan Simona".

    c. U tekstni okvir **Adresa e-pošte** upišite adresu e-pošte dan Britta Simona u servisu Azure Active Directory.

    d. Kliknite **Dodavanje osoba**.




### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da date pristup @Task.

![Dodijeli korisniku][200] 

**Da biste dodijelili Dan Simona Britta da biste @Task, poduzeti sljedeće korake:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **@Task**.

    ![Dodijeli korisniku][202] 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete na @Task pločica na ploči za pristup vam automatski prijavljeni na za vaše @Task aplikacije.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






