<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s HackerOne | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i HackerOne."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Praktični vodič: Azure Active Directory Integracija s HackerOne

U ovom ćete praktičnom vodiču HackerOne integrirati s Azure Active Directory (Azure AD).

Integriranje HackerOne s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup HackerOne
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste HackerOne (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija HackerOne, potrebne su vam sljedeće stavke:

- Azure pretplate
- Na HackerOne jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču, konfiguriranje i testiranje Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje HackerOne iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-hackerone-from-the-gallery"></a>Dodavanje HackerOne iz galerije
Da biste integrirali HackerOne u Azure AD, morate dodati HackerOne iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali HackerOne iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

![Aplikacija][4]

6. U okvir za pretraživanje upišite **HackerOne**.

![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. U oknu s rezultatima odaberite **HackerOne**, a zatim **Dovršeno** da biste dodali aplikaciju.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Nakon toga, konfiguriranje i testiranje Azure AD jedinstvenu prijavu s HackerOne utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u HackerOne korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u HackerOne Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u HackerOne.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s HackerOne, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na HackerOne testiranje korisnika](#creating-a-hackerone-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u potvrdi koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Nakon toga omogućite Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciju HackerOne.

Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

**Da biste konfigurirali Azure AD jedinstvenu prijavu HackerOne, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **HackerOne** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili HackerOne** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju HackerOne pomoću sljedećeg uzorka: **"https://hackerone.com/\<naziv tvrtke\>/authentication"**. 

    b. Obratite se timu za podršku HackerOne putem [support@hackerone.com](mailto:support@hackerone.com) da biste dobili klijentu URL-a ako ga ne znate.

    c. U tekstni okvir **identifikator** upišite URL klijenta. 

    d. Kliknite **Dalje**.


4. Na stranici **Konfiguracija jedinstvenu prijavu na HackerOne** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


1. Prijava za vaš klijent HackerOne kao administrator.

1. Na izborniku na vrhu kliknite **Postavke**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Dođite do "**provjere autentičnosti**", a zatim kliknite "**postavke za dodavanje SAML**".

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. U dijaloškom okviru **Postavke SAML** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    na. U tekstni okvir **Domena e-pošte** upišite registrirane domene.

    b. Azure portala za klasični kopirajte **Jedan prijave URL servisa**i pa ih zalijepite u tekstni okvir za jedan znak na URL-a.

    c. Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.  

       >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)
    
    d. Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim zalijepite ga u **X509 certifikat** tekstni okvir.

    e. Kliknite **Spremi**


1. U dijaloškom okviru Postavke provjere autentičnosti poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    na. Kliknite **Pokreni test**.

    b. Ako je vrijednost argumenta **Status** polja jednako **zadnje testiranje status: stvorili**, obratite se službi za podršku HackerOne putem [support@hackerone.com](mailto:support@hackerone.com) da biste zatražili pregled konfiguracije.


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test

Nakon toga stvorite testnih korisnika na portalu klasični naziva dan Britta Simona.  

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili SIGURNE IZLAGANJE testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   


### <a name="creating-a-hackerone-test-user"></a>Stvaranje HackerOne testnih korisnika

Nakon toga Stvaranje korisnika naziva dan Britta Simona u HackerOne. HackerOne podržava samo-u – vrijeme dodjele resursa, koje po zadanom je omogućena.

Postoji nijedna akcija stavka umjesto vas u ovom odjeljku. Prilikom pristupa HackerOne novog korisnika se stvara Ako još ne postoji. [Konfiguriranje Azure AD jedinstvene prijave](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Ako je potrebno ručno stvoriti korisnika, morate je obratite se timu za podršku potvrdi.




### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Nakon toga omogućite Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju HackerOne.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona HackerOne, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **HackerOne**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Na kraju, testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.  
Kada kliknete pločicu HackerOne na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji HackerOne.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png