<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Ceridian Dayforce HCM | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Ceridian Dayforce HCM."
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


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Praktični vodič: Azure Active Directory Integracija s Ceridian Dayforce HCM

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Ceridian Dayforce HCM s Azure Active Directory (Azure AD).  
Integriranje Ceridian Dayforce HCM s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Ceridian Dayforce HCM
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Ceridian Dayforce HCM (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal


Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Ceridian Dayforce HCM, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U Ceridian Dayforce HCM jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Ceridian Dayforce HCM iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Dodavanje Ceridian Dayforce HCM iz galerije
Da biste konfigurirali Integracija Ceridian Dayforce HCM u Azure AD, morate dodati Ceridian Dayforce HCM iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Ceridian Dayforce HCM iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Ceridian Dayforce HCM**.

    ![Aplikacija](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. U oknu s rezultatima odaberite **Ceridian Dayforce HCM**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Ceridian Dayforce HCM utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Ceridian Dayforce HCM korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Ceridian Dayforce HCM Azure AD korisnik mora uspostaviti.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Ceridian Dayforce HCM, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje HCM za Dayforce Ceridian testiranje korisnika](#creating-a-ceridian-dayforce-hcm-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u HCM Dayforce Ceridian koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Ceridian Dayforce HCM.

Aplikacija Ceridian Dayforce HCM očekuje SAML assertions u određenom obliku. Raditi Dayforce HCM tima prvi put da biste odredili identifikator točan korisnika. Microsoft preporučuje korištenje atributa **"naziv"** kao identifikator korisnika. Možete upravljati vrijednost toga atributa u dijaloškom okviru **"Atrribute"** . Sljedeća slika prikazuje primjer za to. 

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Da biste konfigurirali Azure AD jedinstvenu prijavu Ceridian Dayforce HCM, poduzmite sljedeće korake:**




1. Azure klasični portalu na stranici integraciju aplikacije **Ceridian Dayforce HCM** na izborniku na vrhu kliknite **atribute**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. Na popisu **saml tokena atribute** atributi odaberite atribut naziv, a zatim **Uređivanje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. U dijaloškom okviru **Uređivanje korisnika atribut** poduzeti sljedeće korake:
 
    na. Na popisu **Vrijednost atributa** odaberite atribut korisnika koji želite koristiti za implementaciju.  
    Na primjer, ako želite koristiti u IDZaposlenika kao identifikator jedinstveni korisnički, a koje ste spremili vrijednost atributa u na ExtensionAttribute2, zatim odaberite **user.extensionattribute2**. 

    b. Kliknite **dovrši**.  
    



1. Na izborniku na vrhu kliknite **Brzi početak rada**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Azure klasični portalu na stranici za integraciju aplikacije **Ceridian Dayforce HCM** kliknite **Konfiguriraj jedinstvenu prijavu**.

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Ceridian Dayforce HCM** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Ceridian Dayforce HCM. Za radnog okruženja, koristite sljedeći oblik URL:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Za prikaz, zamijenite DayforcehcmNamespace prostora za naziv vaše okruženje ili ID tvrtke; Ako, na primjer:`https://sso.dayforcehcm.com/contoso`
    
    Za testiranje okruženja, koristite sljedeći oblik URL:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. U tekstni okvir **Odgovor URL** unesite URL koji se koristi Azure AD da biste objavili odgovor.  
    Da biste postigli radnog okruženja, koristite:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Za testiranje okruženja, koristite:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Na stranici **Konfiguracija jedinstvenu prijavu na Ceridian Dayforce HCM** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se službi za podršku Ceridian Dayforce HCM putem e-pošte i pošaljite im sljedeće:

    - Datoteka preuzeta certifikata
    - **Izdavač URL-a**
    - **SAML SSO URL-a** 
    - **Jedan Odjava URL servisa** 


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Stvaranje korisnika test Ceridian Dayforce HCM

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u Ceridian Dayforce HCM korisnika. 

Raditi Ceridian Dayforce HCM timu za podršku da biste dobili korisnika dodaje vaš u aplikaciji Ceridian Dayforce HCM. 





### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Ceridian Dayforce HCM.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Ceridian Dayforce HCM, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Ceridian Dayforce HCM**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu Ceridian Dayforce HCM na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Ceridian Dayforce HCM.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
