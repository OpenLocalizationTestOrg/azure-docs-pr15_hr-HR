<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Jive | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Jive."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Praktični vodič: Azure Active Directory Integracija s Jive

U ovom ćete praktičnom vodiču saznati kako Jive integrirati Azure Active Directory (Azure AD).

Integriranje Jive s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Jive
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Jive (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Jive, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Jive jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Jive iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-jive-from-the-gallery"></a>Dodavanje Jive iz galerije
Da biste konfigurirali Integracija Jive u Azure AD, morate dodati Jive iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Jive iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Jive**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. U oknu s rezultatima odaberite **Jive**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Jive utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Jive određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Jive Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Jive.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Jive, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na Jive testiranje korisnika](#creating-a-jive-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Jive koja je povezana s predstavljanje Azure AD njegove.
4. **[Konfiguriranje dodjeljivanje korisnika](#configuring-user-provisioning)** – Strukturiranje kako omogućiti korisniku dodjeljivanje korisnika servisa Active Directory račune na Jive.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
6. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Jive.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Jive, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Jive** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako želite korisnicima da biste se prijavili Jive** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Jive pomoću sljedećeg uzorka: **https://\<ime klijenta\>. jivecustom.com**.
    
    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Jive** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Prijava za vaš klijent Jive kao administrator.

6. Na izborniku na vrhu kliknite "**Saml**".

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacija](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    na. Na kartici **Općenito** odaberite **omogućeno** .

    b. Kliknite gumb "**Spremi sve postavke saml**".

7. Idite na karticu "**Idp metapodataka**".

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacija](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    na. Kopirati sadržaj XML datoteke preuzete metapodataka, a zatim je zalijepite u tekstni okvir **metapodataka davatelja identiteta (IDP)** .

    b. Kliknite gumb "**Spremi sve postavke saml**". 

8. Idite na karticu "**Mapiranja atributa korisnika**".

    ![Konfiguriranje jedinstvenu prijavu na strani aplikacija](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    na. U tekstni okvir **e-pošte** , kopirajte i zalijepite naziv atributa **pošte** vrijednosti.

    b. U tekstni okvir **ime** kopirajte i zalijepite naziv atributa **givenname** vrijednosti.

    c. U tekstni okvir **Prezime** kopirajte i zalijepite naziv atributa **Prezime** vrijednosti.
    
9. Na portalu za Azure AD odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
![Azure AD jedinstvene prijave][10]

10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
  ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



###<a name="creating-a-jive-test-user"></a>Stvaranje Jive testnih korisnika

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Jive. Raditi Jive podršku za dodavanje korisnika u Jive platforme.


###<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisniku dodjeljivanje korisničkih računa servisa Active Directory za Jive.  
Kao dio ovog postupka, koje su potrebne za pružanje sigurnosni token korisnika ćete morati zatražiti od Jive.com.
  
Sljedeća slika prikazuje primjer dijaloškog okvira povezane u Azure AD:

![Konfiguriranje dodjeljivanje korisnika] (./media/active-directory-saas-jive-tutorial/IC698794.png "Konfiguriranje dodjeljivanje korisnika")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Da biste konfigurirali dodjeljivanje korisnika, učinite sljedeće:

1.  Na portalu za upravljanje Azure, na stranici za integraciju aplikacije **Jive** kliknite **Konfiguriraj dodjele resursa korisnika** da biste otvorili dijaloški okvir **Konfiguriranje dodjele resursa korisnika** .

2.  Na stranici **Unesite vjerodajnice Jive da biste omogućili automatsko korisnika dodjele resursa** , navedite sljedeće postavke konfiguracije:

    1.  U tekstni okvir **Jive administrator korisničko ime** unesite naziv računa Jive koja ima profila **Administrator sustava** u Jive.com dodijeljeni.

    2.  U tekstni okvir **Jive administratorsku lozinku** upišite lozinku za taj račun.

    3.  U tekstni okvir **URL klijentu Jive** upišite URL Jive klijenta.

        >[AZURE.NOTE]Jive klijentu URL je URL koji koristi vaša tvrtka ili ustanova za prijavu u Jive.  
        Obično je URL sljedećem obliku: "www" **.\< Tvrtka ili ustanova\>. jive.com**.

    4.  Kliknite **Provjera valjanosti** da biste provjerili konfiguraciju.

    5.  Kliknite gumb **Dalje** da biste otvorili stranicu **potvrde** .

3.  Na stranici za **potvrdu** kliknite kvačicu da biste spremili konfiguraciju.
  
Možete sada stvoriti testnom računu, pričekajte 10 minuta i provjerite je li sinkronizirani da račun Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Jive.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Jive, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Jive**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Jive na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Jive.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
