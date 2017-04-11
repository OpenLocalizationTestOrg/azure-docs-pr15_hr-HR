<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s e-pošte Skydesk | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Skydesk e-pošte."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Praktični vodič: Azure Active Directory Integracija s Skydesk e-pošte

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Skydesk e-pošte s Azure Active Directory (Azure AD).

Integriranje Skydesk e-pošte s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Skydesk e-pošte
- Možete omogućiti svojim korisnicima da automatski se prijavili u Skydesk e-poštom (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – klasični portal za Azure Active Directory

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Skydesk e-pošte, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U e-pošte Skydesk jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Skydesk e-pošte iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-skydesk-email-from-the-gallery"></a>Dodavanje Skydesk e-pošte iz galerije
Da biste konfigurirali integraciju Skydesk e-pošte u Azure AD, morate dodati Skydesk e-pošte iz galerije popis upravljani SaaS aplikacija.

**Da biste dodali Skydesk e-pošte iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Skydesk e-pošte**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. U oknu s rezultatima odaberite **Skydesk e-pošte**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s e-poštom Skydesk utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku Skydesk e-pošte korisniku u Azure AD. Drugim riječima, veza odnos između programa Azure AD korisnik i povezane Skydesk e-pošte potrebno je uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničkog imena** u e-pošte Skydesk.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Skydesk e-pošte, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje e-pošte Skydesk testiranje korisnika](#creating-a-Skydesk-Email-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Skydesk e-pošte koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji za e-pošte Skydesk.



**Da biste konfigurirali Azure AD jedinstvenu prijavu Skydesk e-pošte, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Skydesk e-pošte** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Skydesk e-pošte** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    na. U tekstni okvir prijavite se na URL unesite URL koji se koristi korisnika za prijavu u aplikaciju Skydesk e-pošte pomoću sljedećeg uzorka: **"https://mail.skydesk.jp/portal/\<naziv tvrtke\>"**.

    b. Kliknite **Dalje**.


4. Na stranici **Konfiguracija jedinstvenu prijavu na Skydesk e-pošte** , poduzmite sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste omogućili SSO **Skydesk**e-pošte, poduzmite sljedeće korake:
 
    na. Prijava na račun e-pošte Skydesk kao administrator.

    b. Na izborniku na vrhu kliknite postavljanje, a zatim odaberite Org. 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. U lijevom oknu kliknite domene.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Kliknite Dodaj domenu.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Unesite naziv domene, a zatim potvrdite domenu.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. U lijevom oknu kliknite na **SAML provjeru autentičnosti**

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Na stranici dijaloški okvir **Provjera autentičnosti SAML** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] Da biste koristili provjeru autentičnosti SAML temelji, trebali biste ili ste **potvrdili domene** ili **portal URL** postavljanje. Možete postaviti na portalu URL s jedinstveni naziv.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    na. Klasični portalu Azure AD kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **URL za prijavu** .

    b. Klasični portalu Azure AD kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir URL **odjavite** .

    c. **Promjena lozinke URL** nije obavezno pa ostavite prazno.

    d. Kliknite na **Početak ključ iz datoteke** da biste odabrali preuzete certifikat Skydesk e-pošte, a zatim kliknite **Otvori** da biste prenijeli certifikata.

    e. Kao **algoritam**, odaberite **RSA**.

    f. Kliknite **u redu** da biste spremili promjene.


7. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
  
    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-skydesk-email-test-user"></a>Stvaranje korisnika test Skydesk e-pošte

U ovom ćete odjeljku stvoriti korisnik naziva dan Britta Simona Skydesk e-pošte.

na. Kliknite na **Korisnički pristup** s lijeve ploče Skydesk e-pošte, a zatim unesite svoje korisničko ime. 

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Ako vam je potrebna skupno Dodavanje korisnika, morate je obratite se timu za podršku Skydesk e-pošte.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Skydesk e-pošte.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Skydesk e-pošte, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
 
    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Skydesk e-pošte**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Skydesk e-pošte u oknu programa Access, koje treba se automatski prijavljeni na u aplikaciji Skydesk e-pošte.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
