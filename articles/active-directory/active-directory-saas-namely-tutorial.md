<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s odnosno | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i bilješke."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Praktični vodič: Azure Active Directory Integracija s bilješke

Cilj ovog praktičnog vodiča je da biste prikazali odnosno integrirati Azure Active Directory (Azure AD).

Integriranje bilješke sa Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup bilješke 
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste bilješke (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD integracije sa bilješke, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U bilješke jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje bilješke iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-namely-from-the-gallery"></a>Dodavanje bilješke iz galerije
Da biste konfigurirali integracije s bilješke u Azure AD, morate dodati bilješke iz galerije popis upravljani SaaS aplikacija.

**Da biste dodali bilješke iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **bilješke**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. U oknu s rezultatima odaberite **bilješke**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s odnosno utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje postoji zamjena u obliku korisniku u bilješke programa korisnika u Azure AD. Drugim riječima, veza odnos između programa Azure AD korisnik i povezane u bilješke mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u bilješke.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s bilješke, koje je potrebno dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranja u bilješke testiranje korisnika](#creating-a-namely-test-user)** – da bi se na postoji zamjena u obliku od dan Britta Simona u bilješke koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u svoje bilješke aplikacije. 




**Da biste konfigurirali Azure AD jedinstvenu prijavu s bilješke, učinite sljedeće:**

1. Azure klasični portalu na **bilješke** Integracija stranici aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili na bilješke** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika da biste se prijavili svoje bilješke aplikacije (npr.: *https://fabrikam.Namely.com/*).

    b. Kliknite **Dalje**.
 
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na bilješke** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


1. U drugom prozoru preglednika, prijavite se na vaše bilješke web-mjesto tvrtke kao administrator.

1. Na alatnoj traci na vrhu kliknite **tvrtke**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Kliknite karticu **Postavke** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Kliknite **SAML**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Na stranici s **Postavkama SAML** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    na. Kliknite **Omogući SAML**. 

    b. Azure klasični portalu na stranicu dijaloga **odnosno konfiguriranje jedinstvenu prijavu na** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **url za DDO davatelja identiteta** . 

    c. Otvorite preuzete certifikat u Bloku za pisanje, kopirajte sadržaj, a pa ih zalijepite u tekstni okvir za **potvrdu davatelja identiteta** .    

    d. Kliknite **Spremi**.


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]


**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-namely-test-user"></a>Stvaranje na bilješke testiranje korisnika

Cilj ovaj odjeljak je stvaranje korisnika pod nazivom dan Britta Simona bilješke.

**Da biste stvorili korisnika pod nazivom dan Britta Simona bilješke, učinite sljedeće:**

1. Prijava za svoje bilješke web-mjesto tvrtke kao administrator.

1. Na alatnoj traci na vrhu kliknite **osobe**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Kliknite karticu **imenik** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Kliknite **Dodaj novu osobu**.



1. U dijaloškom okviru **Dodavanje nove osobe** poduzeti sljedeće korake:

    na. U tekstni okvir **ime** upišite **Britta**.

    b. U tekstni okvir **Prezime** upišite **Dan Simona**.

    c. U tekstni okvir **e-pošte** upišite Britta na adresu e-pošte na portalu za Azure klasični.

    d. Kliknite **Spremi**.





### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta da biste koristili Azure jedinstvenu prijavu, odnosno dodjelu pristup.

![Dodijeli korisniku][200] 

**Da biste dodijelili Dan Simona Britta da biste bilješke, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **bilješke**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete u bilješke pločica na ploči za pristup, koje treba se automatski prijavljeni u da biste svoje bilješke aplikacije.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






