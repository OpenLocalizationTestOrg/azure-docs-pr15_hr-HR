<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Keylight | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Keylight."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Praktični vodič: Azure Active Directory Integracija s Keylight

U ovom ćete praktičnom vodiču saznati kako Keylight integrirati Azure Active Directory (Azure AD).

Integriranje Keylight s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Keylight
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Keylight (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Keylight, potrebne su vam sljedeće stavke:

- Azure pretplate
- Na Keylight jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Keylight iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-keylight-from-the-gallery"></a>Dodavanje Keylight iz galerije
Da biste konfigurirali Integracija Keylight u Azure AD, morate dodati Keylight iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Keylight iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Keylight**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. U oknu s rezultatima odaberite **Keylight**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Keylight utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Keylight, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na Keylight testiranje korisnika](#creating-a-keylight-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Keylight koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Keylight.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Keylight, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Keylight** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 


2. Na stranici **kako biste željeli korisnika da biste se prijavili Keylight** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    na. U tekstni okvir prijavite se na URL unesite URL koji se koristi korisnika za prijavu u aplikaciju Keylight pomoću sljedećeg uzorka: **"https://\<naziv tvrtke\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Na stranici **Konfiguracija jedinstvenu prijavu na Keylight** poduzeti sljedeće korake:
 
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste omogućili SSO u Keylight, učinite sljedeće:
 
    na. Prijava na račun servisa Keylight kao administrator.

    b. Na izborniku na vrhu kliknite **osobe**pa odaberite **Postavljanje Keylight**.
       
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. U treeview s lijeve strane, pritisnite **SAML**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. U dijaloškom okviru **Postavke SAML** kliknite **Uređivanje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Na stranici dijaloški okvir **Uređivanje postavki SAML** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/405.png) 

    na. Da biste postavili **provjeru autentičnosti SAML** **Aktivno**.


    b. Klasični portalu Azure AD kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **URL za prijavu davatelja identiteta** .

    c. Klasični portalu Azure AD kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **URL za odjavite davatelja identiteta** .

    d. Kliknite **Odabir datoteke** da biste odabrali preuzete Keylight certifikat, a zatim **Otvori** da biste prenijeli certifikata.


    e. Odredite **lokaciju SAML korisnički Id** element **NameIdentifier izjave predmet**.
   
    f. Navedite na **Keylight usluga pomoću sljedećeg uzorka: **https://&lt;naziv tvrtke&gt;. keylightgrc.com**.

    g. Postavite **Automatsko implementaciju korisnika** na **aktivni**.

    h. Postavite **automatsko dodjeljivanje vrsta računa** **Cijelog**korisniku.

    li. Kao **automatski dodjele sigurnosne uloge**, odaberite **Standardnu korisnika SAML**.
   
    j. Kao **config automatski dodjele sigurnost**, odaberite **Standardnu Konfiguracija korisnika**.
   
    k. U tekstni okvir atribut e-pošte upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. U tekstni okvir **atribut ime** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. U tekstni okvir **posljednje atribut naziv** upišite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Kliknite **Spremi**.
   
  
   
  
6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje korisnika test Azure klasični portalu naziva dan Britta Simona.

Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]



**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-keylight-test-user"></a>Stvaranje Keylight testnih korisnika

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Keylight. Keylight podržava samo-u – vrijeme dodjele resursa, koje po zadanom je omogućena.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. Novi korisnik stvara se prilikom pristupanja Keylight ako korisnik još ne postoji. 

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnika, morate je obratite se timu za podršku Keylight.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Keylight.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Keylight, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Keylight**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Keylight na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Keylight.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
