<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Questetra BPM paket | Microsoft Aure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Questetra BPM paketa."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Praktični vodič: Azure Active Directory Integracija s Questetra BPM paketa

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Questetra BPM paket s Azure Active Directory (Azure AD).  
Integriranje Questetra BPM paket s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Questetra BPM paketa 
- Možete omogućiti svojim korisnicima da automatski se prijavili u u paketu BPM Questetra (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija Questetra BPM paket, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U [Paket BPM Questetra](https://senbon-imadegawa-988.questetra.net/) jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od tri glavna sastavni blokovi:

1. Dodavanje Questetra BPM paketa iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Dodavanje Questetra BPM paketa iz galerije
Da biste konfigurirali integraciju paketa BPM Questetra u Azure AD, morate dodati Questetra BPM paketa iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Questetra BPM paketa iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Questetra BPM paketa**.

    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **Questetra BPM paketa**, a zatim **Dovršeno** da biste dodali aplikaciju.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Questetra BPM paketa utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u paketu BPM Questetra korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u paketu BPM Questetra Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u paketu BPM Questetra.
 
Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Questetra BPM paket, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje paketa BPM Questetra testiranje korisnika](#creating-a-questetra-bpm-suite-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u paketu BPM Questetra koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Questetra BPM paketa.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Questetra BPM paket, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici Integracija **Questetra BPM paketa** aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][8]

2. Na stranici **kako biste željeli korisnika da biste se prijavili paket BPM Questetra** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][9]


3. U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke **Questetra BPM paketa** kao administrator.

4. Na izborniku na vrhu kliknite **Postavke sustava**. 

    ![Azure AD jedinstvene prijave][10]

5. Da biste otvorili stranicu **SingleSignOnSAML** , kliknite **SSO (SAML)**. 

    ![Azure AD jedinstvene prijave][11]


6. Azure klasični portalu na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake: 

    ![Konfiguriranje postavki aplikacije][13]
 
    na. Na **Paket BPM Questetra** web-mjesto tvrtke, u odjeljku podaci o SP kopirate **ACS URL**, a pa ih zalijepite u tekstni okvir za **Prijavu na URL-a** .

    b. Na **Paket BPM Questetra** web-mjesto tvrtke, u odjeljku podaci o SP kopirate **ID entitet**, a pa ih zalijepite u tekstni okvir **URL izdavač** .

    c. Na **Paket BPM Questetra** web-mjesto tvrtke, u odjeljku podaci o SP kopirate **ACS URL**, a pa ih zalijepite u tekstni okvir **URL adresa za odgovor** .

    d. Kliknite **Dalje**.

 
7. Na stranici **Konfiguracija jedinstvenu prijavu na paket BPM Questetra** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu][14]


8. Na web-mjesto tvrtke **Questetra BPM paket** , poduzmite sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu][15]

    na. Odaberite **Omogući jedinstvenu prijavu**.
     
    b. Azure portala za klasični kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **ID entitet** .

    c. Azure portala za klasični kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **URL stranice za prijavu** .

    d. Azure portala za klasični kopirajte **URL servisa za jednu Sign-Out** vrijednost i pa ih zalijepite u tekstni okvir **URL Sign-out stranice** .

    e. U tekstni okvir **oblik NameID** upišite **urn: organizacija: imena: tc: SAML:1.1:nameid-oblik: adresa**.


    f. Stvaranje osnovni 64 kodirana datoteka iz preuzete certifikata. 

    >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

    g. Otvorite certifikata kodirana base-64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **provjeru valjanosti certifikata** . 

    h. Kliknite **Spremi**.


9. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**. 

    ![Što je Azure AD Connect][17]


10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Što je Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD testnih korisnika][100] 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD testnih korisnika][101] 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD testnih korisnika][102] 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD testnih korisnika][103]
 
    na. Kao **Korisnik vrste**, odaberite **novi korisnik u tvrtki ili ustanovi**.
  
    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite Dalje.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD testnih korisnika][104] 
  
    na. U tekstni okvir **ime** upišite **Britta**. 
 
    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD testnih korisnika][105]  

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD testnih korisnika][106]   
  
    na. Zapišite vrijednost **Novu lozinku**.
  
    b. Kliknite **dovrši**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Stvaranje korisnika test Questetra BPM paketa

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u paketu BPM Questetra korisnika.

**Da biste stvorili korisnik naziva dan Britta Simona u paketu BPM Questetra, učinite sljedeće:**

1.  Prijava na web-mjesto tvrtke Questetra BPM paketa kao administrator.
2.  Idite na **sistemske postavke > popis korisnika > novi korisnik**. 
3.  U dijaloškom okviru novi korisnik poduzeti sljedeće korake: 

    ![Stvaranje testnih korisnika][300] 

    na. U tekstni okvir **naziv** upišite korisničko ime korisnika Britta Azure AD.

    b. U tekstni okvir **e-pošte** upišite korisničko ime Britta na Azure AD.

    c. U tekstni okvir **Lozinka** upišite lozinku.

4.  Kliknite **Dodaj novog korisnika**.



### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta da biste koristili Azure jedinstvenu prijavu dozvolite pristup u paketu BPM Questetra.

![Što je Azure AD Connect][200]

**Da biste dodijelili dan Britta Simona Questetra BPM paket, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Što je Azure AD Connect][201]

2. Na popisu aplikacija odaberite **Questetra BPM paketa**.

    ![Što je Azure AD Connect][205]

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Što je Azure AD Connect][202]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

    ![Što je Azure AD Connect][203]

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Što je Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu Questetra BPM paket na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Questetra BPM paketa.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 