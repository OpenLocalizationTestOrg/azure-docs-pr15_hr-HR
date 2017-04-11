<properties
    pageTitle="Praktični vodič: Azure Active Directory integracije sa softverom HR Cezanne | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Cezanne HR softvera."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Praktični vodič: Azure Active Directory integracije sa softverom HR Cezanne

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji Cezanne HR softver s Azure Active Directory (Azure AD).

Integriranje Cezanne HR softver s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Cezanne HR softver
- Možete omogućiti svojim korisnicima da automatski se prijavili u Cezanne HR softver (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD integracije sa softverom Cezanne HR, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na softver HR Cezanne jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
Cilj ovog praktičnog vodiča je omogućuju vam da biste testirali Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Cezanne HR softvera iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Dodavanje Cezanne HR softvera iz galerije
Da biste konfigurirali integraciju softvera HR Cezanne u Azure AD, morate dodati Cezanne HR softver iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Cezanne HR softver iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .
    
    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Cezanne HR softvera**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. U oknu rezultata odaberite **Cezanne HR softver**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![U galeriji odabir aplikacije](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
Cilj ovaj odjeljak je da bi se prikazala konfiguriranje i testiranje Azure AD jedinstvenu prijavu softverom HR Cezanne utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba određuje korisnika postoji zamjena u obliku u Cezanne HR softver korisniku u Azure AD. Drugim riječima, veza odnos između programa Azure AD korisnik i povezana softverom HR Cezanne mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Cezanne HR softvera.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu sa softverom Cezanne HR, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje softverski HR Cezanne testiranje korisnika](#creating-a-cezanne-hr-software-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Cezanne HR softver koji je povezan s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Cezanne HR softvera.

**Da biste konfigurirali Azure AD jedinstvenu prijavu Cezanne HR softver, poduzmite sljedeće korake:**

1. Na portalu klasični, na stranici za integraciju aplikacije **Cezanne HR softver** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Cezanne HR softver** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    na. U tekstni okvir **Prijavite se na URL** unesite URL pomoću sljedećeg uzorka: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. U tekstni okvir **URL odgovor** , unesite URL pomoću sljedećeg uzorka: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Kliknite **Dalje**

    > [AZURE.NOTE] Uzmite u obzir morate ažurirati te vrijednosti s stvarni na URL za prijavu i URL ADRESE za odgovor. Da biste dobili te vrijednosti, obratite se timu za podršku Cezanne HR softver putem <mailto:info@cezannehr.com>.

4. Na stranici **Konfiguracija jedinstvenu prijavu na Cezanne HR softver** poduzeti sljedeće korake, a zatim kliknite **Dalje**:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    na. Kliknite **Preuzmite certifikat**, a zatim spremite datoteku na računalu.

    b. Kliknite **Dalje**.

5. U prozoru preglednika drugoj web prijave za vaš klijent Cezanne HR softver kao administrator.

6. U lijevom navigacijskom oknu kliknite **Instalacijski program sustava**. Idite na **postavke sigurnosti**. Zatim pronađite **Jedan konfiguracije za prijavu**.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. U okvir **Dopusti korisnicima da biste se prijavili pomoću sljedećih servisa jedan prijavu (SSO)** ploča potvrdite okvir **SAML 2.0** i odaberite mogućnost **Napredna konfiguracija** kvačica.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Kliknite gumb **Dodaj novi** .

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. U odjeljku **Davatelji IDENTITETA za SAML 2.0** poduzeti sljedeće korake.

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    na. Unesite naziv davatelja identiteta kao **Zaslonsko ime**.

    b. Tekstni okvir u **Entitet identifikator** stavite vrijednost **Entitet** ID-a iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    c. Promijenite **SAML povezivanje** "OBJAVI".

    d. U **Krajnju točku sigurnosni Token usluge** tekstni okvir postavite vrijednost od **Jednog prijave URL servisa** iz čarobnjaka za konfiguraciju Azure AD aplikacije.

    e. Unesite "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" **Naziv atributa ID korisnika**.

    f. Kliknite **Prijenos** ikona prijenos certifikata preuzete iz Azure AD.

    g. Kliknite gumb **u redu** . 

10. Kliknite gumb **Spremi** .

    ![Konfiguriranje strani jednom prijava u aplikaciju](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

12. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
    
    ![Azure AD jedinstvene prijave][11]



### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
Cilj ovaj odjeljak je da biste stvorili testnih korisnika na portalu klasični naziva dan Britta Simona.

![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični Portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U tekstni okvir **Prezime** upišite **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:
    
    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Stvaranje korisnika test Cezanne HR softver

Da biste omogućili Azure AD korisnika da se prijavite u Cezanne HR softver, oni mora biti dodijeljena u Cezanne HR softver. U slučaju Cezanne HR softver, dodjeljivanje jest zadatak za ručno.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Dodjela korisnički račun, učinite sljedeće:

1.  Prijavite se u web-mjesto tvrtke Cezanne HR softver kao administrator.

2.  U lijevom navigacijskom oknu kliknite **Instalacijski program sustava**. Otvorite **Upravljanje korisnicima**. Zatim pronađite **Dodaj novog korisnika**.

    ![Novi korisnik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Novi korisnik")

3.  U odjeljku s **Pojedinostima osoba** izvođenje ispod korake:

    ![Novi korisnik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Novi korisnik")

    na. Postavljanje **internog korisnika** kao ISKLJUČENO.

    b. U tekstni okvir **ime** upišite **Britta**.  

    c. U tekstni okvir **Prezime** upišite **Dan Simona**.

    d. U tekstni okvir **e-pošte** upišite adresu e-pošte računa dan Britta Simona.

4.  U odjeljku **Podaci o računu** izvođenje ispod korake:

    ![Novi korisnik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Novi korisnik")

    na. U tekstni okvir **korisničko ime** unesite adresu e-pošte dan Britta Simona.

    b. U tekstni okvir **Lozinka** upišite lozinku za račun dan Britta Simona.

    c. Odaberite **HR Professional** kao **sigurnosnu ulogu**.

    d. Kliknite **u redu**.

5. Idite na karticu za **Jedinstvenu prijavu** , a zatim odaberite **Dodaj novo** u području za **Identifikatore SAML 2.0** .

    ![Korisnik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Korisnik")

6. Odaberite davatelja identiteta za **Davatelja identiteta** i u tekstni okvir **Identifikator korisnika**, unesite adresu e-pošte računa dan Britta Simona.

    ![Korisnik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Korisnik")
    
7. Kliknite gumb **Spremi** .

    ![Korisnik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Korisnik")


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

Cilj ovaj odjeljak je omogućavanja Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju Cezanne HR softver.
    
![Dodijeli korisniku][200]

**Da biste dodijelili dan Britta Simona Cezanne HR softver, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.
    
    ![Dodijeli korisniku][201]

2. Na popisu aplikacija odaberite **Cezanne HR softvera**.
    
    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. Na izborniku na vrhu kliknite **korisnicima**.
    
    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.
    
    ![Dodijeli korisniku][205]

### <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

Cilj ovaj odjeljak je testiranju Azure AD jedan prijave konfiguracije pomoću ploče programa Access.
 
Kada kliknete pločicu Cezanne HR softver na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji Cezanne HR softvera.

## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
