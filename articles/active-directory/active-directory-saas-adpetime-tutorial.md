<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ADP eTime | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i ADP eTime."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Praktični vodič: Azure Active Directory Integracija s ADP eTime

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji ADP eTime s Azure Active Directory (Azure AD).  
Integriranje ADP eTime s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup ADP eTime
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste eTime ADP (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal


Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija ADP eTime, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na ADP eTime jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje ADP eTime iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-adp-etime-from-the-gallery"></a>Dodavanje ADP eTime iz galerije
Da biste konfigurirali Integracija ADP eTime u Azure AD, morate dodati ADP eTime iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali ADP eTime iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **ADP eTime**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. U oknu s rezultatima odaberite **ADP eTime**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu s eTime ADP utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u eTime ADP korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u ADP eTime Azure AD korisnik mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u ADP eTime.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s ADP eTime, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje ADP eTime testiranje korisnika](#creating-a-adpetime-test-user)** – da bi se na postoji zamjena u obliku od dan Britta Simona u eTime ADP koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji eTime ADP.

Aplikacija eTime ADP očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju tokena atributa SAML. Sljedeća slika prikazuje primjer za to. Naziv zahtjeva uvijek će biti **"PersonImmutableID"** i vrijednosti koje ćemo su mapirani ExtensionAttribute2 koja sadrži IDZaposlenika korisnika. Ovdje ispred mapiranje korisnika Azure AD za ADP eTime provest će se na na IDZaposlenika, ali to možete mapirati na neku drugu vrijednost i na temelju postavki aplikacije. Pa vas molimo rad s timom eTime ADP najprije ćete koristiti ispravne identifikator korisnika i mapirati vrijednost s zahtjeva **"PersonImmutableID"** .  

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Prije nego što možete konfigurirati pridruživanju SAML, morate obratite se službi za podršku eTime ADP i zatražite vrijednost atributa Jedinstveni identifikator za vaš klijent. Potreban vam je ta vrijednost da biste konfigurirali prilagođene zahtjeva za aplikaciju.


**Da biste konfigurirali Azure AD jedinstvenu prijavu ADP eTime, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **ADP eTime** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili ADP eTime** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    na. U tekstni okvir **URL odgovor** , unesite URL koristi za prijavu u aplikaciju eTime ADP pomoću sljedećeg uzorka korisnici: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Kliknite **Dalje**.

4. Na stranici **Konfiguracija jedinstvenu prijavu na ADP eTime** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfiguriran za aplikaciju, obratite se službi za podršku eTime ADP i e-pošte datoteku metapodataka prilaganje preuzeti tako da je konfigurirati za integraciju SSO.

    > [AZURE.NOTE] Nakon **ADP eTime** tima konfiguracija instancu, Preuzmi vrijednost **RelayState** od njih. Slijedite u ispod navedenim korake da biste ga konfigurirati. Nakon tu konfiguraciju možete testirati i integraciju sa servisom. Stoga Imajte na umu da je to važna konfiguracija za ovaj integraciju aplikacija za rad.

6. Da biste konfigurirali RelayState vrijednost u Azure AD, poduzmite sljedeće korake: 
    
    na. Prijava na [Portal za upravljanje Azure](https://portal.azure.com) kao administrator.

    b. U lijevom navigacijskom oknu kliknite **Više usluga**. 
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c. U tekstni okvir za **pretraživanje** upišite **Azure Active Directory**, a zatim srodne veze.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Kliknite **aplikacijama**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. U odjeljku **Upravljanje** kliknite **Sve aplikacije**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. U tekstni okvir za **pretraživanje** upišite **ADP eTime**, a zatim srodne veze. 
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. U odjeljku **Upravljanje** kliknite **jedinstvenu prijavu**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Odaberite **Pokaži napredne postavke URL-a**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    li. U tekstni okvir **Stanje prijenosa** upišite vrijednost pomoću sljedećih uzorke:
    
    - Radnog okruženja:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Pripremna okruženja:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. Spremanje postavki.

7. Azure klasični portalu odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  

    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.  
Na popisu korisnika odaberite **Dan Britta Simona**.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    na. Kao **Korisnik vrste**, odaberite **novi korisnik u tvrtki ili ustanovi**.

    b. U tekstni okvir **Korisničko ime** upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-adp-etime-test-user"></a>Stvaranje ADP eTime testnih korisnika

Cilj ovaj odjeljak je stvaranje naziva dan Britta Simona u ADP eTime korisnika. Raditi ADP eTime podršku za dodavanje korisnika u ADP eTime računa. 


> [AZURE.NOTE]Ako je potrebno ručno stvoriti korisnik sustava ćete morati obratite se timu za podršku eTime ADP.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju ADP eTime.

![Dodijeli korisniku][200] 

**Da biste dodijelili ADP eTime dan Britta Simona, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **ADP eTime**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu eTime ADP u oknu programa Access, koje treba se automatski prijavljeni na u aplikaciji eTime ADP.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
