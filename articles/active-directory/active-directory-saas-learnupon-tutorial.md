<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Novatus | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i LearnUpon."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Praktični vodič: Azure Active Directory Integracija s LearnUpon

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji LearnUpon s Azure Active Directory (Azure AD).  
Integriranje LearnUpon s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup LearnUpon
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste LearnUpon (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure Active Directory klasični 


Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija LearnUpon, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na LearnUpon jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje LearnUpon iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-learnupon-from-the-gallery"></a>Dodavanje LearnUpon iz galerije
Da biste konfigurirali Integracija LearnUpon u Azure AD, morate dodati LearnUpon iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali LearnUpon iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **LearnUpon**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. U oknu s rezultatima odaberite **LearnUpon**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s LearnUpon utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u LearnUpon korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u LearnUpon Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u LearnUpon.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s LearnUpon, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na LearnUpon testiranje korisnika](#creating-a-learnupon-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u LearnUpon koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji LearnUpon.



**Da biste konfigurirali Azure AD jedinstvenu prijavu LearnUpon, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **LearnUpon** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili LearnUpon** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    na. U tekstni okvir **URL odgovor** upišite URL servisa potrošača pridruživanju pomoću sljedećeg uzorka:`https://\<companyname\>.learnupon.com/saml/consumer`

    b. Kliknite **Dalje**. 


4. Na stranici **Konfiguracija jedinstvenu prijavu na LearnUpon** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu. Će moramo ovog certifikata i URL-ovi metapodataka (ID entitet, SSO URL za prijavu i URL izgleda za prijavu) da biste postavili SSO LearnUpon strane.

    b. Kliknite **Dalje**.




1. Otvorite neki drugi instanca preglednika i prijavite se u LearnUpon pomoću administratorskog računa. 

1. Kliknite karticu **Postavke** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. Kliknite **Jedinstvenu prijavu - SAML**, a zatim kliknite **Opće postavke** za konfiguriranje postavki SAML.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. U odjeljku **Općenite postavke** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    na. Odaberite **omogućena**.

    b. Kao **verzija**, odaberite **2.0**.

    c. Kao **Preskoči uvjeta**, odaberite " **ne**".

    d. U tekstni okvir **SAML tokena objavljuju parametarskog naziv** upišite naziv parametra objavu zahtjev za URL potrošača SAML naznačen iznad te sadrži pridruživanju SAML da biste provjerili i autentičnost – na primjer **SAMLResponse**.

    e. U tekstni okvir **Oblik naziva identifikator** unesite željenu vrijednost upućuje na to gdje u vašem SAML pridruživanju identifikator korisnika (adresu e-pošte) nalazi – na primjer **urn: organizacija: imena: tc: SAML:1.1:nameid-oblik: adresa**.

    f. U tekstni okvir **Odredite mjesto davatelja** upišite vrijednost koja pokazuje gdje korisnici se šalju ako kliknu na ikoni servisa prenesene sa zaslona Azure klasični prijavu na portal.

    g.in Azure klasični portal kopirajte **URL servisa za jednu Sign-Out**, a zatim je zalijepite u textbos **odjavite URL-a** .

    h. Kliknite **Upravljanje prst ispisuje**, a zatim prijenos ispis prst preuzete certifikata. 


1. Kliknite **Korisničke postavke**, a zatim izvršite sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    na. U tekstni okvir **Oblik identifikator ime** unesite željenu vrijednost nam govore gdje u vašem pridruživanju SAML ime korisnika nalazi – na primjer: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ givenname**.

    b. U tekstni okvir **Zadnjim oblikovanjem identifikator naziv** unesite željenu vrijednost nam govore gdje u vašem pridruživanju SAML Prezime korisnika nalazi – na primjer: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ Prezime**.


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-learnupon-test-user"></a>Stvaranje LearnUpon testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u LearnUpon korisnika. LearnUpon podržava samo-u – vrijeme dodjele resursa, koji je po zadanom omogućen.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. Novi korisnik stvorit će se tijekom pokušaja pristupa LearnUpon Ako još ne postoji. [Konfiguriranje Azure AD jedinstvene prijave](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnik sustava ćete morati obratite se timu za podršku LearnUpon.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju LearnUpon.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona LearnUpon, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **LearnUpon**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu LearnUpon na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji LearnUpon.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png
