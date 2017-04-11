<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Intacct | Microsoft Azure" 
    description="Saznajte kako koristiti Intacct s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Praktični vodič: Azure Active Directory Integracija s Intacct
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Intacct.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Intacct klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Intacct će moći znak u aplikaciju na vašem Intacct web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za Intacct
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Scenarij")
##<a name="enabling-the-application-integration-for-intacct"></a>Omogućivanje integraciju aplikacija za Intacct
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za Intacct.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za Intacct, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Intacct**.

    ![Galerija aplikacije] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Intacct**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti Intacct sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Intacct** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Intacct** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , u tekstni okvir **Intacct na URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://Intacct.com/company*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na Intacct** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Intacct kao administrator.

6.  Kliknite karticu **tvrtka** , a zatim **Podaci o tvrtki**.

    ![Tvrtke] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Tvrtke")

7.  Kliknite karticu **Sigurnost** , a zatim kliknite **Uredi**.

    ![Sigurnost] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Sigurnost")

8.  U odjeljku **znak (SSO)** , učinite sljedeće:

    ![Jedine prijave] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Jedine prijave")

    1.  Odaberite **Omogući znak na**.
    2.  Kao **Vrsta davatelja identiteta**, odaberite **SAML 2.0**.
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Intacct** kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u tekstni okvir **URL izdavač** .
    4.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Intacct** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    5.  Da biste stvorili datoteku **Osnovni 64 kodiran** iz preuzete certifikata.
        
        >[AZURE.TIP]Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir za **potvrdu**
    7.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Intacct, oni mora biti dodijeljena u Intacct.  
U slučaju Intacct, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **Intacct** .

2.  Kliknite karticu **tvrtka** , a zatim kliknite **korisnici**.

    ![Korisnici] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Korisnici")

3.  Kliknite karticu **Dodaj**

    ![Dodavanje] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Dodavanje")

4.  U odjeljku **Podaci o korisniku** učinite sljedeće:

    ![Podaci o korisniku] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Podaci o korisniku")

    1.  Unesite **Korisnički ID**, **Prezime**, **ime**, **adresu e-pošte**, **Naslov** i **telefon** Azure AD račun koji želite dodjele resursa u povezane tekstne okvire.
    2.  Odaberite **administratorskim ovlastima** Azure AD račun koji želite dodjele resursa.
    3.  Kliknite **Spremi**.
        
        >[AZURE.NOTE] Vlasnik računa AAD će primiti poruku e-pošte i slijedite vezu da biste potvrdili svoj račun prije postaje aktivna.

>[AZURE.NOTE] Možete koristiti bilo koji drugi Intacct korisnički račun alate za stvaranje ili API-ji nudi Intacct dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Da biste korisnicima dodijelili Intacct, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Intacct **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).