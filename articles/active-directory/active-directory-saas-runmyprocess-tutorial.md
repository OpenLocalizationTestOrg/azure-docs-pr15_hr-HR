<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s RunMyProcess | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i RunMyProcess."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Praktični vodič: Azure Active Directory Integracija s RunMyProcess

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji RunMyProcess s Azure Active Directory (Azure AD).

Integriranje RunMyProcess s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup RunMyProcess
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste RunMyProcess (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija RunMyProcess, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na RunMyProcess jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje RunMyProcess iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-runmyprocess-from-the-gallery"></a>Dodavanje RunMyProcess iz galerije
Da biste konfigurirali Integracija RunMyProcess u Azure AD, morate dodati RunMyProcess iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali RunMyProcess iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **RunMyProcess**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. U oknu s rezultatima odaberite **RunMyProcess**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s RunMyProcess utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u RunMyProcess određuje korisniku u Azure AD je. Drugim riječima, veza odnosa između tablica i povezane korisnika u RunMyProcess Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u RunMyProcess.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s RunMyProcess, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na RunMyProcess testiranje korisnika](#creating-a-runmyprocess-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u RunMyProcess koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji RunMyProcess.

**Da biste konfigurirali Azure AD jedinstvenu prijavu RunMyProcess, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **RunMyProcess** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili RunMyProcess** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL pomoću sljedećeg uzorka: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Kliknite **Dalje**

    > [AZURE.NOTE] Uzmite u obzir ćete morati ažurirati vrijednost stvarni prijavite se na URL-om. Da biste tu vrijednost, obratite se timu za podršku RunMyProcess putem <mailto:support@runmyprocess.com>.
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na RunMyProcess** kliknite **Preuzmite certifikat** , a zatim spremite datoteku na računalu:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. U prozoru preglednika drugoj web prijave za vaš klijent RunMyProcess kao administrator.

6. Na ploči lijevom navigacijskom oknu kliknite **račun** , a zatim odaberite **Konfiguracija**.

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacije](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Idite na odjeljak **način provjere autentičnosti** i izvođenje ispod korake:

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacija](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    na. Kao **način**, odaberite **SSO s Samlv2**.

    b. Tekstni okvir u **SSO preusmjeravanje** stavite vrijednost **SAML SSO URL** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    c. Tekstni okvir u **odjavite preusmjeravanje** stavite vrijednost **URL servisa za jednu Sign-Out** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    d. Tekstni okvir u **Oblik naziva Id** postavite vrijednost **Oblik naziva identifikator** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    e. Kopirajte sadržaj datoteke preuzete certifikat, a zatim je zalijepite u tekstni okvir za **potvrdu** . 

    f. Kliknite ikonu **Spremi** .
    
8. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

9. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

### <a name="creating-a-runmyprocess-test-user"></a>Stvaranje RunMyProcess testnih korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u RunMyProcess, oni mora biti dodijeljena u RunMyProcess. U slučaju RunMyProcess, dodjeljivanje jest zadatak za ručno.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke RunMyProcess kao administrator.

2.  Kliknite **račun** i odaberite **korisnike** u lijevom navigacijskom oknu, a zatim kliknite **Novi korisnik**.

    ![Novi korisnik] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Novi korisnik")

3.  U odjeljku **Korisničke postavke** poduzeti sljedeće korake:

    ![Profil] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profil")

    na. Unesite **ime** i **e-pošte** valjani AAD računa koji želite dodjele resursa u povezani tekstni okviri.

    b. Odaberite **jezik IDE**, **jezik** i **profila**.

    c. Odaberite **Pošalji račun stvaranja e-pošte me**.

    d. Kliknite **Spremi**.

    >[AZURE.NOTE] Možete koristiti bilo koji drugi RunMyProcess korisnički račun alate za stvaranje ili API-ji nudi RunMyProcess Dodjela resursa za Azure Active Directory korisničke račune.
    

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju RunMyProcess.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona RunMyProcess, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **RunMyProcess**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. traka na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu RunMyProcess na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji RunMyProcess.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
