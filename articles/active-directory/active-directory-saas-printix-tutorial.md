<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Printix | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Printix."
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


# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Praktični vodič: Azure Active Directory Integracija s Printix

U ovom ćete praktičnom vodiču saznati kako Printix integrirati Azure Active Directory (Azure AD).

Integriranje Printix s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Printix
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Printix (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Printix, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Printix jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Printix iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-printix-from-the-gallery"></a>Dodavanje Printix iz galerije
Da biste konfigurirali Integracija Printix u Azure AD, morate dodati Printix iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Printix iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Printix**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/tutorial_printix_01.png)
7. U oknu s rezultatima odaberite **Printix**, a zatim **Dovršeno** da biste dodali aplikaciju.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Printix utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Printix određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Printix Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Printix.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Printix, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na Printix testiranje korisnika](#creating-a-printix-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Printix koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Printix.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Printix, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Printix** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Printix** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_04.png) 

    na. U tekstni okvir **URL odgovor** upišite `https://auth.printix.net/saml/SSO`.
    
    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Printix** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_05.png)

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Prijava za vaš klijent Printix kao administrator.


6. Na izborniku na vrhu, kliknite ikonu u gornjem desnom kutu i odaberite "**provjere autentičnosti**".

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

7. Na kartici **Postavljanje** odaberite **Omogućivanje Azure/Office 365 provjera autentičnosti**

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

8. Na kartici **Azure** unesite URL metapodataka vanjski pristup za tekstni okvir**vanjski pristup metapodataka dokumenta"**. 
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)

    na. Priložene datoteke xml metapodatke koji ste preuzeli u koraku 4 Printix službi za podršku putem "**support@printix.net**". Zatim će prijenos datoteke za xml i unijeti njezin URL metapodataka za vanjski pristup s vama.


9. Kliknite gumb "**testiranje**", a ako je provjera uspjela, kliknite gumb "**u redu**".

    na. Prikazat će se stranica servisa Azure active directory nakon klika na gumb za **testiranje** . Ovdje "test je uspjelo" znači nakon unosa vjerodajnice za vaš račun za Azure test je će popo kopije poruka "Postavke testirati u redu". Kliknite gumb **u redu** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)


10. Kliknite gumb **Spremi** na stranici "**provjere autentičnosti**".


11. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

12. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-printix-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-printix-test-user"></a>Stvaranje korisnik za testiranje sustava Printix

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u Printix korisnika. Printix podržava samo-u – vrijeme dodjele resursa, koji je po zadanom omogućen.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. Novi korisnik stvorit će se tijekom pokušaja pristup Printix Ako još ne postoji. [Konfiguriranje Azure AD jedinstvene prijave](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnik sustava ćete morati obratite se timu za podršku Printix.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Printix.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Printix, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Printix**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-printix-tutorial/tutorial_printix_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Printix na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Printix.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-printix-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-printix-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-printix-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-printix-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-printix-tutorial/tutorial_general_205.png
