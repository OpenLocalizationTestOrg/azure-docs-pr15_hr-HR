<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Qlik smislu Enterprise | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Qlik smislu Enterprise."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Praktični vodič: Azure Active Directory Integracija s Qlik smislu Enterprise

U ovom ćete praktičnom vodiču, Saznajte kako integrirati Qlik smislu Enterprise Azure Active Directory (Azure AD).

Integriranje Qlik smislu Enterprise s Azure AD pruža sljedeće prednosti:

- Možete kontrolirati u Azure AD tko ima pristup Qlik smislu Enterprise
- Možete omogućiti svojim korisnicima da automatski se prijavili u na Qlik smislu Enterprise (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu – Azure klasični portal

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste konfigurirali Azure AD Integracija Qlik smislu Enterprise, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- U Qlik smislu Enterprise jedinstvene prijave omogućeno pretplate na


> [AZURE.NOTE] Da biste testirali korake ovog praktičnog vodiča, ne preporučujemo korištenje radnog okruženja.


Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenarij opis
U ovom ćete praktičnom vodiču testirajte Azure AD jedinstvenu prijavu u okruženje za testiranje.

Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od dva glavna sastavni blokovi:

1. Dodavanje Qlik uvid u tvrtki ili ustanovi iz galerije
2. Konfiguriranje i testiranje Azure AD jedan prijave


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Dodavanje Qlik uvid u tvrtki ili ustanovi iz galerije
Da biste konfigurirali integraciju Enterprise Qlik uvid u Azure AD, morate dodati Qlik smislu Enterprise iz galerije na popis upravljani SaaS aplikacija.

**Da biste dodali Qlik smislu Enterprise iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory][1]
2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Qlik smislu Enterprise**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. U oknu s rezultatima odaberite **Qlik smislu Enterprise**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfiguriranje i testiranje Azure AD jedan prijave
U ovom ćete odjeljku Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Qlik smislu Enterprise utemeljeno na korisniku testa pod nazivom "Britta Dan Simona".

Za jedinstvenu prijavu raditi, Azure AD treba korisnika postoji zamjena u obliku u Qlik smislu Enterprise određuje korisniku u Azure AD. Drugim riječima, veza odnosa između tablica i povezane korisnika u Qlik smislu Enterprise Azure AD korisnik mora uspostaviti.

Taj odnos veza je uspostavljena dodjelom vrijednost **korisničko ime** u Azure AD kao vrijednost **korisničko ime** u Qlik smislu Enterprise.

Konfiguriranje i testiranje Azure AD jedinstvenu prijavu s Qlik smislu Enterprise, morate dovršiti sljedeće sastavni blokovi:

