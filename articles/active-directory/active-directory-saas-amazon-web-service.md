<properties
    pageTitle="Praktični vodič: Azure Active Directory integracije sa servisa Amazon Web (AWS) | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure Active Directory Amazon Web Services (AWS) da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Praktični vodič: Azure Active Directory integracije sa servisa Amazon Web (AWS)

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji servisa Amazon Web (AWS) s Azure Active Directory (Azure AD).  
Integriranje servisa Amazon Web (AWS) s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup da biste servisa Amazon Web (AWS) 
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste servisa Amazon Web (AWS) (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD integracije sa servisa Amazon Web (AWS), potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Do servisa Amazon Web (AWS) jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od tri glavna sastavni blokovi:

1. Dodavanje servisa Amazon Web (AWS) iz galerije 
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Dodavanje servisa Amazon Web (AWS) iz galerije
Da biste konfigurirali integraciju od servisa Amazon Web (AWS) u Azure AD, morate dodati servisa Amazon Web (AWS) u galeriji na popis upravljani SaaS aplikacija.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Da biste dodali servisa Amazon Web (AWS) iz galerije, poduzmite sljedeće korake:

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1] 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** . 
   
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice. 
   
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**. 
   
    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Servisa Amazon Web (AWS)**.
   
    ![Aplikacija][5]

7. U oknu s rezultatima odaberite **Servisa Amazon Web (AWS)**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Aplikacija][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu utemeljeno na korisniku testa pod nazivom "Britta Dan Simona" servisa Web na Amazon (AWS).

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u servisa Amazon Web (AWS) korisniku u Azure AD. Drugim riječima, odnos vezu između programa Azure AD korisnik i povezane u servisa Amazon Web (AWS) mora uspostaviti.  
Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u obliku servisa Amazon Web (AWS).
 
Konfiguriranje i testiranje Azure AD jedinstvene prijave pomoću servisa Amazon Web (AWS), morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jednu jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
4. **[Stvaranje Amazon servisa Web (AWS) testiranje korisnika](#creating-a-halogen-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Amazon Web usluge (AWS) koja je povezana s predstavljanje Azure AD njegove.
5. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfiguriranje Azure AD jednu jedinstvenu prijavu

Cilj ovaj odjeljak je omogućiti Azure AD jedinstvenu prijavu na portalu za Azure klasični i konfiguriranje jedinstvenu prijavu u aplikaciju servisa Amazon Web (AWS).  
Aplikacije servisa Amazon Web (AWS) očekuje SAML assertions u određenom obliku, koje je potrebno da biste dodali prilagođeni atribut mapiranja konfiguraciju **saml tokena atribute** . Sljedeća slika prikazuje primjer za to.


![Konfiguriranje jedinstvenu prijavu][27]

**Da biste konfigurirali Azure AD jedinstvene prijave pomoću servisa Amazon Web (AWS), poduzmite sljedeće korake:**

1. Azure klasični portalu na stranici za integraciju aplikacije **Servisa Amazon Web (AWS)** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu][7]

2. Na stranici **kako biste željeli korisnika da se prijave za servis Amazon Web (AWS)** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu][8]

3. Na stranici **Konfiguracija postavki aplikacije** dijaloški okvir, kliknite Dalje. 

    ![Konfiguriranje postavki aplikacije][9]
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na servis Amazon Web (AWS)** kliknite **Preuzimanje metapodatke**, a zatim spremite datoteku metapodataka lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu][10]

5. U prozoru drugi preglednik, prijave na web-mjesto tvrtke servisa Amazon Web (AWS) kao administrator.

6. Kliknite **Polazno konzolu**.

    ![Konfiguriranje jedinstvenu prijavu][11]

7. Kliknite **Upravljanje pristupom i identitet**. 

    ![Konfiguriranje jedinstvenu prijavu][12]

8. Kliknite **Davatelji identiteta**, a zatim kliknite **Stvaranje davatelja usluga**. 

    ![Konfiguriranje jedinstvenu prijavu][13]

9. Na stranici dijaloški okvir **Konfiguriranje davatelja usluge** , učinite sljedeće: 

    ![Konfiguriranje jedinstvenu prijavu][14]

     na. Kao **Vrsta davatelja usluge**, odaberite **SAML**.

     b. U tekstni okvir **Naziv davatelja** upišite naziv davatelja (npr.: *WAAD*).

     c. Da biste prenijeli metapodataka preuzetu datoteku, kliknite **Odabir datoteke**.

     d. Kliknite **sljedeći korak**.


10. Na stranici dijaloški okvir **Provjera informacija za davatelja** kliknite **Stvori**. 

    ![Konfiguriranje jedinstvenu prijavu][15]

11. Kliknite **uloge**, a zatim kliknite **Stvori novo radno mjesto**. 

    ![Konfiguriranje jedinstvenu prijavu][16]

12. U dijaloškom okviru **Postavljanje naziv uloge** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu][17]

    na. U tekstni okvir **Naziv uloge** upišite naziv uloge (npr.: *TestUser*).

    b. Kliknite **sljedeći korak**.

13. U dijaloškom okviru **Odabir vrste ulogama** poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu][18]

    na. Odaberite **ulogu za pristup davatelja identiteta**.

    b. U odjeljku **Dodjela Web jedinstvenu prijavu (WebSSO) pristup davateljima na SAML** kliknite **Odaberi**.


