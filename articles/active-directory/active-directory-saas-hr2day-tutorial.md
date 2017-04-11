<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s HR2day po Merces | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i HR2day po Merces."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Praktični vodič: Azure Active Directory Integracija s HR2day po Merces

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji HR2day po Merces s Azure Active Directory (Azure AD).  
Integriranje HR2day po Merces s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup HR2day po Merces
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste HR2day po Merces (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija HR2day po Merces, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- HR2day po Merces s jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje HR2day po Merces iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>Dodavanje HR2day po Merces iz galerije
Da biste konfigurirali Integracija HR2day po Merces u Azure AD, morate dodati HR2day po Merces iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali HR2day po Merces iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
 
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **HR2day po Merces**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. U oknu s rezultatima odaberite **HR2day po Merces**, a zatim **Dovršeno** da biste dodali aplikaciju.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s HR2day po Merces utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u HR2day po Merces korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u HR2day po Merces Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u HR2day po Merces.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s HR2day po Merces, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje HR2day po Merces testiranje korisnika](#creating-a-hr2day-by-merces-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u HR2day po Merces koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti HR2day po Merces sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.

Vaše HR2day aplikacija Merces očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju tokena atributa SAML. Sljedeća slika prikazuje primjer za to. 

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Prije nego što možete konfigurirati pridruživanju SAML, morate se obratiti službi za podršku HR2day putem [servicedesk@merces.nl](mailto:servicedesk@merces.nl) i zatražite vrijednost atributa Jedinstveni identifikator za vaš klijent. Potreban vam je tu vrijednost tako da slijedite korake u sljedećem odjeljku.


**Da biste konfigurirali Azure AD jedinstvenu prijavu HR2day po Merces, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici integraciju aplikacije **HR2day po Merces** na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** . 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Da biste dodali mapiranja obavezan atribut, poduzeti sljedeće korake, učinite sljedeće: 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    na. Kliknite **Dodavanje korisnika atribut**.

    b. U tekstni okvir **Naziv atributa** upišite **"ATTR_LOGINCLAIM"**.

    c. Na popisu **Vrijednost atributa** odaberite **Join()**. 

    d. Na popisu **String1** odaberite **User.mail**. 

    e. U tekstni okvir **String2** upišite **Jedinstveni identifikator** dodijelio tim za HR2day. 

    f. U tekstni okvir **Razdjelnik** upišite **@**.

    g. Kliknite **dovrši**.

  
3. Kliknite **Primijeni promjene**.


1. Na izborniku na vrhu kliknite **Brzi početak rada** da biste otvorili dijaloški okvir za **Brzo pokretanje** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili HR2day po Merces** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    na. U tekstni okvir prijavite se na URL unesite URL koji se koristi korisnicima prijave za vaše HR2day aplikacija Merces pomoću sljedećeg uzorka: **"https://\<ime klijenta\>.force.com/\<naziv instance\>"**.

    b. Kliknite **Dalje**.

4. Na stranici **Konfiguracija jedinstvenu prijavu na HR2day po Merces** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se vaše HR2day od tima za podršku Merces putem [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) i priložili datoteku preuzete certifikata e-pošte. I navedite SAML SSO URL, URL izgleda za prijavu i izdavatelj URL tako da je konfigurirati za integraciju SSO.


> [AZURE.NOTE] Provjerite spomenuti Merces timu da ta Integracija moraju entitet ID postaviti s ovom uzorak **https://hr2day.force.com/INSTANCENAME**



6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>Stvaranje HR2day Merces testnih korisnika

Cilj ovaj odjeljak je da biste stvorili korisnik pozove dan Britta Simona u HR2day Merces. Provjerite radite s HR2day od tima za podršku Merces da biste dodali korisnike u HR2day računa. 


> [AZURE.NOTE]Ako je potrebno ručno stvoriti korisnik sustava potrebno je kontakt u HR2day od tima za podršku Merces.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju HR2day po Merces.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona HR2day po Merces, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **HR2day po Merces**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete na HR2day po Merces pločica na ploči za Access, koje treba se automatski prijavljeni na na vašem HR2day Merces aplikacija.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png
