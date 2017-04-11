<properties
    pageTitle="Praktični vodič: Azure Active Directory integracije sa softverom za produktivnost Wizergos | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Wizergos produktivnost softvera."
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


# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Praktični vodič: Azure Active Directory integracije sa softverom za produktivnost Wizergos 

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Wizergos produktivnost softver s Azure Active Directory (Azure AD).

Integriranje Wizergos produktivnost softver s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Wizergos produktivnost softver
- Možete omogućiti svojim korisnicima da automatski se prijavili u Wizergos produktivnost softver (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD integracije sa softverom za produktivnost Wizergos, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na Wizergos produktivnost softver jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Wizergos produktivnost softvera iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Dodavanje Wizergos produktivnost softvera iz galerije
Da biste konfigurirali integraciju softvera Wizergos produktivnost u Azure AD, morate dodati Wizergos produktivnost softver iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Wizergos produktivnost softver iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Wizergos produktivnost softvera**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)

7. U oknu rezultata odaberite **Wizergos produktivnost softver**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu pomoću softvera za produktivnost Wizergos utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u softveru za produktivnost Wizergos korisniku u Azure AD. Drugim riječima, vezu odnosa između tablica i povezane korisnika u Wizergos produktivnost softver Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Wizergos produktivnost softvera.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s BynWizergos produktivnost Softwareder, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje softver za produktivnost Wizergos testiranje korisnika](#creating-a-wizergos-productivity-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Wizergos produktivnost softver koji je povezan s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Wizergos produktivnost softvera.

**Da biste konfigurirali Azure AD jedinstvenu prijavu pomoću softvera za produktivnost Wizergos, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Wizergos produktivnost softver** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Wizergos produktivnost softver** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**:
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)

3. Kliknite **Dalje**na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** :

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)

4. Na stranici **Konfiguracija jedinstvenu prijavu na Wizergos produktivnost softver** kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)

5. U prozoru preglednika drugoj web prijave za vaš klijent Wizergos produktivnost softver kao administrator.

6. Na izborniku hamburger odaberite **administrator**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

7. Na stranici za administratore na izborniku na lijevoj strani odaberite **provjeru autentičnosti** , a zatim kliknite na **Azure AD**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

8. U odjeljku **Provjera autentičnosti** poduzeti sljedeće korake.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)

    na. Ikona **PRIJENOS** da biste prenijeli certifikata preuzete iz Azure AD. 

    b. Tekstni okvir u **Izdavača URL** stavite vrijednost **Izdavač URL** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    c. Tekstni okvir u **Jednom URL za prijavu** postavite vrijednost od **Jednog prijave URL servisa** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    d. U **Jednom Sign-Out URL** -u tekstni okvir postavite vrijednost **URL servisa za jednu Sign-out** iz čarobnjaka za konfiguraciju Azure AD aplikacija.

    e. Kliknite gumb **Spremi** .

9. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

10. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-wizergos-productivity-software-test-user"></a>Stvaranje korisnika test Wizergos produktivnost softver

U ovom ćete odjeljku Stvaranje korisnika naziva dan Britta Simona Wizergos produktivnost softvera. Raditi Wizergos produktivnost softver za podršku putem [support@wizergos.com](emailTo:support@wizergos.com) da biste dodali korisnike u platformu Wizergos produktivnost softvera.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Wizergos produktivnost softver.
    
   ![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona Wizergos produktivnost softver, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Wizergos produktivnost softvera**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)

1. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodijeli korisniku][205]



### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu Wizergos produktivnost softver na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Wizergos produktivnost softvera.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
