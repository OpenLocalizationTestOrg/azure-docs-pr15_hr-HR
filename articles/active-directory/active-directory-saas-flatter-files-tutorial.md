<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Flatter datoteke | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Flatter datoteke."
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


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Praktični vodič: Azure Active Directory Integracija s Flatter datoteka

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Flatter datoteka s Azure Active Directory (Azure AD).  
Integriranje Flatter datoteke s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Flatter datoteka 
- Možete omogućiti svojim korisnicima da automatski se prijavili u Flatter datoteke (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – klasični portal za Azure Active Directory

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija s Flatter datotekama, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Flatter datoteke jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Flatter datoteka iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-flatter-files-from-the-gallery"></a>Dodavanje Flatter datoteka iz galerije
Da biste konfigurirali integraciju Flatter datoteka u Azure AD, morate dodati Flatter datoteke iz galerije popis upravljani SaaS aplikacija.

**Da biste dodali Flatter datoteke iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Flatter datoteke**.


    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. U oknu s rezultatima odaberite **Flatter datoteke**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Flatter datotekama utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Flatter datoteke korisniku u Azure AD. Drugim riječima, veza odnosa između tablica Azure AD korisnik i povezane korisnika u Flatter datoteke mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Flatter datoteke.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Flatter datotekama, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Flatter datoteka testiranje korisnika](#creating-a-halogen-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Flatter datoteke koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu klasični Azure AD i konfiguriranje jedinstvenu prijavu u aplikaciji Flatter datoteke. Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda. Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

Da biste konfigurirali jedinstvene prijave za Flatter datoteke, potrebno vam je registrirane domene. Ako nemate instaliranu na registrirane domene još, kontakt datotekama Flatter službi za podršku putem [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Da biste konfigurirali Azure AD jedinstvenu prijavu Flatter datoteke, poduzmite sljedeće korake:**

1. Azure AD klasični portalu na stranici za integraciju aplikacije **Flatter datoteke** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Flatter datoteke** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Na stranici **Konfiguracija postavki aplikacije** dijaloški okvir, kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Flatter datoteka koristi iste SSO URL za prijavu za sve klijente: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Flatter datoteke** , učinite sljedeće:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


1. Prijava u aplikaciju Flatter datoteke kao administrator.

2. Kliknite nadzorna ploča. 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Kliknite **Postavke**, a zatim na kartici **tvrtke** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    na. Odaberite **koristi SAML 2.0 za provjeru autentičnosti**.

    b. Kliknite **Konfiguriraj SAML**.



2. U dijaloškom okviru **Konfiguriranje SAML** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    na. U tekstni okvir domenu upišite registrirane domene.

    > [AZURE.NOTE] Ako nemate instaliranu na registrirane domene još, kontakt datotekama Flatter službi za podršku putem [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. U na Azure klasični portala na jednom Konfiguriraj prijave na Flatter datoteke dijaloški okvir, a zatim copt jedan prijave URL servisa i pa ih zalijepite u tekstni okvir URL davatelja identiteta.

    c.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

    >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    d.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **Certifikat za davatelja identiteta FlatterFiles** .

    e. Kliknite **Ažuriraj**.

6. Na portalu klasični Azure AD odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Stvaranje korisnika test Flatter datoteka

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u Flatter datotekama.

**Da biste stvorili korisnik naziva dan Britta Simona u Flatter datoteke, učinite sljedeće:**

1. Prijavite se web-mjesto tvrtke **Flatter datoteke** kao administrator.

2. U navigacijskom oknu s lijeve strane kliknite **Postavke**, a zatim korisnici **kartica**.

    ![Cfreate Flatter korisnik datoteke](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Kliknite **Dodaj korisnika**. 

4. U dijaloškom okviru **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Cfreate Flatter korisnik datoteke](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    na. U tekstni okvir **ime** upišite **Britta**.

    b. U tekstni okvir **Prezime** upišite **Dan Simona**. 

    c. U tekstni okvir **Adresa e-pošte** upišite Britta na adresu e-pošte na portalu za Azure klasični.

    d. Kliknite **Pošalji**.   


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Flatter datoteke.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Flatter datoteke, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Flatter datoteke**.

    ![Dodijeli korisniku](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu Flatter datoteke na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Flatter datoteke.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






