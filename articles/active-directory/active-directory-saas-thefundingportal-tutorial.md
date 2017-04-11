<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s u Funding Portal | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i u Funding Portal."
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
    ms.date="09/02/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Praktični vodič: Azure Active Directory Integracija s u Funding Portal

U ovom ćete praktičnom vodiču, Saznajte kako integrirati u Funding Portal Azure Active Directory (Azure AD).

Integriranje u Funding Portal s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup u Funding Portal
- Možete omogućiti svojim korisnicima da automatski se prijavili u alatu Funding portal (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD integracija u Funding Portal, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U **Alatu Funding Portal** jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje. Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Funding portalu iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-the-funding-portal-from-the-gallery"></a>Dodavanje Funding portalu iz galerije
Da biste konfigurirali integraciju u Funding portala u Azure AD, morate dodati na portalu Funding iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali u Funding Portal iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **U Funding Portal**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_01.png)

7. U oknu s rezultatima **U Funding portala**odaberite, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Funding portalu utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku na portalu Funding određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika na portalu Funding Azure AD korisnik mora uspostaviti.
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Funding portalu.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu pomoću portala za u Funding, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje portala za u Funding testiranje korisnika](#creating-a-the-funding-portal-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Funding portalu koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciju u Funding Portal.

Aplikacija Funding Portal očekuje assertions SAML sadrži atribut pod nazivom "externalId1". Vrijednost "externalId1" mora biti prepoznati studentID. Konfigurirajte "externalId1" zahtjeva za ovu aplikaciju. Na kartici **"Atrributes"** aplikacije možete upravljati vrijednosti od ovih atributa. Sljedeća slika prikazuje primjer za to.

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_03.png) 

**Da biste konfigurirali Azure AD jedinstvenu prijavu na Portal Funding, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici integraciju aplikacije **U Funding Portal** na izborniku na vrhu kliknite **atribute**.
     
    ![Konfiguriranje jedinstvenu prijavu][5]

2. U dijaloškom okviru tokena atribute SAML dodajte atribut "externalId1".

    na. Kliknite **Dodavanje atributa korisnika** da biste otvorili dijaloški okvir **Dodavanje atributa korisnika** . 
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_05.png)

    b. U tekstni okvir **Naziv atributa** upišite naziv atributa "externalId1".

    c. Na popisu **Vrijednost atributa** odaberite atribut koji želite koristiti za implementaciju. Ako, na primjer, ako je vrijednost StudentID ste spremili u na ExtensionAttribute1, zatim odaberite user.extensionattribute1.

    d. Kliknite **dovrši**. Zatim kliknite **Primijeni promjene**.

1. Na izborniku na vrhu kliknite **Brzi početak rada**.

    ![Konfiguriranje jedinstvenu prijavu][6]

2. Na portalu klasični, na stranici za integraciju aplikacije **U Funding Portal** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][7] 

3. Na stranici **kako biste željeli korisnika da se prijavite se Portal u alatu Funding** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_06.png)

4. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_07.png)


    na. U tekstni okvir prijavite se na URL unesite URL-a pomoću sljedećeg uzorka: `https://<subdomain>.regenteducation.net/`.

    b. Kliknite **Dalje**.

5. Na stranici **Konfiguracija jedinstvenu prijavu na Portal u alatu Funding** kliknite **Preuzimanje metapodatke**, a zatim spremite datoteku na računalu.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_08.png)

6. Da biste dobili SSO konfiguriran za aplikaciju, obratite se podršci u Funding Portal. Da biste konfigurirali SSO će pomoći s početnim kanala. Ponovno preuzeti Imajte na umu da ćete morati poslati e-pošte, a zatim priložite datoteku metapodataka za info@regenteducation.com.

7. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]

### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-the-funding-portal-test-user"></a>Stvaranje testnih korisnika u Funding Portal

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Funding portalu. Ako ne znate kako dodati dan Britta Simona na portalu Funding, provjerite raditi u Funding Portal za podršku da biste dodali testnih korisnika i omogućili SSO. Obratite joj pri info@regenteducation.com.

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta da biste koristili Azure jedinstvenu prijavu dozvolite pristup u Funding portal.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona u Funding Portal, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Alatu Funding Portal**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_09.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu svi korisnici odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu u Funding Portal na ploči programa Access, koje treba se automatski prijavljeni na u alatu Funding Portal aplikaciju.

## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_205.png
