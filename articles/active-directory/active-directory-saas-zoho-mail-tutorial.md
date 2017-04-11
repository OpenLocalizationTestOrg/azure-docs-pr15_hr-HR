<properties 
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Zoho pošte | Microsoft Azure" 
    description="Saznajte kako koristiti Zoho pošta s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!." 
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
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Praktični vodič: Azure Active Directory Integracija s Zoho pošte
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i Zoho pošta.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Pošta Zoho klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili Zoho pošta će moći znak u aplikaciju na vaše Zoho pošte web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za e-poštu Zoho
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenarij")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Omogućivanje integraciju aplikacija za e-poštu Zoho
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za poštu Zoho.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za poštu Zoho, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **Zoho pošta**.

    ![Galerija aplikacije] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **Zoho pošta**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![Zoho pošte] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho pošte")

##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru Zoho poštu s računa u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Kao dio ovog postupka, koje su potrebne za stvaranje datoteke osnovni 64 kodiranih potvrda.  
Ako niste upoznati s taj postupak, potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Pošta Zoho** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili Zoho pošte** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** , učinite sljedeće:

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Konfiguriranje URL adresa Web App")

    na. U tekstni okvir **Zoho pošta na URL za prijavu** upišite URL pomoću sljedećeg uzorka:`http://<company name>.ZohoMail.com`

    b. Kliknite **Dalje**.


4.  Na stranici **Konfiguracija jedinstvenu prijavu na Zoho pošta** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke Zoho poštu kao administrator.

6.  Otvorite **upravljačku ploču**.

    ![Upravljačka ploča] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Upravljačka ploča")

7.  Kliknite karticu **SAML provjeru autentičnosti** .

    ![Provjera autentičnosti SAML] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "Provjera autentičnosti SAML")

8.  U odjeljku **Pojedinosti o autentičnosti SAML** poduzeti sljedeće korake:

    ![Detalji o SAML provjere autentičnosti] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Detalji o SAML provjere autentičnosti")

    1.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Zoho pošte** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **URL za prijavu** .
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Zoho pošte** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **URL odjavite** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na Zoho pošte** kopirajte vrijednost **URL Promijeni lozinku** i pa ih zalijepite u tekstni okvir **URL Promijeni lozinku** .
    4.  Da biste stvorili datoteku **Osnovni 64 kodirani** iz preuzete certifikata.  

        >[AZURE.TIP] Dodatne informacije potražite u članku [kako pretvoriti binarni certifikat u tekstnu datoteku](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otvorite certifikata kodirana osnovni 64 u Bloku za pisanje, kopirajte na sadržaj u međuspremnik, a zatim je zalijepite u tekstni okvir **PublicKey** .
    6.  Kao **algoritam**, odaberite **RSA**.
    7.  Kliknite **u redu**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Konfiguriranje jedinstvenu prijavu")

##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u Zoho pošta, oni mora biti dodijeljena u Zoho Mail.  
U slučaju Zoho pošta, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se na web-mjesto tvrtke **Zoho poštu** kao administrator.

2.  Idite na **Upravljačka ploča \> pošta i dokumenti**.

3.  Idite na **Detalji o korisniku \> Dodavanje korisnika**.

    ![Dodavanje korisnika] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Dodavanje korisnika")

4.  U dijaloškom okviru **Dodavanje korisnika** , poduzmite sljedeće korake:

    ![Dodavanje korisnika] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Dodavanje korisnika")

    1.  Upišite **ime**, **Prezime**, **ID e-pošte**, **lozinku** valjani Azure Active Directory računa želite dodjele resursa u povezani tekstni okviri.
    2.  Kliknite **u redu**.  

        >[AZURE.NOTE] Vlasnik računa Azure Active Directory primit će poruku e-pošte s vezom na postaje aktivan potvrditi račun.

>[AZURE.NOTE] Možete koristiti bilo koje druge Zoho pošte korisnički račun alate za stvaranje ili API-ji dobili poštom Zoho dodjele resursa AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Da biste korisnicima dodijelili Zoho pošte, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Pošta Zoho **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).