<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Lifesize oblaka | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Lifesize oblaka."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Praktični vodič: Azure Active Directory Integracija s Lifesize oblaka

U ovom ćete praktičnom vodiču, Saznajte kako integrirati Lifesize oblaka Azure Active Directory (Azure AD).

Integriranje oblaka Lifesize s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Lifesize oblak
- Možete omogućiti svojim korisnicima da automatski se prijavili u oblak Lifesize (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Lifesize oblaka, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U Oblaku Lifesize jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje oblaka Lifesize iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Dodavanje oblaka Lifesize iz galerije
Da biste konfigurirali integraciju s oblaka Lifesize u Azure AD, morate dodati Lifesize oblaka iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali oblaka Lifesize iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Lifesize oblaka**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. U oknu s rezultatima odaberite **Lifesize oblaka**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom odjeljku možete konfigurirati i testiranje Azure AD jedinstvenu prijavu s Lifesize oblaka utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u oblak Lifesize određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u oblak Lifesize Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Oblaku Lifesize.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Lifesize oblaka, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje Lifesize oblaka testiranje korisnika](#creating-a-lifesize-cloud-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Oblaku Lifesize koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Lifesize oblaka.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Lifesize oblaka, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Lifesize oblaka** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Oblaku Lifesize** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Lifesize oblak pomoću sljedećeg uzorka: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Oblaku Lifesize** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.


5. Da biste dobili SSO konfigurirana za aplikaciju, prijava u aplikaciju oblaka Lifesize s administratorskim ovlastima.

6. U gornjem desnom kutu kliknite vaše ime, a zatim na stranici **Naprednih postavki**

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. U sada kliknite naprednih postavki **Konfiguriranje SSO** vezu. Otvorit će se stranica SSO za vašoj instanci.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Sada konfigurirati sljedeće vrijednosti u konfiguraciji SSO korisničkog Sučelja.    

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Kopirajte vrijednost izdavač URL iz Azure AD i zalijepite tekst u tekstnom okviru **Izdavač davatelja identiteta** .

    • Kopirajte vrijednost URL daljinskog prijava iz Azure AD i zalijepite tekst u tekstni okvir **URL za prijavu** .

    • Otvorite preuzete certifikat u Bloku za pisanje i kopirajte sadržaj certifikata, bez crta certifikat početka i završetka certifikata, to zalijepite u tekstni okvir **Certifikat X.509** .

    • U mapiranja atributa SAML za tekstni okvir **ime** unesite vrijednost kao **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • U mapiranja atributa SAML za tekstni okvir **Prezime** unesite vrijednost kao **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • U mapiranja atributa SAML za tekstni okvir **e-pošte** unesite vrijednost kao **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Da biste provjerili konfiguraciju klikom na gumb za **testiranje** .

    > [AZURE.NOTE] Za uspješan testiranje morate dovršiti Čarobnjak za konfiguraciju Azure AD i i omogućivanje pristupa za korisnike ili grupe koji mogu izvršavati test.
    
10. Omogućivanje na SSO tako da na gumbu **SSO za omogućivanje** .

11. Sada kliknite gumb **Ažuriraj** kako bi se spremile sve postavke. Generirat će vrijednost RelayState. Kopirajte vrijednost RelayState koja je generiran u tekstni okvir. Moramo tu vrijednost u sljedećim koracima.

12. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

13. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]

14. Sada Prijava na Portal za upravljanje Azure **https://portal.azure.com** pomoću administratorskih vjerodajnica

15. Kliknite na vezu **Više servisa** u lijevom navigacijskom oknu
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Traženje Azure Active Directory i klikom na vezu za **Azure Active Directory**
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. Tražit će sve aplikacije SaaS ispod gumba **Aplikacijama** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Sada kliknite na vezu za **Sve aplikacije** u sljedeći plohu
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Traženje Lifesize aplikaciju za koju želite postaviti na RelayState. 
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Sada kliknite vezu **jedinstvenu prijavu** u na plohu

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Prikazat će se potvrdni okvir **Pokaži napredne postavke URL-a** . Kliknite potvrdni okvir.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Sada možete konfigurirati RelayState za aplikaciju koja se prikazuje na stranici za konfiguraciju Lifesize aplikacije SSO. 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. Spremanje postavki.

### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Stvaranje korisnik za testiranje sustava Lifesize oblaka

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Oblaku Lifesize. Oblak Lifesize ne podržava automatsko korisnika dodjele resursa. Nakon uspješne provjere autentičnosti na Azure AD, korisnik će biti automatski dodjeli u aplikaciji. 


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Lifesize oblak.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Lifesize oblak, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Lifesize oblaka**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Lifesize oblak na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Lifesize oblaka.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
