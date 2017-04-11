<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s hostira Graphite | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i hostira Graphite."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Praktični vodič: Azure Active Directory Integracija s hostira Graphite

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji hostira Graphite s Azure Active Directory (Azure AD).

Integriranje hostira Graphite s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup hostira Graphite
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste hostira Graphite (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija hostira Graphite, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na hostira Graphite jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje hostira Graphite iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-hosted-graphite-from-the-gallery"></a>Dodavanje hostira Graphite iz galerije
Da biste konfigurirali Integracija hostira Graphite u Azure AD, morate dodati hostira Graphite iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali hostira Graphite iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Hostira Graphite**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_01.png)

7. U oknu rezultata odaberite **Hostira Graphite**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s hostira Graphite utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u hostira Graphite korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u hostira Graphite Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u hostira Graphite.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s hostira Graphite, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje Graphite za hostira testiranje korisnika](#creating-a-hosted-graphite-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u hostira Graphite koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji hostira Graphite.

**Da biste konfigurirali Azure AD jedinstvenu prijavu hostira Graphite, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Hostira Graphite** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili hostira Graphite** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_03.png)

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** ako želite Konfiguriranje aplikacije u **IDP pokrenut način**poduzeti sljedeće korake i kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_04.png)

    na. U tekstni okvir **identifikator** upišite URL-a pomoću sljedećeg uzorka:`https://www.hostedgraphite.com/metadata/<user id>`

    b. U tekstni okvir **URL odgovor** , unesite URL pomoću sljedećeg uzorka:`https://www.hostedgraphite.com/complete/saml/<user id>`

    c. Kliknite **Dalje**

4. Ako želite konfigurirati aplikaciju u **SP pokrenut načinu rada** na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** pa kliknite na **"Pokaži napredne postavke (neobavezno)"** i unesite **URL na za prijavu** i kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)

    na. U tekstni okvir **Prijavite se na URL** unesite URL-a pomoću sljedećeg uzorka:`https://www.hostedgraphite.com/login/saml/<user id>/`

    b. Kliknite **Dalje**

    > [AZURE.NOTE] Napominjemo da se razlikuju ne stvarne vrijednosti. Morate ažurirati te vrijednosti s stvarni prijavite se na URL-a, identifikator i URL ADRESE za odgovor. Da biste dobili te vrijednosti, možete ići na Access -> Postavljanje SAML s vaše strane aplikacije ili se obratite hostira Graphite.

5. Na stranici **Konfiguracija jedinstvenu prijavu na hostira Graphite** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.

6. Prijava za vaš klijent hostira Graphite kao administrator.

7. Idite na **SAML Postava stranice** na bočnoj traci (**Access -> Postavljanje SAML**).

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

8. Provjerite je li URL-ove odgovara vašoj konfiguraciji u 3.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

9. Kopirajte **URL izdavača** i **SAML SSO URL** iz Azure AD **entitet ili izdavača ID** i **SSO URL za prijavu** u glavnom računalu Graphite.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_003.png)

9. Odaberite "**samo za čitanje**" kao **zadani korisnička uloga**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

10. Kopirajte sadržaj datoteke preuzete certifikat, a zatim je zalijepite u tekstni okvir **Certifikat X.509** .

     ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

11. Kliknite gumb **Spremi** .

12. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

13. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-hosted-graphite-test-user"></a>Stvaranje korisnika hostira Graphite test

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u hostirane Graphite korisnika. Glavnom računalu Graphite podržava samo-u – vrijeme dodjele resursa, koji je po zadanom omogućen.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. Novi korisnik stvorit će se tijekom pokušaja pristup hostira Graphite Ako još ne postoji.

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnik sustava morati obratiti timu za podršku hostira Graphite putem <mailto:help@hostedgraphite.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju hostira Graphite.
    
![Dodijeli korisniku][200]

**Da biste dodijelili hostira Graphite dan Britta Simona, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Hostira Graphite**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_50.png)

1. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu hostira Graphite na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji hostira Graphite.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_205.png
