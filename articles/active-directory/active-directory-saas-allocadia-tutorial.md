<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Allocadia | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Allocadia."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>Praktični vodič: Azure Active Directory Integracija s Allocadia

U ovom ćete praktičnom vodiču saznati kako Allocadia integrirati Azure Active Directory (Azure AD).

Integriranje Allocadia s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Allocadia
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Allocadia (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Allocadia, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Allocadia jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje. Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Allocadia iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-allocadia-from-the-gallery"></a>Dodavanje Allocadia iz galerije
Da biste konfigurirali Integracija Allocadia u Azure AD, morate dodati Allocadia iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Allocadia iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Allocadia**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. U oknu s rezultatima odaberite **Allocadia**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Allocadia utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Allocadia određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Allocadia Azure AD korisnik mora uspostaviti.
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Allocadia.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Allocadia, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje programa Allocadia testiranje korisnika](#creating-an-allocadia-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Allocadia koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Allocadia.


Aplikacija Allocadia očekuje SAML assertions u određenom obliku. Konfigurirajte sljedeće zahtjevima za ovu aplikaciju. Na kartici **"Atrribute"** aplikacije možete upravljati vrijednosti od ovih atributa. Sljedeća slika prikazuje primjer za to. 

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png) 

**Da biste konfigurirali Azure AD jedinstvenu prijavu Hightail, poduzmite sljedeće korake:**


1. Azure klasični portalu na stranici integraciju aplikacije **Allocadia** na izborniku na vrhu kliknite **atribute**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png) 


2. U dijaloškom okviru **SAML tokena atribute** za svaki redak prikazana u tablici u nastavku, učinite sljedeće:

  	| Naziv atributa | Vrijednost atributa |
  	| --- | --- |    
  	| ime | User.givenname |
  	| Prezime  | User.surname |
  	| e-pošte | User.Mail |
    

    na. Kliknite **Dodavanje atributa korisnika** da biste otvorili dijaloški okvir **Dodavanje Attribure korisnika** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png) 


    b. U tekstni okvir **Attrubute naziv** upišite naziv atributa koji se prikazuju za tom retku.

    c. Na popisu **Vrijednost atributa** selsect vrijednost atributa koji se prikazuju za tom retku.

    d. Kliknite **dovrši**.  
    

3. Na izborniku na vrhu kliknite **Brzi početak rada**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)  


4. Na stranici **kako biste željeli korisnika da biste se prijavili Allocadia** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png) 

5. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png) 

    na. U okviru IDENTIFIKATOROM upišite URL sljedećeg uzorka: da biste postigli test okruženje pomoću URL-a kao **"https://na2standby.allocadia.com"** , a radnom okruženju koristite **"https://na2.allocadia.com"**

    b. U odgovor URL unesite URL sljedećeg uzorka: za testiranje okruženje za korištenje uzorka URL-a kao **"https://na2standby.allocadia.com/allocadia/saml/SSO"** , a radnom okruženju koristite **"https://na2.allocadia.com/allocadia/saml/SSO"**


6. Na stranici **Konfiguracija jedinstvenu prijavu na Allocadia** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png) 

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


7.  Da biste dobili SSO konfiguriran za aplikaciju, kontaktu [Allocadia](mailTo:support@allocadia.com) podršku i oni pomoći će konfigurirati SSO. Provjerite Imajte na umu da ćete morati poslati e-pošte, a zatim priložite preuzeti datoteku metapodataka za konfiguriranje SSO na strani Allocadia.
 
    > [AZURE.NOTE] Ponovno provjerite je li Allocadia tima postavite vrijednost identifikatora u okruženju test kao **"https://na2standby.allocadia.com"** i radnom okruženju, trebala bi biti: **"https://na2.allocadia.com"**


8. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

9. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test

U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.
Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-allocadia-test-user"></a>Stvaranje korisnik za testiranje sustava Allocadia

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Allocadia. Podrška aplikacija Allocadia samo u vrijeme korisnika dodjele resursa. Ako ste konfigurirali u zahtjevima prema uputama iznad odjeljka **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** pa ga će dodjelu resursa korisnicima u aplikaciji. 


> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnika ili skupna korisnika, morate se obratiti timu za podršku Allocadia.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Allocadia.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Allocadia, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Allocadia**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.
Kada kliknete pločicu Allocadia na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Allocadia.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png
