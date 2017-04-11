<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Prepoznaj | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Prepoznaj."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Praktični vodič: Azure Active Directory Integracija s Prepoznaj

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Prepoznaj s Azure Active Directory (Azure AD).

Integriranje Prepoznaj s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Prepoznaj
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Prepoznaj (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Prepoznaj, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Prepoznaj jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Prepoznaj iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-recognize-from-the-gallery"></a>Dodavanje Prepoznaj iz galerije
Da biste konfigurirali Integracija Prepoznaj u Azure AD, morate dodati Prepoznaj iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Prepoznaj iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Prepoznaj**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. U oknu rezultata odaberite **Prepoznaj**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Prepoznaj utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Prepoznaj korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Prepoznaj Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Prepoznaj.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Prepoznaj, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na Prepoznaj testiranje korisnika](#creating-a-recognize-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Prepoznaj koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Prepoznaj.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Prepoznaj, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Prepoznaj** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Prepoznaj** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    na. U tekstni okvir **Prijavite se na URL** unesite URL pomoću sljedećeg uzorka: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. U tekstni okvir **identifikator** upišite URL-a pomoću sljedećeg uzorka: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Kliknite **Dalje**

    > [AZURE.NOTE] Ako ne znate o URL-ove, upišite URL-ove uzorka s uzorkom primjer. Da biste te vrijednosti, možete upućivati korak 9 više pojedinosti ili obratite se timu za podršku Prepoznaj putem <mailto:support@recognizeapp.com>.

4. Na stranici **Konfiguracija jedinstvenu prijavu na Prepoznaj** kliknite **Preuzmite certifikat** , a zatim spremite datoteku na računalu:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. U prozoru preglednika drugoj web prijave za vaš klijent Prepoznaj kao administrator.

6. U gornjem desnom kutu kliknite **izbornik**. Idite na **administrator tvrtke**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. U lijevom navigacijskom oknu kliknite **Postavke**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. U odjeljku **Postavke SSO** poduzeti sljedeće korake.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    na. Kao **Omogućiti SSO**, odaberite **Dalje**.

    b. U polju **ID entitet IDP** tekstni okvir postavite vrijednost **Izdavač URL** iz čarobnjaka za konfiguraciju aplikacije Azure AD.

    c. U **Sso odredišni url** tekstni okvir postavite vrijednost od **Jednog prijave URL servisa** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    d. **A sporim odredišni url** tekstni okvir postavite vrijednost **URL servisa za jednu Sign-Out** iz čarobnjaka za konfiguraciju Azure AD aplikacija.

    e. Certifikat preuzetu datoteku otvorili u blok za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **potvrdu** . 

    f. Kliknite gumb **Spremi postavke** . 

9. Uz odjeljak **Postavke SSO** kopirajte URL u odjeljku **url metapodataka za davatelja usluga**.
    
    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Otvorite **metapodataka URL veze** u odjeljku prazne preglednika da biste preuzeli metapodataka dokumenta. Zatim koristite vrijednost EntityDescriptor Prepoznaj koje vam je dao za **identifikator** u dijaloškom okviru **Konfiguriranje postavki aplikacije** .

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

12. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U tekstni okvir **Prezime** upišite **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-recognize-test-user"></a>Stvaranje Prepoznaj testnih korisnika

Da biste omogućili Azure AD korisnika da se prijavite u Prepoznaj, oni mora biti dodijeljena u Prepoznaj. U slučaju Prepoznaj, dodjeljivanje jest zadatak za ručno.

Ta aplikacija ne podržava SCIM dodjele resursa, ali nema sinkronizacije koja zamjenski korisnika koji se dodjeljuje korisnika. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Dodjela korisnički račun, učinite sljedeće:

1.  Prijavite se u web-mjesto tvrtke Prepoznaj kao administrator.

2.  U gornjem desnom kutu kliknite **izbornik**. Idite na **administrator tvrtke**.

3.  U lijevom navigacijskom oknu kliknite **Postavke**.

4.  U odjeljku **Sinkroniziranju korisničkih** poduzeti sljedeće korake.
    
    ![Novi korisnik] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Novi korisnik")

    na. Kao **Omogućena sinkronizacija**, odaberite **Dalje**.

    b. Kao **odaberite Sinkroniziraj davatelja**odaberite **Microsoft / Office 365**.

    c. Kliknite **Pokreni sinkronizaciju korisnika**

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Prepoznaj.
    
![Dodijeli korisniku][200]

**Da biste dodijelili Prepoznaj dan Britta Simona, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Prepoznaj**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodijeli korisniku][205]

### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu Prepoznaj na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Prepoznaj.

## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
