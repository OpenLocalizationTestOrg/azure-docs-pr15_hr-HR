<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s SAP tvrtke ByDesign | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i ByDesign tvrtke SAP-a."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Praktični vodič: Azure Active Directory Integracija s ByDesign tvrtke SAP-a

U ovom ćete praktičnom vodiču, Saznajte kako integrirati SAP tvrtke ByDesign Azure Active Directory (Azure AD).

Integriranje SAP tvrtke ByDesign s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup SAP-a tvrtke ByDesign
- Možete omogućiti svojim korisnicima da automatski se prijavili u za SAP tvrtke ByDesign (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija s programom SAP tvrtke ByDesign, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U SAP tvrtke ByDesign jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje SAP tvrtke ByDesign iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Dodavanje SAP tvrtke ByDesign iz galerije
Da biste konfigurirali Integracija SAP tvrtke ByDesign u Azure AD, morate dodati SAP tvrtke ByDesign iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali SAP tvrtke ByDesign iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.


3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **ByDesign tvrtke SAP-a**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. U oknu s rezultatima odaberite **ByDesign tvrtke SAP**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s SAP tvrtke ByDesign utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u SAP tvrtke ByDesign određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u SAP tvrtke ByDesign Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u ByDesign tvrtke SAP-a.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s ByDesign tvrtke SAP-a, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje ByDesign za tvrtke SAP testiranje korisnika](#creating-an-sap-business-bydesign-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u ByDesign tvrtke SAP koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom se odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji ByDesign tvrtke SAP-a.

Aplikacija za SAP tvrtke ByDesign očekuje SAML assertions u određenom obliku. Konfigurirajte sljedeće zahtjevima za ovu aplikaciju. Na kartici **"Atrribute"** aplikacije možete upravljati vrijednosti od ovih atributa. Sljedeća slika prikazuje primjer za to. 


**Da biste konfigurirali Azure AD jedinstvenu prijavu ByDesign tvrtke SAP-a, učinite sljedeće:**


1. Azure klasični portalu na stranici integraciju aplikacije **SAP tvrtke ByDesign** na izborniku na vrhu kliknite **atribute**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Na popisu tokena atribute SAML atribute odaberite atribut nameidentifier, a zatim **Uređivanje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. U dijaloškom okviru Uređivanje korisnika atribut poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    na. Na popisu vrijednost atributa odaberite **ExtractMailPrefix()** fuction

    b. Na popisu pošta odaberite atribut korisnika koji želite koristiti za implementaciju. 
    Na primjer, ako želite koristiti u IDZaposlenika kao identifikator jedinstveni korisnički, a koje ste spremili vrijednost atributa u na ExtensionAttribute2, zatim odaberite **user.extensionattribute2**. 

    c. Kliknite **dovrši**. 
    

4. Na portalu klasični, na stranici za integraciju aplikacije **SAP tvrtke ByDesign** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

5. Na stranici **kako biste željeli korisnika da biste se prijavili SAP tvrtke ByDesign** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju SAP tvrtke ByDesign pomoću sljedećeg uzorka:`https://<servername>.sapbydesign.com`
    
    b. Kliknite **Dalje**
 
7. Na stranici **Konfiguracija jedinstvenu prijavu na SAP tvrtke ByDesign** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


8. Da biste dobili SSO konfiguriran za aplikaciju, učinite sljedeće:

    na. Prijavite se portal SAP tvrtke ByDesign s administratorskim ovlastima.

    b. Dođite do **aplikacija i upravljanje uobičajeni zadatak** , a zatim kliknite karticu **Davatelja identiteta** .

    c. Kliknite **Novi davatelja identiteta** i odaberite metapodataka XML datoteku koju ste preuzeli s portala za Azure klasični. Uvozom metapodatke sustav automatski prenosi potreban potpis certifikata i certifikata za šifriranje.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Da biste obuhvatili **URL servisa za korisničke pridruživanju** u zahtjev za SAML, odaberite **Uključi pridruživanju potrošača URL za servis**.

    e. Kliknite **Aktiviraj jedinstvenu prijavu**.

    f. Spremite promjene.

    g. Kliknite karticu **Moja sustava** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. Kopirajte **SSO URL** i zalijepite u tekstni okvir **Azure AD za prijavu URL-a** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    li. Odredite hoće li zaposlenika ručno na raspolaganju prijave s korisnički ID i lozinku ili SSO tako da odaberete **Odabira davatelja identiteta za ručno**.

    j. U odjeljku **SSO URL** Navedite URL koji želite koristiti po zaposleniku za prijavu u sustav. 
    U na URL poslali zaposlenika padajućeg popisa, možete odabrati između sljedećih mogućnosti:
    
    **URL koji nisu jedinstvene Prijave**
 
    Sustav šalje samo URL normalni sustava zaposlenika. Zaposlenika ne možete prijaviti pomoću jedinstvene Prijave, i morate koristite lozinku ili potvrda umjesto toga.

    **SSO URL-A** 

    Sustav šalje samo URL SSO zaposlenika. Zaposlenika možete se prijaviti pomoću jedinstvene Prijave. Zahtjev za provjeru autentičnosti se preusmjerava putem na IdP.

    **Automatski odabir**
 
    Ako SSO nije aktivna, sustav šalje URL normalni sustava zaposlenika. Ako je aktivan SSO sustav provjerava ima li zaposlenika lozinku. Ako je dostupan lozinke, SSO URL i SSO za osobe koje nisu URL šalju zaposlenika. Međutim, ako je zaposlenika bez lozinke, samo URL SSO se šalje zaposlenika.

    k. Spremite promjene.

9. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Stvaranje korisnik sustava SAP tvrtke ByDesign test

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u ByDesign tvrtke SAP-a. Raditi SAP tvrtke ByDesign timu za podršku da biste dodali korisnike u platformu ByDesign tvrtke SAP-a. 

> [AZURE.NOTE] Provjerite je li vrijednost NameID podudarati s polje username u platformu ByDesign tvrtke SAP-a.

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku omogućiti Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da svoj dodjeljivanju ByDesign tvrtke SAP-a.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona ByDesign tvrtke SAP-a, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **ByDesign tvrtke SAP-a**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu ByDesign tvrtke SAP-a na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji ByDesign tvrtke SAP-a.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