1. **[Konfiguriranje Azure AD jedinstvenu prijavu](#configuring-azure-ad-single-sign-on)** – da biste omogućili korisnicima da biste koristili ovu značajku.
2. **[Stvaranje Azure AD testiranje korisnika](#creating-an-azure-ad-test-user)** – da biste testirali Azure AD jedinstvenu prijavu s dan Britta Simona.
3. **[Stvaranje Enterprise za odgovara Qlik testiranje korisnika](#creating-a-qliksense-enterprise-test-user)** – da bi je postoji zamjena u obliku od dan Britta Simona u Qlik smislu tvrtke koja je povezana s predstavljanje Azure AD njegove.
4. **[Dodjela Azure AD testiranje korisnika](#assigning-the-azure-ad-test-user)** – da biste omogućili Dan Simona Britta da biste koristili Azure AD jedinstvenu prijavu.
5. **[Testiranje jedinstvenu prijavu](#testing-single-sign-on)** – da biste provjerili funkcionira li konfiguracije.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvenu prijavu

U ovom ćete odjeljku Omogućivanje Azure AD jedinstvenu prijavu na portalu klasični i konfiguriranje jedinstvenu prijavu u aplikaciji Qlik smislu Enterprise.


**Da biste konfigurirali Azure AD jedinstvenu prijavu Qlik smislu Enterprise, učinite sljedeće:**

1. Na portalu klasični, na stranici Integracija **Qlik smislu Enterprise** aplikacije, kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .
     
    ![Konfiguriranje jedinstvenu prijavu][6] 

2. Na stranici **kako biste željeli korisnika da biste se prijavili Qlik smislu Enterprise** odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Na stranici dijaloški okvir **Konfiguriranje postavki aplikacije** poduzeti sljedeće korake:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    na. U tekstni okvir **Prijavite se na URL** unesite URL koji se koristi korisnika za prijavu u aplikaciju Qlik smislu Enterprise pomoću sljedećeg uzorka: **https://\<Qlik uvid u potpunosti Qualifed naziv glavnog računala\>: 443 / < virtualne prefiks Proxy\>/samlauthn/**.
    
    > [AZURE.NOTE] Imajte na umu završne kosa crta na kraju ovog URI.  Potreban je.

    b. Kliknite **Dalje**
 
4. Na stranici **Konfiguracija jedinstvenu prijavu na Qlik smislu Enterprise** , učinite sljedeće:

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    na. Kliknite **Preuzmi metapodatke**, a zatim spremite datoteku na računalu.  Se pripremite da biste uredili tu datoteku metapodataka prije prijenosa na poslužitelj Qlik odgovara.

    b. Kliknite **Dalje**.

5. Priprema vanjskog pristupa metapodataka XML datoteke tako da možete prenijeti koji odgovara Qlik poslužitelj.

    > [AZURE.NOTE] Prije prijenosa metapodataka IdP poslužitelj odgovara Qlik datoteku treba urediti da biste uklonili podatke iz radi pravilnog između Azure AD i Qlik smislu poslužitelja.

    ![QlikSense][qs24]

    na. Otvorite datoteku FederationMetaData.xml preuzete iz Azure u uređivaču teksta.

    b. Traženje vrijednosti **RoleDescriptor**.  Pojavit će se četiri stavke (dva parove otvorenih i zatvorenih element oznake).

    c. Brisanje oznaka RoleDescriptor i sve podatke u međuvremenu iz datoteke.

    d. Spremite datoteku i nastavak blizini koristiti kasnije u ovom dokumentu.

6. Pomaknite se da biste u Qlik smislu Qlik upravljanja konzole (QMC) kao korisnik možete stvoriti virtualne proxy konfiguracije.

7. U QMC, kliknite stavku izbornika virtualne proxy poslužitelja.

    ![QlikSense][qs6] 

8. Pri dnu zaslona kliknite gumb Stvori novu.

    ![QlikSense][qs7]

9. Pojavit će se zaslon za uređivanje virtualne proxy.  Na desnoj strani zaslona je izbornik za podnošenje mogućnosti konfiguracije vidljivi.

    ![QlikSense][qs9]

10. Mogućnost za identifikaciju izbornika potvrđen okvir, unesite opisne informacije za konfiguraciju Azure virtualne proxy poslužitelja.

    ![QlikSense][qs8]  
    
    na. Polje opisa je neslužbeni naziv za konfiguraciju virtualne proxy.  Unesite vrijednost za opis.
    
    b. Polje prefiks označava krajnju točku virtualne proxy za povezivanje s Qlik smislu s Azure AD jedinstvenu prijavu.  Unesite naziv jedinstveni prefiks ovaj virtualni proxy poslužitelja.

    c. Vremensko ograničenje neaktivnosti (u minutama) je vremenskog ograničenja za veze do ovaj virtualni proxy poslužitelja.

    d. U sesiju kolačića zaglavlja je naziv kolačića pohrani sesiju identifikator za sesiju odgovara Qlik korisnik prima nakon uspješne provjere autentičnosti.  Taj naziv mora biti jedinstvena.

11. Kliknite na izborniku Mogućnosti provjere autentičnosti da bi se prikazala.  Pojavit će se zaslon za provjeru autentičnosti.

    ![QlikSense][qs10]

    na. Padajući izbornik **način pristupa anonimni** određuje ako anonimni korisnici mogu pristupiti Qlik smislu putem virtualne proxyja.  Zadana mogućnost je bez anonimni korisnik.

    b. Padajući izbornik **način provjere autentičnosti** određuje shemu za provjeru autentičnosti virtualne proxy će koristiti.  S padajućeg popisa odaberite SAML.  Dodatne mogućnosti pojavit će se kao rezultat.

    c. U **SAML glavno računalo URI polja**za unos će korisnici naziv glavnog računala unesite pristupiti Qlik smislu putem virtualne proxy poslužitelja u ovom SAML.  Glavnog računala je uri odgovara Qlik poslužitelja.

    d. U polju **ID SAML entitet**, unesite iste upisana polje URI SAML glavnog računala.

    e. **SAML IdP metapodataka** je datoteka uređivati neke starije verzije u odjeljku **Uređivanje vanjski pristup metapodataka iz Azure AD konfiguracije** .  Da biste uklonili podatke iz radi pravilnog između Azure AD **prije prijenosa metapodataka IdP datoteku treba urediti** i Qlik smislu poslužitelja.  **Pogledajte gore navedene upute ako je datoteka još za uređivanje.**  Ako datoteku uredio kliknite gumb Pregledaj i odaberite datoteku uređivati metapodataka da biste prenijeli na konfiguraciju virtualne proxy.

    f. Unesite naziv atributa ili referenca sheme SAML atributa koji predstavlja **korisnički ID** Azure AD će poslati poslužitelj Qlik odgovara.  Informacije o shemi referenca dostupan je u konfiguracije objave zaslonima Azure aplikacije.  Da biste koristili naziv atributa, **Unesite http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.

    g. Unesite vrijednost za **imenika korisnika** bit će priložene korisnicima kada ih autentičnost s poslužiteljem Qlik smislu putem Azure AD.  Koji nisu vrijednosti mora staviti u **uglate zagrade []**.  Da biste koristili šalje u pridruživanju Azure AD SAML atribut, unesite naziv atributa u ovom tekstni okvir **bez** uglatih zagrada.

    h. **Potpisivanje algoritam SAML** postavlja za konfiguraciju virtualne proxy za potpisivanje certifikata servisa davatelja (u ovom slučaju Qlik smislu server).  Ako poslužitelj odgovara Qlik koristi pouzdana ustanova generira pomoću RSA Poboljšana Microsoft i AES Cryptographic Provider, promijenite algoritam potpisivanja SAML **SHA 256**.

    li. Odjeljak za mapiranje atributa SAML omogućuje atribute kao što su grupe slati Qlik osjećaj za korištenje u sigurnosna pravila.

12. Kliknite mogućnost izbornika da bi se prikazala za ujednačavanje opterećenja.  Pojavit će se zaslon opterećenja.

    ![QlikSense][qs11]

13. Kliknite Dodaj novi poslužitelj čvor gumb, odaberite modul čvor ili čvorove odgovara Qlik će poslati sesija za potrebe za ujednačavanje opterećenja i kliknite gumb Dodaj.

    ![QlikSense][qs12]

14. Kliknite mogućnost izbornika Dodatno da bi se prikazala. Pojavit će se zaslon Dodatno.

    ![QlikSense][qs13]

    na. Glavno računalo bijeli popis označava hostnames koji su prihvatili prilikom povezivanja s poslužiteljem Qlik odgovara.  **Unesite naziv glavnog računala korisnici će odrediti prilikom povezivanja s poslužiteljem Qlik odgovara.** Glavnog računala je iste vrijednosti kao uri glavno računalo SAML bez na https://.

15. Kliknite gumb Primijeni.

    ![QlikSense][qs14]

16. Kliknite u redu da biste prihvatili poruku s upozorenjem koja navodi proxy poslužitelji povezane s virtualne proxy će se ponovno pokrenuti.

    ![QlikSense][qs15]

17. Na desnoj strani zaslona, pojavit će se izbornik Associated stavki.  Kliknite mogućnost izbornika proxy poslužitelja.

    ![QlikSense][qs16]

18. Pojavit će se zaslon proxy poslužitelja.  Kliknite gumb veze pri dnu da biste se povezali proxy virtualne proxy poslužitelja.

    ![QlikSense][qs17]

19. Odaberite čvor proxy koji će podržavaju ovaj virtualni proxy vezu i kliknite gumb vezu.  Nakon povezivanja, proxy će se prikazati u odjeljku povezani proxy poslužitelja.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Nakon oko pet do deset sekundi, prikazat će se poruka osvježavanje QMC.  Kliknite gumb Osvježi QMC.

    ![QlikSense][qs20]

21. Kad u QMC osvježava, kliknite stavku izbornika virtualne proxy poslužitelja. Novi unos proxy virtualne SAML nalazi se u tablici na zaslonu.  Jednim klikom na stavku virtualne proxy.

    ![QlikSense][qs51]

22. Gumb za preuzimanje SP metapodataka će aktivirati pri dnu zaslona.  Kliknite gumb Preuzmi SP metapodataka da biste spremili metapodatke u datoteku.

    ![QlikSense][qs52]

23. Otvorite datoteku sp metapodataka.  Pridržavajte se stavka **entityID** i **AssertionConsumerService** stavke.  Ove vrijednosti se ne razlikuju **identifikator** i na **prijavite se na URL-** u konfiguraciji aplikacije Azure AD. Ako se ne podudaranje trebali biste ih zamijeniti čarobnjaka za konfiguraciju Azure AD aplikacije.

    ![QlikSense][qs53]

24. Na portalu klasični odaberite potvrdu jedan konfiguracije za prijavu pa zatim kliknite **Dalje**.
    
    ![Azure AD jedinstvene prijave][10]

25. Na stranici za **potvrdu jedan prijave** kliknite **dovrši**.  
 
    ![Azure AD jedinstvene prijave][11]


### <a name="creating-an-azure-ad-test-user"></a>Stvaranje Azure AD korisnik test
U ovom ćete odjeljku Stvaranje test korisnika na portalu klasični naziva dan Britta Simona.


![Stvaranje Azure AD korisnika][20]

**Da biste stvorili testnih korisnika u Azure AD, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste prikazali popis korisnika, na izborniku na vrhu, kliknite **korisnicima**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Da biste otvorili dijaloški okvir **Dodavanje korisnika** na alatnoj traci na dnu, kliknite **Dodaj korisnika**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Na stranici dijaloški okvir **Recite nam o korisniku** , učinite sljedeće:  ![stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    na. Kao vrstu korisnika, odaberite novi korisnik u tvrtki ili ustanovi.

    b. U korisničko ime **tekstni okvir**upišite **BrittaSimon**.

    c. Kliknite **Dalje**.

6.  Na stranici dijaloški **Korisničkog profila** , učinite sljedeće: ![stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    na. U tekstni okvir **ime** upišite **Britta**.  

    b. U zadnji tekstni okvir **Naziv** , vrstu **Dan Simona**.

    c. U tekstni okvir **Zaslonski naziv** upišite **Dan Britta Simona**.

    d. Na popisu **uloga** odaberite **korisnik**.

    e. Kliknite **Dalje**.

7. Na stranici dijaloški okvir **Dohvati privremenu lozinku** , kliknite **Stvori**.

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Na stranici za **Početak privremenu lozinku** dijaloški poduzeti sljedeće korake:

    ![Stvaranje Azure AD korisnik test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    na. Zapišite vrijednost **Novu lozinku**.

    b. Kliknite **dovrši**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Stvaranje korisnik testiranje sustava Qlik smislu Enterprise

U ovom ćete odjeljku stvoriti korisnik naziva dan Britta Simona u Qlik smislu Enterprise. Raditi Qlik smislu Enterprise timu za podršku da biste dodali korisnike u platformu Qlik smislu Enterprise.


### <a name="assigning-the-azure-ad-test-user"></a>Dodjela korisnika test Azure AD

U ovom ćete odjeljku Omogućivanje Dan Simona Britta koristiti Azure jedinstvenu prijavu tako da svoj dodjeljivanju Qlik smislu Enterprise.

![Dodijeli korisniku][200] 

**Da biste dodijelili dan Britta Simona Qlik smislu Enterprise, učinite sljedeće:**

1. Na portalu klasični da biste otvorili prikaz aplikacija u prikazu direktorija, kliknite **aplikacije** u gornji izbornik.

    ![Dodijeli korisniku][201] 

2. Na popisu aplikacija odaberite **Qlik smislu Enterprise**.

    ![Konfiguriranje jedinstvenu prijavu](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. Na izborniku na vrhu kliknite **korisnicima**.

    ![Dodijeli korisniku][203]

4. Na popisu korisnika odaberite **Dan Britta Simona**.

5. Na alatnoj traci na dnu kliknite **Dodijeli**.

    ![Dodijeli korisniku][205]


## <a name="testing-single-sign-on"></a>Testiranje jedinstvenu prijavu

U ovom ćete odjeljku testirajte Azure AD jedan prijave konfiguraciju pomoću ploče programa Access.

Kada kliknete pločicu Enterprise Qlik uvid u oknu programa Access, koje treba se automatski prijavljeni na u aplikaciji Qlik smislu Enterprise.


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png