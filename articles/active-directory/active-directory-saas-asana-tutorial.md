<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Asana | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Asana."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Praktični vodič: Azure Active Directory Integracija s Asana

U ovom ćete praktičnom vodiču saznati kako Asana integrirati Azure Active Directory (Azure AD).

Integriranje Asana s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Asana
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Asana (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Asana, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na **Asana** jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje. Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Asana iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-asana-from-the-gallery"></a>Dodavanje Asana iz galerije
Da biste konfigurirali Integracija Asana u Azure AD, morate dodati Asana iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Asana iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Asana**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. U oknu s rezultatima odaberite **Asana**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Asana utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Asana određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Asana Azure AD korisnik mora uspostaviti.
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Asana.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Asana, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje programa Asana testiranje korisnika](#creating-an-Asana-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Asana koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD s jedinstvenom prijavom

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Asana.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Asana, poduzmite sljedeće korake:**

1. Na izborniku na vrhu kliknite **Brzi početak rada**.

    ![Konfiguriranje jedinstvenu prijavu][6]
2. Na portalu klasični, na stranici za integraciju aplikacije **Asana** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][7] 

3. Na stranici **kako biste željeli korisnika da biste se prijavili Asana** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    na. U tekstni okvir prijavite se na URL unesite URL-a pomoću sljedećeg uzorka:`https://app.asana.com`

    c. Kliknite **Dalje**.

5. Na stranici **Konfiguracija jedinstvenu prijavu na Asana** kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu. Osim toga, kopirajte vrijednost za SAML SSO URL.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Desnom tipkom miša kliknite certifikat, zatim otvorite datoteku certifikata pomoću Blok za pisanje ili Preferirani uređivač teksta. Kopiranje sadržaja između s početka i završetka naslov certifikata. Ovo je certifikat X.509 koju ćete koristiti u Asana za konfiguriranje SSO.

6. U prozoru drugi preglednik, prijavu u aplikaciju Asana. Da biste konfigurirali SSO u Asana, i to klikom na naziv radnog prostora u gornjem desnom kutu zaslona pristupiti postavke radnog prostora. Zatim kliknite na ** \<naziva radnog prostora\> postavke**. 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. U prozoru **Postavke tvrtke ili ustanove** , kliknite **Administracija**. Zatim kliknite **Članovi morate se prijaviti putem SAML** da biste omogućili konfiguraciju SSO. Izvedi sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    na. U texbox **URL stranice za prijavu** na URL-ove iz Azure AD zalijepite znak SAML.

    b. U tekstni okvir **Certifikat X.509** zalijepite X.509 certifikat koji ste kopirali iz Azure AD.

9. Kliknite **Spremi**. Ako vam je potrebna pomoć, idite na [Asana vodič za postavljanje SSO](https://asana.com/guide/help/premium/authentication#gl-saml) .

7. Otvorite stranicu **Konfiguriraj jedinstvenu prijavu na Asana** u Azure AD, odaberite potvrdu jedan konfiguracije za prijavu, a zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-asana-test-user"></a>Stvaranje korisnik za testiranje sustava Asana

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Asana.

1. Na **Asana**, prijeđite na odjeljak **timovima** u lijevom oknu. Kliknite gumb znak plus. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Upišite e-pošte britta.simon@contoso.com u tekstni okvir i zatim odaberite **Pozovi**.
3. Kliknite **Pošalji pozivnicu**. Novi korisnik će primiti poruku e-pošte u svoj račun e-pošte. Ana morat ćete stvoriti i provjeriti račun.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Asana.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Asana, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Asana**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu svi korisnici odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je da biste testirali vaše Azure AD jedinstvenu prijavu.

Idite na stranicu za prijavu Asana. Tekstni okvir adresa e-pošte Umetni adresu e-pošte britta.simon@contoso.com. Tekstni okvir Lozinka u ostavite prazno, a zatim kliknite **Prijava**. Bit ćete preusmjereni na stranicu za prijavu Azure AD. Dovršite Azure AD vjerodajnice. Sada ste prijavljeni Asana.

## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
