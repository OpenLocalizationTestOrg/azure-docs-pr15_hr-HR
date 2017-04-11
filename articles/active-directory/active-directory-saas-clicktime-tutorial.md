<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ClickTime | Microsoft Azure" 
    description="Saznajte kako koristiti ClickTime s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Praktični vodič: Azure Active Directory Integracija s ClickTime

U ovom ćete praktičnom vodiču saznati kako ClickTime integrirati Azure Active Directory (Azure AD).

Integriranje ClickTime s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup ClickTime
- Možete omogućiti svojim korisnicima da automatski se prijavili u da biste ClickTime (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacije Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija ClickTime, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Na ClickTime jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje ClickTime iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave

##<a name="adding-clicktime-from-the-gallery"></a>Dodavanje ClickTime iz galerije

Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ClickTime.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za ClickTime, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **ClickTime**.

    ![Galerija aplikacije] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **ClickTime**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s ClickTime utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u ClickTime određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u ClickTime Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u ClickTime.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s ClickTime, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje na ClickTime testiranje korisnika](#creating-a-clicktime-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u ClickTime koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ClickTime sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  


>[AZURE.IMPORTANT] Da biste mogli konfigurirati jedinstvenu prijavu na klijentu ClickTime, morate najprije obratite ClickTime za tehničku podršku da biste dobili Ova funkcija nije omogućena.

**Da biste konfigurirali Azure AD jedinstvenu prijavu ClickTime, poduzmite sljedeće korake:**

1.  Azure klasični portalu na stranici za integraciju aplikacije **ClickTime** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Omogućivanje jedinstvenu prijavu] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Omogućivanje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ClickTime** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Konfiguriranje jedinstvenu prijavu")

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    na. U tekstni okvir **IdentifierL** upišite URL-a pomoću sljedećeg uzorka: **https://app.clicktime.com/sp/**
    
    b. U tekstni okvir **URL odgovor** , unesite URL pomoću sljedećeg uzorka: **https://app.clicktime.com/Login/**

    c. Kliknite **Dalje**

4.  Na stranici **Konfiguracija jedinstvenu prijavu na ClickTime** da biste preuzeli certifikat, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Konfiguriranje jedinstvenu prijavu")

4.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke ClickTime kao administrator.

5.  Na alatnoj traci na vrhu kliknite **Postavke**, a zatim **Postavke sigurnosti**.

6.  U odjeljku Konfiguriranje **Jedan preference za prijavu** učinite sljedeće:

    ![Postavke sigurnosti] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Postavke sigurnosti")

    na.  Odaberite **Dopusti** prijavu pomoću jedne prijavu (SSO) za **Azure AD**.
    
    b.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ClickTime** kopirajte vrijednost **Jedne prijave URL servisa** i pa ih zalijepite u tekstni okvir **Krajnjoj točki davatelja identiteta** .

    c.  Otvorite certifikata kodirana osnovni 64 u **Bloku za pisanje**, kopirajte sadržaj, a zatim je zalijepite u tekstni okvir **Certifikat X.509** .
    
    d.  Kliknite **Spremi**.

7.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika

Da biste omogućili Azure AD korisnika da se prijavite u ClickTime, oni mora biti dodijeljena u ClickTime.  
U slučaju ClickTime, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **ClickTime** .

2.  Na alatnoj traci na vrhu kliknite **tvrtke**, a zatim **osobe**.

    ![Osobe] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Osobe")

3.  Kliknite **Dodavanje osoba**.

    ![Dodavanje osoba] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Dodavanje osoba")

4.  U odjeljku nove osobe poduzeti sljedeće korake:

    ![Osobe] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Osobe")

    na.  U tekstni okvir **Adresa e-pošte** upišite adresu e-pošte vašeg računa za Azure AD.
    
    b.  U tekstni okvir **ime i prezime** upišite naziv vašeg računa za Azure AD.  

    >[AZURE.NOTE] Ako želite, možete postaviti dodatna svojstva novi objekt osoba.

    c.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi ClickTime korisnički račun alate za stvaranje ili API-ji nudi ClickTime Dodjela Azure AD korisničke račune.

### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da njezinim dodjeljivanju ClickTime.

![Dodijeli korisniku][200]

Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

**Da biste dodijelili dan Britta Simona ClickTime, učinite sljedeće**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **ClickTime**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]

## <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu
U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu ClickTime na ploči programa Access, koje treba se automatski prijavljeni na u aplikaciji ClickTime.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png