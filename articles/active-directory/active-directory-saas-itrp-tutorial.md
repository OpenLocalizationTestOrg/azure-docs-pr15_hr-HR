<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s ITRP | Microsoft Azure" 
    description="Saznajte kako koristiti ITRP s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Praktični vodič: Azure Active Directory Integracija s ITRP
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i ITRP.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   ITRP klijenta
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili ITRP će moći znak u aplikaciju na vašem ITRP web-mjesto tvrtke (znak servisa davatelja pokrenut na) ili pomoću [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za ITRP
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Scenarij")
##<a name="enabling-the-application-integration-for-itrp"></a>Omogućivanje integraciju aplikacija za ITRP
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za ITRP.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za ITRP, poduzmite sljedeće korake:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **ITRP**.

    ![Galerija aplikacije] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **ITRP**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti ITRP sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za ITRP potrebno dohvatiti otisak prsta vrijednost iz certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **ITRP** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Konfiguriranje jedinstvenu prijavu")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili ITRP** odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **ITRP URL za prijavu** upišite URL pomoću sljedećeg uzorka "*https://\<klijentu naziv\>. ITRP.com*", a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na ITRP** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno kao **c:\\ITRP.cer**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke ITRP kao administrator.

6.  Na alatnoj traci na vrhu kliknite **Postavke**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  U lijevom navigacijskom oknu odaberite **Jedinstvenu prijavu**.

    ![Jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Jedinstvenu prijavu")

8.  U konfiguraciji odjeljku jedinstvenu prijavu poduzeti sljedeće korake:

    ![Jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Jedinstvenu prijavu")

    ![Jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Jedinstvenu prijavu")

    1.  Kliknite **Omogući**.
    2.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ITRP** kopirajte **URL daljinskog odjavite** vrijednost, a zatim je zalijepite u tekstni okvir **URL daljinskog odjavite** .
    3.  Azure klasični portalu na dijalošku stranicu **Konfiguriraj jedinstvenu prijavu na ITRP** kopirajte vrijednost **SAML SSO URL** i pa ih zalijepite u tekstni okvir **SAML SSO URL** .
    4.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir za **Potvrdu otiska prsta** .
        
        >[AZURE.TIP]Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    5.  Kliknite **Spremi**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u ITRP, oni mora biti dodijeljena u ITRP.  
U slučaju ITRP, dodjeljivanje jest zadatak za ručno.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Dodjela korisničkih računa, poduzmite sljedeće korake:

1.  Prijavite se u klijent **ITRP** .

2.  Na alatnoj traci na vrhu kliknite **zapisa**.

    ![Administrator] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Administrator")

3.  Na skočnom izborniku odaberite **osobe**.

    ![Osobe] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Osobe")

4.  Kliknite **Dodavanje nove osobe** ("+").

    ![Administrator] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Administrator")

5.  U dijaloškom okviru Dodavanje nove osobe poduzeti sljedeće korake:

    ![Korisnik] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Korisnik")

    1.  Upišite **naziv**, **e-pošte** valjan račun za AAD želite dodjele resursa.
    2.  Kliknite **Spremi**.

>[AZURE.NOTE] Možete koristiti bilo koji drugi ITRP korisnički račun alate za stvaranje ili API-ji nudi ITRP dodjele resursa AAD korisničkih računa.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Da biste korisnicima dodijelili ITRP, poduzmite sljedeće korake:

1.  Na portalu za Azure AD stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **ITRP **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).