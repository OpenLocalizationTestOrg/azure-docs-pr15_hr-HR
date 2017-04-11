<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s UserEcho | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i UserEcho."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Praktični vodič: Azure Active Directory Integracija s UserEcho

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji UserEcho s Azure Active Directory (Azure AD).  
Integriranje UserEcho s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup UserEcho 
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste UserEcho (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija UserEcho, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na UserEcho jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje UserEcho iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-userecho-from-the-gallery"></a>Dodavanje UserEcho iz galerije
Da biste konfigurirali Integracija UserEcho u Azure AD, morate dodati UserEcho iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali UserEcho iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **UserEcho**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_01.png)

7. U oknu s rezultatima odaberite **UserEcho**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s UserEcho utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u UserEcho korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u UserEcho Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u UserEcho.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s UserEcho, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na UserEcho testiranje korisnika](#creating-a-userecho-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u UserEcho koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji UserEcho. 




**Da biste konfigurirali Azure AD jedinstvenu prijavu UserEcho, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **UserEcho** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili UserEcho** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koriste vaši korisnici da biste se prijavili aplikaciju UserEcho (npr.: *https://fabrikam.UserEcho.com/*).

    b. Kliknite **Dalje**.
 
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na UserEcho** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


1. U drugom prozoru preglednika, prijavite se web-mjesto tvrtke UserEcho kao administrator.

1. Na alatnoj traci na vrhu kliknite svoje korisničko ime proširite izbornik, a zatim **Postavljanje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

1. Kliknite **integracije**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 


1. Kliknite **web-mjesto**, a zatim kliknite **jedinstvenu prijavu (SAML2)**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 


1. Na stranici **jedinstvenu prijavu (SAML)** , poduzmite sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png) 

    na. Kao **SAML omogućena**, odaberite **da**. 

    b. Azure klasični portalu na na Konfiguriraj jedinstvenu prijavu na UserEcho dijalošku stranicu kopirajte vrijednost **Jedne prijave URL servisa** , a zatim je zalijepite u tekstni okvir **SAML SSO URL** .

    c. Azure klasični portalu na na Konfiguriraj jedinstvenu prijavu na stranicu dijaloga UserEcho, kopirajte **URL daljinskog odjavite** vrijednost, a pa ih zalijepite u tekstni okvir **URL udaljene logoout** . 

    d. Otvorite preuzete certifikat u Bloku za pisanje, kopirajte sadržaj, a pa ih zalijepite u tekstni okvir **Certifikat X.509** .    

    e. Kliknite **Spremi**.


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_09.png)  

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_05.png)  

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-userecho-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-userecho-test-user"></a>Stvaranje UserEcho testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u UserEcho korisnika.

**Da biste stvorili korisnik naziva dan Britta Simona u UserEcho, učinite sljedeće:**

1. Prijava na web-mjesto tvrtke UserEcho kao administrator.

1. Na alatnoj traci na vrhu kliknite svoje korisničko ime proširite izbornik, a zatim **Postavljanje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

1. Kliknite **korisnici**, proširite odjeljak **korisnika** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png) 

1. Kliknite **korisnici**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png) 

1. Kliknite **Pozovi novog korisnika**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)


1. U dijaloškom okviru **pozivanje novog korisnika** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png) 

    na. U tekstni okvir **naziv** upišite **Dan Britta Simona**.

    b. U tekstni okvir **e-pošte** upišite Britta na adresu e-pošte na portalu za Azure klasični.

    c. Kliknite **Pozovi**.

Pozivnica se šalje Britta, čime se omogućuje joj da biste počeli koristiti UserEcho. 



### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju UserEcho.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona UserEcho, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **UserEcho**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu UserEcho na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji UserEcho.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_205.png






