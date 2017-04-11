<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Portal za upravljanje oblaka za Microsoft Azure | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Portal za upravljanje oblaka za Microsoft Azure."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Praktični vodič: Azure Active Directory Integracija s Portal za upravljanje oblaka za Microsoft Azure

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Portal za upravljanje oblaka za Microsoft Azure s Azure Active Directory (Azure AD).  
Integriranje Portal za upravljanje oblaka za Microsoft Azure s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Portal za upravljanje oblaka za Microsoft Azure
- Možete omogućiti svojim korisnicima da automatski se prijavljeni na portal za upravljanje oblaka za Microsoft Azure (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal


Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Portal za upravljanje oblaka za Microsoft Azure, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Portal za upravljanje oblaka za Microsoft Azure s jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Portal za upravljanje oblaka za Microsoft Azure iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Dodavanje Portal za upravljanje oblaka za Microsoft Azure iz galerije
Da biste konfigurirali Integracija Portal za upravljanje oblaka za Microsoft Azure u Azure AD, morate dodati Portal za upravljanje oblaka za Microsoft Azure iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Portal za upravljanje oblaka za Microsoft Azure iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Portal za upravljanje oblaka za Microsoft Azure**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_01.png)

7. U oknu s rezultatima odaberite **Portal za upravljanje oblaka za Microsoft Azure**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu pomoću portala za upravljanje oblaka za Microsoft Azure utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u oblak Portal za upravljanje za Microsoft Azure korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u oblak Portal za upravljanje za Microsoft Azure Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u oblak Portal za upravljanje za Microsoft Azure.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu pomoću portala za upravljanje oblaka za Microsoft Azure, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Portal za upravljanje oblaka za Microsoft Azure testiranje korisnika](#creating-a-newsignature-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u oblak Portal za upravljanje za Microsoft Azure koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u oblak Portal za upravljanje u za aplikaciju Microsoft Azure.



**Da biste konfigurirali Azure AD jedinstvenu prijavu Portal za upravljanje oblaka za Microsoft Azure, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Portal za upravljanje oblaka za Microsoft Azure** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da se prijavite se Portal za upravljanje oblaka za Microsoft Azure** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnicima prijavu na Portal za upravljanje oblaka za aplikaciju Microsoft Azure pomoću sljedećeg uzorka:`https://portal.igcm.com/<instance name>`

    b. Kliknite **Dalje**.


4. Na stranici **Konfiguracija jedinstvenu prijavu na Portal za upravljanje oblaka za Microsoft Azure** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_05.png) 

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se Portal za upravljanje oblaka za Microsoft Azure timu za podršku na [jczernuszka@newsignature.com](mailTo:jczernuszka@newsignature.com) i e-pošte Priloži datoteku preuzete certifikata. I navedite izdavač URL, SAML SSO URL i jedan znak Out URL servisa tako da je konfigurirati za integraciju SSO.


6. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Stvaranje Portal za upravljanje oblaka za Microsoft Azure testnih korisnika

Cilj ovaj odjeljak je stvaranje korisnika pod naslovom dan Britta Simona portalu za upravljanje oblaka za Microsoft Azure. Raditi oblaka Portal za upravljanje za podršku za Microsoft Azure da biste dodali korisnike na portalu za upravljanje oblaka za račun sustava Microsoft Azure. 


> [AZURE.NOTE]Ako je potrebno ručno stvoriti korisnik sustava ćete morati oblaka Portal za upravljanje zatražite od tima za podršku Microsoft Azure.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Portal za upravljanje oblaka za Microsoft Azure.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Portal za upravljanje oblaka za Microsoft Azure, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Portal za upravljanje oblaka za Microsoft Azure**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete Portal za upravljanje oblak na servisu Microsoft Azure ploča programa Access, koje treba se automatski prijavljeni na za Portal za upravljanje oblaka za aplikaciju Microsoft Azure.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_205.png
