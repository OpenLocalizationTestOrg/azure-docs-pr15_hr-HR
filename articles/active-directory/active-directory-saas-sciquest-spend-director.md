<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Director provedu SciQuest | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i SciQuest provedu Director."
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


# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Praktični vodič: Azure Active Directory Integracija s Director provedu SciQuest

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Director provedu SciQuest s Azure Active Directory (Azure AD).  
Integriranje Director provedu SciQuest s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Director provedu SciQuest 
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste SciQuest provedu Director (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija Director provedu SciQuest, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na SciQuest provedu Director jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje SciQuest provedu Director iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Dodavanje SciQuest provedu Director iz galerije
Da biste konfigurirali Integracija SciQuest provedu Director u Azure AD, morate dodati SciQuest provedu Director u galeriji na popis upravljani SaaS aplikacija.

**Da biste dodali Director provedu SciQuest iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **sciQuest provedu director**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **Director provedu SciQuest**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija][6]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s provedu SciQuest Director utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Director provedu SciQuest korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Director provedu SciQuest Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Director provedu SciQuest.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s SciQuest provedu Director, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jednu jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje SciQuest provedu Director testiranje korisnika](#creating-a-halogen-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u SciQuest Director u provedu u koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfiguriranje Azure AD jednu jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Director provedu SciQuest.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Director provedu SciQuest, učinite sljedeće:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Director provedu SciQuest** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][8]

2. Na stranici **kako biste željeli korisnika da biste se prijavili Director provedu SciQuest** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][9]

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake: 

    ![Konfiguriranje postavki aplikacije][10]
 
     3.1. U tekstni okvir **Prijavite se na URL** unesite URL koriste vaši korisnici da biste se prijavili vaše aplikacije Director provedu SciQuest pomoću sljedećeg uzorka: *https://.* sciquest.com/.**

     3,2. U tekstni okvir **URL odgovor** upišite jednaku vrijednost koju ste unijeli u tekstni okvir za **Prijavu na URL-a** . 

     3,3. Kliknite **Dalje**.
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Director provedu SciQuest** kliknite **Preuzmi metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Što je Azure AD Connect][11]

5. Kontakt SciQuest podržava da biste omogućili ovu metodu provjere autentičnosti pomoću iznad preuzete metapodataka.

6. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** . 

    ![Što je Azure AD Connect][15]

10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    




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
  
 
### <a name="creating-a-sciquest-spend-director-test-user"></a>Stvaranje korisnika Director provedu SciQuest test

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u Director provedu SciQuest korisnika.

Trebate obratiti službi za podršku provedu Director SciQuest i pošaljite im detalje o testnom računu da biste je stvorili.

Osim toga, možete koristiti i samo-u – vrijeme dodjele resursa, jedan prijave značajka koja podržava Director provedu SciQuest.  
Ako je omogućeno samo-u – vrijeme dodjele resursa, korisnici automatski stvaraju Director provedu SciQuest tijekom na jednom pokušaja prijave ne postoje li. Značajka nema potrebe da biste ručno stvorili korisnici jedan prijave postoji zamjena u obliku.

Da biste dobili samo-u – vrijeme dodjeljivanje omogućena, morate obratite se službi za podršku provedu Director SciQuest.
  

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Director provedu SciQuest.

![Što je Azure AD Connect][200]

**Da biste dodijelili dan Britta Simona SciQuest provedu Director, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Što je Azure AD Connect][201]

2. Na popisu aplikacija odaberite **Director provedu SciQuest**.

    ![Što je Azure AD Connect][202]

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Što je Azure AD Connect][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

    ![Što je Azure AD Connect][204]

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Što je Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu SciQuest provedu Director u oknu programa Access, koje treba se automatski prijavljeni na u aplikaciji Director provedu SciQuest.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png

