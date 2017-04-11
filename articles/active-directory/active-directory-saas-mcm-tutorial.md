<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s MCM | Microsoft Azure" 
    description="Saznajte kako koristiti MCM s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Praktični vodič: Azure Active Directory Integracija s MCM
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciji MCM s Azure Active Directory (Azure AD).

Integriranje MCM s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup MCM
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste MCM (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija MCM, potrebne su vam sljedeće stavke:

- Valjani Azure pretplate
- Na MCM jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje MCM iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave

## <a name="adding-mcm-from-the-gallery"></a>Dodavanje MCM iz galerije
Da biste konfigurirali Integracija MCM u Azure AD, morate dodati MCM iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali MCM iz galerije, poduzmite sljedeće korake:**

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **MCM**.

    ![Galerija aplikacija] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Galerija aplikacija")

7.  U oknu s rezultatima odaberite **MCM**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s MCM utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u MCM korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u MCM Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u MCM.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s MCM, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na MCM testiranje korisnika](#creating-a-mcm-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u MCM koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu
  
U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji MCM.

**Da biste konfigurirali Azure AD jedinstvenu prijavu MCM, poduzmite sljedeće korake:**

1.  Azure klasični portalu na stranici za integraciju aplikacije **MCM** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili MCM** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Microsoft Azure AD jedinstvene prijave] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD jedinstvene prijave")

3.  Na stranici dijaloški okvir Konfiguriranje postavki aplikacije poduzeti sljedeće korake:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Konfiguriranje URL adresa Web App")

    na. U tekstni okvir **URL na za prijavu** upišite: `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Kliknite **Dalje**

4.  Na stranici **Konfiguracija jedinstvenu prijavu na MCM** kliknite **Preuzmi metapodataka**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Konfiguriranje jedinstvenu prijavu")

5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se službi za podršku MCM. Prilaganje datoteke preuzete metapodatke te je zajednički koristite s timom MCM da biste postavili SSO kod sebe.

6.  Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Konfiguriranje jedinstvenu prijavu")

7. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Konfiguriranje jedinstvenu prijavu")


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test

Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   

###<a name="creating-a-mcm-test-user"></a>Stvaranje MCM testnih korisnika
  
U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u MCM. Raditi MCM podršku za dodavanje korisnika u MCM platforme.

>[AZURE.NOTE]Možete koristiti bilo koji drugi MCM korisnički račun alate za stvaranje ili API-ji nudi MCM dodjele resursa AAD korisničkih računa.


###<a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD
  
Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju MCM.
    
![Dodjela korisnika] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Dodjela korisnika")

**Da biste dodijelili dan Britta Simona MCM, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodjela korisnika] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Dodjela korisnika")

2. Na popisu aplikacija odaberite **MCM**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodjela korisnika] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Dodjela korisnika")

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodjela korisnika] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Dodjela korisnika")


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu MCM na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji MCM.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)