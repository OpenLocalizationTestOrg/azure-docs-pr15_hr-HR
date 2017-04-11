<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s OpsGenie | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i OpsGenie."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Praktični vodič: Azure Active Directory Integracija s OpsGenie

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji OpsGenie s Azure Active Directory (Azure AD).

Integriranje OpsGenie s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup OpsGenie
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste OpsGenie (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija OpsGenie, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na OpsGenie jedinstvenu prijavu omogućeno pretplate


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje. 

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje OpsGenie iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-opsgenie-from-the-gallery"></a>Dodavanje OpsGenie iz galerije
Da biste konfigurirali Integracija OpsGenie u Azure AD, morate dodati OpsGenie iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali OpsGenie iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **OpsGenie**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. U oknu s rezultatima odaberite **OpsGenie**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s OpsGenie utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba znati korisnika postoji zamjena u obliku u OpsGenie korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u OpsGenie Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u OpsGenie.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s OpsGenie, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje na OpsGenie testiranje korisnika](#creating-a-opsgenie-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u OpsGenie koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji OpsGenie.



**Da biste konfigurirali Azure AD jedinstvenu prijavu OpsGenie, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **OpsGenie** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili OpsGenie** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    na. U tekstni okvir prijavite se na URL unesite URL koji se koristi korisnika za prijavu u aplikaciju OpsGenie pomoću sljedećeg uzorka: **"https://app.opsgenie.com/auth/login"**.

    > [AZURE.NOTE] Obratite se vaš [OpsGenie službi za podršku](mailto:support@opsgenie.com) ako vam je potrebna Prijava na URL.

    b. Kliknite **Dalje**.


4. Na stranici **Konfiguracija jedinstvenu prijavu na OpsGenie** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu. Će moramo ovog certifikata i URL-ovi metapodataka (ID entitet, SSO URL za prijavu i URL izgleda za prijavu) da biste postavili SSO OpsGenie strane.

    b. Kliknite **Dalje**.


5. Otvorite neki drugi prozor preglednika, a zatim zapisnika u OpsGenie kao administrator.

6. Kliknite **Postavke**, a zatim kliknite karticu za **Jedinstvenu prijavu** .
 
    ![OpsGenie jedinstvene prijave](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. Da biste omogućili SSO, odaberite **omogućeno**.

    ![Postavke OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. U odjeljku **davatelj usluga** karticu **Azure Active Directory** .

    ![Postavke OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Na stranici dijaloški Azure Active Directory, učinite sljedeće:
 
    ![Postavke OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    na. Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na OpsGenie** kopirajte vrijednost za **Jedan znak na URL servisa** i pa ih zalijepite u tekstni okvir **SAML 2.0 krajnje** .

    b. Stvaranje osnovni 64 kodirana datoteka iz preuzete certifikata.      
    
    > [AZURE.NOTE] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be).

    c. Otvaranje certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik i pa ih zalijepite u tekstni okvir **X.500 certifikata** .

    d. Kliknite **Spremi promjene**.


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.



![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-opsgenie-test-user"></a>Stvaranje OpsGenie testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u OpsGenie korisnika. 

1.  U prozoru web-preglednika, prijavite se u klijentu OpsGenie kao administrator.

2.  Idite na popis korisnika tako da kliknete **korisnika** u lijevom oknu.
   
    ![Postavke OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Kliknite "**Dodavanje korisnika**".

3.  U dijaloškom okviru **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Postavke OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    na. U tekstni okvir **e-pošte** upišite adresu e-pošte Britta korisnika u servisu Azure Active Directory.

    b. U tekstni okvir **Ime i prezime** upišite **Dan Britta Simona**.

    c. Kliknite **Spremi**. 

Britta će primiti poruku e-pošte s uputama za postavljanje svoj profil.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju OpsGenie.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona OpsGenie, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
 
    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **OpsGenie**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu OpsGenie na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji OpsGenie.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
