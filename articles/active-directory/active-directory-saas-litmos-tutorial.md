<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Litmos | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Litmos."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Praktični vodič: Azure Active Directory Integracija s Litmos

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Litmos s Azure Active Directory (Azure AD).  
Integriranje Litmos s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Litmos 
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Litmos (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure Active Directory 

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija Litmos, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Litmos jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od tri glavna sastavni blokovi:

1. Dodavanje Litmos iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-litmos-from-the-gallery"></a>Dodavanje Litmos iz galerije
Da biste konfigurirali Integracija Litmos u Azure AD, morate dodati Litmos iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Litmos iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Litmos**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **Litmos**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Litmos utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Litmos korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Litmos Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Litmos.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Litmos, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na Litmos testiranje korisnika](#creating-a-halogen-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Litmos koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu klasični Azure AD i konfiguriranje jedinstvenu prijavu u aplikaciji Litmos.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

U sklopu konfiguracije, morate prilagodite **SAML tokena atribute** Litmos aplikacije.  

![Azure AD jedinstvene prijave][17] 

**Da biste konfigurirali Azure AD jedinstvenu prijavu Litmos, poduzmite sljedeće korake:**

1. Azure AD klasični portalu na stranici za integraciju aplikacije **Litmos** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Litmos** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
 
    ![Azure AD jedinstvene prijave][7] 


1. Prijava na web-mjesto tvrtke Litmos (npr.: *https://azureapptest.litmos.com/account/Login*) kao administrator.

    ![Azure AD jedinstvene prijave][21] 


1. Na navigacijskoj traci na lijevoj strani kliknite **računi**.

    ![Azure AD jedinstvene prijave][22] 


1. Kliknite karticu **integracije** .

    ![Azure AD jedinstvene prijave][23] 


1. Na kartici **integracije** pomaknite se do **3 integracije strana**, a zatim karticu **SAML 2.0** .

    ![Azure AD jedinstvene prijave][24] 

1. Kopirajte vrijednost u odjeljku **je u SAML endoiint za litmos:**.

    ![Azure AD jedinstvene prijave][26] 


3. Azure klasični portalu na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Azure AD jedinstvene prijave][8] 
 
    na. U tekstni okvir **identifikator** upišite URL koristi za prijavu u aplikaciju Litmos korisnika (npr.: *https://azureapptest.litmos.com/account/Login*).
     
    b. U tekstni okvir **URL odgovor** zalijepite vrijednost koju ste kopirali iz aplikacije Litmos u prethodnom koraku.

    c. Kliknite **Dalje**.
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Litmos** poduzeti sljedeće korake:

    ![Azure AD jedinstvene prijave][2] 

    na. Kliknite Preuzimanje certifikat, a zatim spremite datoteku na računalu.


1. U aplikaciji **Litmos** poduzeti sljedeće korake:

    ![Azure AD jedinstvene prijave][25] 

    na. Kliknite **Omogući SAML**.

    b. Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

    >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    c. Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **SAML certifikat X.509** .

    d. Kliknite **Spremi promjene**.


6. Na portalu klasični Azure AD odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
  
    ![Azure AD jedinstvene prijave][11]


20. Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** . 

    ![Konfiguriranje jedinstvenu prijavu][12]


24. U dijaloškom okviru **Dodavanje atributa korisnik** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu][14]

  	| Naziv atributa | Vrijednost atributa |
  	| ---            | ---             |
  	| E-pošte          | User.Mail       |
  	| Ime      | User.givenname  |
  	| Prezime       | User.surname    |

    Za svaki redak podatke iz gornje tablice, učinite sljedeće:
   
    na. Kliknite **Dodavanje korisnika atribut**. 

    ![Konfiguriranje jedinstvenu prijavu][15]


    na. U tekstni okvir **Naziv atributa** upišite **Naziv atributa** koje se prikazuje za taj redak.

    b. Odaberite **Vrijednost atributa** koja se prikazuje za taj redak.

    c. Kliknite **dovrši** da biste zatvorili dijaloški okvir **Dodavanje atributa korisnika** .


25. Kliknite **Primijeni promjene**. 

    ![Konfiguriranje jedinstvenu prijavu][16]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Portal za Azure clasic**na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    na. Kao **Korisnik vrste**, odaberite **novi korisnik u tvrtki ili ustanovi**.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Stvaranje Litmos testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u Litmos korisnika.  
Litmos aplikacija podržava samo u vrijeme dodjele resursa. To znači korisnički račun automatski se stvara po potrebi tijekom pokušaja pristup aplikaciji pomoću ploče programa Access.

**Da biste stvorili korisnik naziva dan Britta Simona u Litmos, učinite sljedeće:**


1. Prijava na web-mjesto tvrtke Litmos (npr.: *https://azureapptest.litmos.com/account/Login*) kao administrator.

    ![Azure AD jedinstvene prijave][21] 


1. Na navigacijskoj traci na lijevoj strani kliknite **računi**.

    ![Azure AD jedinstvene prijave][22] 


1. Kliknite karticu **integracije** .

    ![Azure AD jedinstvene prijave][23] 


1. Na kartici **integracije** pomaknite se do **3 integracije strana**, a zatim karticu **SAML 2.0** .

    ![Azure AD jedinstvene prijave][24] 

1. Odaberite **Automatsko generiranje korisnika:**.

    ![Azure AD jedinstvene prijave][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Litmos.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Litmos, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Litmos**.

    ![Dodijeli korisniku][202] 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu Litmos na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Litmos.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





