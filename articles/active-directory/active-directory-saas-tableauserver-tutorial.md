<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s poslužiteljem Tableau | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Tableau poslužitelja."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Praktični vodič: Azure Active Directory Integracija s poslužiteljem Tableau

Cilj ovog praktičnog vodiča će vam pokazati kako integrirati Tableau Server Azure Active Directory (Azure AD).

Integriranje Tableau poslužitelj s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Tableau poslužitelja
- Možete omogućiti svojim korisnicima da automatski se prijavili na poslužitelj Tableau (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija s poslužiteljem Tableau, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na poslužitelju Tableau jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje poslužitelja Tableau iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-tableau-server-from-the-gallery"></a>Dodavanje poslužitelja Tableau iz galerije
Da biste konfigurirali integraciju Tableau poslužitelja u Azure AD, morate dodati Tableau poslužitelja iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Tableau poslužitelja iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 
 
    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Tableau poslužitelja**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. U oknu s rezultatima odaberite **Tableau poslužitelj**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s poslužiteljem Tableau utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku Tableau poslužitelju korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Tableau Server Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Tableau poslužitelja.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s poslužiteljem Tableau, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Tableau poslužitelja testiranje korisnika](#creating-a-tableauserver-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Tableau poslužitelja koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Tableau poslužitelja.

Tableau poslužiteljska aplikacija očekuje SAML assertions u određenom obliku. Sljedeća slika prikazuje primjer za to. 

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Da biste konfigurirali Azure AD jedinstvenu prijavu Tableau poslužitelj, učinite sljedeće:**


1. Azure klasični portalu na stranici integraciju aplikacije **Tableau poslužitelja** na izborniku na vrhu kliknite **atribute**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. U dijaloškom okviru **SAML tokena atribute** poduzeti sljedeće korake:

    

    na. Kliknite **Dodavanje atributa korisnika** da biste otvorili dijaloški okvir **Dodavanje Attribure korisnika** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. U tekstni okvir **Naziv Attrubute** unesite **korisničko ime**.

    c. Na popisu **Vrijednost atributa** selsect **user.displayname**.

    d. Kliknite **dovrši**.  
    



1. Na izborniku na vrhu kliknite **Brzi početak rada**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 



2. Na stranici **kako biste željeli korisnika da se prijaviti na poslužitelj Tableau** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    na. U tekstni okvir **URL za prijavu** upišite URL poslužitelja Tableau. 

    b. U identifikator okvir Kopiraj u 

    c. Kliknite **Dalje**


4. Na stranici **Konfiguracija jedinstvenu prijavu na poslužitelju Tableau** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


6. Da biste dobili SSO konfiguriran za aplikaciju, morate prijave za vaš klijent Tableau poslužitelju kao administrator.

    na. U konfiguraciji poslužitelja Tableau karticu **SAML** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Potvrdite okvir **Koristi SAML za jedinstvenu prijavu**.

    c. Pronađite datoteku metapodataka za vanjski pristup preuzeli s portala za Azure klasični, a zatim je prijenos u **datoteku SAML Idp metapodataka**.

    d. Poslužitelj tableau vratiti URL-URL koji poslužitelj Tableau korisnici će pristupa, kao što su http://tableau_server. Ne preporučuje se korištenje http://localhost. Pomoću URL-a s završne kosa crta (na primjer, http://tableau_server/) nije podržano. Kopiranje **Tableau poslužitelj vratiti URL-a** i zalijepite u tekstni okvir Azure AD **Prijavite se na URL** kao što je prikazano u koraku 3

    e. ID entitet SAML – ID entitet služi kao jedinstvena identifikacija instalaciju Tableau poslužitelj tako da na IdP. Ovdje možete unijeti URL poslužitelja Tableau ponovno, ako želite, ali ga ne mora biti Tableau poslužitelja URL-a. Kopirajte **SAML entitet ID** i zalijepite u tekstni okvir Azure AD **IDENTIFIKATOROM** kao što je prikazano u koraku 3.

    f. Kliknite **Izvezi u datoteku metapodataka** i otvorite je u aplikacije za uređivanje teksta. Pronađite pridruživanju URL servisa za korisničke s Http Post i indeksirati 0 i kopirajte URL. Sada zalijepite u tekstni okvir Azure AD **URL ADRESE za odgovor** kao što je prikazano u 3. 

    g. Kliknite **u redu** na stranici Configiuration Tableau poslužitelja.

    > [AZURE.NOTE] Ako vam je potrebna pomoć prilikom konfiguriranja SAML na poslužitelju Tableau pa pogledajte članak [Konfiguriranje SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**. 
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    na. Kao **Korisnik vrste**, odaberite **novi korisnik u tvrtki ili ustanovi**.

    b. U tekstni okvir **Korisničko ime** upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-tableau-server-test-user"></a>Stvaranje korisnika test Tableau poslužitelja

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona Tableau poslužitelju. Morate Dodjela resursa za sve korisnike na poslužitelju Tableau. Imajte na umu te korisničko ime korisnika moraju se podudarati vrijednost koja ste konfigurirali u Azure AD atribut prilagođene **korisničko ime**. Uz ispravan mapiranje integraciju sa servisom surađivati [Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnik sustava ćete morati zatražite od administratora poslužitelja Tableau u tvrtki ili ustanovi.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta da biste koristili Azure jedinstvenu prijavu dozvolite pristup Tableau poslužitelj.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Tableau poslužitelj, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
 
    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Poslužitelj za Tableau**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Tableau poslužitelju u oknu programa Access, koje treba se automatski prijavljeni na u aplikaciji Tableau poslužitelja.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
