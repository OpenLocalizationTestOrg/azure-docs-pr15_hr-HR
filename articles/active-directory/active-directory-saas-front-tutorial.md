<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s prednje | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i naprijed."
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


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Praktični vodič: Azure Active Directory Integracija s prvi plan

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji prednju s Azure Active Directory (Azure AD).

Integriranje prednju s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup u prvi plan
- Možete omogućiti svojim korisnicima da automatski se prijavljeni u prvi plan (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija prednju, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U prvi plan jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje prednju iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-front-from-the-gallery"></a>Dodavanje prednju iz galerije
Da biste konfigurirali Integracija naprijed u Azure AD, morate dodati prednju iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali prednju iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **prednju**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. U oknu rezultata odaberite **prednju**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s prednje utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje postoji zamjena u obliku korisniku ispred korisnik sustava u Azure AD. Drugim riječima, odnos vezu između programa Azure AD korisnik i povezane ispred mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** ispred.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s prednje, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na prednju testiranje korisnika](#creating-a-front-test-user)** – da bi se na postoji zamjena u obliku od Britta Dan Simona ispred koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom se odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji za prvi plan.

**Da biste konfigurirali Azure AD jedinstvenu prijavu prednju, učinite sljedeće:**

1. Klasični portalu na **prednju** Integracija stranici aplikacije kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako želite korisnicima da biste se prijavili prednju** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** ako želite Konfiguriranje aplikacije u **IDP pokrenut način**poduzeti sljedeće korake i kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    na. U tekstni okvir **identifikator** upišite URL-a pomoću sljedećeg uzorka:`https://<company name>.frontapp.com`

    b. U tekstni okvir **URL odgovor** , unesite URL pomoću sljedećeg uzorka:`https://<company name>.frontapp.com/sso/saml/callback`

    c. Kliknite **Dalje**

4. Ako želite konfigurirati aplikaciju u **SP pokrenut načinu rada** na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** pa kliknite na **"Pokaži napredne postavke (neobavezno)"** i unesite **URL na za prijavu** i kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    na. U tekstni okvir **Prijavite se na URL** unesite URL-a pomoću sljedećeg uzorka:`https://<company name>.frontapp.com`

    b. Kliknite **Dalje**

    > [AZURE.NOTE] Napominjemo da se razlikuju ne stvarne vrijednosti. Morate ažurirati te vrijednosti s stvarni prijavite se na URL-a, identifikator i URL ADRESE za odgovor. Da biste dobili te vrijednosti, možete upućivati **korak 12** detalje ili se obratite prednju putem [support@frontapp.com](emailTo:support@frontapp.com).

5. Na stranici **Konfiguracija jedinstvenu prijavu na prednju** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.

6. Prijava u prvi plan klijent kao administrator.

7. Idite na **postavke (ikona zupčanika ikona pri dnu lijeva bočna traka) > Preferences (Preference)**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Kliknite vezu za **Jedinstvenu prijavu** .

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. Odaberite **SAML** padajući popis **Jedinstvenu prijavu**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. U **Točka unosa** tekstni okvir postavite vrijednost od **Jednog prijave URL servisa** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. Kopirajte sadržaj datoteke preuzete certifikat i pa ih zalijepite u tekstni okvir za **potvrdu potpisnik** .

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Provjerite je li URL-ove odgovara vašoj konfiguraciji u 3.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

13. Kliknite gumb **Spremi** .

14. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

15. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-front-test-user"></a>Stvaranje korisnika prednji plan testiranja

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u radu Front.Please prednju timu za podršku da biste dodali korisnike u prvi plan računa.

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju u prvi plan.
    
![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona u prvi plan, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **sučelja**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu prednji plan na ploči programa Access, koje treba se automatski prijavljeni na u prvi plan aplikaciju.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
