<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Cisco Spark | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Cisco Spark."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Praktični vodič: Azure Active Directory Integracija s Cisco Spark

U ovom ćete praktičnom vodiču saznati kako Cisco Spark integrirati Azure Active Directory (Azure AD).

Integriranje Cisco Spark s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Cisco Spark
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste Cisco Spark (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Cisco Spark, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na **Cisco Spark** jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje. Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Cisco Spark iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-cisco-spark-from-the-gallery"></a>Dodavanje Cisco Spark iz galerije
Da biste konfigurirali Integracija Cisco Spark u Azure AD, morate dodati Cisco Spark iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Cisco Spark iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Cisco Spark**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. U oknu s rezultatima odaberite **Cisco Spark**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Spark Cisco utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Cisco Spark određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Cisco Spark Azure AD korisnik mora uspostaviti.
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Cisco Spark. Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Cisco Spark, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Cisco Spark testiranje korisnika](#creating-a-cisco-spark-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Spark Cisco koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Cisco Spark.

Aplikacija Spark Cisco očekuje assertions SAML sadrži određene atribute. Konfigurirajte sljedećim atributima za ovu aplikaciju. Na kartici **"Atrributes"** aplikacije možete upravljati vrijednosti od ovih atributa. Sljedeća slika prikazuje primjer za to.

![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Da biste konfigurirali Azure AD jedinstvenu prijavu Cisco Spark, poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici integraciju aplikacije **Cisco Spark** na izborniku na vrhu kliknite **atribute**.
     
    ![Konfiguriranje jedinstvenu prijavu][5]


2. U dijaloškom okviru **SAML tokena atribute** poduzeti sljedeće korake:

    na. Kliknite **Dodavanje atributa korisnika** da biste otvorili dijaloški okvir **Dodavanje Attribure korisnika** .

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. U tekstni okvir **Naziv atributa** upišite **uid**.
    
    c. Na popisu **Vrijednost atributa** odaberite **user.userprincipal**.
    
    d. Kliknite **dovrši**. Nakon toga **Primijeni promjene** pri dnu stranice.

3. Na izborniku na vrhu kliknite **Brzi početak rada**.

    ![Konfiguriranje jedinstvenu prijavu][6]

4. Na portalu klasični, na stranici za integraciju aplikacije **Cisco Spark** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][7] 

5. Na stranici **kako biste željeli korisnika da biste se prijavili Cisco Spark** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    na. U tekstni okvir prijavite se na URL unesite URL pomoću sljedećeg uzorka: `https://web.ciscospark.com/#/signin`.

    b. Kliknite **Dalje**.


7. Na stranici **Konfiguracija jedinstvenu prijavu na Cisco Spark** kliknite **Preuzimanje metapodataka**, a zatim spremite datoteku na računalu.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Prijavite se [Cisco oblaka suradnju](https://admin.ciscospark.com/) Management pomoću cijelog administratorske vjerodajnice.
9. Odaberite **Postavke** , a zatim u odjeljku **Provjera autentičnosti** kliknite **Izmijeni**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Odaberite **Integrate davatelja identiteta 3 proizvođača. (Dodatno)** i prijeđite na sljedeći zaslon.
11. Kliknite **Preuzmi datoteku metapodataka** i spremite datoteku na računalu.
12. Na stranici **Uvoz Idp metapodataka** povucite i ispustite datoteke metapodataka Azure AD na stranicu ili koristiti mogućnost preglednika datoteku da biste pronašli i prijenos datoteke metapodataka Azure AD. Zatim odaberite **zahtijevaju certifikat potpisao ustanove za izdavanje certifikata u metapodacima (sigurnije)** i kliknite **Dalje**. 

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Odaberite **Testiraj SSO vezu**, a kad se otvori na novoj kartici preglednika, uspješnoj Azure AD prijavom.
14. Vratite se na karticu preglednika **Cisco oblaka suradnju upravljanje** . Ako je provjera uspjela, odaberite **Ovaj test je uspio. Omogućite mogućnost za jedinstvenu prijavu** i kliknite **Dalje**.

7. Na portalu klasični Azure AD odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

8. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]

### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:
 
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-cisco-spark-test-user"></a>Stvaranje Cisco Spark testnih korisnika

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Cisco Spark. U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona u Cisco Spark.

1. Idite na [Cisco oblaka suradnju upravljanje](https://admin.ciscospark.com/) s cijelog administratorske vjerodajnice.
2. Kliknite **korisnici** , a zatim **Upravljanje korisnicima**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. U prozoru za **Upravljanje korisnički** odaberite **Ručno dodavanje ili izmjenu korisnika** , a zatim kliknite **Dalje**.
4. Odaberite **imena i adrese e-pošte**. Zatim popunite tekstni okvir kao pratite:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    na. U tekstni okvir **ime** upišite **Britta**

    b. U tekstni okvir **Prezime** upišite **Dan Simona**

    c. U tekstni okvir **Adresa e-pošte** upišite**britta.simon@contoso.com**

5. Kliknite znak plus da biste dodali dan Britta Simona. Zatim kliknite **Dalje**.
6. U prozoru **Dodavanje usluga za korisnike** , kliknite **Spremi** , a zatim **Završi**.

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Cisco Spark.

![Dodijeli korisniku][200] 

**Da biste dodijelili Cisco Spark dan Britta Simona, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Cisco Spark**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203] 

1. Na popisu svi korisnici odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.

Kada kliknete pločicu Cisco Spark na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Cisco Spark.

## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