14. U dijaloškom okviru **Uspostaviti pouzdanost** poduzeti sljedeće korake:  

    ![Konfiguriranje jedinstvenu prijavu][19]
     
     na. Kao davatelj SAML, odaberite davatelja usluge SAML stvorite previousley (npr.: *WAAD*) 

     b. Kliknite **sljedeći korak**.


15. U dijaloškom okviru **Potvrda uloga pouzdanosti** kliknite **Sljedeći korak**. 

    ![Konfiguriranje jedinstvenu prijavu][32]


16. U dijaloškom okviru **Prilaganje pravila** kliknite **Sljedeći korak**.  

    ![Konfiguriranje jedinstvenu prijavu][33]


17. U dijaloškom okviru **Pregled** poduzeti sljedeće korake:   

    ![Konfiguriranje jedinstvenu prijavu][34]

     na. Kopirajte vrijednost **AZNAJ uloge** .

     b. Kopirajte vrijednost AZNAJ **Pouzdana entiteti** .

     c. Kliknite **Stvori uloge**. 

18. Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.

    ![Što je Azure AD Connect][20]

19. Na stranici za **potvrdu jedan prijave** kliknite **dovrši** da biste zatvorili dijaloški okvir **Konfiguracija jedinstvenu prijavu** .

    ![Što je Azure AD Connect][22]


20. Na izborniku na vrhu kliknite **atribute** da biste otvorili dijaloški okvir **SAML tokena atribute** . 

    ![Konfiguriranje jedinstvenu prijavu][21]

21. Kliknite **Dodavanje korisnika atribut**. 

    ![Konfiguriranje jedinstvenu prijavu][23]

22. U dijaloškom okviru Dodavanje atributa korisnik poduzeti sljedeće korake. 

    ![Konfiguriranje jedinstvenu prijavu][24] 

    na. U tekstni okvir **Naziv atributa** upišite **https://aws.amazon.com/SAML/Attributes/Role**.

    b. U tekstni okvir **Vrijednost atributa** upišite **[AZNAJ uloga vrijednost], [vrijednost pouzdana entitet AZNAJ]**.

    >[AZURE.TIP] To su vrijednosti koje ste kopirali u dijaloškom okviru pregled kada ste stvorili vaša uloga. 

    c. Kliknite **dovrši** da biste zatvorili dijaloški okvir **Dodavanje atributa korisnika** .

23. Kliknite **Dodavanje korisnika atribut**. 

    ![Konfiguriranje jedinstvenu prijavu][23]


24. U dijaloškom okviru Dodavanje atributa korisnik poduzeti sljedeće korake. 

    ![Konfiguriranje jedinstvenu prijavu][25]


     na. U tekstni okvir **Naziv atributa** upišite **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. U tekstni okvir **Vrijednost atributa** upišite ili odaberite **user.userprincipalname** s padajućeg popisa.
     
    ![Konfiguriranje jedinstvenu prijavu][35]
    

     c. Kliknite **dovrši** da biste zatvorili dijaloški okvir **Dodavanje atributa korisnika** .


25. Kliknite **Primijeni promjene**. 

    ![Konfiguriranje jedinstvenu prijavu][26]





### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test

Cilj ovaj odjeljak je da biste stvorili testnih korisnika u Azure klasični portalu naziva dan Britta Simona.

![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**. 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.
  2. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.
  3. Kliknite Dalje.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: 

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U na **Prezime** txtbox, vrsta **Dan Simona**.
  
    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.
  
    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.
  
    b. Kliknite **dovrši**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Stvaranje testnih korisnika servisa Amazon Web (AWS)

Cilj ovaj odjeljak je da biste stvorili korisnik naziva dan Britta Simona u servisa Amazon Web (AWS).

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Da biste stvorili korisnik naziva dan Britta Simona u servisa Amazon Web (AWS), učinite sljedeće:

1. Prijavite se na web-mjesto tvrtke **Servisa Amazon Web (AWS)** kao administrator.

2. Kliknite ikonu **Polazno konzolu** . 

    ![Konfiguriranje jedinstvenu prijavu][11]

3. Kliknite identiteta i pristup upravljanje. 

    ![Konfiguriranje jedinstvenu prijavu][28]

4. Na nadzornoj ploči kliknite korisnici, a zatim stvorite novi korisnici. 

    ![Konfiguriranje jedinstvenu prijavu][29]

5. U dijaloškom okviru Create User poduzeti sljedeće korake: 

    ![Konfiguriranje jedinstvenu prijavu][30]

     na. U tekstne okvire **Unesite imena korisnika** , upišite korisničko ime za dan Brita Simona (userprincipalname) Azure AD.

     b. Kliknite **Stvori**.




### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta da biste koristili Azure jedinstvenu prijavu dozvolite pristup da biste servisa Amazon Web (AWS).

![Dodijeli korisniku][31]

**Da biste dodijelili dan Britta Simona CloudPassage, učinite sljedeće:**

1. Azure portala za klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][26]

2. Na popisu aplikacija odaberite **Servisa Amazon Web (AWS)**.

    ![Dodijeli korisniku][27]

1. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][25]

1. Na popisu korisnika odaberite **Dan Britta Simona**.

2. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][29]

### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.  
Kada kliknete pločicu servisa Amazon Web (AWS) na ploči za pristup, koje treba se automatski prijavljeni na u aplikaciju servisa Amazon Web (AWS).


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















