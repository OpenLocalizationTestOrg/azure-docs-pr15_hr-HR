<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s & frankly | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i & frankly."
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
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Praktični vodič: Azure Active Directory Integracija s & frankly

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji & frankly Azure Active Directory (Azure AD).

Integriranje & frankly s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup & frankly
- Možete omogućiti korisnike da automatski prijavljeni na & frankly (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija s programom, i frankly, vam potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- A & frankly jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje i frankly iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-frankly-from-the-gallery"></a>Dodavanje i frankly iz galerije
Da biste konfigurirali integraciju sa servisom sustava & frankly Azure AD, najprije morate dodati i frankly iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali & frankly iz galerije, učinite sljedeće:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **& frankly**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. U oknu rezultata odaberite **i frankly**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala kako konfigurirati i testiranje Azure AD jedne prijave s & frankly utemeljene na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku & frankly korisnik sustava u Azure AD. Drugim riječima, odnos vezu između programa Azure AD korisnik i povezane u & frankly mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u & frankly.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s & frankly, koje je potrebno dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na & frankly testnih korisnika](#creating-a-&frankly-test-user)** – da bi se na postoji zamjena u obliku od dan Britta Simona u & frankly tj povezana za predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u vašem & frankly aplikacije.

**Da biste konfigurirali Azure AD jedne prijave s & frankly, učinite sljedeće:**

1. Klasični portalu na **& frankly** Integracija stranici aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da se prijave na & frankly** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** ako želite Konfiguriranje aplikacije u **IDP pokrenut način**poduzeti sljedeće korake i kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    na. U tekstni okvir **identifikator** upišite URL-a pomoću sljedećeg uzorka:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. U tekstni okvir **URL odgovor** , unesite URL pomoću sljedećeg uzorka:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c. Kliknite **Dalje**

4. Ako želite konfigurirati aplikaciju u **SP pokrenut načinu rada** na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** , zatim na **"Pokaži napredne postavke (neobavezno)"** pa unesite **URL na za prijavu** i kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    na. U tekstni okvir **Prijavite se na URL** unesite URL-a pomoću sljedećeg uzorka:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Kliknite **Dalje**

    > [AZURE.NOTE] Napominjemo da se razlikuju ne stvarne vrijednosti. Morate ažurirati te vrijednosti s stvarni prijavite se na URL-a, identifikator i URL ADRESE za odgovor. Kontakt [help@andfrankly.com](emailTo:help@andfrankly.com) da biste dobili te vrijednosti.

5. Na na **konfigurirati jedinstvenu prijavu na & frankly** stranice, učinite sljedeće korake i kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.

6. Da biste dobili SSO konfiguriran za aplikaciju, obratite se vaš & frankly službi za podršku putem [help@andfrankly.com](emailTo:help@andfrankly.com). Prilaganje datoteke preuzete metapodataka i zajedničko korištenje i frankly timu da biste postavili SSO kod sebe.

7. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-frankly-test-user"></a>Stvaranje na & frankly testnih korisnika

U ovom ćete odjeljku stvoriti korisnika pod nazivom dan Britta Simona u & frankly. Provjerite radite & frankly službi za podršku putem [help@andfrankly.com](emailTo:help@andfrankly.com) da biste dodali korisnike u & frankly platforme.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta da biste koristili Azure jedinstvenu prijavu dozvolite pristup & frankly.
    
![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona & frankly, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **i frankly**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete & frankly pločica na ploči programa Access, trebali biste dobiti automatski prijavljeni u da biste na & frankly aplikacije.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
