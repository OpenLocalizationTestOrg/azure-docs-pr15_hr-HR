<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s QuickHelp | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i QuickHelp."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Praktični vodič: Azure Active Directory Integracija s QuickHelp

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji QuickHelp s Azure Active Directory (Azure AD).  
Integriranje QuickHelp s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup QuickHelp 
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste QuickHelp (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija QuickHelp, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na QuickHelp jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje QuickHelp iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-quickhelp-from-the-gallery"></a>Dodavanje QuickHelp iz galerije
Da biste konfigurirali Integracija QuickHelp u Azure AD, morate dodati QuickHelp iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali QuickHelp iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **QuickHelp**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **QuickHelp**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s QuickHelp utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".


Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s QuickHelp, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na QuickHelp testiranje korisnika](#creating-a-quickhelp-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u QuickHelp koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji QuickHelp.


**Da biste konfigurirali Azure AD jedinstvenu prijavu QuickHelp, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **QuickHelp** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili QuickHelp** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][7] 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje postavki aplikacije][8] 
 
     na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnicima prijave na web-mjesto QuickHelp (npr.:* https://quickhelp.com/bsiazure/*).

     > [AZURE.NOTE] Obratite se službi za podršku QuickHelp ako ne znate vrijednost prijavite se na URL-a.

     b. Kliknite **Dalje**.

 
4. Na stranici **Konfiguracija jedinstvenu prijavu na QuickHelp** izvedite sljedeće korake: kliknite **Preuzimanje metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Što je Azure AD Connect][9] 



1. Prijava na web-mjesto tvrtke QuickHelp kao administrator.

2. Na izborniku na vrhu kliknite **administrator**.

    ![Konfiguriranje jedinstvenu prijavu][21]


1. Na izborniku **Administrator QuickHelp** kliknite **Postavke**.

    ![Konfiguriranje jedinstvenu prijavu][22]

1. Kliknite **Postavke provjere autentičnosti**.

1. Na stranici **Postavke provjere autentičnosti** poduzeti sljedeće korake

    ![Konfiguriranje jedinstvenu prijavu][23]

    na. Kao **SSO vrsta**, odaberite **WSFederation**.

    b. Da biste prenijeli datoteke preuzete Azure metapodataka, kliknite **Pregledaj**, pronađite datoteku, završili, zatim kliknite **Prijenos metapodataka**.

    c. U tekstni okvir **e-pošte** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    d. U prvi tekstni okvir **naziv** , **vrstu http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. U tekstni okvir **Prezime** , **Vrsta http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    f. **Akcijska traka**, kliknite **Spremi**.







6. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Što je Azure AD Connect][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Što je Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  
Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-quickhelp-test-user"></a>Stvaranje QuickHelp testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u QuickHelp korisnika.
Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u QuickHelp korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u QuickHelp Azure AD korisnik mora uspostaviti.

QuickHelp podržava samo-u – vrijeme dodjele resursa. To znači da, ako je potrebno, korisnički račun automatski se stvara u QuickHelp i račun povezan s računom za Azure AD.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju QuickHelp.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona QuickHelp, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **QuickHelp**.

    ![Dodijeli korisniku][202] 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu QuickHelp na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji QuickHelp.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png




