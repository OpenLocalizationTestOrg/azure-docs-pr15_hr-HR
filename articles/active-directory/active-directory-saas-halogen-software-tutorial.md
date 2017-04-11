<properties
    pageTitle="Praktični vodič: Azure Active Directory integracije sa softverom Halogen"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Halogen softver."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Praktični vodič: Azure Active Directory integracije sa softverom Halogen

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Halogen softvera s Azure Active Directory (Azure AD).

Integriranje Halogen softvera s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Halogen softver 
- Možete omogućiti svojim korisnicima da automatski se prijavili u Halogen softver (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD integracije sa softverom Halogen, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na softver Halogen jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Halogen softvera iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-halogen-software-from-the-gallery"></a>Dodavanje Halogen softvera iz galerije
Da biste konfigurirali integraciju Halogen softvera u Azure AD, morate dodati Halogen softver iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Halogen softver iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice. 

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **halogen softvera**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **Halogen softver**, a zatim **Dovršeno** da biste dodali aplikaciju.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu sa softverom Halogen utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Halogen softver korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Halogen softver Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Halogen softvera.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu pomoću softvera za Halogen, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jednu jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Halogen softver testiranje korisnika](#creating-a-halogen-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Halogen softver koji je povezan s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfiguriranje Azure AD jednu jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Halogen softvera.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Halogen softver, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Softver Halogen** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][8]

2. Na stranici **kako biste željeli korisnika da biste se prijavili softver Halogen** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][9]

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:  ![Konfiguriranje postavki aplikacije][10]
 
     na. u tekstni okvir **Prijavite se na URL** unesite URL koriste vaši korisnici da biste se prijavili softver Halogen aplikacija pomoću sljedećeg uzorka: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Kliknite **Dalje**.
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Halogen softver** kliknite **Preuzimanje metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.
    
    ![Što je Azure AD Connect][11]

5. U prozoru drugi preglednik, prijavu u aplikaciju **Halogen softver** kao administrator.

6. Kliknite karticu **Mogućnosti** . 

    ![Što je Azure AD Connect][12]


7. U lijevom navigacijskom oknu kliknite **SAML konfiguracije**. 

    ![Što je Azure AD Connect][13]

8. Na stranici **Konfiguracija SAML** , učinite sljedeće:  ![što je Azure AD Connect][14]

    na. Kao **Jedinstveni identifikator**, odaberite **NameID**.

    b. Kao **Jedinstveni identifikator karte da biste**, odaberite **korisničko ime**.

    c. Da biste prenijeli metapodataka preuzetu datoteku, kliknite **Pregledaj** da biste odabrali datoteku, a zatim **Prenesi datoteku**.

    d. Da biste testirali konfiguraciju, kliknite **Pokreni testiranje**. 

    > [AZURE.NOTE] Ćete morati pričekati poruke "*na SAML test je dovršena. Zatvorite prozor*". Zatvorite prozor preglednika otvorena. Potvrdni okvir **Omogući SAML** omogućena samo ako test je dovršena.

    e. Odaberite **Omogući SAML**.
    
    f. Kliknite **Spremi promjene**. 


9. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** . 

    ![Što je Azure AD Connect][15]

10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Što je Azure AD Connect][16]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Što je Azure AD Connect][100] 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Što je Azure AD Connect][101] 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Što je Azure AD Connect][102] 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Što je Azure AD Connect][103] 
 
    na. Kao **Korisnik vrste**, odaberite **novi korisnik u tvrtki ili ustanovi**.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite Dalje.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Što je Azure AD Connect][104] 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U na **Prezime** txtbox, vrsta **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Što je Azure AD Connect][105]  

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Što je Azure AD Connect][106]   

    na. Zapišite vrijednost **Novu lozinku**.
    b. Kliknite **dovrši**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Stvaranje korisnika test Halogen softver

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona Halogen softvera.

**Da biste stvorili korisnik naziva dan Britta Simona u Halogen softver, učinite sljedeće:**

1. Prijava u aplikaciju **Halogen softver** kao administrator.

2. Kliknite karticu **Centar za korisnika** , a zatim kliknite **Create User**.

    ![Što je Azure AD Connect][300]  

3. Na stranici dijaloški okvir **Novi korisnik** , učinite sljedeće:

    ![Što je Azure AD Connect][301]

    na. U tekstni okvir **ime** upišite **Britta**. 
  
    b. U tekstni okvir **Prezime** upišite **Dan Simona**.
  
    c. U tekstni okvir **korisničko ime** upišite **korisničko ime dan Brita Simona na portalu za Azure klasični**.
  
    d. U tekstni okvir **Lozinka** upišite lozinku za Britta.
  
    e. Kliknite **Spremi**.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Halogen softver.

![Što je Azure AD Connect][200]

**Da biste dodijelili dan Britta Simona Halogen softver, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Što je Azure AD Connect][201]

2. Na popisu aplikacija odaberite **Halogen softvera**.

    ![Što je Azure AD Connect][202]

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Što je Azure AD Connect][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

    ![Što je Azure AD Connect][204]

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Što je Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Halogen softver na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Halogen softvera.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png