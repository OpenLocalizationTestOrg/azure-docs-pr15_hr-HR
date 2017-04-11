<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s 23 videozapis | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i 23 videozapis."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Praktični vodič: Azure Active Directory Integracija s 23 videozapis

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji 23 videozapis s Azure Active Directory (Azure AD).  
Integriranje 23 videozapis s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup 23 videozapis 
- Možete omogućiti svojim korisnicima da automatski se prijavili u 23 videozapis (jedinstvenu prijavu) s računa za Azure AD

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija 23 videozapis, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na 23 videozapis jedinstvene prijave na omogućeno pretplate


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje videozapisa 23 iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-23-video-from-the-gallery"></a>Dodavanje videozapisa 23 iz galerije
Da biste konfigurirali integraciju 23 videozapisa u Azure AD, morate dodati 23 videozapis iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali 23 videozapis iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **23 videozapis**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **23 videozapis**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s 23 videozapis utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u 23 videozapis korisniku u Azure AD. Drugim riječima, veza odnos između programa Azure AD korisnik i povezane u 23 videozapis mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u 23 videozapis.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s 23 videozapis, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje 23 videozapisa testiranje korisnika](#creating-a-23-video-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u 23 videozapis koji je povezan s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji 23 videozapis.

**Da biste konfigurirali Azure AD jedinstvenu prijavu 23 videozapis, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **23 videozapisa** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili na 23 videozapis** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][7] 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Azure AD jedinstvene prijave][8] 
 
     na. U tekstni okvir **URL odgovor** , unesite URL koristi korisnika za prijavu na web-mjesto 23 videozapis (npr.: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] Active Directory Integracija pomoću SAML 2.0 dostupna je za sve korisnike videozapis 23. Obratite se podršci na [support@23company.com](mailto:support@23company.com) ako vam je potrebna povezane metapodataka.

     b. Kliknite **Dalje**.
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na 23 videozapis** poduzeti sljedeće korake:

    ![Azure AD jedinstvene prijave][9] 

    na. Kliknite Preuzimanje certifikat, a zatim spremite datoteku na računalu.

    b. Zatražite od 23 videozapis podršku putem [support@23company.com](mailto:support@23company.com), pošaljite im preuzete certifikat **Izdavača URL**, **Jedan prijave URL servisa**, a zatim **Jednu Sign-Out URL-a**i zamolite ga s postavljanjem SSO za 23 aplikacije videozapis. 

    c. Kliknite **Dalje**.


6. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
  
    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **ulogama** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-23-video-test-user"></a>Stvaranje 23 videozapis testnih korisnika

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u videozapisu 23.

**Da biste stvorili korisnik naziva dan Britta Simona u 23 videozapis, učinite sljedeće:**

1. Prijavite se vaše 23 videozapis web-mjesto tvrtke kao administrator.

1. Idite na **Postavke**.


2. U sekciji **korisnika** kliknite **Konfiguriraj**.

    ![Dodijeli korisniku][400]

3. Kliknite **Dodaj novog korisnika**. 

    ![Dodijeli korisniku][401]

4. U odjeljku **Pozovite nekoga da biste se pridružili ovo web-mjesto** , učinite sljedeće:

    ![Dodijeli korisniku][402]

    na. U tekstni okvir **Adresa e-pošte** upišite adresu e-pošte dan Britta Simona Azure AD.

    b. Kliknite **Dodaj korisnika**.   


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika probno Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju 23 videozapis.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona 23 videozapis, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **23 videozapis**.

    ![Dodijeli korisniku][202] 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu 23 videozapis na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji 23 videozapis.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




